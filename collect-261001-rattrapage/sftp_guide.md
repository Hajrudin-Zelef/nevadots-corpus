# Guide SFTP complet — OpenSSH, chroot, supervision et automatisation

**Public :** chef de service systèmes & énergies, administrateurs, techniciens d'exploitation.
**Objectif :** mettre en place, sécuriser, superviser et dépanner un serveur SFTP d'entreprise
(OpenSSH, Debian/Ubuntu) : dépôts clients, échanges comptables/bancaires, réception de scans de copieurs.
**Pré-requis :** notions d'administration Linux (Debian/Ubuntu), SSH, permissions Unix.

> Ce guide suit les bonnes pratiques « production » : clés uniquement, chroot systématique,
> pas de shell pour les comptes SFTP, journalisation détaillée, quotas, supervision et sauvegarde.

---

## 1. SFTP : de quoi parle-t-on ?

- **SFTP (SSH File Transfer Protocol)** n'a rien à voir avec FTP : c'est un sous-protocole de SSH (port 22/TCP).
- Tout transite dans un tunnel chiffré : authentification, commandes, données.
- Remplace avantageusement FTP/FTPS en entreprise : un seul port à ouvrir, un seul service à durcir.
- Le serveur SFTP d'OpenSSH se présente sous deux formes :
  - le binaire externe `sftp-server` ;
  - le serveur intégré `internal-sftp` (recommandé : fonctionne dans un chroot sans binaires externes).

| Protocole | Port(s) | Chiffrement | Recommandation |
|---|---|---|---|
| FTP | 21 + ports passifs | Aucun | À bannir |
| FTPS (FTP+TLS) | 21 + ports passifs | TLS | Toléré si partenaire l'impose (cf. § 62) |
| SFTP | 22 | SSH | **Recommandé** |
| SCP | 22 | SSH | Obsolète (utiliser SFTP) |
| rsync-over-SSH | 22 | SSH | Excellent pour synchro/miroir (cf. § 47) |

---

## 2. Glossaire rapide (version détaillée au § 74)

| Terme | Sens |
|---|---|
| chroot | Enfermer un utilisateur dans un sous-dossier qui devient sa racine `/` |
| subsystem | Sous-système SSH invoqué à la demande (ici `sftp`) |
| ForceCommand | Directive sshd qui impose la commande exécutée, quel que soit le client |
| Match | Bloc conditionnel dans `sshd_config` (par utilisateur, groupe, adresse…) |
| internal-sftp | Serveur SFTP intégré à sshd, sans dépendance externe |
| Batch mode | Mode non interactif de `sftp` (`-b fichier`) pour les scripts |
| Quota | Limite d'espace disque par utilisateur/groupe |
| Clé SSH | Paire cryptographique (publique/privée) pour s'authentifier sans mot de passe |

---

## 3. Architecture cible

```
/srv/sftp/                  <- racine des chroots (root:root, 0755)
├── client_acme/            <- chroot de l'utilisateur sftp_acme
│   ├── depot/              <- inscriptible par sftp_acme (dépôt entrant)
│   ├── restitutions/       <- inscriptible par l'exploitant, lecture seule pour le client
│   └── .ssh/               <- NON : les clés restent hors chroot !
├── compta/                <- chroot du service comptabilité
│   ├── envoi_banque/
│   └── recus_banque/
└── scans_copieurs/         <- chroot du compte technique des copieurs
    └── entrees/            <- les MFP y déposent les PDF numérisés
```

Principes :

1. **Un chroot par périmètre** (client, service, usage), jamais un chroot unique partagé.
2. Le répertoire chroot appartient à `root:root` en `0755` (exigence OpenSSH : chaque composant du chemin doit appartenir à root et ne pas être inscriptible par l'utilisateur).
3. Les dossiers de dépôt à l'intérieur appartiennent à l'utilisateur SFTP.
4. Les clés publiques sont gérées **hors chroot** (`/etc/ssh/authorized_keys/%u` ou `~/.ssh` si pas de chroot sur le home — voir § 18).

---

## 4. Installation d'OpenSSH Server (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
ssh -V            # vérifie la version, ex : OpenSSH_9.6p1
ss -tlnp | grep :22
```

Fichiers importants :

| Fichier | Rôle |
|---|---|
| `/etc/ssh/sshd_config` | Configuration du serveur |
| `/etc/ssh/sshd_config.d/` | Fragments de configuration (priorité selon l'ordre) |
| `/var/log/auth.log` | Journal d'authentification (Debian/Ubuntu) |
| `/var/log/syslog` | Journal système (si SyslogFacility AUTH par défaut) |

> Sauvegardez toujours `sshd_config` avant de le modifier :
> `sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak-$(date +%F)`

---

## 5. Vérifier la configuration avant de recharger

```bash
sudo sshd -t          # teste la syntaxe ; silencieux = OK
sudo sshd -T | grep -i -E 'chroot|forcecommand|subsystem|passwordauth'
sudo systemctl reload ssh
```

Règle d'or : **ne jamais fermer votre session SSH active** avant d'avoir testé une nouvelle
connexion dans un second terminal. Une erreur de `sshd_config` peut vous enfermer dehors.

---

## 6. Le subsystem SFTP : internal-sftp vs sftp-server

Dans `/etc/ssh/sshd_config` :

```
# Recommandé : serveur intégré, compatible chroot sans binaires
Subsystem sftp internal-sftp

# Ancienne forme (binaire externe) — à éviter avec chroot :
# Subsystem sftp /usr/lib/openssh/sftp-server
```

Pourquoi `internal-sftp` ?

- Aucune dépendance (libc, /dev/*) à recopier dans chaque chroot.
- Accepte les options `-f` (niveau de log) et `-l` (facility) directement.
- Se combine avec `ChrootDirectory` + `ForceCommand internal-sftp` sans bidouillage.

Exemple avec journalisation renforcée (voir § 50) :

```
Subsystem sftp internal-sftp -f AUTH -l VERBOSE
```

---

## 7. Créer le groupe sftpusers

```bash
sudo groupadd sftpusers
```

Tous les comptes « transfert uniquement » seront membres de ce groupe. Les blocs `Match Group sftpusers`
dans `sshd_config` appliqueront d'un coup : chroot, pas de shell, clés uniquement.

Convention de nommage proposée :

| Préfixe | Usage | Exemple |
|---|---|---|
| `sftp_` | Comptes clients/partenaires externes | `sftp_acme`, `sftp_banque` |
| `svc_` | Comptes techniques internes | `svc_copieur_etage2`, `svc_compta` |
| `adm_` | Comptes d'exploitation (hors chroot) | `adm_sftp` |

---

## 8. Créer un utilisateur SFTP sans shell

```bash
# Compte client ACME : pas de shell, pas de mot de passe, groupe sftpusers
sudo useradd -m -d /srv/sftp/client_acme -s /usr/sbin/nologin \
  -G sftpusers -c "Dépôt SFTP client ACME" sftp_acme
sudo passwd -l sftp_acme      # verrouille le mot de passe
```

- `-s /usr/sbin/nologin` : aucun shell interactif possible.
- `-G sftpusers` : rattache au groupe (groupe principal laissé à son groupe privé).
- `passwd -l` : verrouille l'authentification par mot de passe (les clés SSH restent utilisables).

Vérification :

```bash
id sftp_acme
getent passwd sftp_acme
```

---

## 9. Arborescence chroot correcte (permissions root:root)

```bash
sudo mkdir -p /srv/sftp/client_acme/{depot,restitutions}
sudo chown root:root /srv/sftp /srv/sftp/client_acme
sudo chmod 0755 /srv/sftp /srv/sftp/client_acme
sudo chown sftp_acme:sftpusers /srv/sftp/client_acme/depot
sudo chmod 0750 /srv/sftp/client_acme/depot
sudo chown root:sftpusers /srv/sftp/client_acme/restitutions
sudo chmod 0750 /srv/sftp/client_acme/restitutions
```

Règles OpenSSH (strictes, sinon la connexion échoue) :

- Chaque composant du chemin du `ChrootDirectory` doit appartenir à **root**.
- Aucun composant ne doit être **inscriptible** par l'utilisateur ou son groupe.
- À l'intérieur du chroot, créez des sous-dossiers dont l'utilisateur est propriétaire.

Test de conformité (script, voir § 71) :

```bash
namei -l /srv/sftp/client_acme
# tout, jusqu'à client_acme inclus : root root, sans bit w pour group/other
```

---

## 10. Bloc Match dans sshd_config (le cœur du dispositif)

```
# --- SFTP entreprise : comptes de transfert uniquement ---
Match Group sftpusers
    ChrootDirectory /srv/sftp/%u
    ForceCommand internal-sftp
    PasswordAuthentication no
    PermitTunnel no
    AllowAgentForwarding no
    AllowTcpForwarding no
    X11Forwarding no
```

> ⚠️ `%u` = nom d'utilisateur. Ici le chroot est `/srv/sftp/sftp_acme` — adaptez la convention :
> soit nommez les dossiers comme les utilisateurs, soit utilisez un `Match User` par compte
> avec un `ChrootDirectory` explicite (voir § 11).

Après modification :

```bash
sudo sshd -t && sudo systemctl reload ssh
```

Ordre de lecture : les blocs `Match` doivent figurer **à la fin** de `sshd_config`,
après toutes les directives globales.

---

## 11. Chroot par utilisateur (quand %u ne suffit pas)

Si le dossier chroot ne porte pas le nom de l'utilisateur :

```
Match User sftp_acme
    ChrootDirectory /srv/sftp/client_acme
    ForceCommand internal-sftp
    PasswordAuthentication no

Match User svc_copieur_etage2
    ChrootDirectory /srv/sftp/scans_copieurs
    ForceCommand internal-sftp
    PasswordAuthentication no
```

Et par groupe pour un périmètre commun :

```
Match Group sftp_compta
    ChrootDirectory /srv/sftp/compta
    ForceCommand internal-sftp
```

Un utilisateur peut appartenir à plusieurs groupes : le **premier** bloc `Match` correspondant gagne.

---

## 12. Clés SSH uniquement : génération côté client

```bash
# Sur le poste client / le serveur d'envoi
ssh-keygen -t ed25519 -f ~/.ssh/id_sftp_acme -C "sftp_acme@client"
# Protégez la clé privée par une phrase de passe pour un usage interactif ;
# pour un batch automatisé, clé dédiée SANS phrase de passe, restreinte (voir § 44).
```

Déposer la clé publique côté serveur (l'utilisateur n'a pas de home inscriptible) :

```bash
sudo mkdir -p /etc/ssh/authorized_keys
sudo sh -c 'cat > /etc/ssh/authorized_keys/sftp_acme'   # coller la clé publique, Ctrl+D
sudo chown root:root /etc/ssh/authorized_keys/sftp_acme
sudo chmod 0644 /etc/ssh/authorized_keys/sftp_acme
```

Dans `sshd_config` (global ou dans le bloc Match) :

```
AuthorizedKeysFile /etc/ssh/authorized_keys/%u
```

Alternative : `AuthorizedKeysFile .ssh/authorized_keys` avec un home **hors chroot**.

---

## 13. Tester la première connexion

```bash
# Depuis le client possédant la clé privée
sftp -i ~/.ssh/id_sftp_acme sftp_acme@sftp.entreprise.lan
```

Session type :

```
sftp> pwd
Remote working directory: /
sftp> ls -la
drwxr-xr-x  4 root     root     4096 ... .
drwxr-xr-x  4 root     root     4096 ... ..
drwxr-x---  2 sftp_acme sftpusers 4096 ... depot
drwxr-x---  2 root     sftpusers 4096 ... restitutions
sftp> cd depot
sftp> put rapport.pdf
Uploading rapport.pdf to /depot/rapport.pdf
sftp> ls -l
-rw-r--r-- 1 sftp_acme sftpusers 182340 ... rapport.pdf
sftp> bye
```

Points à valider : pas de `!` (shell) disponible, `cd /` reste dans le chroot,
impossible de remonter au-dessus.

---

## 14. Commandes SFTP interactives essentielles

| Commande | Effet |
|---|---|
| `pwd` / `lpwd` | Dossier distant / local courant |
| `ls [-la]` / `lls` | Lister distant / local |
| `cd` / `lcd` | Changer de dossier distant / local |
| `get fichier` / `put fichier` | Télécharger / envoyer un fichier |
| `get -r dossier` / `put -r dossier` | Récursif |
| `mkdir` / `rmdir` / `rm` | Créer/supprimer |
| `rename a b` | Renommer/déplacer |
| `chmod` / `chown` / `chgrp` | Permissions (si autorisées) |
| `df -h` | Espace disque visible du chroot |
| `progress` | Barre de progression |
| `reget` / `reput` | Reprise d'un transfert interrompu |
| `bye` / `exit` / `quit` | Quitter |

Options utiles de la commande `sftp` :

| Option | Effet |
|---|---|
| `-i clé` | Clé privée à utiliser |
| `-P port` | Port non standard |
| `-b fichier` | Batch : exécute les commandes du fichier |
| `-B taille` | Taille des buffers (gros fichiers) |
| `-R nbre` | Nombre de requêtes en vol (débit) |
| `-o Opt=Val` | Passe une option ssh_config |

---

## 15. Reprise de transfert (reget / reput)

```bash
sftp> reput gros_fichier.iso
# reprend là où le transfert s'était arrêté (ajoute à la fin du fichier partiel)
sftp> reget archive.zip
```

- `reput`/`reget` comparent la taille du fichier partiel distant/local et reprennent l'envoi.
- Indispensable sur liens instables ou pour de gros fichiers (ISO, sauvegardes, scans groupés).
- Avec `lftp`, la reprise est automatique (`mirror --continue`, voir § 42).

---

## 16. Forcer le chiffrement et vérifier l'empreinte du serveur

Premier contact : vérifiez l'empreinte (fingerprint) hors bande :

```bash
# Côté serveur : empreintes à communiquer au partenaire
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
sudo ssh-keygen -lf /etc/ssh/ssh_host_rsa_key.pub
# Format visuel "randomart" pour lecture au téléphone :
sudo ssh-keygen -lvf /etc/ssh/ssh_host_ed25519_key.pub
```

Côté client, au premier `sftp`, comparez l'empreinte affichée avec celle fournie
par l'exploitant (jamais « oui » aveugle en production).

---

## 17. Durcir sshd_config (global)

```
# --- Durcissement global ---
Port 22
Protocol 2
PermitRootLogin no
PasswordAuthentication no            # clés uniquement pour tout le monde
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
AllowAgentForwarding no
AllowTcpForwarding no
PermitTunnel no
MaxAuthTries 3
LoginGraceTime 30
ClientAliveInterval 300
ClientAliveCountMax 2
MaxSessions 5
```

> Si certains comptes admin ont encore besoin du mot de passe, ne rouvrez pas globalement :
> utilisez un `Match User adm_xxx` ciblé, ou mieux, déployez-leur des clés.

---

## 18. Où stocker les clés autorisées ?

Trois stratégies :

| Stratégie | AuthorizedKeysFile | Avantages | Inconvénients |
|---|---|---|---|
| A. Centralisé (recommandé) | `/etc/ssh/authorized_keys/%u` | Hors chroot, géré par root, audit simple | Un fichier par utilisateur à créer |
| B. Home hors chroot | `/home/%u/.ssh/authorized_keys` avec home hors chroot | Classique | Le home doit exister hors chroot |
| C. Dans le chroot | `%h/.ssh/authorized_keys` | — | **Déconseillé** : le chroot appartient à root, l'utilisateur ne peut pas gérer ses clés, et sshd exige des permissions strictes |

Mise en place de la stratégie A :

```bash
sudo install -d -o root -g root -m 0755 /etc/ssh/authorized_keys
# un fichier par compte, 0644 root:root (voir § 12)
```

---

## 19. Rotation des clés

Procédure :

1. Le partenaire génère une nouvelle paire et transmet la **publique** par canal authentifié.
2. Ajoutez-la en **plus** de l'ancienne dans `/etc/ssh/authorized_keys/<user>`.
3. Le partenaire teste la nouvelle clé.
4. Retirez l'ancienne clé du fichier.
5. Tracez l'opération (qui, quand, pourquoi) dans votre journal d'exploitation.

```bash
# Ajouter sans écraser :
sudo sh -c 'cat >> /etc/ssh/authorized_keys/sftp_acme'   # coller, Ctrl+D
sudo chmod 0644 /etc/ssh/authorized_keys/sftp_acme
# Vérifier qu'il y a bien 2 lignes, tester, puis supprimer l'ancienne :
sudo sed -i '/ancienne-cle-commentaire/d' /etc/ssh/authorized_keys/sftp_acme
```

Fréquence conseillée : annuelle, ou à chaque départ d'un interlocuteur / compromission suspectée.

---

## 20. Comptes techniques : copieurs multifonctions (MFP)

Cas métier : les copieurs/IMFP du parc déposent leurs numérisations sur le SFTP
(lien avec la maintenance des copieurs : le scan-to-SFTP remplace le scan-to-SMB,
plus sûr et sans compte AD à gérer sur chaque machine).

```bash
sudo useradd -r -d /srv/sftp/scans_copieurs -s /usr/sbin/nologin \
  -G sftpusers -c "Dépôt scans copieurs" svc_copieur
sudo passwd -l svc_copieur
sudo mkdir -p /srv/sftp/scans_copieurs/entrees
sudo chown root:root /srv/sftp/scans_copieurs && sudo chmod 0755 /srv/sftp/scans_copieurs
sudo chown svc_copieur:sftpusers /srv/sftp/scans_copieurs/entrees
sudo chmod 0730 /srv/sftp/scans_copieurs/entrees   # écriture seule + lecture du nom
```

Sur le copieur (ex. panneau Kyocera/équivalent) : protocole SFTP, port 22,
authentification par clé (coller la privée générée pour ce compte) ou login/mot de passe
**dédié** si le modèle ne gère pas les clés — dans ce cas, `Match User` avec
`PasswordAuthentication yes` ciblé (voir § 58).

Organisation conseillée : un sous-dossier par machine dans `entrees/` pour trier ensuite :

```
/srv/sftp/scans_copieurs/entrees/
├── etage1_accueil/
├── etage2_compta/
└── atelier/
```

---

## 21. Réception et tri automatisé des scans

Script de tri (toutes les 5 min via cron, ou déclenché par inotify) :

```bash
#!/bin/bash
# /usr/local/sbin/tri_scans.sh — classe les PDF par machine et par date
SRC=/srv/sftp/scans_copieurs/entrees
DST=/srv/partage/scans_tries
for machine in "$SRC"/*/; do
  m=$(basename "$machine")
  for f in "$machine"*.pdf "$machine"*.PDF; do
    [ -e "$f" ] || continue
    jour=$(date +%F)
    dest="$DST/$m/$jour"
    mkdir -p "$dest"
    mv "$f" "$dest/"
    logger -t tri_scans "classé $f -> $dest/"
  done
done
```

```cron
*/5 * * * * root /usr/local/sbin/tri_scans.sh
```

> Le script tourne en root (ou avec ACL sur `entrees/`) : le compte `svc_copieur`
> n'a accès qu'en écriture à son dépôt, il ne peut ni lire ni supprimer les scans des autres machines.

---

## 22. Échanges comptables et bancaires

Exigences typiques : confidentialité, traçabilité, horodatage, pas de suppression.

Arborescence :

```
/srv/sftp/compta/
├── envoi_banque/      # la compta dépose les fichiers de virement (écriture compta)
├── recus_banque/      # la banque dépose les relevés (écriture banque, lecture compta)
└── archives/          # écriture exploitant seul (gel des échanges)
```

Durcissement :

- Deux comptes distincts : `svc_compta` (interne) et `sftp_banque` (partenaire).
- Permissions croisées : chacun écrit dans son dossier, lit (sans écrire) dans celui de l'autre.
- Interdire la suppression : pas de droit `w` sur le dossier parent pour les comptes SFTP,
  ou utiliser l'attribut immuable sur les fichiers archivés (`chattr +i`).

```bash
sudo chown svc_compta:sftp_compta /srv/sftp/compta/envoi_banque
sudo chmod 0750 /srv/sftp/compta/envoi_banque
sudo chown sftp_banque:sftp_compta /srv/sftp/compta/recus_banque
sudo chmod 0750 /srv/sftp/compta/recus_banque
```

---

## 23. Dépôt de fichiers clients (espace d'échange)

Modèle « boîte aux lettres » par client :

- `depot/` : le client **écrit**, ne **relit pas** (0750, propriétaire client) — évite qu'un client
  voie les fichiers d'un autre et limite l'exfiltration.
- `restitutions/` : l'exploitant **écrit**, le client **lit** (0750, propriétaire root, groupe du client).

Checklist de création d'un espace client :

- [ ] Utilisateur `sftp_<client>` créé, shell nologin, mot de passe verrouillé
- [ ] Membre de `sftpusers`
- [ ] Chroot `/srv/sftp/<perimetre>` root:root 0755
- [ ] Sous-dossiers avec bons propriétaires/permissions
- [ ] Clé publique installée dans `/etc/ssh/authorized_keys/<user>`
- [ ] Test de connexion + `put`/`get` depuis un poste de test
- [ ] Quota appliqué (voir § 24)
- [ ] Fiche d'exploitation renseignée (contacts, volumétrie, rétention)

---

## 24. Quotas disque (project quota XFS ou quota ext4)

Sans quota, un client peut remplir le disque et bloquer tout le serveur.

**Option ext4 (classique)** :

```bash
sudo apt install -y quota quotatool
# activer dans /etc/fstab : ajouter usrquota,grpquota aux options de /srv
# UUID=... /srv ext4 defaults,usrquota,grpquota 0 2
sudo mount -o remount /srv
sudo quotacheck -cum /srv && sudo quotaon /srv
sudo setquota -u sftp_acme 10485760 11534336 0 0 /srv   # 10 Go soft, 11 Go hard
sudo repquota /srv
```

**Option XFS (project quotas, conseillée si /srv est en XFS)** :

```bash
# /etc/fstab : ...,pquota
sudo xfs_quota -x -c 'project -s -p /srv/sftp/client_acme 101' /srv
sudo xfs_quota -x -c 'limit -p bhard=10g 101' /srv
```

Surveillance : alerte si usage > 80 % (voir § 52).

---

## 25. Limiter la bande passante par utilisateur (optionnel)

`sshd_config` ne gère pas nativement la limitation de débit SFTP. Solutions :

- **trickle** côté client pour les envois planifiés.
- **QoS réseau** (tc) sur le serveur si un flux écrase les autres.
- En pratique : les quotas + fenêtres de transfert planifiées (cron) suffisent le plus souvent.

---

## 26. Chiffrement au repos (si données sensibles)

Le chiffrement SSH ne protège que le **transport**. Pour les données au repos :

| Solution | Cas d'usage |
|---|---|
| LUKS sur le volume `/srv` | Vol/perte du serveur, mise au rebut des disques |
| eCryptfs / fscrypt | Chiffrement par dossier (plus complexe à opérer) |
| Chiffrement applicatif (GPG) par le partenaire | Secret de bout en bout, le serveur ne voit que du chiffré |

Exemple GPG côté partenaire avant dépôt :

```bash
gpg --encrypt --recipient compta@entreprise.lan virements.csv
# déposer virements.csv.gpg sur le SFTP
```

---

## 27. Sauvegarde des dépôts SFTP

Ce qui doit être sauvegardé :

- [ ] Le contenu des chroots (`/srv/sftp`) — données métier
- [ ] `/etc/ssh/` (clés hôtes + `authorized_keys/`) — sans les clés hôtes, tous les clients verront une alerte MITM après restauration
- [ ] `/etc/ssh/sshd_config*` et fragments
- [ ] Scripts d'exploitation (`/usr/local/sbin/tri_scans.sh`, etc.)
- [ ] Journaux (`/var/log/auth.log`, syslog) selon la durée légale de conservation

Exemple avec Borg (voir aussi le guide Debian/Ubuntu) :

```bash
export BORG_REPO=/mnt/sauvegardes/borg-sftp
borg create --stats $BORG_REPO::'{hostname}-{now:%F}' \
  /srv/sftp /etc/ssh /usr/local/sbin/tri_scans.sh
borg prune --keep-daily=7 --keep-weekly=4 --keep-monthly=6 $BORG_REPO
```

Règle 3-2-1 : 3 copies, 2 supports, 1 hors site. Testez la restauration **au moins** une fois par trimestre.

---

## 28. Restauration après sinistre

Procédure type :

1. Réinstaller Debian/Ubuntu + `openssh-server`.
2. Restaurer `/etc/ssh/` (clés hôtes identiques → pas d'alerte côté clients).
3. Restaurer `sshd_config` et vérifier avec `sshd -t`.
4. Recréer utilisateurs/groupes avec **les mêmes UID/GID** (sinon les propriétaires des fichiers restaurés seront faux).
5. Restaurer `/srv/sftp` avec les permissions.
6. Rejouer le script de contrôle des permissions (voir § 71).
7. Test de connexion avec un compte de test avant réouverture aux partenaires.

> Astuce : sauvegardez `/etc/passwd`, `/etc/group`, `/etc/shadow` (ou au minimum la liste
> des UID/GID SFTP) pour recréer les comptes à l'identique.

---

## 29. Journalisation de base : auth.log

Par défaut, OpenSSH journalise via syslog (facility AUTH) :

```bash
# Connexions réussies / échouées
sudo grep -i "sshd" /var/log/auth.log | tail -20
# Échecs d'authentification
sudo grep "Failed password" /var/log/auth.log
sudo grep "Accepted publickey" /var/log/auth.log
```

Exemple de ligne utile :

```
sshd[1234]: Accepted publickey for sftp_acme from 203.0.113.10 port 51234 ssh2: ED25519 SHA256:...
```

On y voit : qui (`sftp_acme`), d'où (`203.0.113.10`), avec quelle clé (empreinte SHA256).

---

## 30. Journalisation détaillée des transferts (VERBOSE)

Pour savoir **qui a transféré quoi**, augmentez le niveau de log du subsystem :

```
Subsystem sftp internal-sftp -f AUTH -l VERBOSE
```

Puis `sudo systemctl reload ssh`. Vous obtiendrez dans `/var/log/auth.log` :

```
sftp-server[1234]: open "/depot/rapport.pdf" flags WRITE,CREATE,TRUNCATE mode 0644
sftp-server[1234]: close "/depot/rapport.pdf" bytes read 0 written 182340
```

- `open ... WRITE` = début d'envoi (upload) ; `READ` = début de téléchargement.
- `close ... bytes read/written` = fin de transfert avec les volumes.
- Corrélé avec la ligne `Accepted publickey for <user>`, on reconstitue : utilisateur, IP, fichier, sens, volume, heure.

> `-l INFO` donne les open/close sans les détails ; `-l VERBOSE` est le bon compromis.
> Évitez `-l DEBUG` en production (volume énorme).

---

## 31. Séparer les logs SFTP (rsyslog)

`/etc/rsyslog.d/40-sftp.conf` :

```
# Les logs du subsystem SFTP dans un fichier dédié
:programname, isequal, "sftp-server" /var/log/sftp.log
& stop
```

```bash
sudo systemctl restart rsyslog
sudo logrotate --debug /etc/logrotate.d/sftp 2>/dev/null  # tester si conf créée
```

`/etc/logrotate.d/sftp` :

```
/var/log/sftp.log {
    weekly
    rotate 12
    compress
    missingok
    notifempty
    create 0640 root adm
    sharedscripts
    postrotate
        /usr/lib/rsyslog/rsyslog-rotate 2>/dev/null || true
    endscript
}
```

---

## 32. Exploiter les logs : qui a transféré quoi ?

One-liners d'exploitation quotidienne :

```bash
# Transferts du jour (uploads = WRITE, downloads = READ)
sudo grep "sftp-server" /var/log/sftp.log | grep "$(date +%b' '%d)" | grep -E 'open|close'

# Volumes par fichier aujourd'hui
sudo awk '/close/ {print $NF, $(NF-2)}' /var/log/sftp.log | sort | uniq -c

# Tous les uploads d'un utilisateur sur 7 jours
sudo grep "sftp-server" /var/log/sftp.log | grep 'flags WRITE' | grep -B5 "sftp_acme"

# Fichiers supprimés (opération remove)
sudo grep -i "remove" /var/log/sftp.log
```

Script de rapport quotidien (`/usr/local/sbin/rapport_sftp.sh`) : voir § 71.

---

## 33. Supervision : ce qu'il faut surveiller

| Indicateur | Seuil d'alerte | Moyen |
|---|---|---|
| Service ssh actif | down | systemd / sonde |
| Espace disque `/srv` | > 80 % | quota + `df` |
| Quota par utilisateur | > 85 % | `repquota` / `xfs_quota` |
| Échecs d'authentification | > 20 / 5 min / IP | fail2ban (§ 55) |
| Transferts anormaux | volume > 3× la moyenne | rapport quotidien (§ 32) |
| Certificats/clés expirés | J-30 | inventaire des clés (§ 19) |
| Âge des fichiers en dépôt | > rétention définie | `find -mtime` |
| Intégrité `sshd_config` | modification inattendue | AIDE / checksum |

---

## 34. Sonde de service (check simple)

```bash
#!/bin/bash
# /usr/local/sbin/check_sftp.sh — sonde applicative SFTP
SFTP_USER="svc_sonde"
SFTP_HOST="sftp.entreprise.lan"
SFTP_KEY="/etc/sonde/id_sftp_sonde"
echo "ls" | sftp -b - -i "$SFTP_KEY" -o ConnectTimeout=10 \
  -o BatchMode=yes "$SFTP_USER@$SFTP_HOST" >/dev/null 2>&1
if [ $? -eq 0 ]; then echo "SFTP OK"; exit 0
else echo "SFTP KO"; exit 2; fi
```

- Compte `svc_sonde` en lecture seule sur un chroot dédié.
- `BatchMode=yes` : échoue proprement sans interaction (pas de prompt).
- À brancher sur Zabbix/Nagios/Prometheus (user parameter / exporter / node).

---

## 35. Alertes disque et quota

```bash
#!/bin/bash
# Alerte si /srv > 80 % ou si un quota dépasse 85 %
SEUIL=80
USAGE=$(df --output=pcent /srv | tail -1 | tr -dc '0-9')
[ "$USAGE" -ge "$SEUIL" ] && \
  echo "/srv à $USAGE %" | mail -s "[ALERTE] Disque SFTP" admin@entreprise.lan
```

Pour les quotas individuels, parsez `repquota -u /srv` en cron quotidien et alertez par utilisateur.

---

## 36. Rotation et conservation des journaux

- `auth.log` : rotation hebdomadaire par défaut (logrotate) — conservez 12 mois si vos obligations
  (bancaires, clients) l'exigent.
- `/var/log/sftp.log` : voir § 31 (12 semaines par défaut, à ajuster).
- Pensez à **centraliser** : transférez les logs vers un serveur syslog/SIEM
  (immuabilité en cas de compromission du serveur SFTP).

```bash
# rsyslog : envoi vers le collecteur (TLS recommandé)
# /etc/rsyslog.d/50-forward.conf
action(type="omfwd" target="logs.entreprise.lan" port="6514" protocol="tcp")
```

---

## 37. Pare-feu : n'ouvrir que le nécessaire

```bash
# UFW : autoriser SSH/SFTP uniquement
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

Durcissement par source (recommandé pour les partenaires à IP fixe) :

```bash
# nftables : n'autoriser que les IP partenaires sur le port 22
sudo nft add rule inet filter input tcp dport 22 \
  ip saddr { 203.0.113.10, 198.51.100.0/24 } accept
sudo nft add rule inet filter input tcp dport 22 drop
```

Et dans `sshd_config`, double verrou par utilisateur :

```
Match User sftp_banque
    AllowUsers sftp_banque@203.0.113.10
```

> `AllowUsers`/`DenyUsers`/`AllowGroups` se placent dans la partie **globale**,
> pas dans un bloc `Match` (restriction d'OpenSSH).

---

## 38. fail2ban : bannir les attaquants

```bash
sudo apt install -y fail2ban
```

`/etc/fail2ban/jail.d/sshd.local` :

```ini
[sshd]
enabled = true
port = 22
maxretry = 5
findtime = 600
bantime = 3600
```

Commandes :

```bash
sudo fail2ban-client status sshd
sudo fail2ban-client set sshd banip 203.0.113.99     # bannir manuellement
sudo fail2ban-client set sshd unbanip 203.0.113.99
```

> Les comptes SFTP n'ayant pas de mot de passe, le brute-force par mot de passe est inopérant ;
> fail2ban protège surtout les comptes admin restants et réduit le bruit.

---

## 39. Pas de shell, pas de tunnel, pas de X11 (rappel)

Le bloc `Match Group sftpusers` doit contenir (voir § 10) :

```
ForceCommand internal-sftp
AllowTcpForwarding no
AllowAgentForwarding no
X11Forwarding no
PermitTunnel no
```

Vérifiez qu'un compte SFTP ne peut pas exécuter de commande :

```bash
ssh -i ~/.ssh/id_sftp_acme sftp_acme@sftp.entreprise.lan 'id'
# attendu : "This service allows sftp connections only." puis fermeture
```

---

## 40. Utilisateurs fictifs : ne jamais utiliser de vrais noms

Dans toute la documentation et les exemples de ce guide, les utilisateurs
(`sftp_acme`, `sftp_banque`, `svc_copieur`…) et les domaines (`entreprise.lan`,
`203.0.113.0/24`, `198.51.100.0/24`) sont **fictifs** (plages de documentation
RFC 5737 / RFC 2606). Adaptez à votre réalité sans jamais exposer de vrais
identifiants dans des exemples partagés.

---

## 41. Mode batch : sftp -b pour les scripts

Fichier de commandes `envoi.txt` :

```
cd depot
put /var/exports/rapport_quotidien.csv
bye
```

Exécution :

```bash
sftp -b envoi.txt -i ~/.ssh/id_sftp_acme -o BatchMode=yes sftp_acme@sftp.entreprise.lan
```

- `-o BatchMode=yes` : échoue au lieu de demander un mot de passe (indispensable en cron).
- Code retour non nul si une commande échoue → testable avec `$?`.
- Pour des batchs complexes, préférez `lftp` (voir § 42) ou un script Python/paramiko.

---

## 42. lftp : miroir, reprise et robustesse

```bash
sudo apt install -y lftp
```

```bash
# Miroir montant (upload) avec reprise et suppression distante des fichiers disparus
lftp -u sftp_acme, sftp://sftp.entreprise.lan <<'EOF'
mirror --reverse --continue --delete --verbose /var/exports/ /depot/
bye
EOF
```

Options clés :

| Option | Effet |
|---|---|
| `--reverse` | Envoi local → distant (sans = téléchargement) |
| `--continue` | Reprise des fichiers partiels |
| `--delete` | Supprime côté distant ce qui n'existe plus en local |
| `--only-newer` | Ne transfère que les fichiers plus récents |
| `--parallel=4` | 4 transferts simultanés |
| `--exclude` | Exclure un motif |

Authentification par clé avec lftp :

```bash
# ~/.lftprc ou variables d'environnement
# lftp utilise l'agent SSH ou la clé par défaut ; sinon :
lftp -u sftp_acme sftp://sftp.entreprise.lan -e "set sftp:connect-program 'ssh -i /chemin/cle'; mirror ..."
```

---

## 43. Automatisation par cron (comptes techniques)

```bash
# /etc/cron.d/envoi_sftp — envoi quotidien à 2h05
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
5 2 * * * svc_export /usr/local/bin/envoi_quotidien.sh >>/var/log/envoi_sftp.log 2>&1
```

`/usr/local/bin/envoi_quotidien.sh` :

```bash
#!/bin/bash
set -euo pipefail
CLE=/etc/svc_export/id_ed25519
HOTE=sftp.partenaire.lan
USER=sftp_expediteur
SRC=/var/exports/a_envoyer
[ -f "$CLE" ] || { echo "clé absente"; exit 1; }
sftp -b /dev/stdin -i "$CLE" -o BatchMode=yes -o ConnectTimeout=15 "$USER@$HOTE" <<EOF
cd depot
mput $SRC/*.csv
bye
EOF
logger -t envoi_sftp "envoi quotidien terminé : $?"
```

Permissions : clé `0600` appartenant à l'utilisateur du cron, script `0750`.

---

## 44. Clés dédiées par automatisme (principe)

- **Une paire de clés par flux automatisé**, jamais la clé d'un humain.
- Clé **sans phrase de passe** (sinon le cron bloque), stockée `0600`, propriétaire = compte du cron.
- Restreignez côté serveur si possible (`Match User` dédié + chroot + `AllowUsers user@ip`).
- Documentez : quel flux, quelle machine source, quel compte, date de création, date de rotation prévue.
- En cas de compromission de la machine source : révoquez **immédiatement** la clé (suppression de la ligne dans `authorized_keys`), générez-en une nouvelle.

---

## 45. Python + paramiko (alternative scriptée)

```bash
pip install paramiko
```

```python
import paramiko
cle = paramiko.Ed25519Key.from_private_key_file("/etc/svc_export/id_ed25519")
transport = paramiko.Transport(("sftp.partenaire.lan", 22))
transport.connect(username="sftp_expediteur", pkey=cle)
sftp = paramiko.SFTPClient.from_transport(transport)
sftp.chdir("depot")
sftp.put("/var/exports/rapport.csv", "depot/rapport.csv",
         callback=lambda e, t: print(f"{e}/{t}"))
sftp.close(); transport.close()
```

> paramiko est pratique pour la logique métier (renommage, contrôle, notification),
> mais `sftp -b` / `lftp` suffisent pour du transfert pur.

---

## 46. Transferts fiables : checksum et idempotence

- Après un `put`, vérifiez l'intégrité : comparez un checksum SHA256 calculé en local
  avec celui recalculé après `get` (ou demandez au partenaire de publier un `.sha256`).
- Rendez les dépôts **idempotents** : nommez les fichiers avec la date
  (`factures_2026-09-26.csv`) pour éviter les écrasements silencieux.
- Protocole « fichier témoin » : envoyez d'abord `fichier.csv`, puis `fichier.csv.ok`.
  Le consommateur ne traite que les fichiers ayant leur `.ok` (transfert terminé garanti).

```bash
# côté émetteur
sftp> put factures_2026-09-26.csv /depot/
sftp> put factures_2026-09-26.csv.ok /depot/
# côté consommateur : ignorer tout fichier sans .ok correspondant
```

---

## 47. rsync vs SFTP : que choisir ?

| Critère | rsync-over-SSH | SFTP |
|---|---|---|
| Delta de transfert | Oui (n'envoie que les différences) | Non (fichier entier) |
| Reprise | Oui (`--partial`) | Oui (`reget`/`reput`, manuel) |
| Miroir | Natif (`--delete`) | Via `lftp mirror` |
| Chroot simple | Non (besoin d'un shell + rsync distant) | Oui (`internal-sftp`) |
| Comptes sans shell | Compliqué | **Natif** |
| Partenaires non techniques | Non | Oui (FileZilla/WinSCP) |

**Règle pratique :**

- Interne / technique / gros volumes redondants → **rsync over SSH** (avec un compte shell restreint, `rrsync`).
- Partenaires, clients, copieurs, comptes sans shell → **SFTP**.

---

## 48. rsync-over-SSH en pratique (pour mémoire)

```bash
# Envoi avec reprise, compression, suppression distante
rsync -avz --partial --progress -e "ssh -i ~/.ssh/id_rsync -p 22" \
  /var/exports/ svc_rsync@srv.entreprise.lan:/srv/depots/
```

> rsync exige un shell et le binaire rsync côté distant : **incompatible** avec
> `ForceCommand internal-sftp`. Pour des partenaires en chroot, restez sur SFTP.

---

## 49. Clients graphiques : FileZilla

Configuration d'un site (Gestionnaire de sites) :

| Champ | Valeur |
|---|---|
| Protocole | SFTP - SSH File Transfer Protocol |
| Hôte | `sftp.entreprise.lan` |
| Port | 22 |
| Type d'authentification | Fichier de clé |
| Utilisateur | `sftp_acme` |
| Fichier de clé | `id_sftp_acme.ppk` (converti via PuTTYgen si besoin) ou PEM direct |

Réglages recommandés : Édition → Paramètres → Transferts → limiter les transferts
simultanés à 2–4, activer la reprise.

---

## 50. Clients graphiques : WinSCP

- Nouveau site → Protocole **SFTP**, port 22, authentification par clé
  (`.ppk` via PuTTYgen, ou clé OpenSSH récente acceptée directement).
- Avancé → SSH → Authentification : cocher « Tentative d'authentification par clé »,
  désactiver « Tentative d'authentification par mot de passe » si clés uniquement.
- Scripting WinSCP pour automatisation Windows :

```bat
winscp.com /command ^
  "open sftp://sftp_acme@sftp.entreprise.lan/ -privatekey=C:\cles\id_sftp_acme.ppk -hostkey=""ssh-ed25519 256 AA:BB:...""" ^
  "put C:\exports\*.csv /depot/" ^
  "exit"
```

> `-hostkey` fige l'empreinte du serveur : le script échoue si elle change (anti-MITM).

---

## 51. Fiche réflexe : créer un compte SFTP complet (checklist)

```bash
U=sftp_nouveauclient ; PERIM=client_nouveau
sudo useradd -m -d /srv/sftp/$PERIM -s /usr/sbin/nologin -G sftpusers -c "Dépôt SFTP $PERIM" $U
sudo passwd -l $U
sudo mkdir -p /srv/sftp/$PERIM/{depot,restitutions}
sudo chown root:root /srv/sftp/$PERIM && sudo chmod 0755 /srv/sftp/$PERIM
sudo chown $U:sftpusers /srv/sftp/$PERIM/depot && sudo chmod 0750 /srv/sftp/$PERIM/depot
sudo chown root:$U /srv/sftp/$PERIM/restitutions 2>/dev/null || sudo chown root:sftpusers /srv/sftp/$PERIM/restitutions
sudo chmod 0750 /srv/sftp/$PERIM/restitutions
# clé publique du partenaire :
sudo sh -c "cat > /etc/ssh/authorized_keys/$U"   # coller, Ctrl+D
sudo chmod 0644 /etc/ssh/authorized_keys/$U
# bloc Match si chroot != /srv/sftp/$U (voir § 11), puis :
sudo sshd -t && sudo systemctl reload ssh
# quota, test de connexion, fiche d'exploitation
```

---

## 52. Supprimer / désactiver proprement un compte

```bash
U=sftp_ancienclient
# 1. Révoquer l'accès immédiatement (sans supprimer les données)
sudo sh -c "echo '# révoqué le $(date +%F)' > /etc/ssh/authorized_keys/$U"
# 2. Vérifier qu'il n'y a plus de session
sudo ss -tnp | grep :22
# 3. Archiver les données (selon contrat / obligations légales)
sudo tar -czf /srv/archives/${U}_$(date +%F).tar.gz /srv/sftp/<perimetre>
# 4. Après le délai de rétention : supprimer
sudo userdel -r $U   # -r supprime aussi le home si c'est le chroot : ATTENTION
sudo rm -rf /srv/sftp/<perimetre>
```

> Ne supprimez jamais un chroot sans archive préalable et validation du responsable.

---

## 53. Sécurité : durcissement du serveur hôte

- Mises à jour automatiques de sécurité (`unattended-upgrades`) — OpenSSH est critique.
- Partition `/srv` dédiée (un dépôt plein ne bloque pas le système).
- Pas d'autres services exposés sur la machine SFTP (cloisonnement).
- `PermitRootLogin no`, comptes admin en clés uniquement.
- Audit périodique : `lynis`, ou au minimum relecture annuelle de `sshd_config`.
- Sauvegarde chiffrée hors site (voir § 27).

---

## 54. Sécurité : journaliser aussi les échecs

Les échecs sont dans `/var/log/auth.log` :

```bash
# Top 10 des IP en échec aujourd'hui
sudo grep "$(date '+%b %e')" /var/log/auth.log | grep -i "failed" | \
  grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -c | sort -rn | head
# Utilisateurs inexistants tentés
sudo grep "Invalid user" /var/log/auth.log | awk '{print $(NF-2)}' | sort | uniq -c | sort -rn | head
```

Toute tentative sur un compte SFTP existant avec mot de passe doit être **impossible**
(`PasswordAuthentication no`) : si vous en voyez réussir, c'est une urgence.

---

## 55. fail2ban : aller plus loin (filtres SFTP)

Le jail `sshd` couvre déjà les échecs d'authentification. Ajoutez une surveillance
des scans agressifs :

```ini
# /etc/fail2ban/jail.d/sshd.local — version renforcée
[sshd]
enabled = true
maxretry = 5
findtime = 600
bantime = 86400
```

Et un jail récidiviste :

```ini
[recidive]
enabled = true
filter = recidive
logpath = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime = 604800
findtime = 86400
maxretry = 3
```

---

## 56. TLS/FTPS : quand le partenaire impose le FTP(S)

Parfois un partenaire (banque, administration) n'accepte que FTPS. Dans ce cas,
**vsftpd** en mode minimal et sécurisé, sur une machine dédiée si possible.

```bash
sudo apt install -y vsftpd
```

`/etc/vsftpd.conf` (extrait sécurisé) :

```
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
ssl_enable=YES
force_local_logins_ssl=YES
force_local_data_ssl=YES
ssl_tlsv1_2=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
pasv_min_port=50000
pasv_max_port=50100
```

Points de vigilance :

- Certificat TLS valide (Let's Encrypt ou PKI interne), TLS 1.2 minimum.
- Ouvrir le port 21 **et** la plage passive dans le pare-feu (contraignant : c'est
  l'un des gros désavantages de FTPS vs SFTP).
- `chroot_local_user=YES` + comptes sans shell comme pour SFTP.
- Préférez toujours renégocier vers SFTP quand c'est possible.

---

## 57. Couper le FTP classique (port 21 non chiffré)

- Ne **jamais** laisser `ssl_enable=NO` avec des comptes réels.
- Si un vsftpd historique tourne encore en clair : plan de migration
  (informer les partenaires, délai, bascule, extinction).
- Surveillez : `sudo ss -tlnp | grep :21` ne doit rien afficher une fois la migration terminée.

---

## 58. Mot de passe : quand on ne peut pas l'éviter (copieurs anciens)

Certains MFP anciens ne gèrent pas les clés SSH. Dernier recours, ciblé :

```
Match User svc_copieur_ancien
    ChrootDirectory /srv/sftp/scans_copieurs
    ForceCommand internal-sftp
    PasswordAuthentication yes
```

- Mot de passe **long et unique** (généré, pas choisi), stocké au coffre.
- Restreindre l'IP source si le copieur a une IP fixe (`AllowUsers`).
- Planifier le remplacement du matériel : le mot de passe est une dette de sécurité.
- Surveiller les connexions de ce compte dans les logs (alerte sur toute anomalie).

---

## 59. Dépannage — méthode générale

1. Reproduire avec `sftp -v` (verbeux) : la cause est souvent dans les 20 dernières lignes.
2. Regarder côté serveur en temps réel : `sudo journalctl -u ssh -f` et `/var/log/auth.log`.
3. Vérifier dans l'ordre : réseau/port → authentification → autorisations (Match) → chroot/permissions → quotas/disque.
4. Ne changer qu'**une** chose à la fois, `sshd -t`, `reload`, retester.

```bash
# Client : mode verbeux
sftp -v -i ~/.ssh/id_sftp_acme sftp_acme@sftp.entreprise.lan
# Serveur : suivre les logs
sudo tail -f /var/log/auth.log
```

---

## 60. Cas 1 — « Permission denied » dans le chroot

**Symptômes :** connexion OK, mais `put` échoue : `Permission denied`.

**Causes fréquentes :**

| Cause | Diagnostic | Remède |
|---|---|---|
| Le dossier appartient à root | `ls -la` dans le chroot | `chown user:groupe dossier` |
| Droits 0755 sur le dossier de dépôt | `stat dossier` | `chmod 0750` + bon propriétaire |
| Quota atteint | `repquota /srv` | augmenter ou purger |
| Disque plein | `df -h /srv` | libérer de l'espace |
| Fichier existant non inscriptible | `ls -l fichier` | vérifier le propriétaire |

> Rappel : le **chroot lui-même** doit rester `root:root 0755` ; c'est le **sous-dossier**
> de dépôt qui doit appartenir à l'utilisateur.

---

## 61. Cas 2 — la connexion se ferme aussitôt

**Symptômes :** `Connection closed` / `client_loop: send disconnect` juste après l'authentification.

**Pistes :**

1. Permissions du chroot non conformes (composant inscriptible par l'utilisateur) → sshd refuse silencieusement. Vérifier avec `namei -l /srv/sftp/...`.
2. `ChrootDirectory` pointe vers un chemin inexistant → `sshd -T | grep chroot`.
3. `ForceCommand internal-sftp` + `Subsystem sftp /usr/lib/openssh/sftp-server` incohérent → uniformiser sur `internal-sftp`.
4. Shell inexistant dans `/etc/shells` ? Avec `internal-sftp` ce n'est pas requis, mais un `Match` mal ordonné peut appliquer un autre bloc.
5. Regarder `auth.log` : `fatal: bad ownership or modes for chroot directory` = diagnostic direct.

---

## 62. Cas 3 — « Bad ownership or modes for chroot directory »

C'est le message explicite du cas 2. Procédure de réparation :

```bash
C=/srv/sftp/client_acme
sudo chown root:root "$C"
sudo chmod 0755 "$C"
# remonter la hiérarchie : /srv, /srv/sftp doivent aussi être root:root sans w groupe/autre
sudo chown root:root /srv /srv/sftp
sudo chmod 0755 /srv /srv/sftp
namei -l "$C"   # contrôle visuel final
sudo systemctl reload ssh
```

> Piège classique : avoir fait `chown -R sftp_acme /srv/sftp/client_acme`
> « pour que ça marche » — c'est exactement ce qui casse le chroot.

---

## 63. Cas 4 — clé refusée (« Permission denied (publickey) »)

Checklist :

- [ ] La clé **publique** (pas la privée !) est dans `/etc/ssh/authorized_keys/<user>`
- [ ] Fichier `0644 root:root`, dossier `0755` — sshd est strict aussi ici
- [ ] Pas de saut de ligne coupé / pas de retour chariot Windows dans la clé
- [ ] `AuthorizedKeysFile` pointe bien vers `/etc/ssh/authorized_keys/%u`
- [ ] Le compte n'est pas verrouillé par `DenyUsers` / `AllowUsers`
- [ ] Côté client : la bonne clé est proposée (`sftp -v` montre les clés essayées)
- [ ] L'empreinte dans `auth.log` correspond à la clé attendue

```bash
# Comparer l'empreinte proposée par le client et celle installée
sudo ssh-keygen -lf /etc/ssh/authorized_keys/sftp_acme
```

---

## 64. Cas 5 — « subsystem request failed on channel 0 »

- Le client demande le subsystem `sftp` mais le serveur ne le propose pas :
  vérifier la ligne `Subsystem sftp internal-sftp` (décommentée, syntaxe valide).
- Après modification : `sshd -t` puis `reload` (un `reload` oublié est la cause n°1).
- Conflit avec un `Match` qui écrase le subsystem ? `sshd -T -C user=sftp_acme,addr=... | grep subsystem`.

---

## 65. Cas 6 — transferts lents

| Piste | Action |
|---|---|
| Fenêtre TCP / latence | `sftp -R 64 -B 262144` (plus de requêtes en vol, gros buffers) |
| Chiffrement CPU-bound | Choisir une cipher rapide : `-o Ciphers=aes128-gcm@openssh.com` |
| Parallélisme | `lftp` avec `--parallel=4`, ou découper en plusieurs fichiers |
| Disque saturé côté serveur | `iostat -x 1`, vérifier le stockage |
| QoS / shaping opérateur | Tester hors heures ouvrées, mesurer avec `iperf3` |

```bash
# Test comparatif rapide
time sftp -b batch.txt -i cle user@hote   # batch avec un get d'un gros fichier
```

---

## 66. Cas 7 — caractères accentués / espaces dans les noms de fichiers

- SFTP transmet les noms en UTF-8 : forcer `LC_ALL=C.UTF-8` côté scripts si la locale est en POSIX.
- Dans les batchs, quoter les noms avec espaces : `put "mon fichier.pdf"`.
- Côté Windows (WinSCP), vérifier le jeu de caractères : UTF-8 par défaut, à conserver.
- Pour les flux automatisés : **normaliser** les noms à la source (sans accents, sans espaces,
  `facture_2026-09-26.pdf`) — voir § 46.

---

## 67. Cas 8 — le partenaire « ne voit pas » le fichier déposé

- Le fichier est-il complet ? Protocole `.ok` (voir § 46) : sans lui, le consommateur attend.
- Droits : fichier déposé en `0600` par défaut (umask du client) → le consommateur (autre UID)
  ne peut pas le lire. Harmoniser avec `umask` ou forcer les droits après dépôt.
- Mauvais dossier : `pwd` dans le chroot ≠ chemin absolu du serveur. Documenter l'arborescence vue par le client.
- Horloge désynchronisée : un fichier « du futur » peut perturber les tris par date → NTP partout (chrony).

---

## 68. Cas 9 — « Disk quota exceeded » / « No space left on device »

```bash
df -h /srv                          # disque plein ?
df -i /srv                          # inodes épuisés ? (beaucoup de petits fichiers)
sudo repquota -u /srv | grep sftp_  # quota utilisateur atteint ?
```

Actions :

1. Identifier les gros consommateurs : `du -sh /srv/sftp/*`.
2. Purger selon la politique de rétention (voir § 69) — jamais à la main sans traçabilité.
3. Augmenter le quota si légitime (`setquota`), avec validation.
4. Alerter : un dépôt qui grossit brutalement peut signaler un dysfonctionnement chez l'émetteur.

---

## 69. Cas 10 — saturation par accumulation (politique de rétention)

Sans purge, tout dépôt finit par saturer. Définir par périmètre :

| Périmètre | Rétention | Action |
|---|---|---|
| Dépôt clients (`depot/`) | 90 jours après traitement | Archivage puis suppression |
| Échanges bancaires | Durée légale (à valider) | Archivage chiffré |
| Scans copieurs (`entrees/`) | 30 jours après tri | Suppression auto |
| Restitutions | 180 jours | Archivage |

```bash
# Purge des scans triés de plus de 30 jours (avec log)
find /srv/partage/scans_tries -type f -mtime +30 -delete -print | \
  logger -t purge_scans
```

> Toujours logger les purges, et exclure les périmètres à obligation légale.

---

## 70. Cas 11 — horodatage incohérent entre client et serveur

- Tous les serveurs en NTP/chrony : `timedatectl status` → `System clock synchronized: yes`.
- Les logs (`auth.log`, `sftp.log`) utilisent l'heure serveur : la documenter (UTC ou locale ?).
- Dans les rapports, convertir explicitement : « heures Europe/Paris ».
- Un fichier daté du futur bloque les `mirror --only-newer` : purger ou retoucher avec `touch`.

---

## 71. Scripts d'exploitation prêts à l'emploi

### 71.1 Contrôle des permissions des chroots

```bash
#!/bin/bash
# /usr/local/sbin/check_chroot.sh — à lancer après chaque création/modif
BASE=/srv/sftp
OK=1
for d in "$BASE"/*/; do
  own=$(stat -c '%U:%G %a' "$d")
  if [ "$own" != "root:root 755" ]; then
    echo "ANOMALIE: $d -> $own (attendu root:root 755)"
    OK=0
  fi
done
[ $OK -eq 1 ] && echo "Tous les chroots sont conformes."
exit $((1-OK))
```

### 71.2 Rapport quotidien des transferts

```bash
#!/bin/bash
# /usr/local/sbin/rapport_sftp.sh — envoi par mail chaque matin
LOG=/var/log/sftp.log
HIER=$(date -d yesterday '+%b %e')
{
  echo "=== Transferts SFTP du $HIER ==="
  grep "$HIER" "$LOG" | grep -c 'flags WRITE' | xargs echo "Uploads :"
  grep "$HIER" "$LOG" | grep -c 'flags READ'  | xargs echo "Downloads :"
  echo "=== Top fichiers ==="
  grep "$HIER" "$LOG" | grep 'close' | awk '{print $(NF-2), $NF}' | sort -rn | head -20
} | mail -s "[SFTP] Rapport quotidien" admin@entreprise.lan
```

```cron
0 7 * * * root /usr/local/sbin/rapport_sftp.sh
```

---

## 72. Pense-bête de poche (une page)

```
CONNEXION
  sftp -i cle user@hote
  sftp -b batch.txt -o BatchMode=yes user@hote

TRANSFERT
  put / get / mput / mget / reput / reget
  lftp : mirror --reverse --continue --delete src/ /dst/

DIAGNOSTIC
  sftp -v ...                    # côté client
  tail -f /var/log/auth.log      # côté serveur
  sshd -T | grep -i chroot       # config effective
  namei -l /srv/sftp/xxx         # permissions du chroot

PERMISSIONS CHROOT
  chroot            -> root:root 0755 (toute la hiérarchie !)
  dossier de dépôt  -> user:sftpusers 0750

BLOC MATCH TYPE
  Match Group sftpusers
      ChrootDirectory /srv/sftp/%u
      ForceCommand internal-sftp
      PasswordAuthentication no
      AllowTcpForwarding no / X11Forwarding no

URGENCES
  Révoquer une clé : vider /etc/ssh/authorized_keys/<user>
  Bannir une IP   : fail2ban-client set sshd banip <ip>
  Disque plein    : df -h /srv ; du -sh /srv/sftp/* ; purger selon rétention
```

---

## 73. Glossaire détaillé

| Terme | Définition |
|---|---|
| SFTP | SSH File Transfer Protocol : transfert de fichiers dans un tunnel SSH (port 22) |
| FTPS | FTP + TLS : FTP historique sécurisé par TLS (ports 21 + plage passive) |
| SCP | Ancien utilitaire de copie via SSH, supplanté par SFTP |
| Subsystem | Service invoqué par sshd à la demande du client (`sftp`, ici `internal-sftp`) |
| internal-sftp | Implémentation du serveur SFTP intégrée à sshd, sans binaire externe |
| sftp-server | Binaire externe historique (`/usr/lib/openssh/sftp-server`) |
| ChrootDirectory | Directive sshd : dossier qui devient `/` pour l'utilisateur |
| ForceCommand | Force la commande exécutée pour une session (ici `internal-sftp`) |
| Match | Bloc conditionnel de `sshd_config` (User, Group, Address, Host) |
| AuthorizedKeysFile | Fichier(s) contenant les clés publiques autorisées (`%u` = utilisateur) |
| BatchMode | Option client : échoue au lieu d'interagir (indispensable en script/cron) |
| nologin | Shell factice (`/usr/sbin/nologin`) : interdit toute connexion interactive |
| Quota | Limite d'espace (bloc) ou de nombre de fichiers (inode) par utilisateur |
| SyslogFacility | Canal syslog utilisé par sshd pour journaliser (AUTH par défaut) |
| VERBOSE | Niveau de log donnant les open/close de fichiers (qui a transféré quoi) |
| fail2ban | Bannit temporairement les IP après N échecs d'authentification |
| chattr +i | Attribut immuable : fichier insupprimable même par root (sans retirer l'attribut) |
| MFP | Multifunction Printer : copieur multifonction (impression/scan) |
| Rétention | Durée de conservation des fichiers avant archivage/suppression |
| 3-2-1 | Règle de sauvegarde : 3 copies, 2 supports différents, 1 hors site |

---

## 74. Les 12 erreurs classiques (à afficher en salle d'exploitation)

| # | Erreur | Conséquence | Réflexe |
|---|---|---|---|
| 1 | `chown -R user` sur le chroot | Connexion fermée aussitôt | Le chroot reste `root:root 0755` |
| 2 | Oublier `sshd -t` + `reload` | Config non appliquée, panne au reboot | Toujours tester avant de recharger |
| 3 | Fermer sa session SSH avant de tester | Enfermé dehors | Tester dans un 2ᵉ terminal |
| 4 | Clé **privée** déposée côté serveur | Authentification impossible | Seule la **publique** va dans `authorized_keys` |
| 5 | Bloc `Match` pas en fin de fichier | Directives ignorées / erreurs | `Match` toujours après le global |
| 6 | Pas de quota | Un client remplit le disque | Quota dès la création du compte |
| 7 | Mot de passe au lieu de clé « pour dépanner » | Dette de sécurité | Clé dédiée, `Match` ciblé, traçé |
| 8 | `Subsystem sftp` pointant vers l'ancien binaire + chroot | Échec du subsystem | `internal-sftp` partout |
| 9 | Droits `0777` « pour que ça marche » | Fuite de données entre clients | `0750` + bons propriétaires |
| 10 | Pas de `.ok` / contrôle d'intégrité | Fichiers incomplets traités | Protocole fichier témoin + checksum |
| 11 | Logs jamais regardés | Incident découvert trop tard | Rapport quotidien + alertes |
| 12 | Pas de sauvegarde des clés hôtes | Alertes MITM chez tous les clients après restauration | Sauvegarder `/etc/ssh/` |

---

## 75. Cas pratique 1 — onboarding d'un nouveau client (commenté)

**Contexte :** le client « ACME » doit déposer chaque soir ses bons de commande (CSV, ~50 Mo/jour).

```bash
# 1. Création du compte (voir checklist § 51)
U=sftp_acme
sudo useradd -m -d /srv/sftp/client_acme -s /usr/sbin/nologin -G sftpusers \
  -c "Dépôt SFTP client ACME" $U
sudo passwd -l $U

# 2. Arborescence : le client écrit dans depot/, lit dans restitutions/
sudo mkdir -p /srv/sftp/client_acme/{depot,restitutions}
sudo chown root:root /srv/sftp/client_acme && sudo chmod 0755 /srv/sftp/client_acme
sudo chown $U:sftpusers /srv/sftp/client_acme/depot && sudo chmod 0750 /srv/sftp/client_acme/depot
sudo chown root:sftpusers /srv/sftp/client_acme/restitutions && sudo chmod 0750 /srv/sftp/client_acme/restitutions

# 3. Clé publique fournie par ACME (canal authentifié : mail signé, portail, téléphone)
sudo sh -c "cat > /etc/ssh/authorized_keys/$U"   # coller la clé, Ctrl+D
sudo chmod 0644 /etc/ssh/authorized_keys/$U

# 4. Quota : 50 Mo/jour x 90 jours de rétention ≈ 5 Go -> quota 8 Go
sudo setquota -u $U 8388608 8912896 0 0 /srv

# 5. Test croisé : depuis un poste de test, put + get + tentative d'évasion du chroot
# 6. Fiche d'exploitation : contacts ACME, volumétrie, rétention 90 j, rotation clé annuelle
# 7. Communiquer au client : hôte, port, utilisateur, empreinte du serveur (§ 16),
#    arborescence vue de son côté, protocole .ok, format de nommage des fichiers
```

**Points de vigilance :** prévenir ACME que `restitutions/` est en lecture seule ;
planifier la purge à 90 jours (§ 69) ; ajouter le rapport quotidien (§ 71.2).

---

## 76. Cas pratique 2 — échange bancaire sécurisé (commenté)

**Contexte :** la comptabilité envoie les virements et récupère les relevés ; la banque impose un SFTP (tant mieux).

```bash
# 1. Deux comptes distincts : un interne, un partenaire
sudo useradd -m -d /srv/sftp/compta -s /usr/sbin/nologin -G sftp_compta \
  -c "Comptabilité interne" svc_compta
sudo useradd -m -d /srv/sftp/compta -s /usr/sbin/nologin -G sftp_compta \
  -c "Partenaire bancaire" sftp_banque
sudo passwd -l svc_compta sftp_banque

# 2. Chroot commun au groupe (voir § 11)
# Match Group sftp_compta
#     ChrootDirectory /srv/sftp/compta
#     ForceCommand internal-sftp

# 3. Permissions croisées : chacun écrit chez soi, lit chez l'autre
sudo mkdir -p /srv/sftp/compta/{envoi_banque,recus_banque,archives}
sudo chown root:root /srv/sftp/compta && sudo chmod 0755 /srv/sftp/compta
sudo chown svc_compta:sftp_compta /srv/sftp/compta/envoi_banque && sudo chmod 0750 /srv/sftp/compta/envoi_banque
sudo chown sftp_banque:sftp_compta /srv/sftp/compta/recus_banque && sudo chmod 0750 /srv/sftp/compta/recus_banque
sudo chown root:root /srv/sftp/compta/archives && sudo chmod 0750 /srv/sftp/compta/archives
```

**Exploitation :**

- Les fichiers de virement sont chiffrés GPG avant dépôt (§ 26).
- Un script déplace chaque nuit les fichiers traités vers `archives/` puis `chattr +i`.
- Rétention selon les obligations légales/comptables (à valider avec la DAF).
- Restriction IP : `AllowUsers sftp_banque@<ip_banque>` si l'IP est fixe (§ 37).

---

## 77. Cas pratique 3 — réception des scans des copieurs (commenté)

**Contexte :** 6 copieurs déposent leurs numérisations ; le service veut un tri automatique par machine et par jour (voir § 20-21).

```bash
# 1. Compte technique unique (ou un par site si besoin de cloisonner)
sudo useradd -r -d /srv/sftp/scans_copieurs -s /usr/sbin/nologin -G sftpusers \
  -c "Dépôt scans copieurs" svc_copieur
sudo passwd -l svc_copieur

# 2. Dépôt en écriture seule : le copieur ne doit ni relire ni lister les autres scans
sudo mkdir -p /srv/sftp/scans_copieurs/entrees/{etage1,etage2,atelier}
sudo chown root:root /srv/sftp/scans_copieurs && sudo chmod 0755 /srv/sftp/scans_copieurs
sudo chown svc_copieur:sftpusers /srv/sftp/scans_copieurs/entrees
sudo chmod 0730 /srv/sftp/scans_copieurs/entrees
sudo chown svc_copieur:sftpusers /srv/sftp/scans_copieurs/entrees/*
sudo chmod 0730 /srv/sftp/scans_copieurs/entrees/*

# 3. Clé dédiée installée sur chaque copieur (ou une paire par copieur si le modèle le permet)
# 4. Tri automatique toutes les 5 minutes (script § 21)
# 5. Purge à 30 jours après tri (§ 69)
```

**Lien maintenance :** le scan-to-SFTP supprime la dépendance au SMB (plus de compte
de service AD à maintenir sur chaque MFP, plus de SMBv1 qui traîne). En cas de changement
de copieur, il suffit de déployer la clé sur la nouvelle machine.

**Dépannage typique :** « le copieur n'envoie plus » → vérifier (1) l'espace disque,
(2) que l'IP du copieur n'a pas changé (si restriction), (3) `auth.log` pour l'erreur exacte,
(4) l'horloge du copieur (certificats/clés sensibles au temps).

---

## 78. Cas pratique 4 — migration FTPS vers SFTP d'un partenaire (commenté)

**Contexte :** un partenaire historique utilise encore FTPS (vsftpd). Objectif : bascule sans coupure.

1. **Préparer** le compte SFTP en parallèle (`sftp_partenaire`), sans toucher au FTPS.
2. **Communiquer** : hôte, port 22, utilisateur, empreinte serveur, arborescence, protocole `.ok`.
3. **Doubler** les flux pendant 2 semaines : le partenaire envoie sur les deux canaux ;
   comparer les volumes (rapport § 71.2 vs logs vsftpd).
4. **Bascule** : le partenaire n'envoie plus qu'en SFTP ; lecture seule sur le FTPS.
5. **Extinction** : après 1 mois sans fichier sur le FTPS, couper vsftpd, fermer le port 21
   et la plage passive au pare-feu, archiver les logs.
6. **Bilan** : mettre à jour la fiche d'exploitation et prévenir la supervision
   (les sondes FTPS doivent pointer vers le SFTP).

---

## 79. Cas pratique 5 — export automatisé nocturne (commenté)

**Contexte :** l'ERP exporte chaque nuit à 1h un fichier vers le SFTP d'un partenaire.

```bash
#!/bin/bash
# /usr/local/bin/export_erp.sh — appelé par cron à 1h00
set -euo pipefail
CLE=/etc/erp_sftp/id_ed25519        # 0600, propriétaire erp
HOTE=sftp.partenaire.lan
USER=sftp_erp
SRC=/var/erp/exports
DEST=depot
DATE=$(date +%F)

# 1. L'ERP a déjà généré ${SRC}/export_${DATE}.csv (vérifier sa présence)
FICHIER="${SRC}/export_${DATE}.csv"
[ -f "$FICHIER" ] || { echo "export manquant"; exit 1; }

# 2. Checksum pour le contrôle d'intégrité côté partenaire
sha256sum "$FICHIER" > "${FICHIER}.sha256"

# 3. Envoi : fichier puis .sha256 puis témoin .ok
sftp -b /dev/stdin -i "$CLE" -o BatchMode=yes -o ConnectTimeout=20 "$USER@$HOTE" <<EOF
cd $DEST
put $FICHIER
put ${FICHIER}.sha256
put /dev/null export_${DATE}.csv.ok
bye
EOF

# 4. Traçabilité locale
logger -t export_erp "export $DATE envoyé, code $?"
```

```cron
0 1 * * * erp /usr/local/bin/export_erp.sh >>/var/log/export_erp.log 2>&1
```

**Garde-fous :** `BatchMode=yes` (pas de blocage), `ConnectTimeout`, nom de fichier daté
(idempotence), checksum, témoin `.ok`, log centralisé, alerte si le fichier source est absent,
clé dédiée (§ 44), rotation annuelle.

---

## 80. Quiz — 10 questions

1. Pourquoi le répertoire désigné par `ChrootDirectory` doit-il appartenir à `root:root`
   en `0755`, et que se passe-t-il sinon ?
2. Quelle différence entre `Subsystem sftp internal-sftp` et
   `Subsystem sftp /usr/lib/openssh/sftp-server` dans un contexte chroot ?
3. Un partenaire vous envoie sa clé **privée** par mail « pour configurer le serveur ».
   Que faites-vous ?
4. Où stocker les clés publiques des comptes chrootés, et pourquoi pas dans le chroot ?
5. À quoi sert l'option `-o BatchMode=yes` dans un script cron, et que se passe-t-il sans elle ?
6. Comment savoir, après coup, quel utilisateur a déposé quel fichier et quel volume ?
7. Un `put` échoue avec `Permission denied` alors que la connexion réussit : citez 3 causes possibles
   et leur diagnostic.
8. Pourquoi faut-il un protocole de fichier témoin (`.ok`) pour les flux automatisés ?
9. rsync-over-SSH ou SFTP pour un partenaire externe sans shell : lequel et pourquoi ?
10. Citez 4 directives du bloc `Match Group sftpusers` qui empêchent tout usage autre que le transfert.

---

## 81. Quiz — réponses

1. OpenSSH exige que chaque composant du chemin du chroot appartienne à root et ne soit
   inscriptible par personne d'autre (sécurité : sinon l'utilisateur pourrait modifier son
   environnement racine). Sinon : `fatal: bad ownership or modes for chroot directory`
   et connexion fermée aussitôt (voir § 62).
2. `internal-sftp` est intégré à sshd : aucune dépendance externe, il fonctionne dans un
   chroot vide. Le binaire `sftp-server` exigerait de recopier libc et devices dans chaque
   chroot. `internal-sftp` accepte aussi `-f`/`-l` pour la journalisation (voir § 6, § 30).
3. On refuse : la clé privée ne voyage jamais. On demande la clé **publique** (`.pub`),
   transmise par un canal authentifié, et on rappelle au partenaire de régénérer sa paire
   puisque sa privée a circulé en clair (voir § 12, § 44).
4. Dans `/etc/ssh/authorized_keys/%u` (centralisé, géré par root, hors chroot) : le chroot
   appartient à root, l'utilisateur ne pourrait pas gérer ses clés, et sshd impose des
   permissions strictes que le chroot complique (voir § 18).
5. `BatchMode=yes` fait échouer proprement au lieu d'afficher un prompt (mot de passe,
   host key) : sans elle, le cron se bloque en attendant une entrée qui ne viendra jamais (voir § 41, § 43).
6. Avec `Subsystem sftp internal-sftp -f AUTH -l VERBOSE` : les lignes `open ... WRITE/READ`
   et `close ... bytes read/written` dans `/var/log/sftp.log`, corrélées aux lignes
   `Accepted publickey for <user> from <ip>` de `auth.log` (voir § 30, § 32).
7. (a) Le dossier de dépôt appartient à root ou n'est pas inscriptible → `ls -la`/`stat` ;
   (b) quota atteint → `repquota` ; (c) disque plein → `df -h` (voir § 60).
8. Le `.ok` n'est déposé qu'après le transfert complet : le consommateur ne traite que les
   fichiers accompagnés de leur témoin, ce qui élimine les fichiers partiels (voir § 46).
9. **SFTP** : rsync exige un shell et le binaire rsync côté distant, incompatible avec
   `ForceCommand internal-sftp` ; SFTP fonctionne nativement avec des comptes sans shell
   en chroot (voir § 47).
10. `ForceCommand internal-sftp`, `AllowTcpForwarding no`, `AllowAgentForwarding no`,
    `X11Forwarding no` (et `PermitTunnel no`) — voir § 10 et § 39.

---

## 82. Pour aller plus loin

- **Documentation OpenSSH** : `man sshd_config` (directives `Match`, `ChrootDirectory`,
  `ForceCommand`), `man sftp`, `man sftp-server`.
- **Automatisation avancée** : remplacez les batchs shell par Ansible (module `ansible.builtin.copy`
  via SFTP, gestion des `authorized_keys` en masse).
- **Haute disponibilité** : deux serveurs SFTP derrière un VIP (keepalived), `/srv/sftp`
  répliqué (DRBD ou rsync), clés hôtes identiques sur les deux nœuds.
- **Chiffrement de bout en bout** : GPG systématique pour les flux sensibles (§ 26),
  avec gestion des clés via un trousseau d'équipe.
- **SIEM** : centralisez `/var/log/sftp.log` et `auth.log` (rsyslog TLS, § 36),
  créez des tableaux de bord (volumes par client, échecs par IP).
- **Conformité** : alignez rétention et journalisation sur vos obligations
  (bancaires, clients, RGPD : minimisation et durées définies).
- **Guides compagnons** : `debian_ubuntu_guide.md` (bases système), `proxmox_guide.md`
  (virtualisation et sauvegarde), `onduleurs_ups_guide.md` (continuité électrique du serveur).

---

## 83. Modèle de fiche d'exploitation par compte

```
Compte            : sftp_acme
Périmètre/chroot  : /srv/sftp/client_acme
Usage             : dépôt nocturne des bons de commande (CSV)
Responsable int.  : _______________      Contact partenaire : _______________
Volumétrie        : ~50 Mo/jour          Rétention : 90 jours
Quota             : 8 Go (soft) / 8,5 Go (hard)
Clé installée le  : _______________      Rotation prévue le : _______________
Empreinte clé     : SHA256:_______________
Restriction IP    : _______________
Créé le           : _______________ par _______________
Observations      : _______________
```

---

## 84. Checklist de mise en production du serveur

- [ ] OpenSSH installé, `sshd_config` durci (§ 17) et testé (`sshd -t`)
- [ ] `Subsystem sftp internal-sftp -f AUTH -l VERBOSE` actif
- [ ] Bloc `Match Group sftpusers` en fin de fichier (§ 10)
- [ ] Groupe `sftpusers` créé, comptes sans shell, mots de passe verrouillés
- [ ] Chroots `root:root 0755` vérifiés (`check_chroot.sh`, § 71.1)
- [ ] Clés publiques installées (`/etc/ssh/authorized_keys/%u`), test de connexion OK
- [ ] Quotas appliqués et alertes configurées (§ 24, § 35)
- [ ] fail2ban actif sur `sshd` (§ 38, § 55)
- [ ] Pare-feu : 22/TCP restreint aux sources légitimes (§ 37)
- [ ] Logs SFTP séparés + logrotate + centralisation (§ 31, § 36)
- [ ] Rapport quotidien des transferts (§ 71.2) et sonde de service (§ 34)
- [ ] Sauvegarde `/srv/sftp` + `/etc/ssh` testée (restauration à blanc, § 28)
- [ ] Politique de rétention définie par périmètre (§ 69)
- [ ] Fiches d'exploitation remplies (§ 83)
- [ ] Procédure de révocation de clé connue de l'astreinte (§ 52)

---

## 85. Conclusion

Un serveur SFTP d'entreprise tient sur cinq piliers : **chroot systématique**,
**clés uniquement**, **pas de shell**, **journalisation qui dit qui a transféré quoi**,
et **exploitation** (quotas, supervision, sauvegarde, rétention). Avec ce socle,
les dépôts clients, les échanges bancaires et les scans des copieurs reposent sur
une infrastructure simple, auditable et robuste — un seul port, un seul service,
des procédures écrites. Relisez ce guide à chaque onboarding de partenaire, et
faites-en la référence de votre équipe.

---

## 86. Durcissement cryptographique (ciphers, KEX, MAC)

Restreignez les algorithmes aux valeurs modernes dans `sshd_config` :

```
# N'autoriser que les échanges de clés robustes
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group18-sha512
# Chiffrements AEAD / modernes uniquement
Ciphers aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes128-ctr
# MAC avec EtM (encrypt-then-mac)
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com
# Clés hôtes : privilégier Ed25519
HostKey /etc/ssh/ssh_host_ed25519_key
```

Vérifiez ce que négocie réellement un client :

```bash
# Côté client, en verbeux : repérer "kex: algorithm:" et "cipher:"
sftp -v sftp_acme@sftp.entreprise.lan 2>&1 | grep -E "kex:|cipher|MAC"
# Scanner la configuration offerte par le serveur :
nmap --script ssh2-enum-algos -p 22 sftp.entreprise.lan
```

> Attention : un durcissement trop agressif peut casser de vieux clients/partenaires
> (copieurs anciens, librairies Java datées). Testez chaque partenaire après changement
> et gardez une fenêtre de compatibilité documentée.

---

## 87. SFTP derrière un bastion (jump host)

Si le serveur SFTP n'est pas exposé directement sur Internet :

```bash
# Le client passe par le bastion (une seule commande)
sftp -J admin@bastion.entreprise.lan sftp_acme@sftp-prive.entreprise.lan

# Ou en ~/.ssh/config côté client :
Host sftp-entreprise
    HostName sftp-prive.entreprise.lan
    User sftp_acme
    IdentityFile ~/.ssh/id_sftp_acme
    ProxyJump admin@bastion.entreprise.lan
```

Avantages : le serveur SFTP n'a que le bastion comme source autorisée sur le port 22
(`AllowUsers *@bastion` ou règle pare-feu), journalisation centralisée des accès,
pas d'exposition directe. Inconvénient : le bastion devient critique (HA, supervision).

---

## 88. Montée en charge et tuning

| Paramètre | Effet | Valeur de départ |
|---|---|---|
| `MaxSessions` | Sessions multiplexées par connexion | 5–10 |
| `MaxStartups` | Connexions simultanées non authentifiées (`10:30:60`) | défaut OK, durcir si flood |
| `ClientAliveInterval` | Détection des sessions mortes | 300 s |
| `LoginGraceTime` | Temps max pour s'authentifier | 30 s |
| Buffers SFTP (`-B`, `-R`) | Débit sur liens à forte latence | `-B 262144 -R 64` |

Autres leviers :

- **Stockage** : `/srv` sur RAID ou volume rapide ; surveillez `iostat -x` pendant les pics.
- **Parallélisme** : `lftp --parallel` ou plusieurs flux plutôt qu'un seul gros fichier.
- **Fenêtres de transfert** : étalez les batchs partenaires (cron à heures décalées)
  pour éviter le pic de 2h du matin.
- **Limites système** : `nofile`/`nproc` suffisants si des dizaines de partenaires
  transfèrent en parallèle (`/etc/security/limits.conf`, systemd `LimitNOFILE`).

---

## 89. Annexe — sshd_config complet commenté (modèle)

```
# ============ /etc/ssh/sshd_config — modèle SFTP entreprise ============
Port 22
Protocol 2

# Clés hôtes (Ed25519 en priorité)
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key

# --- Authentification : clés uniquement ---
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes

# --- Crypto moderne ---
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group18-sha512
Ciphers aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com

# --- Robustesse ---
LoginGraceTime 30
MaxAuthTries 3
MaxSessions 10
ClientAliveInterval 300
ClientAliveCountMax 2

# --- Divers ---
X11Forwarding no
AllowAgentForwarding no
AllowTcpForwarding no
PermitTunnel no
PrintMotd no

# --- SFTP : serveur intégré + logs détaillés ---
Subsystem sftp internal-sftp -f AUTH -l VERBOSE

# --- Clés publiques centralisées hors chroot ---
AuthorizedKeysFile /etc/ssh/authorized_keys/%u

# --- Restriction d'accès globale (exemple) ---
# AllowGroups sftpusers sftp_compta admins

# ============ BLOCS MATCH : TOUJOURS EN FIN DE FICHIER ============
# Comptes de transfert : chroot, pas de shell, pas de mot de passe, pas de tunnel
Match Group sftpusers
    ChrootDirectory /srv/sftp/%u
    ForceCommand internal-sftp
    PasswordAuthentication no
    AllowTcpForwarding no
    AllowAgentForwarding no
    X11Forwarding no
    PermitTunnel no

# Comptabilité : chroot commun au groupe
Match Group sftp_compta
    ChrootDirectory /srv/sftp/compta
    ForceCommand internal-sftp
    PasswordAuthentication no
    AllowTcpForwarding no
    AllowAgentForwarding no
    X11Forwarding no
    PermitTunnel no

# Exception ciblée : vieux copieur sans support des clés (dette à résorber)
# Match User svc_copieur_ancien
#     ChrootDirectory /srv/sftp/scans_copieurs
#     ForceCommand internal-sftp
#     PasswordAuthentication yes
```

Après toute modification : `sudo sshd -t && sudo systemctl reload ssh`.

---

## 90. Annexe — script d'onboarding complet

```bash
#!/bin/bash
# /usr/local/sbin/sftp_onboard.sh — crée un espace client de A à Z
# Usage : sudo sftp_onboard.sh sftp_acme client_acme "Dépôt SFTP client ACME"
set -euo pipefail
U=${1:?usage: $0 <user> <perimetre> "<commentaire>"}
PERIM=${2:?}
COMMENT=${3:-"Compte SFTP $U"}
BASE=/srv/sftp

# 1. Utilisateur sans shell, mot de passe verrouillé
useradd -m -d "$BASE/$PERIM" -s /usr/sbin/nologin -G sftpusers -c "$COMMENT" "$U"
passwd -l "$U"

# 2. Arborescence : chroot root:root 0755, dépôt inscriptible par l'utilisateur
mkdir -p "$BASE/$PERIM"/{depot,restitutions}
chown root:root "$BASE/$PERIM"; chmod 0755 "$BASE/$PERIM"
chown "$U":sftpusers "$BASE/$PERIM/depot"; chmod 0750 "$BASE/$PERIM/depot"
chown root:sftpusers "$BASE/$PERIM/restitutions"; chmod 0750 "$BASE/$PERIM/restitutions"

# 3. Clé publique (collée par l'exploitant)
echo "Collez la clé PUBLIQUE de $U puis Ctrl+D :"
sh -c "cat > /etc/ssh/authorized_keys/$U"
chmod 0644 "/etc/ssh/authorized_keys/$U"

# 4. Contrôles
echo "--- Contrôle chroot ---"; namei -l "$BASE/$PERIM" | head -8
echo "--- Contrôle sshd ---"; sshd -t && echo "sshd_config OK"

# 5. Rappel des actions manuelles restantes
cat <<EOF
Actions restantes :
  - [ ] setquota -u $U <soft> <hard> 0 0 /srv   (quota)
  - [ ] bloc Match si chroot != /srv/sftp/$U, puis systemctl reload ssh
  - [ ] test : sftp -i <cle_test> $U@<hote> (put/get + tentative d'évasion)
  - [ ] fiche d'exploitation § 83 remplie
EOF
logger -t sftp_onboard "compte $U créé (périmètre $PERIM)"
```

> Ce script standardise les créations : même arborescence, mêmes permissions,
> mêmes contrôles à chaque fois. Adaptez les valeurs (quota, groupe) à votre contexte.
