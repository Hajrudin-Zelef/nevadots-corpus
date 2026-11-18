# Guide SSH approfondi

**Pour Zelef — chef de service systèmes, sysadmin.**
SSH est ton outil quotidien : ce guide va du protocole aux cas de production,
du `~/.ssh/config` bien tenu au durcissement complet d'un serveur, avec
dépannage, supervision, sauvegarde des clés et cas pratiques commentés.

- **Public :** administrateurs systèmes, usage quotidien intensif.
- **Plateformes :** exemples principalement Debian/Ubuntu, transposables RHEL.
- **Convention :** `client$` = machine locale, `serveur#` = machine distante (root),
  `serveur$` = machine distante (utilisateur normal).
- **Avertissement :** les clés, empreintes et adresses IP de ce guide sont
  **fictives**. Ne partage **jamais** une clé privée : ni par mail, ni dans un
  ticket, ni dans un dépôt Git. Une clé privée qui a fuité = une clé à révoquer.

> Mode d'emploi : les sections 1 à 8 posent les bases, 9 à 40 couvrent l'usage
> client avancé, 41 à 53 le serveur, 54 à 59 la supervision et la sauvegarde,
> 60 la mise à jour, 61 à 74 le dépannage, 75 à 79 les cas pratiques,
> 80 à 85 les aide-mémoire, le glossaire, le quiz et les ressources.

---

## Sommaire

1. Objectifs du guide
2. SSH : le protocole en notions
3. Chiffrement : ce qu'il faut retenir
4. Handshake : établissement d'une session (notions)
5. Méthodes d'authentification
6. Installation d'OpenSSH
7. Premier contact : `ssh`, clés d'hôte, `known_hosts`
8. Vérifier l'empreinte d'un hôte (TOFU)
9. `ssh_config` client : structure
10. `Host` et alias
11. `HostName`, `User`, `Port`
12. `IdentityFile` : une clé par hôte
13. Options client par hôte : les plus utiles
14. `Match`, `Include` et organisation du fichier
15. Générer une clé ed25519
16. RSA 4096 : quand s'en servir encore
17. Passphrase : la choisir et la gérer
18. `ssh-agent` et `ssh-add`
19. Lancer l'agent au démarrage (systemd, keychain, Windows)
20. Agent forwarding : fonctionnement et **dangers**
21. `ssh-copy-id` et déploiement des clés
22. `authorized_keys` : format
23. Restreindre par origine : `from=`
24. Clés à usage unique : `command=`, `no-pty`
25. `no-agent-forwarding`, `no-X11-forwarding`, `no-port-forwarding`
26. `ProxyJump` et bastions
27. Chaînes de sauts : plusieurs bastions
28. Multiplexage : `ControlMaster`, `ControlPath`, `ControlPersist`
29. Mesurer le gain du multiplexage
30. Tunnels locaux (`-L`)
31. Tunnels distants (`-R`)
32. Tunnels dynamiques (`-D`, SOCKS)
33. Cas concret : accéder à une UI web interne
34. Cas concret : exposer un service local vers l'extérieur
35. Tunnels : bonnes pratiques et pièges
36. SCP : copies simples
37. SFTP : transferts interactifs et batch
38. rsync over SSH
39. Optimiser les transferts (compression, algorithmes)
40. X11 forwarding : usage et prudence
41. `sshd_config` : structure et rechargement
42. Durcissement : authentification (`PermitRootLogin`, `PasswordAuthentication`)
43. Durcissement : `AllowUsers` / `AllowGroups`
44. Durcissement : `MaxAuthTries`, `MaxSessions`, `LoginGraceTime`
45. Algorithmes modernes : Ciphers, MACs, KexAlgorithms
46. Désactiver ce qui ne sert pas (X11, tunnels sélectifs)
47. Bannières et informations divulguées
48. fail2ban pour SSH
49. 2FA par TOTP
50. Clé + 2FA combinés (`AuthenticationMethods`)
51. Certificats SSH : tour d'horizon
52. Certificats : créer une CA et signer des clés
53. Certificats côté serveur : confiance et révocation
54. Supervision : les logs d'authentification
55. Audit des connexions : `last`, `who`, `ss`
56. Alertes sur connexions suspectes
57. Sauvegarder ses clés (critique)
58. Rotation des clés
59. Inventaire des clés autorisées sur le parc
60. Mettre à jour OpenSSH
61. Dépannage : la méthode (`-v`, `-vvv`)
62. Cas 1 : `Permission denied (publickey)`
63. Cas 2 : `Host key verification failed`
64. Cas 3 : la connexion freeze / timeout
65. Cas 4 : l'agent ne transmet pas la clé (forwarding)
66. Cas 5 : connexion lente à s'établir (DNS, GSSAPI)
67. Cas 6 : `Too many authentication failures`
68. Cas 7 : `WARNING: UNPROTECTED PRIVATE KEY FILE`
69. Cas 8 : `ssh-copy-id` échoue
70. Cas 9 : le tunnel ne répond pas / port déjà utilisé
71. Cas 10 : multiplexage, `ControlPath` trop long
72. Cas 11 : X11 forwarding sans `DISPLAY`
73. Cas 12 : SFTP en chroot qui échoue
74. Les erreurs classiques (tableau récapitulatif)
75. Cas pratique 1 : bastion d'équipe
76. Cas pratique 2 : clé restreinte pour déploiement (`command=`)
77. Cas pratique 3 : accès à une UI d'administration via tunnel
78. Cas pratique 4 : backup distant avec rsync over SSH
79. Cas pratique 5 : jump host multi-sites
80. Pense-bête de poche : commandes
81. Pense-bête : `ssh_config` minimal propre
82. Pense-bête : `sshd_config` durci minimal
83. Glossaire
84. Quiz (10 questions + réponses)
85. Pour aller plus loin

---

## 1. Objectifs du guide

Ce guide a trois objectifs :

1. **Maîtriser l'outil** : configurer un client SSH rapide, fiable et agréable
   (alias, multiplexage, bastions, tunnels) pour un usage quotidien intensif.
2. **Sécuriser** : durcir les serveurs (clés uniquement, 2FA, fail2ban,
   algorithmes modernes), gérer les clés comme des secrets de production.
3. **Exploiter** : superviser les accès, sauvegarder les clés, dépanner vite
   avec une méthode et des cas documentés.

À la fin, tu dois pouvoir : écrire un `~/.ssh/config` d'équipe propre,
durcir un `sshd_config` de zéro, diagnostiquer 90 % des pannes SSH en moins
de 5 minutes, et expliquer à ton équipe pourquoi l'agent forwarding est
dangereux.

## 2. SSH : le protocole en notions

SSH (Secure Shell) est un protocole réseau chiffré qui fournit :

- un **shell distant sécurisé** (le cas d'usage historique, remplaçant Telnet,
  rlogin, rsh qui transmettaient tout en clair, mots de passe inclus) ;
- un **transport générique** : copie de fichiers (SCP/SFTP), tunnels TCP,
  proxy SOCKS, X11, rsync, Git, etc. ;
- une **authentification forte** : clés cryptographiques, certificats,
  second facteur.

Architecture en trois couches (modèle du RFC 4251) :

| Couche | Rôle | Exemple |
|---|---|---|
| Transport (RFC 4253) | Chiffrement, intégrité, authentification du **serveur** | Négociation des algorithmes, échange de clés |
| Authentification utilisateur (RFC 4252) | Authentification du **client** | publickey, password, keyboard-interactive |
| Connexion (RFC 4254) | Multiplexage de **canaux** logiques | shell, exec, sftp, tcpip-forward |

Point clé : **une seule connexion TCP** transporte plusieurs canaux
(shell + SFTP + tunnels simultanés). C'est ce qui rend le multiplexage
(section 28) et les tunnels (sections 30-32) possibles.

Port par défaut : **22/TCP**. Le changer (section 42) réduit le bruit des
robots mais n'est pas une mesure de sécurité à lui seul.

## 3. Chiffrement : ce qu'il faut retenir

Sans entrer dans la cryptographie, voici le minimum opérationnel :

- **Chiffrement symétrique** (rapide, pour les données) : `chacha20-poly1305`,
  `aes256-gcm`, `aes128-ctr`. Négocié à chaque connexion.
- **Échange de clés** (pour se mettre d'accord sur la clé de session sans
  l'exposer) : `curve25519-sha256` (moderne, rapide), `diffie-hellman-group16-sha512`.
  Fournit la **confidentialité persistante** (perfect forward secrecy) : même si
  la clé privée du serveur fuit un jour, les sessions passées restent illisibles.
- **Clé d'hôte** (authentification du serveur) : le serveur prouve son identité
  avec sa clé privée d'hôte (`/etc/ssh/ssh_host_*`) ; le client vérifie avec
  l'empreinte stockée dans `~/.ssh/known_hosts`.
- **Signatures** (authentification du client par clé) : le client signe un
  défi avec sa clé privée ; le serveur vérifie avec la clé publique de
  `authorized_keys`. La clé privée **ne voyage jamais** sur le réseau.

Retiens surtout : avec l'authentification par clé, **rien de secret ne transite**
(même chiffré) hormis la signature d'un défi unique. C'est structurellement
plus sûr qu'un mot de passe, même avec un bon mot de passe.

Algorithmes à éviter aujourd'hui : `3des`, `blowfish`, `arcfour`, `md5`-based
MACs, `diffie-hellman-group1-sha1`, clés RSA < 2048 bits, DSA. Voir section 45
pour la configuration moderne.

## 4. Handshake : établissement d'une session (notions)

Séquence simplifiée d'une connexion `ssh zelef@serveur` :

1. **TCP** : le client ouvre une connexion vers le port 22 du serveur.
2. **Négociation** : client et serveur échangent leurs listes d'algorithmes
   (KEX, chiffrement, MAC, compression) et choisissent le meilleur commun.
3. **Échange de clés** : Diffie-Hellman / Curve25519 produit une clé de session
   éphémère ; tout le reste de la session est chiffré et intègre.
4. **Authentification du serveur** : le serveur signe avec sa clé d'hôte ; le
   client compare l'empreinte à `known_hosts` (sections 7-8).
5. **Authentification du client** : `publickey` (signature d'un défi),
   `password` ou `keyboard-interactive` (2FA), selon la config du serveur.
6. **Ouverture des canaux** : shell, commande distante, SFTP, tunnels.

En pratique, les étapes 1 à 4 prennent une fraction de seconde en LAN ; c'est
le multiplexage (section 28) qui les rend quasi gratuites pour les connexions
suivantes vers le même hôte.

## 5. Méthodes d'authentification

| Méthode | Principe | Usage |
|---|---|---|
| `publickey` | Signature par clé privée du client | **Standard en production** |
| `password` | Mot de passe transmis dans le tunnel chiffré | À désactiver sur Internet (section 42) |
| `keyboard-interactive` | Échanges question/réponse (PAM) | Supporte le **2FA TOTP** (section 49) |
| `hostbased` | Confiance entre machines | Rare, fragile, à éviter |
| `gssapi-with-mic` | Kerberos | Environnements AD/Kerberos |

Ordre essayé par le client : configurable via `PreferredAuthentications`.
En production, vise : `publickey` obligatoire, éventuellement `+ keyboard-interactive`
pour le second facteur (section 50).

## 6. Installation d'OpenSSH

**Debian / Ubuntu :**

```bash
client$ sudo apt update && sudo apt install -y openssh-client
serveur# apt update && apt install -y openssh-server
serveur# systemctl enable --now ssh
serveur# systemctl status ssh
```

**RHEL / Rocky / AlmaLinux :**

```bash
serveur# dnf install -y openssh-server
serveur# systemctl enable --now sshd
```

**Windows 10/11** (client et serveur intégrés depuis 2018) :

```powershell
# En administrateur
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

**macOS** : client préinstallé. Serveur : Réglages → Général → Partage →
« Connexion à distance » (ou `sudo systemsetup -setremotelogin on`).

Vérifier la version (utile pour le dépannage et les CVE) :

```bash
client$ ssh -V
OpenSSH_9.6p1 Ubuntu-3ubuntu13.2, OpenSSL 3.0.13 30 Jan 2024
```

## 7. Premier contact : `ssh`, clés d'hôte, `known_hosts`

```bash
client$ ssh zelef@192.0.2.10
The authenticity of host '192.0.2.10 (192.0.2.10)' can't be established.
ED25519 key fingerprint is SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz0123456789ABc.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Ce message apparaît **une seule fois par hôte** : c'est le modèle TOFU
(Trust On First Use). En tapant `yes`, l'empreinte est ajoutée à
`~/.ssh/known_hosts`. Aux connexions suivantes, si l'empreinte **change**,
SSH refuse de se connecter : c'est la protection contre l'homme-du-milieu.

Commandes utiles :

```bash
client$ ssh-keygen -l -f ~/.ssh/known_hosts   # lister les empreintes connues
client$ ssh-keygen -F 192.0.2.10              # chercher un hôte précis
client$ ssh-keygen -R 192.0.2.10              # retirer un hôte (après réinstall)
client$ ssh -o StrictHostKeyChecking=accept-new zelef@192.0.2.10
```

`StrictHostKeyChecking=accept-new` : ajoute automatiquement les hôtes inconnus
mais **refuse** toujours un changement d'empreinte. Bon compromis pour les
scripts (jamais `=no`, qui désactive toute vérification).

## 8. Vérifier l'empreinte d'un hôte (TOFU)

Le TOFU aveugle (`yes` sans réfléchir) est vulnérable à une attaque
homme-du-milieu **lors de la première connexion**. En production, vérifie
l'empreinte par un canal indépendant :

**Côté serveur** (à faire une fois, à conserver dans ta doc d'infra) :

```bash
serveur# for f in /etc/ssh/ssh_host_*_key.pub; do
  echo "== $f"; ssh-keygen -l -f "$f"
done
# 256 SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz0123456789ABc root@web1 (ED25519)
# 3072 SHA256:ZyXwVuTsRqPoNmLkJiHgFeDcBa9876543210ZYx root@web1 (RSA)
```

**Alternatives propres au TOFU aveugle :**

- **SSHFP dans le DNS** (avec DNSSEC) : `ssh -o VerifyHostKeyDNS=yes`.
  Publie avec `ssh-keygen -r web1.example.com`.
- **Certificats SSH** (section 51) : plus de TOFU du tout, la CA signe les
  clés d'hôte.
- **Inventaire interne** : page wiki avec les empreintes de chaque serveur,
  vérifiée hors bande.

En pratique d'équipe : documente les empreintes au provisionnement
(Ansible peut les afficher), et apprends à tes collègues à comparer
**au moins les 8 premiers et 8 derniers caractères** avant de taper `yes`.

## 9. `ssh_config` client : structure

Le fichier `~/.ssh/config` (permissions `600`) évite de retaper les options.
Priorité : **la première valeur obtenue gagne** (contrairement à
`sshd_config`). Ordre de lecture : ligne de commande > `~/.ssh/config` >
`/etc/ssh/ssh_config`.

```bash
client$ ls -l ~/.ssh/config
-rw------- 1 zelef zelef 2341 Sep 26 2026 /home/zelef/.ssh/config
client$ chmod 600 ~/.ssh/config   # si besoin
```

Structure type :

```
# Bloc global : s'applique à tout (mettre les défauts personnels)
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    HashKnownHosts yes
    StrictHostKeyChecking accept-new

# Blocs spécifiques : le premier bloc correspondant gagne pour chaque option
Host web1
    HostName 192.0.2.10
    User zelef
    Port 2222
    IdentityFile ~/.ssh/id_ed25519_web
```

Tester la résolution effective d'un hôte (indispensable au débogage) :

```bash
client$ ssh -G web1 | grep -E '^(hostname|user|port|identityfile|proxyjump) '
hostname 192.0.2.10
user zelef
port 2222
identityfile ~/.ssh/id_ed25519_web
```

`ssh -G` affiche la configuration finale après fusion : c'est **l'outil n°1**
pour comprendre « pourquoi SSH ne prend pas ma clé / mon port ».

## 10. `Host` et alias

`Host` définit un motif ; tout ce qui suit s'applique aux hôtes correspondants.
Les motifs supportent `*` et `?`.

```
# Alias simple et mémorisable
Host web1
    HostName 192.0.2.10
    User zelef

# Motif : tous les serveurs d'un site
Host *.site-paris.example.com
    User admin
    ProxyJump bastion-paris

# Motif : tout un /24 via un bastion (pratique, à manier avec soin)
Host 10.20.30.*
    User zelef
    ProxyJump bastion

# Plusieurs noms pour le même bloc
Host db db-principal
    HostName 192.0.2.50
    User postgres-admin
```

Règles d'or :

- Des alias **courts et stables** (`web1`, `db`, `bastion`) plutôt que des IP
  dans les scripts et la doc d'équipe.
- Le bloc `Host *` **toujours en dernier** : comme la première valeur gagne,
  un `Host *` placé en premier écraserait les blocs spécifiques.
- `ssh web1` utilise l'alias ; `ssh 192.0.2.10` utilise l'IP brute (et donc
  potentiellement d'autres options — attention aux surprises avec les motifs
  en `*`).

## 11. `HostName`, `User`, `Port`

```
Host web1
    HostName 192.0.2.10        # le vrai nom/IP ; l'alias reste "web1"
    User zelef                 # fini les zelef@ à chaque fois
    Port 2222                  # si sshd n'écoute pas sur 22
```

- `HostName` accepte un nom DNS, une IPv4 ou IPv6.
- `User` : l'utilisateur distant. Ne mets jamais `root` ici par défaut ;
  crée un bloc dédié explicite si tu en as besoin (avec un commentaire).
- `Port` : utile quand le DSI impose un port non standard ou quand plusieurs
  sshd tournent derrière une même IP (NAT).

Astuce : `%h` et `%p` dans `ProxyCommand`/`ProxyJump` reprennent l'hôte et le
port calculés — pratique pour les modèles génériques (section 27).

## 12. `IdentityFile` : une clé par hôte

```
Host web1
    HostName 192.0.2.10
    IdentityFile ~/.ssh/id_ed25519_web

Host github-perso
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_perso

Host *
    IdentitiesOnly yes
```

- `IdentityFile` peut être répété : SSH essaie chaque clé dans l'ordre.
- **`IdentitiesOnly yes`** : n'essaie **que** les clés configurées (pas celles
  de l'agent). Sans ça, SSH propose toutes les clés de l'agent, ce qui peut
  déclencher `Too many authentication failures` (section 67) et divulgue
  quelles clés tu possèdes.
- Séparer les clés par usage (perso / pro / client X) limite l'impact d'une
  compromission et clarifie l'inventaire (section 59).

## 13. Options client par hôte : les plus utiles

```
Host web1
    HostName 192.0.2.10
    # Rester connecté à travers les NAT/firewalls qui coupent les sessions idle
    ServerAliveInterval 60      # ping toutes les 60 s
    ServerAliveCountMax 3       # 3 échecs -> coupe proprement
    # Ne pas transmettre l'agent par défaut (voir section 20)
    ForwardAgent no
    # Ne pas faire suivre X11 par défaut
    ForwardX11 no
    # Compression pour les liens lents (pas en LAN)
    Compression yes
    # Timeout de connexion : ne pas attendre 2 minutes sur un hôte mort
    ConnectTimeout 10
    # Journaliser dans un fichier dédié pour cet hôte sensible
    LogLevel VERBOSE
```

Tableau des options client les plus rentables :

| Option | Effet | Quand l'utiliser |
|---|---|---|
| `ServerAliveInterval` / `ServerAliveCountMax` | keepalive | Toujours (NAT, Wi-Fi) |
| `ConnectTimeout` | délai max de connexion | Scripts, inventaires |
| `IdentitiesOnly yes` | limite les clés essayées | Toujours avec plusieurs clés |
| `StrictHostKeyChecking accept-new` | TOFU strict | Scripts non interactifs |
| `BatchMode yes` | jamais de prompt | Cron, Ansible |
| `LogLevel` | verbosité | `VERBOSE` pour auditer |
| `AddKeysToAgent yes` | ajoute la clé à l'agent à la 1re utilisation | Confort + passphrase (section 18) |

## 14. `Match`, `Include` et organisation du fichier

`Match` applique un bloc selon des **critères** (pas seulement le nom) :

```
# Appliquer à tous les hôtes sauf le bastion lui-même
Match host !bastion
    ProxyJump bastion

# Options différentes selon le réseau d'origine
Match originalhost 192.168.*.*
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
```

`Include` découpe la config en fichiers (pratique en équipe) :

```
Include ~/.ssh/config.d/*.conf
```

Organisation recommandée :

```
~/.ssh/
├── config                  # squelette : Include + Host * global
├── config.d/
│   ├── 00-defaults.conf    # défauts globaux
│   ├── 10-perso.conf       # machines perso
│   ├── 20-prod.conf        # production (généré/déployé par Ansible)
│   └── 30-clients.conf     # un fichier par client
└── known_hosts
```

Avantage : le fichier `20-prod.conf` peut être **déployé et versionné** par
l'équipe, sans toucher aux alias personnels de chacun.

## 15. Générer une clé ed25519

Ed25519 est le choix par défaut moderne : 256 bits, rapide, clés courtes.

```bash
client$ ssh-keygen -t ed25519 -C "zelef@poste-prod-$(date +%Y-%m-%d)" -f ~/.ssh/id_ed25519
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase): ********
Enter same passphrase again: ********
Your identification has been saved in /home/zelef/.ssh/id_ed25519
Your public key has been saved in /home/zelef/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz0123456789ABc zelef@poste-prod-2026-09-26
```

- `-C` : commentaire = **traçabilité** (qui, quelle machine, quand). Toujours
  le renseigner : dans 2 ans, tu sauras à quoi correspond cette clé.
- `-f` : nom explicite, surtout si tu gères plusieurs clés.
- **Permissions** : `600` sur la privée (`ssh-keygen` le fait), `644` sur la
  publique. SSH refuse une clé privée trop permissive (section 68).

Vérifier une clé existante :

```bash
client$ ssh-keygen -l -f ~/.ssh/id_ed25519.pub
256 SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz0123456789ABc zelef@poste-prod-2026-09-26 (ED25519)
client$ ssh-keygen -y -f ~/.ssh/id_ed25519   # régénère la publique depuis la privée
```

## 16. RSA 4096 : quand s'en servir encore

```bash
client$ ssh-keygen -t rsa -b 4096 -C "zelef@legacy" -f ~/.ssh/id_rsa_4096
```

Garde RSA 4096 pour :

- les **vieux équipements** (switches, iLO/iDRAC anciens, appliances) qui ne
  connaissent pas ed25519 ;
- les **vieux serveurs** (OpenSSH < 6.5 côté serveur, rare aujourd'hui).

Règles :

- **4096 bits minimum** (2048 est le plancher tolérable, 1024 = à détruire).
- Ne génère plus de DSA (`ssh-dss`) ni d'ECDSA : DSA est désactivé par défaut
  depuis OpenSSH 7.0, ECDSA n'apporte rien face à ed25519.
- Si un équipement n'accepte que RSA 1024 ou DSA : c'est un **signal d'alerte**
  (firmware à mettre à jour, équipement à remplacer), pas une config à banaliser.

## 17. Passphrase : la choisir et la gérer

Une clé **sans passphrase** = quiconque lit le fichier se connecte partout où
la clé est autorisée. Sur un poste de travail, c'est inacceptable.

- Mets **toujours** une passphrase sur les clés d'un poste (portable++).
- Exceptions légitimes (clés **sans** passphrase) : comptes de service pour
  scripts/cron — mais alors clé **restreinte** (`command=`, `from=`, section 24)
  et jamais sur un poste utilisateur.
- Changer la passphrase sans changer la clé :

```bash
client$ ssh-keygen -p -f ~/.ssh/id_ed25519
```

- Une passphrase n'a pas besoin d'être compliquée à taper 50 fois par jour :
  c'est `ssh-agent` (section 18) qui la mémorise pour la session. Choisis-la
  **longue** (4-5 mots), pas « complexe ».

## 18. `ssh-agent` et `ssh-add`

L'agent garde les clés déchiffrées en mémoire : tu tapes la passphrase **une
fois** par session, plus jamais ensuite.

```bash
client$ eval "$(ssh-agent -s)"
Agent pid 1234
client$ ssh-add ~/.ssh/id_ed25519
Enter passphrase for /home/zelef/.ssh/id_ed25519: ********
Identity added: /home/zelef/.ssh/id_ed25519 (zelef@poste-prod-2026-09-26)
client$ ssh-add -l        # clés chargées
256 SHA256:AbCd... (ED25519)
client$ ssh-add -t 8h ~/.ssh/id_ed25519   # durée de vie limitée : bonne hygiène
client$ ssh-add -d ~/.ssh/id_ed25519      # retirer une clé
client$ ssh-add -D                        # tout vider (verrouillage de poste)
```

Bonnes pratiques :

- `-t <durée>` : limite la durée de vie en mémoire (ex. `8h` = journée de
  travail). Si le poste est compromis, la fenêtre d'abus est bornée.
- `AddKeysToAgent yes` dans `ssh_config` : la première utilisation d'une clé
  l'ajoute à l'agent automatiquement (avec `AddKeysToAgent 2h` pour la durée).
- `ssh-add -D` en verrouillant ton poste le soir, ou via le script de
  verrouillage d'écran.

## 19. Lancer l'agent au démarrage

**Linux + systemd (user service)** — `~/.config/systemd/user/ssh-agent.service` :

```ini
[Unit]
Description=SSH key agent

[Service]
Type=simple
Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
ExecStart=/usr/bin/ssh-agent -D -a $SSH_AUTH_SOCK

[Install]
WantedBy=default.target
```

```bash
client$ systemctl --user enable --now ssh-agent
# Dans ~/.bashrc ou ~/.profile :
export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"
```

**GNOME / Ubuntu desktop** : le trousseau GNOME fournit un agent compatible
(`SSH_AUTH_SOCK` déjà positionné) ; `ssh-add` suffit.

**macOS** : l'agent est lancé par le système ; persistance dans le trousseau :

```
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_ed25519
```

**Windows (OpenSSH natif)** : service `ssh-agent` à passer en automatique :

```powershell
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

## 20. Agent forwarding : fonctionnement et **dangers**

`ssh -A bastion` (ou `ForwardAgent yes`) expose ton agent local **au serveur
distant** via un socket. Depuis ce serveur, `ssh autre-machine` utilise tes
clés locales sans les copier. Pratique pour les chaînes de sauts… et dangereux :

- **Quiconque a root sur le serveur distant peut utiliser ton agent** tant que
  tu es connecté (il ne peut pas *extraire* les clés, mais il peut *signer*
  avec : se connecter partout où tes clés sont autorisées).
- Le risque persiste même après déconnexion si le socket n'est pas nettoyé.

Règles d'équipe :

1. **Jamais** de forwarding vers une machine que tu n'administres pas
   entièrement (mutualisé, client, prestataire).
2. Préfère **`ProxyJump`** (section 26) : l'authentification vers la cible se
   fait **depuis ton poste**, le bastion ne voit que du trafic chiffré.
3. Si tu dois vraiment forwarder (vieux workflow) : `ForwardAgent yes`
   **uniquement** sur le bloc `Host` concerné, jamais dans `Host *`, et
   déconnecte-toi dès que c'est fini.
4. Alternative moderne : les **certificats SSH** (section 51) suppriment le
   besoin de forwarding dans la plupart des cas.

Vérifier qu'un forwarding est actif (et le couper si surprise) :

```bash
serveur$ echo "$SSH_AUTH_SOCK"
# vide = pas de forwarding ; /tmp/ssh-XXX/agent.PID = forwarding actif
```

## 21. `ssh-copy-id` et déploiement des clés

```bash
client$ ssh-copy-id -i ~/.ssh/id_ed25519.pub zelef@web1
```

`ssh-copy-id` ajoute ta clé publique à `~/.ssh/authorized_keys` distant en
créant le répertoire avec les bonnes permissions. Options utiles :

```bash
client$ ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 zelef@web1   # port custom
client$ ssh-copy-id -i ~/.ssh/id_ed25519.pub -o ProxyJump=bastion zelef@web1
```

À l'échelle d'un parc, **n'utilise pas `ssh-copy-id` à la main** : déploie
`authorized_keys` via Ansible (template + `authorized_key`), Puppet ou ton
provisionnement cloud (cloud-init). `ssh-copy-id` reste parfait pour les
machines isolées et le dépannage.

Vérification post-déploiement :

```bash
client$ ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes web1 'echo OK'
```

## 22. `authorized_keys` : format

Fichier `~/.ssh/authorized_keys` (permissions `600`, répertoire `~/.ssh` en
`700`) côté serveur. Une clé par ligne :

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEXEMPLEdefictifNePasUtiliser zelef@poste-prod-2026-09-26
```

Format : `<type> <clé en base64> <commentaire>`. Les lignes vides et celles
commençant par `#` sont ignorées. Les **options** se placent en début de ligne,
séparées par des virgules (sections 23-25) :

```
from="10.0.0.0/8",command="/usr/local/bin/backup.sh",no-pty ssh-ed25519 AAAAC3... zelef@backup
```

Erreurs fréquentes : clé coupée/collée avec un saut de ligne au milieu,
commentaire contenant une virgule non échappée dans les options, permissions
`~/.ssh` ou home trop permissives (le serveur refuse alors silencieusement la
clé si `StrictModes yes`, défaut).

## 23. Restreindre par origine : `from=`

```
from="192.168.10.0/24,2001:db8::/32" ssh-ed25519 AAAAC3... zelef@poste-admin
from="bastion.example.com" ssh-ed25519 AAAAC3... deploy@ci
```

- La clé n'est acceptée que si la connexion vient de ces adresses/noms.
- **Oblige** à passer par le bastion ou le réseau d'administration : même si la
  clé privée fuit, elle est inutilisable depuis Internet.
- Le motif DNS est résolu à chaque connexion : avec `UseDNS no` côté serveur
  (recommandé, section 65), préfère les IP/CIDR.
- Combine avec `AllowUsers`/`AllowGroups` côté serveur pour la défense en
  profondeur.

## 24. Clés à usage unique : `command=`, `no-pty`

```
command="/usr/local/bin/deploy.sh",no-pty,no-agent-forwarding,no-X11-forwarding,no-port-forwarding ssh-ed25519 AAAAC3... ci@runner
```

- `command="..."` : **ignore** la commande demandée par le client et exécute
  toujours celle-ci. Idéal pour : scripts de backup, déploiement CI, tunnels
  dédiés, `rrsync`.
- Le script reçoit la commande originale dans `$SSH_ORIGINAL_COMMAND` et peut
  décider de l'autoriser ou non (pattern classique : wrapper qui valide).
- `no-pty` : pas de terminal interactif → la clé ne sert qu'en non-interactif.
- Toujours combiner `command=` avec les `no-*` (section 25) : sinon la clé
  « restreinte » peut quand même ouvrir des tunnels.

Exemple de wrapper :

```bash
#!/bin/bash
# /usr/local/bin/deploy.sh — n'autorise que le script de déploiement attendu
case "$SSH_ORIGINAL_COMMAND" in
  "/opt/app/deploy.sh production") exec /opt/app/deploy.sh production ;;
  *) echo "Commande non autorisée : $SSH_ORIGINAL_COMMAND" >&2; exit 1 ;;
esac
```

## 25. `no-agent-forwarding`, `no-X11-forwarding`, `no-port-forwarding`

Tableau des options de restriction par clé :

| Option | Effet |
|---|---|
| `no-agent-forwarding` | Interdit le forwarding d'agent avec cette clé |
| `no-X11-forwarding` | Interdit le X11 forwarding |
| `no-port-forwarding` | Interdit `-L`, `-R`, `-D` |
| `no-pty` | Pas d'allocation de pseudo-terminal |
| `no-user-rc` | Ignore `~/.ssh/rc` |
| `permitopen="host:port"` | Avec `no-port-forwarding` absent : limite les tunnels `-L` à ces destinations |
| `permitlisten="port"` | Limite les tunnels `-R` à ces ports |

Recette « clé de service minimale » :

```
from="10.0.5.0/24",command="/usr/local/bin/backup.sh",no-pty,no-agent-forwarding,no-X11-forwarding,no-port-forwarding ssh-ed25519 AAAAC3... backup@client
```

Principe : **moindre privilège**. Une clé de backup n'a pas besoin d'un shell,
ni de tunnels, ni de forwarding d'agent. Si elle fuit, l'attaquant ne peut
exécuter que le script de backup depuis le réseau de backup.

## 26. `ProxyJump` et bastions

Le bastion (jump host) est le point d'entrée unique vers un réseau privé.
`ProxyJump` (option `-J`) fait transiter la connexion **chiffrée de bout en
bout** : le bastion relaie des octets, il ne voit ni tes clés ni le contenu.

```bash
client$ ssh -J bastion web1-interne
client$ ssh -J zelef@bastion:2222 web1-interne
```

En config (la forme à privilégier) :

```
Host bastion
    HostName bastion.example.com
    User zelef

Host web1-interne
    HostName 10.10.1.10
    User zelef
    ProxyJump bastion
```

Puis simplement `ssh web1-interne`. Avantages sur le forwarding d'agent :

- L'authentification vers la cible part **de ton poste** (l'agent local signe).
- Le bastion ne peut pas réutiliser tes clés.
- Fonctionne avec `scp`, `sftp`, `rsync`, `git`, `ansible` sans option spéciale.

Copie de fichiers à travers un bastion :

```bash
client$ scp -J bastion rapport.pdf web1-interne:/tmp/
client$ rsync -avz -e "ssh -J bastion" ./app/ web1-interne:/opt/app/
```

## 27. Chaînes de sauts : plusieurs bastions

`ProxyJump` accepte une liste séparée par des virgules (évaluée de gauche à
droite, du plus proche au plus lointain) :

```bash
client$ ssh -J bastion-paris,bastion-dmz srv-dmz-interne
```

```
Host bastion-paris
    HostName bastion.paris.example.com
    User zelef

Host bastion-dmz
    HostName 10.20.0.5
    User zelef
    ProxyJump bastion-paris

Host srv-dmz-interne
    HostName 10.30.0.8
    User zelef
    ProxyJump bastion-dmz
```

Pour les cas exotiques (bastion qui n'est pas SSH, ou besoin d'une commande
spécifique), l'ancêtre `ProxyCommand` reste disponible :

```
Host srv-via-vieux-bastion
    HostName 10.40.0.9
    ProxyCommand ssh -W %h:%p vieux-bastion
```

`ssh -W %h:%p` ouvre un simple relais TCP via le bastion : c'est exactement ce
que `ProxyJump` fait en interne, en plus simple. `ProxyCommand` avec `nc` ou
des wrappers est à éviter sauf besoin réel (chaque maillon ajoute de la
fragilité).

Conseil d'architecture : **un bastion par zone réseau**, pas un bastion
mondial. Chaque saut supplémentaire = latence + point de panne. Au-delà de
deux sauts, interroge le design réseau.

## 28. Multiplexage : `ControlMaster`, `ControlPath`, `ControlPersist`

Le multiplexage réutilise **une seule connexion TCP** pour plusieurs sessions
vers le même hôte : le 2e `ssh`, le `scp`, le `rsync` passent par le tunnel
déjà établi. Résultat : connexion quasi instantanée, une seule authentification.

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h:%p
    ControlPersist 10m
```

- `ControlMaster auto` : la première connexion devient maîtresse, les
  suivantes s'y rattachent automatiquement.
- `ControlPath` : chemin du socket de contrôle. `%r` = user, `%h` = hôte,
  `%p` = port. **Crée le répertoire** (`mkdir -p ~/.ssh/sockets`) sinon rien
  ne fonctionne, silencieusement.
- `ControlPersist 10m` : la connexion maîtresse survit 10 minutes après la
  fermeture du dernier client → les connexions suivantes restent instantanées
  même si tu as tout fermé.

Gestion manuelle :

```bash
client$ ssh -O check web1        # la connexion maîtresse est-elle active ?
client$ ssh -O stop web1         # fermer proprement la maîtresse
client$ ssh -O exit web1         # variante : ferme aussi les sessions actives
client$ ssh -S ~/.ssh/sockets/zelef@web1:22 -O check web1
```

Points d'attention :

- Ne pas multiplexer vers des hôtes **non fiables** : toutes les sessions
  partagent le même niveau de confiance.
- `ControlPersist` long sur un portable = session qui survit au changement de
  réseau et freeze (section 64). `10m` est un bon compromis ; `yes` (illimité)
  seulement sur poste fixe.
- Avec `ProxyJump`, le multiplexage s'applique aussi au saut : gros gain sur
  les chaînes de bastions.

## 29. Mesurer le gain du multiplexage

```bash
client$ time ssh web1 'exit'          # sans multiplexage (1re connexion)
real    0m1.842s
client$ time ssh web1 'exit'          # avec multiplexage (maîtresse active)
real    0m0.041s
```

Ordre de grandeur typique : **10 à 40× plus rapide** sur les connexions
suivantes, surtout à travers un bastion (2 handshakes évités) ou sur un lien
avec de la latence (chaque aller-retour du handshake coûte).

Test propre :

```bash
client$ for i in 1 2 3; do /usr/bin/time -f "%es" ssh -o ControlMaster=no web1 'exit'; done
client$ ssh -fN web1   # ouvre une maîtresse en fond (avec ControlPersist)
client$ for i in 1 2 3; do /usr/bin/time -f "%es" ssh web1 'exit'; done
```

En pratique d'équipe : Ansible, scripts de déploiement et `for` loops sur 50
machines passent de plusieurs minutes à quelques secondes. C'est le réglage
client au **meilleur ratio effort/gain** de tout ce guide.

## 30. Tunnels locaux (`-L`)

`-L [bind:]port_local:hôte_distant:port_distant` : un port ouvert **sur ton
poste** est relayé vers une destination **vue depuis le serveur SSH**.

```bash
# Le port 8080 local -> interface web d'admin du serveur (vue depuis le serveur)
client$ ssh -L 8080:localhost:9090 web1
# Puis http://localhost:8080 dans le navigateur
```

```
[ton poste]:8080 ---(chiffré via SSH)---> [web1] ---> localhost:9090 (vu par web1)
```

Variantes :

```bash
# Atteindre une autre machine du réseau privé via le serveur SSH
client$ ssh -L 3306:db-interne:3306 bastion
# Le tunnel n'écoute que sur 127.0.0.1 par défaut (bien). Pour l'ouvrir au LAN :
client$ ssh -L 0.0.0.0:8080:localhost:9090 web1     # nécessite GatewayPorts=yes côté serveur
```

En config :

```
Host tunnel-monitoring
    HostName web1
    LocalForward 8080 localhost:9090
    # -N : pas de shell, -f : en arrière-plan
```

```bash
client$ ssh -N -f tunnel-monitoring
```

Cas d'usage rois : accéder à une UI web interne (section 33), à une base de
données (MySQL/PostgreSQL), à un Elasticsearch/Kibana sans les exposer.

## 31. Tunnels distants (`-R`)

`-R [bind:]port_distant:localhost:port_local` : l'inverse — un port ouvert
**sur le serveur SSH** est relayé vers **ton poste** (ou ton réseau).

```bash
# Exposer ton serveur web local (port 8000) sur le port 9000 du serveur distant
client$ ssh -R 9000:localhost:8000 web1
# Depuis web1 : curl http://localhost:9000 -> atteint ton poste
```

```
[web1]:9000 ---(chiffré via SSH)---> [ton poste] ---> localhost:8000
```

Côté serveur, `sshd_config` contrôle ce qui est autorisé :

```
# Autoriser les -R mais seulement sur loopback (défaut sûr)
GatewayPorts no
# Pour exposer sur toutes les interfaces (à manier avec précaution) :
# GatewayPorts yes
# Pour autoriser seulement certains ports :
PermitListen 9000 9001
```

**Sécurité** : un `-R` avec `GatewayPorts yes` expose ton service local à tout
le réseau du serveur distant (voire Internet). Vérifie toujours sur quelle
interface le port distant écoute (`ss -tlnp` côté serveur). En équipe,
préfère `PermitListen` avec une liste blanche de ports.

Cas d'usage : donner à un collègue/serveur distant l'accès temporaire à un
service de ton poste, webhook de test, démo.

## 32. Tunnels dynamiques (`-D`, SOCKS)

`-D port_local` : ouvre un **proxy SOCKS** sur ton poste. Tout le trafic que
tu y envoies est relayé par le serveur SSH, avec résolution DNS côté serveur
si le client le demande.

```bash
client$ ssh -D 1080 bastion
# Firefox : proxy SOCKS5 manuel -> 127.0.0.1:1080, "Proxy DNS via SOCKS5" coché
# curl :
client$ curl --socks5-hostname 127.0.0.1:1080 http://intranet.local/
```

En config :

```
Host socks-bastion
    HostName bastion
    DynamicForward 1080
```

Différences avec `-L` :

| | `-L` (local) | `-D` (dynamique) |
|---|---|---|
| Destination | fixée à l'ouverture | choisie par connexion |
| Protocole | TCP brut | SOCKS4/5 |
| Usage | 1 service précis | navigation / multi-services |

Attention : le proxy SOCKS ne chiffre que **jusqu'au serveur SSH** ; au-delà,
c'est le protocole applicatif qui compte (HTTPS reste HTTPS, HTTP reste
clair sur le LAN distant). Et tout ton trafic navigateur passe par le serveur
distant : à réserver aux besoins pro, pas au surf perso.

## 33. Cas concret : accéder à une UI web interne

Situation : Proxmox sur `pve1` (10.10.1.5:8006), Grafana sur `mon1`
(10.10.1.20:3000), accessibles uniquement depuis le LAN, bastion en entrée.

```
Host bastion
    HostName bastion.example.com
    User zelef

Host pve1
    HostName 10.10.1.5
    User root
    ProxyJump bastion
    LocalForward 8006 localhost:8006

Host mon1
    HostName 10.10.1.20
    User zelef
    ProxyJump bastion
    LocalForward 3000 localhost:3000
```

```bash
client$ ssh -N -f pve1 && ssh -N -f mon1
# https://localhost:8006 -> Proxmox (accepte le certificat autosigné, normal)
# http://localhost:3000  -> Grafana
```

Pourquoi c'est mieux que d'exposer les UI : zéro port ouvert sur Internet,
authentification SSH (clé + éventuellement 2FA) **avant** même d'atteindre
l'applicatif, chiffrement de bout en bout. C'est le pattern standard pour
iDRAC/iLO, interfaces d'onduleurs (lien avec ton guide UPS), switchs web.

Astuce : `-N -f` + `ExitOnForwardFailure yes` dans la config pour que SSH
échoue franchement si le port local est déjà pris, plutôt que de tourner sans
tunnel (section 70).

## 34. Cas concret : exposer un service local vers l'extérieur

Situation : tu développes un webhook sur `localhost:5000`, un partenaire doit
le tester depuis Internet, via ton serveur `web1` public.

Côté serveur (`/etc/ssh/sshd_config.d/tunnels.conf`) :

```
# Autoriser ce -R précis, sur l'interface publique, port dédié
GatewayPorts clientspecified
PermitListen 18080
```

Côté client :

```bash
client$ ssh -R 0.0.0.0:18080:localhost:5000 web1
# Le partenaire accède à http://web1.example.com:18080
```

Garde-fous :

1. `PermitListen` borne les ports possibles (pas de squat du 80/443).
2. Protège l'applicatif exposé (authentification, pas de debug).
3. **Temporaire** : ce n'est pas une architecture d'hébergement. Pour du
   durable, reverse proxy (nginx/traefik) + TLS + authentification.
4. Journalise : `LogLevel VERBOSE` côté serveur trace les ouvertures de tunnels.

## 35. Tunnels : bonnes pratiques et pièges

Checklist avant d'ouvrir un tunnel en production :

- [ ] `ExitOnForwardFailure yes` : échec franc si le port est occupé.
- [ ] Bind sur `127.0.0.1` par défaut ; `0.0.0.0` seulement si besoin réel,
      avec `GatewayPorts` maîtrisé côté serveur.
- [ ] Pas de tunnel permanent sans supervision : un tunnel mort = un service
      mort silencieusement. Préfère `autossh` (reconnexion auto) :

```bash
client$ autossh -M 0 -N -f -o ServerAliveInterval=30 -o ServerAliveCountMax=3 \
    -L 8080:localhost:9090 web1
```

- [ ] Documente chaque tunnel persistant (qui, quoi, pourquoi, jusqu'à quand).
- [ ] Ne fais jamais transiter de données sensibles par un tunnel vers un
      serveur que tu n'administres pas.

Pièges classiques :

| Symptôme | Cause probable |
|---|---|
| `bind: Address already in use` | Port local occupé (autre tunnel, appli) |
| Tunnel « ouvert » mais rien ne répond | `ExitOnForwardFailure` absent + port occupé |
| Le tunnel coupe après quelques minutes | NAT/firewall idle → `ServerAliveInterval` |
| `-R` inaccessible depuis l'extérieur | `GatewayPorts no` côté serveur |

## 36. SCP : copies simples

SCP reste le plus rapide pour un fichier ou une arborescence simple :

```bash
client$ scp rapport.pdf web1:/tmp/                    # vers le distant
client$ scp web1:/var/log/syslog ./                   # depuis le distant
client$ scp -r ./site/ web1:/var/www/                 # récursif
client$ scp -P 2222 -i ~/.ssh/id_ed25519 f.bin web1:/tmp/   # port + clé
client$ scp -3 posteA:/f.bin posteB:/tmp/             # distant -> distant via ton poste
```

Note moderne : depuis OpenSSH 9.0, `scp` utilise le protocole SFTP en
sous-main (plus de `scp -O` sauf vieux serveurs). Les options restent les
mêmes à l'usage.

Limites : pas de reprise sur échec, pas de différentiel, permissions parfois
approximatives. Pour du sérieux, voir rsync (section 38).

## 37. SFTP : transferts interactifs et batch

SFTP = FTP sécurisé par SSH (à ne pas confondre avec FTPS). Session
interactive :

```bash
client$ sftp web1
sftp> lpwd / pwd          # répertoires local / distant
sftp> lls / ls
sftp> put rapport.pdf /tmp/
sftp> mput *.log
sftp> get /var/log/syslog ./
sftp> mkdir /tmp/depot && chmod 750 /tmp/depot
sftp> bye
```

Mode batch (scripts) :

```bash
client$ sftp -b batch.txt web1
# batch.txt :
#   put rapport.pdf /tmp/
#   chmod 640 /tmp/rapport.pdf
#   bye
client$ echo "put rapport.pdf /tmp/" | sftp -b - web1
```

Côté serveur, pour un accès **fichiers uniquement** (sans shell), le chroot
SFTP est la bonne architecture (voir cas 12, section 73) :

```
Match Group sftp-only
    ChrootDirectory /srv/sftp/%u
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

## 38. rsync over SSH

L'outil roi des synchronisations : différentiel (ne transfère que les
changements), reprise, préservation des permissions.

```bash
# Syntaxe de base
client$ rsync -avz --progress ./site/ web1:/var/www/site/
# -a archive (permissions, dates, liens), -v verbeux, -z compression

# Options SSH personnalisées via -e
client$ rsync -avz -e "ssh -p 2222 -i ~/.ssh/id_ed25519" ./site/ web1:/var/www/

# À travers un bastion
client$ rsync -avz -e "ssh -J bastion" ./site/ web1-interne:/var/www/

# Supprimer côté distant ce qui n'existe plus en local (DANGEREUX sans --dry-run)
client$ rsync -avz --delete --dry-run ./site/ web1:/var/www/site/
```

Le `/` final sur la source change tout : `./site/` synchronise le **contenu**,
`./site` crée le **répertoire** lui-même. En cas de doute : `--dry-run`
d'abord.

Pour les gros transferts : `--partial --progress` (reprise), `--bwlimit=10000`
(Ko/s, pour ne pas saturer un lien de prod en journée).

## 39. Optimiser les transferts (compression, algorithmes)

- `-z` (rsync/scp) ou `Compression yes` : utile sur liens lents ou données
  compressibles (logs, textes). **Inutile voire contre-productif** en LAN
  10 Gb/s ou sur données déjà compressées (zip, jpg, vidéos) : ça consomme du
  CPU pour rien.
- Algorithme de chiffrement rapide pour les gros volumes :
  `chacha20-poly1305` est souvent le plus rapide sans AES-NI ; avec AES-NI,
  `aes128-gcm` gagne. Test :

```bash
client$ scp -c chacha20-poly1305 gros-fichier.bin web1:/tmp/   # si dispo
# ou en config :
Host transfert-rapide
    HostName web1
    Ciphers chacha20-poly1305@openssh.com
```

- `NoneSwitch`/`none` cipher : existe en patch uniquement, **à proscrire**.
- Paralléliser : plusieurs `rsync` sur des sous-répertoires > un seul flux,
  sur les liens à forte latence (chaque flux TCP a sa fenêtre).

## 40. X11 forwarding : usage et prudence

Permet d'afficher une application graphique distante sur ton poste :

```bash
client$ ssh -X web1          # forwarding X11 "non fiable" (restrictions)
client$ ssh -Y web1          # forwarding "fiable" (pleins droits) — À ÉVITER
```

```
Host appli-graphique
    HostName web1
    ForwardX11 yes
    ForwardX11Trusted no     # équivalent -X, jamais -Y par défaut
```

**Prudence** (c'est un vecteur d'attaque réel) :

- `-Y`/`ForwardX11Trusted yes` donne au serveur distant un accès quasi total à
  ton serveur X local (capture d'écran, injection de touches). Ne l'utilise
  que vers des machines de confiance absolue, et jamais en `Host *`.
- Le X11 forwarding transmet aussi des données non chiffrées côté serveur X
  local si celui-ci écoute en TCP : sur un poste moderne c'est du socket Unix,
  OK.
- Alternative moderne : pour une appli graphique, préfère un accès via
  navigateur + tunnel `-L` (section 33), ou RDP/VNC chiffré.

Côté serveur : `X11Forwarding no` par défaut dans le durcissement (section 46),
`yes` uniquement sur les serveurs qui en ont besoin.

## 41. `sshd_config` : structure et rechargement

Fichier : `/etc/ssh/sshd_config` (+ `/etc/ssh/sshd_config.d/*.conf` sur les
versions récentes — **préfère ce répertoire** pour tes réglages, le fichier
principal restant celui du paquet).

Règles inverses du client : **la première valeur obtenue gagne aussi**, mais
les blocs `Match` doivent être **en fin de fichier** (tout ce qui suit un
`Match` en fait partie jusqu'au prochain `Match`).

```bash
serveur# sshd -T    # affiche la config EFFECTIVE (après fusion) — outil n°1
serveur# sshd -t    # teste la syntaxe avant de recharger (obligatoire !)
serveur# systemctl reload ssh     # recharge sans couper les sessions existantes
```

**Procédure de modification sans risque** (à afficher dans ton équipe) :

1. `cp /etc/ssh/sshd_config.d/hardening.conf{,.bak-$(date +%F)}`
2. Éditer.
3. `sshd -t` → doit être silencieux (silence = OK).
4. `systemctl reload ssh`.
5. **Tester depuis une 2e session déjà ouverte** avant de fermer la 1re.
   (Garde toujours une session de secours ouverte quand tu touches au SSH.)

## 42. Durcissement : authentification

Le cœur du durcissement. Fichier `/etc/ssh/sshd_config.d/10-auth.conf` :

```
# --- Authentification : clés uniquement ---
PermitRootLogin no
# Variante : autoriser root UNIQUEMENT par clé (transition, à éviter à terme)
# PermitRootLogin prohibit-password
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
PubkeyAuthentication yes
```

Détails :

- `PermitRootLogin no` : root ne se connecte plus directement. Passe par un
  utilisateur normal + `sudo` (traçabilité : on sait **qui** est devenu root).
  `prohibit-password` n'autorise root que par clé : compromis acceptable en
  transition, mais `no` reste la cible.
- `PasswordAuthentication no` : coupe les attaques par dictionnaire (99 % du
  bruit Internet). **Vérifie que tes clés fonctionnent avant** (sinon tu te
  enfermes dehors — d'où la session de secours, section 41).
- `UsePAM yes` : garde PAM pour la gestion de session/motd même sans auth par
  mot de passe. Avec `ChallengeResponseAuthentication no`, PAM ne proposera pas
  de 2FA par défaut (section 49 pour l'activer proprement).
- `PermitEmptyPasswords no` (défaut) : à vérifier explicitement.

Vérification :

```bash
serveur# sshd -T | grep -Ei 'permitrootlogin|passwordauthentication|pubkeyauthentication'
permitrootlogin no
passwordauthentication no
pubkeyauthentication yes
```

## 43. Durcissement : `AllowUsers` / `AllowGroups`

Liste blanche : seuls ces utilisateurs/groupes peuvent se connecter. Tout le
reste est rejeté **avant** même l'authentification.

```
# /etc/ssh/sshd_config.d/20-access.conf
AllowGroups ssh-users admins
# ou, plus fin :
# AllowUsers zelef deploy@10.0.0.0/8
DenyUsers backup-test
```

- `AllowUsers` accepte le format `user@hôte-ou-motif` : `deploy@10.0.5.*`
  restreint ce compte au réseau de déploiement.
- Crée un groupe `ssh-users` : l'ajout/retrait d'accès devient `usermod -aG`,
  traçable et réversible, au lieu d'éditer la config SSH.
- Ordre d'évaluation : `DenyUsers`, `AllowUsers`, `DenyGroups`, `AllowGroups`.
  Si `AllowUsers`/`AllowGroups` est présent, seuls les listés passent.

## 44. Durcissement : `MaxAuthTries`, `MaxSessions`, `LoginGraceTime`

```
# /etc/ssh/sshd_config.d/30-limits.conf
MaxAuthTries 3            # essais d'authentification par connexion (défaut 6)
MaxSessions 4             # sessions multiplexées max par connexion réseau
LoginGraceTime 30         # 30 s pour s'authentifier, sinon déconnexion
MaxStartups 10:30:60      # anti-flood : voir ci-dessous
ClientAliveInterval 300   # détecte les sessions mortes côté serveur
ClientAliveCountMax 2
```

- `MaxStartups 10:30:60` : à partir de 10 connexions non authentifiées
  simultanées, 30 % de rejet, jusqu'à 100 % à 60. Freine les floods sans
  bloquer un usage légitime (Ansible ouvre beaucoup de connexions : ajuste si
  besoin, ex. `20:30:100`).
- `ClientAliveInterval`/`ClientAliveCountMax` : le serveur coupe les sessions
  mortes (complément de `ServerAliveInterval` côté client, section 13).
- Ces limites réduisent l'impact des scans mais **ne remplacent pas fail2ban**
  (section 48).

## 45. Algorithmes modernes : Ciphers, MACs, KexAlgorithms

```
# /etc/ssh/sshd_config.d/40-crypto.conf
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,diffie-hellman-group16-sha512
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com
HostKeyAlgorithms ssh-ed25519,rsa-sha2-512,rsa-sha2-256
CASignatureAlgorithms ssh-ed25519,rsa-sha2-512,rsa-sha2-256
```

- `-etm` (encrypt-then-mac) : les MAC modernes, à préférer aux `-etm`-less.
- Ce qui est **exclu** ici (et c'est voulu) : `3des`, `blowfish`, `arcfour`,
  `hmac-md5`, `hmac-sha1`, `diffie-hellman-group1-sha1`, `ssh-dss`.
- **Compatibilité** : des clients très anciens (vieux équipements réseau) ne
  supporteront pas cette liste. Plutôt que d'affaiblir le serveur global,
  utilise un `Match` ciblé (section 14 côté serveur) ou mets ces équipements
  derrière un bastion avec une config dédiée.
- Après changement : `sshd -t && systemctl reload ssh`, puis teste depuis tes
  clients habituels (`ssh -v` montre les algorithmes négociés, ligne
  `debug1: kex: ...`).

## 46. Désactiver ce qui ne sert pas

```
# /etc/ssh/sshd_config.d/50-features.conf
X11Forwarding no
AllowTcpForwarding no        # coupe -L/-R/-D ... à assouplir par Match si besoin
PermitTunnel no              # pas de VPN layer3 sur SSH
# Si les tunnels sont nécessaires pour certains utilisateurs seulement :
# Match Group tunnel-users
#     AllowTcpForwarding local
```

Stratégie : **tout fermé par défaut**, ouverture au cas par cas via `Match`
(en fin de fichier, rappel section 41) :

```
Match Group dev-tunnel
    AllowTcpForwarding local
    PermitOpen db-interne:5432
```

`PermitOpen` restreint les destinations des tunnels locaux : même avec un
tunnel autorisé, l'utilisateur ne peut viser que la base de données prévue.
Défense en profondeur concrète.

## 47. Bannières et informations divulguées

```
# /etc/ssh/sshd_config.d/60-banner.conf
Banner /etc/ssh/banner.txt
DebianBanner no              # ne pas annoncer "Debian-*" dans la version
VersionAddendum none         # OpenSSH >= 9.8 : masque la version patch
PrintMotd no                 # le motd via PAM suffit
PrintLastLog yes             # utile : "Last login: ..." alerte en cas d'accès anormal
```

Contenu type de `/etc/ssh/banner.txt` :

```
********************************************************************
* Accès réservé aux personnes autorisées. Toute activité est       *
* journalisée. Déconnectez-vous immédiatement si vous n'êtes pas   *
* autorisé à accéder à ce système.                                 *
********************************************************************
```

Pourquoi : la bannière légale aide en cas d'incident (prouve l'avertissement),
et masquer la version exacte complique le ciblage automatisé des CVE. Ce
n'est pas une protection forte, c'est de l'hygiène.

## 48. fail2ban pour SSH

fail2ban bannit temporairement les IP qui accumulent des échecs (complément
indispensable même avec `PasswordAuthentication no` : ça coupe le bruit et les
tentatives sur les clés).

```bash
serveur# apt install -y fail2ban
```

`/etc/fail2ban/jail.d/sshd.local` :

```ini
[sshd]
enabled = true
port = 22
maxretry = 5
findtime = 10m
bantime = 1h
# Recidive : les récidivistes sont bannis plus longtemps
[recidive]
enabled = true
```

```bash
serveur# systemctl enable --now fail2ban
serveur# fail2ban-client status sshd
serveur# fail2ban-client set sshd unbanip 203.0.113.7   # débannir (toi, un jour)
```

Bonnes pratiques :

- Mets ton IP de bureau / ton bastion en `ignoreip` (dans `jail.local`) pour
  ne jamais te bannir toi-même.
- `bantime` progressif : 1h puis `recidive` 1 semaine pour les robots
  persistants.
- Sur un parc, centralise : fail2ban + journald distant, ou un WAF/règles
  firewall partagées. Un attaquant banni sur web1 doit l'être partout.
- Alternative/complément : `sshd` + nftables `recent`, ou port-knocking pour
  les serveurs très exposés.

## 49. 2FA par TOTP

Deuxième facteur via PAM (`libpam-google-authenticator`), compatible avec les
applis TOTP (FreeOTP, Aegis — préfère-les à Google Authenticator).

```bash
serveur# apt install -y libpam-google-authenticator
serveur$ google-authenticator   # en tant que l'utilisateur concerné
# -> flasher le QR code, noter les codes de secours (sur papier, au coffre)
```

`/etc/pam.d/sshd` : ajouter (ou décommenter selon la distro) :

```
auth required pam_google_authenticator.so nullok
```

`nullok` = les utilisateurs sans TOTP configuré passent quand même (phase de
transition). **Retire `nullok`** quand tout le monde est enrôlé.

`/etc/ssh/sshd_config.d/70-2fa.conf` :

```
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
```

Ici : clé **ET** TOTP obligatoires (la virgule = « et »). Sans la ligne
`AuthenticationMethods`, le TOTP serait proposé en *alternative* à la clé,
pas en *complément* — nuance critique (section 50).

## 50. Clé + 2FA combinés (`AuthenticationMethods`)

Syntaxe : la virgule = ET, l'espace = OU.

```
# Clé ET (mot de passe OU TOTP) — souple
AuthenticationMethods publickey,keyboard-interactive password,keyboard-interactive

# Clé ET TOTP — strict (recommandé pour l'admin)
AuthenticationMethods publickey,keyboard-interactive

# Par population, avec Match (en fin de fichier !)
Match Group admins
    AuthenticationMethods publickey,keyboard-interactive
Match Group deploy
    AuthenticationMethods publickey
```

Points d'attention :

- Teste **toujours** depuis une session de secours (section 41) : une
  `AuthenticationMethods` mal écrite = porte claquée.
- `keyboard-interactive` couvre le TOTP via PAM **et** les questions PAM
  classiques : vérifie qu'aucun module PAM n'accepte un simple mot de passe
  en fallback (relis `/etc/pam.d/sshd` en entier).
- Ansible et scripts non interactifs **ne passent pas** le TOTP : prévois un
  groupe/une clé de service en `publickey` seul, restreinte (`command=`,
  `from=`, sections 23-24), ou des certificats à courte durée de vie
  (section 51).

## 51. Certificats SSH : tour d'horizon

Les certificats SSH remplacent la gestion manuelle d'`authorized_keys` : une
**autorité de certification (CA)** interne signe les clés publiques des
utilisateurs (et des hôtes). Le serveur fait confiance à la CA, pas à chaque
clé individuelle.

Avantages décisifs en équipe / parc :

- **Zéro `authorized_keys` à déployer** : ajouter un arrivant = signer sa clé,
  pas toucher 200 serveurs.
- **Expiration intégrée** : `-V -1w:+52w` (valide 1 semaine avant → 52 après,
  typiquement courte : quelques heures/jours pour les accès sensibles).
- **Principals** : le certificat dit *qui* tu es (`zelef`, `deploy`) ; le
  serveur mappe les principals aux comptes autorisés.
- **Révocation** centralisée via `RevokedKeys` (section 53).
- Fini le TOFU : les clés d'hôte sont elles aussi signées par la CA.

Inconvénients : il faut opérer une CA (sécuriser sa clé privée **comme un
secret critique**, idéalement HSM ou machine dédiée offline), et outiller la
signature (petit service interne, HashiCorp Vault SSH, ou script signé).

Quand l'adopter : à partir d'une dizaine de serveurs et/ou d'arrivées-départs
fréquents. En dessous, `authorized_keys` déployé par Ansible reste très bien.

## 52. Certificats : créer une CA et signer des clés

**Sur la machine CA (dédiée, chiffrée, sauvegardée)** :

```bash
ca$ ssh-keygen -t ed25519 -f /etc/ssh/ca_user -C "CA utilisateurs $(date +%Y)"
ca$ ssh-keygen -t ed25519 -f /etc/ssh/ca_host -C "CA hotes $(date +%Y)"
# Protéger : chmod 600, passphrase forte, sauvegarde chiffrée hors site
```

**Signer la clé d'un utilisateur** (l'utilisateur envoie sa `.pub`, jamais sa
privée) :

```bash
ca$ ssh-keygen -s /etc/ssh/ca_user -I "zelef-20260926" -n zelef,deploy \
    -V -5m:+8h -z 42 /tmp/id_ed25519_zelef.pub
# -I : identifiant (audit), -n : principals, -V : validité, -z : n° série
# Produit : /tmp/id_ed25519_zelef-cert.pub -> à renvoyer à l'utilisateur
```

Côté client, le certificat se place à côté de la clé et est utilisé
automatiquement :

```bash
client$ ls ~/.ssh/
id_ed25519  id_ed25519-cert.pub  id_ed25519.pub
client$ ssh -i ~/.ssh/id_ed25519 web1   # le -cert.pub est trouvé seul
client$ ssh-keygen -L -f ~/.ssh/id_ed25519-cert.pub   # inspecter le certificat
```

**Signer une clé d'hôte** (fini le TOFU) :

```bash
ca$ ssh-keygen -s /etc/ssh/ca_host -I "web1-20260926" -h -n web1.example.com \
    -V -1w:+52w /tmp/ssh_host_ed25519_key.pub
# -h : certificat d'HÔTE, -n : noms d'hôte couverts
```

## 53. Certificats côté serveur : confiance et révocation

`/etc/ssh/sshd_config.d/80-certs.conf` :

```
# Faire confiance à la CA pour les utilisateurs...
TrustedUserCAKeys /etc/ssh/ca_user.pub
# ...et pour les hôtes (les clients vérifient via KnownHostsCommand ou config)
TrustedCAKeys /etc/ssh/ca_host.pub
HostCertificate /etc/ssh/ssh_host_ed25519_key-cert.pub
# Révocation centralisée
RevokedKeys /etc/ssh/revoked_keys
```

Mapper les principals aux comptes locaux si besoin (`AuthorizedPrincipalsFile`) :

```
AuthorizedPrincipalsFile /etc/ssh/auth_principals/%u
# /etc/ssh/auth_principals/zelef contient : zelef
# /etc/ssh/auth_principals/deploy contient : deploy ci-deploys
```

**Révocation** (départ d'un salarié, clé compromise) :

```bash
serveur# ssh-keygen -k -f /etc/ssh/revoked_keys /tmp/cert-a-revoquer-cert.pub
serveur# systemctl reload ssh
```

Exploitation au quotidien : un petit script/CI signe les clés (`-V :+8h`),
les utilisateurs récupèrent leur certificat le matin (ou via Vault), aucun
`authorized_keys` à maintenir. Le départ d'un collaborateur = ne plus signer
+ ajouter le dernier certificat à `revoked_keys`. C'est le niveau « équipe
structurée » de la gestion SSH.

---

# PARTIE SERVEUR — EXPLOITATION

## 54. Supervision : les logs d'authentification

Sur Debian/Ubuntu : `/var/log/auth.log`. Sur RHEL : `/var/log/secure`.
Via systemd partout : `journalctl`.

```bash
serveur# tail -f /var/log/auth.log
serveur# journalctl -u ssh -f
# Connexions réussies :
serveur# grep "Accepted" /var/log/auth.log | tail
# Échecs :
serveur# grep "Failed" /var/log/auth.log | tail
# Déconnectés / timeouts :
serveur# grep -E "Disconnected|timeout" /var/log/auth.log | tail
```

Avec `LogLevel VERBOSE` (recommandé en production), chaque connexion réussie
journalise **l'empreinte de la clé utilisée** :

```
Accepted publickey for zelef from 192.0.2.25 port 51234 ssh2:
ED25519 SHA256:AbCdEfGhIjKlMnOpQrStUvWxYz0123456789ABc
```

C'est précieux : en cas d'incident, tu sais **quelle clé** a été utilisée, pas
seulement quel compte. Conserve ces logs (forward vers un SIEM/syslog
central : une machine compromise ne doit pas pouvoir effacer ses traces
locales).

Requêtes utiles :

```bash
# Top des IP en échec sur 24h
serveur# grep "Failed password" /var/log/auth.log | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -c | sort -rn | head
# Connexions root tentées (doivent être à zéro si PermitRootLogin no)
serveur# grep "Failed.*root" /var/log/auth.log | wc -l
# Tunnels ouverts (avec LogLevel VERBOSE)
serveur# grep -i "forward" /var/log/auth.log | tail
```

## 55. Audit des connexions : `last`, `who`, `ss`

```bash
serveur# last -n 20            # historique des connexions (wtmp)
serveur# lastb -n 20           # tentatives ÉCHOUÉES (btmp) — à surveiller
serveur# who / w               # qui est connecté MAINTENANT
serveur# ss -tnp | grep :22    # connexions TCP actuelles sur SSH
serveur# ps aux | grep sshd    # processus sshd par session
```

Lecture d'une ligne `sshd` : `sshd: zelef@pts/0` = session interactive de
zelef ; `sshd: zelef [net]` = phase réseau (pas encore authentifié) ; trop de
`[net]` persistants = scan ou flood en cours (croiser avec `MaxStartups`,
section 44, et fail2ban, section 48).

Rituel hebdo (5 minutes) sur les serveurs exposés :

1. `lastb | head` : des IP inconnues en masse ?
2. `grep Accepted /var/log/auth.log` : des comptes/heures inhabituels ?
3. `fail2ban-client status sshd` : le bannissement tourne-t-il ?
4. Clés `authorized_keys` : cf. inventaire section 59 (mensuel).

## 56. Alertes sur connexions suspectes

Exemples de signaux à alerter (SIEM, ou simple script + mail) :

- Connexion réussie **root** alors que `PermitRootLogin no` (config dérivées ?).
- Connexion depuis un pays/ASN inhabituel pour l'équipe.
- Connexion à 3h du matin un dimanche (selon tes horaires).
- `Accepted` pour un compte de service normalement non interactif.
- Rafale de `Failed password` depuis une IP interne (mouvement latéral ?).

Exemple minimaliste (cron toutes les 5 min, à adapter) :

```bash
#!/bin/bash
# /usr/local/bin/ssh-alert.sh — alerte sur connexion root réussie
if journalctl -u ssh --since "5 minutes ago" | grep -q "Accepted.*for root"; then
  echo "ALERTE: connexion root SSH détectée sur $(hostname) à $(date)" \
    | mail -s "[SEC] SSH root $(hostname)" astreinte@example.com
fi
```

Mieux : forwarder `auth.log` vers ta stack (rsyslog → Loki/ELK/Wazuh) et écrire
les règles là-bas. Le script ci-dessus dépanne en attendant.

## 57. Sauvegarder ses clés (critique)

Tes clés privées **sont** ton identité d'admin. Les perdre = perdre l'accès ;
les exposer = compromettre le parc. Les deux se traitent par la sauvegarde
**chiffrée**.

Stratégie recommandée :

1. **Chiffrement** : archive chiffrée (pas de clé en clair sur un disque
   externe, un NAS ou un cloud).

```bash
client$ tar -czf - ~/.ssh | gpg --symmetric --cipher-algo AES256 -o ssh-backup-2026-09-26.tar.gz.gpg
# Restauration :
client$ gpg -d ssh-backup-2026-09-26.tar.gz.gpg | tar -xzf - -C /tmp/restauration
```

2. **3-2-1** : 3 copies, 2 supports différents, 1 hors site (coffre, cloud
   chiffré). La passphrase GPG **sur papier au coffre**, pas dans le même
   cloud.
3. **Quoi sauvegarder** : `~/.ssh/` entier (privées + publiques + config +
   known_hosts), la passphrase dans un gestionnaire de mots de passe
   d'équipe (Bitwarden/Vaultwarden), et **la liste des serveurs où chaque clé
   est déployée** (sinon la restauration est aveugle).
4. **Clés de CA** (section 52) : sauvegarde **offline** dédiée (clé USB
   chiffrée au coffre), jamais sur le réseau.

Ce qu'il ne faut **jamais** faire :

- ❌ Commiter une clé privée dans Git (même « temporairement », même en privé).
- ❌ L'envoyer par mail, Slack, ticket.
- ❌ La laisser dans `/tmp`, un partage réseau, une image Docker/VM.
- ❌ La stocker en clair dans un cloud « parce que c'est pratique ».

Test de restauration **une fois par an** : restaure sur une machine jetable et
vérifie qu'une connexion aboutit. Une sauvegarde non testée n'est pas une
sauvegarde.

## 58. Rotation des clés

Politique simple et tenable :

| Événement | Action |
|---|---|
| Départ d'un collaborateur | Supprimer ses clés de tous les `authorized_keys` **le jour J** (+ révoquer certificats) |
| Compromission suspectée (poste volé, malware) | Nouvelle clé **immédiatement**, ancienne révoquée partout |
| Rotation préventive | Tous les 12-24 mois pour les clés humaines, 6-12 mois pour les clés de service |
| Changement de périmètre | Nouvelle clé dédiée plutôt que réutiliser l'ancienne |

Procédure de rotation sans coupure :

1. Générer la nouvelle clé (nouveau commentaire daté).
2. La déployer **en plus** de l'ancienne (Ansible/`ssh-copy-id`).
3. Vérifier la connexion avec la nouvelle (`ssh -i nouvelle`).
4. Retirer l'ancienne de tous les `authorized_keys`.
5. Archiver l'ancienne clé (au cas où un serveur aurait été oublié, 30 jours),
   puis la détruire (`shred -u`).

Avec les certificats (section 51), la rotation devient triviale : courte durée
de vie (`-V :+8h`) = rotation **automatique** quotidienne.

## 59. Inventaire des clés autorisées sur le parc

Le risque silencieux : des `authorized_keys` qui accumulent les clés
d'anciens collègues, de prestataires, de tests oubliés. Audit trimestriel :

```bash
# Sur chaque serveur : lister toutes les clés autorisées avec leur commentaire
serveur# for f in /home/*/.ssh/authorized_keys /root/.ssh/authorized_keys; do
  [ -f "$f" ] && echo "== $f" && ssh-keygen -l -f "$f"
done
```

Version Ansible (à mettre dans ton dépôt d'exploitation) :

```yaml
- name: Inventaire des authorized_keys
  hosts: all
  tasks:
    - name: Lister les clés autorisées
      ansible.builtin.shell: |
        for f in /home/*/.ssh/authorized_keys /root/.ssh/authorized_keys; do
          [ -f "$f" ] && echo "== $f" && ssh-keygen -l -f "$f"
        done
      register: cles
      changed_when: false
    - ansible.builtin.debug: var=cles.stdout_lines
```

Règles d'hygiène :

- Chaque ligne doit avoir un **commentaire identifiable** (`user@machine-date`).
  Ligne sans commentaire = à clarifier ou à supprimer.
- Zéro clé d'ancien salarié, zéro clé « test », zéro doublon.
- Croise avec les logs `Accepted ... ED25519 SHA256:...` (section 54) : une clé
  autorisée mais **jamais utilisée depuis 6 mois** est une candidate à la
  suppression.

## 60. Mettre à jour OpenSSH

OpenSSH évolue vite (nouvelles KEX, retraits d'algorithmes faibles, CVE). Reste
sur les versions de ta distribution **à jour** plutôt que de compiler à la main
(sauf besoin précis).

**Debian / Ubuntu :**

```bash
serveur# apt update && apt install --only-upgrade openssh-server openssh-client
serveur# sshd -t && systemctl reload ssh
serveur# ssh -V   # vérifier
```

**Après une mise à jour majeure**, vérifie :

1. `sshd -T` : des options **dépréciées** ? (ex. `ChallengeResponseAuthentication`
   renommé `KbdInteractiveAuthentication` dans les versions récentes —
   l'ancien nom reste accepté un temps, mais migre.)
2. Les algorithmes par défaut : une montée de version peut **désactiver**
   d'anciens algos (ex. RSA-SHA1) et couper de vieux clients/équipements.
   Teste tes cas limites (équipements réseau anciens, scripts).
3. `man sshd_config` de la version installée : la doc embarquée fait foi, pas
   un tuto de 2019.

**Politique** : applique les mises à jour de sécurité sous 48h sur les serveurs
exposés (unattended-upgrades bien configuré, cf. ton guide Debian/Ubuntu),
fenêtre planifiée pour les montées de version majeures, avec test de
non-régression SSH (une connexion clé + un `rsync` + un tunnel si tu en as).

---

# DÉPANNAGE

## 61. Dépannage : la méthode (`-v`, `-vvv`)

Devant une panne SSH, toujours dans cet ordre :

1. **Reproduire avec verbosité** : `ssh -v cible` (souvent suffisant),
   `ssh -vvv cible` pour les cas tordus. Lis **les 20 dernières lignes** :
   la cause y est presque toujours (`Offering public key`, `Authentications
   that can continue`, `No more authentication methods to try`).
2. **Vérifier la config effective** : `ssh -G cible` (section 9). 50 % des
   « pannes » sont une option du `~/.ssh/config` qu'on avait oubliée.
3. **Regarder côté serveur** en parallèle : `journalctl -u ssh -f` ou
   `/var/log/auth.log`. Le serveur dit souvent exactement pourquoi il refuse
   (`Authentication refused: bad ownership or modes`, `User zelef not allowed
   because not listed in AllowUsers`).
4. **Isoler** : tester sans la config (`ssh -F /dev/null`), sans l'agent
   (`ssh -o IdentitiesOnly=yes -i clé`), depuis un autre réseau.

```bash
client$ ssh -v web1 2>&1 | tail -30
client$ ssh -F /dev/null -i ~/.ssh/id_ed25519 zelef@192.0.2.10
```

Et surtout : **garde une session ouverte** quand tu modifies `sshd_config`
(section 41). Le dépannage commence par ne pas s'enfermer dehors.

## 62. Cas 1 : `Permission denied (publickey)`

Le classique. Message complet typique :

```
zelef@web1: Permission denied (publickey).
```

Diagnostic avec `ssh -v` : repère ces lignes :

```
debug1: Offering public key: /home/zelef/.ssh/id_ed25519 ED25519 SHA256:...
debug1: Authentications that can continue: publickey
debug1: No more authentication methods to try.
```

Le serveur a **refusé toutes les clés proposées** (ou aucune n'a été proposée).
Checklist, dans l'ordre :

1. **La bonne clé est-elle proposée ?** `ssh -v` → `Offering public key:`.
   Si ta clé n'apparaît pas : `IdentityFile` manquant, `IdentitiesOnly yes`
   sans `IdentityFile`, ou agent vide (`ssh-add -l`).
2. **La clé publique est-elle déployée ?** Compare l'empreinte locale
   (`ssh-keygen -l -f ~/.ssh/id_ed25519.pub`) avec `authorized_keys` distant
   (via un autre accès).
3. **Permissions côté serveur** : `~/.ssh` en `700`, `authorized_keys` en
   `600`, **home en 755 max** (un home en 777 fait refuser la clé avec
   `StrictModes yes`). Le log serveur dit `bad ownership or modes`.
4. **La clé est-elle restreinte ?** Options `from=` (tu n'es pas sur le bon
   réseau), `command=` (normal si c'est une clé de service).
5. **Serveur** : `PubkeyAuthentication yes` ? L'utilisateur est-il dans
   `AllowUsers`/`AllowGroups` ? Le compte est-il verrouillé (`passwd -S`) ?
6. **Trop de clés proposées** avant la bonne → voir cas 6 (section 67).

Réparation express des permissions :

```bash
serveur$ chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys && chmod 755 ~
```

## 63. Cas 2 : `Host key verification failed`

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
```

Significations possibles, par probabilité :

1. **Le serveur a été réinstallé** (clés d'hôte régénérées) — cas le plus
   fréquent en labo/homelab.
2. **Tu tombes sur une autre machine** (IP réattribuée en DHCP, NAT qui a
   changé, DNS menteur).
3. **Attaque homme-du-milieu** — rare, mais c'est exactement ce que ce message
   est censé détecter : ne le contourne pas à l'aveugle sur un réseau hostile.

Conduite à tenir :

```bash
# 1. Vérifier la cause (réinstall ? nouvelle IP ?)
# 2. Comparer l'empreinte proposée avec l'inventaire (section 8)
# 3. Si légitime, purger l'ancienne entrée puis reconnecter :
client$ ssh-keygen -R web1
client$ ssh-keygen -R 192.0.2.10
client$ ssh web1   # vérifier l'empreinte affichée avant "yes"
```

**Ne fais jamais** : `StrictHostKeyChecking=no` en global, ni supprimer tout
`known_hosts` « pour aller vite ». En automatisation, utilise
`StrictHostKeyChecking=accept-new` (section 7), jamais `no`.

## 64. Cas 3 : la connexion freeze / timeout

Symptômes : `ssh web1` reste bloqué, ou la session se fige après quelques
minutes d'inactivité.

**À la connexion** :

```bash
client$ ssh -v web1
# Bloqué sur "Connecting to 192.0.2.10 [192.0.2.10] port 22." -> réseau/firewall
```

- `telnet 192.0.2.10 22` / `nc -vz 192.0.2.10 22` : si ça ne répond pas, le
  problème est **réseau** (firewall, routage, sshd arrêté), pas SSH.
- Bloqué après `SSH-2.0-OpenSSH...` : filtrage applicatif ou MTU (rare).
- Avec `-J` : c'est le **bastion** qui est injoignable, pas la cible.

**Session qui freeze en inactivité** : un NAT/firewall coupe les connexions
TCP idle. Remède client (section 13) :

```
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

Et côté serveur : `ClientAliveInterval 300` (section 44). Pour les tunnels
persistants : `autossh` (section 35).

**Freeze sur gros transfert** : suspecte le MTU (VPN/PPPoE) → teste avec des
petits paquets, ou `ping -M do -s 1472`.

## 65. Cas 4 : l'agent ne transmet pas la clé (forwarding)

Symptôme : `ssh -A bastion` puis, sur le bastion, `ssh cible` demande un mot
de passe ou échoue alors que ça marche en direct depuis ton poste.

Diagnostic :

```bash
# Sur TON poste :
client$ ssh-add -l          # l'agent a-t-il la clé ? Sinon : ssh-add
client$ echo $SSH_AUTH_SOCK # le socket est-il défini ?
# Sur le BASTION :
bastion$ echo $SSH_AUTH_SOCK # vide = pas de forwarding arrivé
bastion$ ssh-add -l          # doit lister TES clés (via le socket transmis)
```

Causes fréquentes :

1. `ForwardAgent yes` absent (et pas de `-A`). Vérifie `ssh -G bastion | grep forwardagent`.
2. **Le serveur l'interdit** : `AllowAgentForwarding no` dans `sshd_config`.
3. `sudo -s` sur le bastion : `sudo` **ne conserve pas** `SSH_AUTH_SOCK` par
   défaut → l'agent « disparaît » en root. (Et c'est tant mieux : voir les
   dangers section 20.)
4. Multiplexage : une connexion maîtresse ouverte **sans** `-A` est réutilisée
   par les suivantes → le forwarding ne s'active pas. `ssh -O stop bastion`
   puis reconnecte avec `-A`.

Rappel : la bonne réponse est souvent **ne pas utiliser le forwarding** mais
`ProxyJump` (section 26).

## 66. Cas 5 : connexion lente à s'établir (DNS, GSSAPI)

Symptôme : 10 à 30 secondes entre `ssh` et le prompt mot de passe/shell, alors
que le réseau est sain.

Coupables usuels, dans l'ordre :

1. **Résolution DNS inverse côté serveur** : avec `UseDNS yes` (défaut
   historique), sshd résout l'IP du client avant d'accepter. Si le DNS est
   lent/absent → attente. Remède :

```
# /etc/ssh/sshd_config.d/30-limits.conf
UseDNS no
```

   (Sauf si tu utilises `from=` avec des noms DNS ou `AllowUsers user@host` —
   dans ce cas, soigne ton DNS plutôt que de couper.)

2. **GSSAPI côté client** : le client tente une auth Kerberos qui timeout.
   Remède client :

```
Host *
    GSSAPIAuthentication no
```

3. **IPv6 qui timeout** avant fallback IPv4 : `AddressFamily inet` côté client
   pour forcer IPv4 (diagnostic, pas une solution durable).

Mesure : `time ssh -o GSSAPIAuthentication=no web1 'exit'` avant/après pour
objectiver le gain.

## 67. Cas 6 : `Too many authentication failures`

```
Received disconnect from 192.0.2.10 port 22:2: Too many authentication failures
```

Le client a proposé **plus de clés** que `MaxAuthTries` (défaut 6) n'en
autorise : typique quand l'agent contient 10 clés et que la bonne est la 8e.
Le serveur coupe avant même d'essayer la bonne.

Remèdes :

```bash
# Ponctuel : forcer LA clé
client$ ssh -o IdentitiesOnly=yes -i ~/.ssh/id_ed25519_web web1
```

```
# Durable : dans ~/.ssh/config
Host web1
    IdentityFile ~/.ssh/id_ed25519_web
    IdentitiesOnly yes
```

- `IdentitiesOnly yes` limite aux `IdentityFile` déclarés (+ ceux de la ligne
  de commande), sans piocher dans l'agent.
- Alternative : vider l'agent des clés inutiles (`ssh-add -d`), ou augmenter
  `MaxAuthTries` côté serveur (mauvaise idée : ça aide aussi les attaquants).

## 68. Cas 7 : `WARNING: UNPROTECTED PRIVATE KEY FILE`

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/zelef/.ssh/id_ed25519' are too open.
```

La clé privée est lisible par d'autres → SSH **refuse de l'utiliser**.
Souvent après une copie via clé USB, un `umask` exotique, ou une restauration
de sauvegarde.

```bash
client$ chmod 600 ~/.ssh/id_ed25519
client$ chmod 700 ~/.ssh
client$ chmod 644 ~/.ssh/id_ed25519.pub ~/.ssh/known_hosts
client$ chmod 600 ~/.ssh/config
```

Règle mémo : `700` le répertoire, `600` les privés et la config, `644` les
publiques et `known_hosts`. Si le fichier vient d'un support non fiable
(USB trouvée, pièce jointe), **considère la clé comme compromise** : régénère
(section 58), ne te contente pas de `chmod`.

## 69. Cas 8 : `ssh-copy-id` échoue

```
ERROR: failed to open ID file '/home/zelef/.ssh/id_ed25519.pub': No such file
```

- La **publique** n'existe pas : régénère-la depuis la privée
  (`ssh-keygen -y -f ~/.ssh/id_ed25519 > ~/.ssh/id_ed25519.pub`), ou pointe
  `-i` vers la bonne.
- `Permission denied` pendant le `ssh-copy-id` : normal si le serveur
  n'accepte que les clés et que tu n'en as pas encore déployé — c'est
  l'œuf et la poule. Solutions : mot de passe temporaire (puis le couper),
  console (iDRAC/IPMI), ou déploiement par Ansible/cloud-init.
- Le serveur refuse l'écriture : home en lecture seule, quota, ou
  `AuthorizedKeysFile` pointant ailleurs (`sshd -T | grep authorizedkeysfile`).
- Doublons : `ssh-copy-id` ne déduplique pas ; relancé 3 fois = 3 lignes
  identiques. Nettoie avec `sort -u` (en vérifiant avant d'écraser).

## 70. Cas 9 : le tunnel ne répond pas / port déjà utilisé

```
bind [127.0.0.1]:8080: Address already in use
channel_setup_fwd_listener_tcpip: cannot listen to port: 8080
```

1. **Port déjà pris** : un autre tunnel, ou l'appli elle-même. `ss -tlnp |
   grep 8080` pour identifier le squatteur.
2. **Tunnel « démarré » mais mort** : sans `ExitOnForwardFailure yes`, SSH
   continue sans le tunnel si le bind échoue → tu crois que ça marche. Ajoute :

```
Host tunnel-*
    ExitOnForwardFailure yes
```

3. **La destination vue par le serveur est fausse** : dans
   `-L 8080:localhost:9090`, `localhost` est résolu **par le serveur**. Si le
   service écoute sur une autre interface du serveur, utilise son IP ou
   `127.0.0.1` explicite.
4. **`AllowTcpForwarding no`** côté serveur (section 46) : le tunnel est
   refusé silencieusement ou avec `open failed: administratively prohibited`.
   Vérifie `sshd -T | grep allowtcpforwarding` et les blocs `Match`.
5. **Tunnel `-R` inaccessible** : `GatewayPorts` (section 31).

## 71. Cas 10 : multiplexage, `ControlPath` trop long

```
muxserver_listen: link mux listener: path too long
```

Les sockets Unix ont une limite de ~107 caractères. `~/.ssh/sockets/%r@%h:%p`
avec un home profond + un FQDN long la dépasse.

Solutions :

```
Host *
    # %C = hash court de (user, host, port) : compact et unique
    ControlPath ~/.ssh/sockets/%C
    # ou, radical : /tmp
    # ControlPath /tmp/ssh-%C
```

- Vérifie que `~/.ssh/sockets/` existe (`mkdir -p`).
- `%C` évite aussi les caractères spéciaux des FQDN dans le nom de fichier.
- Diagnostic : `ssh -O check cible` → `Master running` ou message d'erreur
  explicite.

## 72. Cas 11 : X11 forwarding sans `DISPLAY`

Symptôme : `ssh -X web1`, puis `xclock` → `Error: Can't open display:`.

1. **Côté client** : `echo $DISPLAY` **avant** ssh doit être non vide. En SSH
   depuis un terminal sans X (ou Wayland sans XWayland), il n'y a rien à
   forwarder.
2. **`ForwardX11 yes`** actif ? `ssh -G web1 | grep -i forwardx11`.
3. **Côté serveur** : `X11Forwarding yes` dans `sshd_config` + paquet
   `xauth` installé (`command -v xauth`). Sans `xauth`, sshd ne peut pas
   créer le cookie d'autorisation et le forwarding échoue silencieusement.
4. Après connexion, `echo $DISPLAY` doit donner quelque chose comme
   `localhost:10.0`. Si vide : relis les points 2-3, et `ssh -v` (ligne
   `Requesting X11 forwarding` / `X11 forwarding request failed`).

Rappel section 40 : `-X` plutôt que `-Y`, et jamais en `Host *`.

## 73. Cas 12 : SFTP en chroot qui échoue

Config type qui échoue mystérieusement :

```
Match Group sftp-only
    ChrootDirectory /srv/sftp/%u
    ForceCommand internal-sftp
```

Checklist du chroot SFTP :

1. **Propriété** : chaque composant du chemin (`/srv`, `/srv/sftp`, `/srv/sftp/alice`)
   doit appartenir à **root:root** et ne pas être accessible en écriture au
   groupe/autres. Sinon : `fatal: bad ownership or modes for chroot directory`.
   Le répertoire **inscriptible** par l'utilisateur est un **sous-répertoire**
   (ex. `/srv/sftp/alice/upload` en `alice:sftp-only`).
2. `Subsystem sftp internal-sftp` doit exister dans `sshd_config` (le binaire
   externe ne marche pas en chroot sans les libs).
3. L'utilisateur ne doit pas avoir de shell valide nécessaire : avec
   `ForceCommand internal-sftp`, un shell `/bin/false` ou `/usr/sbin/nologin`
   est OK et même recommandé.
4. Tester : `sftp alice@serveur` puis `journalctl -u ssh -f` en parallèle —
   le message `fatal:` donne le composant fautif exact.

```bash
serveur# mkdir -p /srv/sftp/alice/upload
serveur# chown root:root /srv/sftp /srv/sftp/alice
serveur# chmod 755 /srv/sftp /srv/sftp/alice
serveur# chown alice:sftp-only /srv/sftp/alice/upload
```

## 74. Les erreurs classiques (tableau récapitulatif)

| Message / symptôme | Cause la plus probable | Section |
|---|---|---|
| `Permission denied (publickey)` | Clé non déployée / permissions / `from=` | 62 |
| `Host key verification failed` | Clé d'hôte changée (réinstall, IP réattribuée) | 63 |
| Connexion bloquée sur `Connecting to...` | Firewall / routage / sshd arrêté | 64 |
| Session qui freeze en inactivité | NAT idle → `ServerAliveInterval` | 64 |
| `Too many authentication failures` | Trop de clés dans l'agent, `IdentitiesOnly` absent | 67 |
| `UNPROTECTED PRIVATE KEY FILE` | Permissions > 600 sur la clé privée | 68 |
| `Connection refused` | sshd arrêté ou n'écoute pas ce port/IP | 64 |
| `Connection timed out` | Firewall qui drop, mauvaise IP, routage | 64 |
| `No route to host` | Routage / hôte éteint / mauvais réseau | 64 |
| `open failed: administratively prohibited` | `AllowTcpForwarding no` ou `PermitOpen` | 70 |
| `Bad owner or permissions on ~/.ssh/config` | Config lisible par d'autres → `chmod 600` | 68 |
| Prompt mot de passe malgré la clé | `PasswordAuthentication` encore actif + clé refusée (lire `-v`) | 62 |
| `Agent admitted failure to sign` | Agent sans la clé ou clé corrompue → `ssh-add -D` + `ssh-add` | 18 |
| `WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED` | Voir cas 2 | 63 |
| `ssh: Could not resolve hostname` | DNS / faute de frappe dans l'alias | 10 |

---

# CAS PRATIQUES COMMENTÉS

## 75. Cas pratique 1 : bastion d'équipe

**Contexte** : 15 serveurs en 10.10.0.0/16, un bastion public
`bastion.example.com`, 4 admins. Objectif : accès simple, sans forwarding
d'agent, avec traçabilité.

**`~/.ssh/config` de chaque admin** (déployé par Ansible, `config.d/20-prod.conf`) :

```
Host bastion
    HostName bastion.example.com
    User zelef
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

# Tout le LAN passe par le bastion, sans exception
Host 10.10.*.*
    User zelef
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    ProxyJump bastion
    ServerAliveInterval 60
    ServerAliveCountMax 3

# Alias mémorisable pour les serveurs fréquents
Host web1
    HostName 10.10.1.10
Host db1
    HostName 10.10.2.10
    User dba
```

Usage : `ssh web1`, `scp dump.sql db1:/tmp/`, `rsync -e ssh` — tout passe par
le bastion **sans y laisser de clés** (ProxyJump, pas de `-A`).

**Côté bastion** (`sshd_config.d/`) : `PasswordAuthentication no`,
`AllowGroups ssh-users`, `AllowTcpForwarding yes` (nécessaire au ProxyJump),
fail2ban actif, `auth.log` forwardé au SIEM. Le bastion **ne stocke aucune clé
privée d'admin** : il ne fait que relayer.

**Traçabilité** : `LogLevel VERBOSE` → chaque `Accepted` loggue l'empreinte de
la clé → on sait qui s'est connecté où et quand, sans agent de supervision
supplémentaire.

## 76. Cas pratique 2 : clé restreinte pour déploiement (`command=`)

**Contexte** : un runner CI doit déployer l'application sur `prod1`, sans
jamais obtenir un shell.

**Sur `prod1`**, utilisateur dédié `deployer` (shell `/bin/false`, groupe
`deploy`) :

```bash
serveur# useradd -m -s /bin/false -G deploy deployer
```

`/home/deployer/.ssh/authorized_keys` (une seule ligne) :

```
from="10.0.5.0/24",command="/usr/local/bin/deploy.sh",no-pty,no-agent-forwarding,no-X11-forwarding,no-port-forwarding ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEXEMPLEdefictifNePasUtiliser ci@runner-01
```

`/usr/local/bin/deploy.sh` :

```bash
#!/bin/bash
# N'autorise que la commande de déploiement attendue, rien d'autre.
set -euo pipefail
case "${SSH_ORIGINAL_COMMAND:-}" in
  "deploy production")
    logger -t deploy "Déploiement production lancé par $USER"
    exec /opt/app/scripts/deploy-production.sh
    ;;
  *)
    echo "Commande non autorisée." >&2
    logger -t deploy "Tentative non autorisée : ${SSH_ORIGINAL_COMMAND:-<vide>}"
    exit 1
    ;;
esac
```

**Côté CI** : la clé privée est un secret du runner (jamais dans le dépôt),
sans passphrase (usage non interactif) mais **restreinte** comme ci-dessus.
Test :

```bash
ci$ ssh -i deploy_key deployer@prod1 "deploy production"   # OK
ci$ ssh -i deploy_key deployer@prod1 "rm -rf /"            # refusé (command=)
ci$ ssh -i deploy_key deployer@prod1                       # pas de shell (no-pty)
```

Si cette clé fuit : l'attaquant ne peut exécuter que le déploiement, depuis le
réseau CI uniquement. C'est le pattern à généraliser pour **tous** les comptes
de service (backup, supervision, batch).

## 77. Cas pratique 3 : accès à une UI d'administration via tunnel

**Contexte** : interface iDRAC d'un serveur (`idrac-srv1`, 10.10.1.99:443),
inaccessible depuis le LAN admin. Accès via `srv1` (10.10.1.10) qui, lui, est
joignable en SSH.

```
Host srv1
    HostName 10.10.1.10
    User zelef
    ProxyJump bastion
    # Tunnel persistant vers l'iDRAC, via srv1
    LocalForward 8443 10.10.1.99:443
    ExitOnForwardFailure yes
```

```bash
client$ ssh -N -f srv1
client$ ss -tlnp | grep 8443     # vérifier que le tunnel écoute
# https://localhost:8443 -> iDRAC (avertissement certificat : normal, c'est
# l'autosigné de l'iDRAC, vérifie l'empreinte via la console physique la 1re fois)
```

**Pourquoi via `srv1` et pas en direct** : l'iDRAC n'est pas un bastion SSH
fiable pour faire du `ProxyJump` (son serveur SSH est minimaliste) ; en
revanche `srv1` relaie très bien le TCP. Le `LocalForward` est résolu **côté
srv1** : `10.10.1.99` est l'adresse vue depuis `srv1`.

**Hygiène** : tunnel à la demande (`ssh -N -f` quand tu en as besoin, `ssh -O
stop` après), jamais de `GatewayPorts` ici (l'iDRAC reste accessible
uniquement depuis ton poste). Pour un besoin d'équipe permanent, c'est un
bastion web authentifié (ou un VPN), pas un tunnel artisanal.

## 78. Cas pratique 4 : backup distant avec rsync over SSH

**Contexte** : sauvegarder `/data` de `prod1` vers `backup1` chaque nuit, avec
une clé restreinte en lecture seule.

**Sur `prod1`**, utilisateur `backup` + clé restreinte (pattern `rrsync`) :

```bash
serveur# apt install -y rsync   # fournit /usr/bin/rrsync
```

`/home/backup/.ssh/authorized_keys` :

```
from="10.0.6.20",command="/usr/bin/rrsync -ro /data/",no-pty,no-agent-forwarding,no-X11-forwarding,no-port-forwarding ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEXEMPLEdefictifNePasUtiliser backup@backup1
```

`rrsync -ro /data/` : expose **en lecture seule** (`-ro`) uniquement `/data`.
Même avec la clé, impossible d'écrire sur `prod1` ou de lire ailleurs.

**Sur `backup1`**, script cron (`/usr/local/bin/pull-backup.sh`) :

```bash
#!/bin/bash
set -euo pipefail
SRC="backup@prod1:/data/"
DST="/srv/backups/prod1/$(date +%Y-%m-%d)/"
LOG="/var/log/pull-backup.log"
mkdir -p "$DST"
rsync -a --delete --link-dest=/srv/backups/prod1/latest \
  -e "ssh -i /root/.ssh/id_backup -o IdentitiesOnly=yes -o BatchMode=yes" \
  "$SRC" "$DST" >>"$LOG" 2>&1
ln -sfn "$DST" /srv/backups/prod1/latest
```

- `--link-dest` : déduplication par hard links (snapshots quotidiens peu
  coûteux, style rsnapshot).
- `BatchMode=yes` : le cron ne doit jamais bloquer sur un prompt.
- La clé privée est sur `backup1` uniquement, `600`, sans passphrase (compte
  de service), restreinte côté `prod1` comme ci-dessus.

## 79. Cas pratique 5 : jump host multi-sites

**Contexte** : 3 sites (Paris, Lyon, usine), chacun avec son bastion, un admin
nomade. Objectif : une config unique, des alias stables, du multiplexage pour
la fluidité.

```
# --- Bastions ---
Host bastion-paris
    HostName bastion.paris.example.com
    User zelef
Host bastion-lyon
    HostName bastion.lyon.example.com
    User zelef
Host bastion-usine
    HostName bastion.usine.example.com
    User zelef
    Port 2222

# --- Sites : motif par site ---
Host *.paris
    ProxyJump bastion-paris
Host *.lyon
    ProxyJump bastion-lyon
Host *.usine
    ProxyJump bastion-usine

# --- Défauts communs ---
Host *.paris *.lyon *.usine
    User zelef
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    ServerAliveInterval 60
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%C
    ControlPersist 10m

# --- Alias métier ---
Host pve-paris
    HostName pve1.paris
Host sw-usine
    HostName 10.30.0.2.usine
    User admin
```

Usage : `ssh pve-paris`, `ssh sw-usine`. Le motif `*.usine` + `ProxyJump`
fait le reste. Le multiplexage (`ControlPersist 10m`) rend la 2e connexion
instantanée même à travers deux sauts — critique quand on administre 30
switches d'usine à la chaîne.

**Exploitation** : ce fichier est versionné (dépôt d'équipe), le bloc
« bastions » est généré par l'inventaire. Un nouveau site = 4 lignes.

---

# AIDE-MÉMOIRE

## 80. Pense-bête de poche : commandes

```bash
# Connexion de base
ssh zelef@web1
ssh -p 2222 zelef@web1
ssh -J bastion web1-interne              # via un bastion
ssh -J b1,b2 cible                       # deux sauts

# Diagnostic
ssh -v cible / ssh -vvv cible            # verbosité
ssh -G cible                             # config effective (LE réflexe)
sshd -T                                  # config serveur effective
sshd -t && systemctl reload ssh          # tester puis recharger

# Clés
ssh-keygen -t ed25519 -C "qui@ou-quand" -f ~/.ssh/id_ed25519
ssh-keygen -l -f cle.pub                 # empreinte
ssh-keygen -R hote                       # purger known_hosts
ssh-keygen -p -f ~/.ssh/id_ed25519       # changer la passphrase
ssh-copy-id -i cle.pub user@hote

# Agent
eval "$(ssh-agent -s)" ; ssh-add -t 8h ~/.ssh/id_ed25519
ssh-add -l / -d cle / -D
ssh -O check cible / ssh -O stop cible   # multiplexage

# Tunnels
ssh -L 8080:localhost:9090 web1          # local  : poste -> serveur
ssh -R 9000:localhost:8000 web1          # distant: serveur -> poste
ssh -D 1080 bastion                      # SOCKS
ssh -N -f -L 8080:localhost:9090 web1    # tunnel en fond, sans shell
autossh -M 0 -N -f -L ...                # tunnel persistant auto-réparé

# Transferts
scp -r ./dir web1:/tmp/
sftp web1
rsync -avz --progress -e "ssh -J bastion" ./dir/ cible:/dest/
rsync -avz --delete --dry-run ./dir/ cible:/dest/   # simuler avant --delete

# Serveur : qui est là / qui a essayé
who ; last -n 20 ; lastb -n 20
ss -tnp | grep :22
grep -E "Accepted|Failed" /var/log/auth.log | tail
journalctl -u ssh -f
fail2ban-client status sshd
```

## 81. Pense-bête : `ssh_config` minimal propre

```
# ~/.ssh/config — squelette recommandé
Include ~/.ssh/config.d/*.conf

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    ConnectTimeout 10
    HashKnownHosts yes
    StrictHostKeyChecking accept-new
    IdentitiesOnly yes
    ForwardAgent no
    ForwardX11 no
    GSSAPIAuthentication no
    AddKeysToAgent yes
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%C
    ControlPersist 10m
    ExitOnForwardFailure yes
    LogLevel INFO
```

Avec `mkdir -p ~/.ssh/sockets && chmod 700 ~/.ssh && chmod 600 ~/.ssh/config`.
Tout le reste (bastions, alias, tunnels) va dans `config.d/`.

## 82. Pense-bête : `sshd_config` durci minimal

```
# /etc/ssh/sshd_config.d/99-hardening.conf — base durcie
Port 22
PermitRootLogin no
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
PubkeyAuthentication yes
PermitEmptyPasswords no
AllowGroups ssh-users
MaxAuthTries 3
MaxSessions 4
LoginGraceTime 30
MaxStartups 10:30:60
ClientAliveInterval 300
ClientAliveCountMax 2
UseDNS no
X11Forwarding no
AllowTcpForwarding no
PermitTunnel no
LogLevel VERBOSE
Banner /etc/ssh/banner.txt
DebianBanner no
PrintMotd no
PrintLastLog yes
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,diffie-hellman-group16-sha512
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes128-ctr
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com

# Ouvertures ciblées APRÈS (en fin de fichier) :
# Match Group tunnel-users
#     AllowTcpForwarding local
```

Checklist de mise en service : `sshd -t` → `reload` → test depuis session de
secours → `sshd -T | grep -Ei 'permitrootlogin|passwordauthentication'` →
fail2ban actif → logs qui partent au central.

## 83. Glossaire

| Terme | Définition |
|---|---|
| Bastion / jump host | Serveur passerelle, unique point d'entrée SSH vers un réseau privé |
| `authorized_keys` | Fichier listant les clés publiques autorisées pour un compte |
| CA (SSH) | Autorité qui signe des certificats SSH utilisateurs/hôtes |
| Certificat SSH | Clé publique signée par une CA, avec principals et dates de validité |
| Chiffrement symétrique | Chiffrement des données avec une clé partagée (rapide) |
| Cipher | Algorithme de chiffrement symétrique (ex. chacha20-poly1305) |
| ECDSA / Ed25519 | Algorithmes de signature ; ed25519 = choix moderne |
| Forward secrecy | Propriété : la compromission future d'une clé ne déchiffre pas le passé |
| GSSAPI | Authentification via Kerberos |
| Handshake | Négociation initiale : algos, échange de clés, authentifications |
| KEX | Key exchange : protocole d'établissement de la clé de session |
| `known_hosts` | Empreintes des serveurs déjà rencontrés (protection MITM) |
| MAC | Code d'authentification de message : garantit l'intégrité |
| MITM | Homme-du-milieu : attaquant interposé entre client et serveur |
| Multiplexage | Réutilisation d'une connexion TCP pour plusieurs sessions |
| PAM | Modules d'authentification pluggables (Linux) |
| Passphrase | Mot de passe protégeant une clé privée |
| Principal | Identité portée par un certificat SSH (ex. `zelef`) |
| ProxyJump | Relais SSH chiffré de bout en bout via un bastion (`-J`) |
| SCP / SFTP | Protocoles de copie de fichiers sur SSH |
| SOCKS | Protocole de proxy ; `-D` ouvre un proxy SOCKS via SSH |
| TOFU | Trust On First Use : on fait confiance à la 1re empreinte vue |
| TOTP | Mot de passe à usage unique basé sur le temps (2FA) |
| Tunnel `-L/-R/-D` | Redirection de port locale / distante / dynamique via SSH |
| 2FA | Authentification à deux facteurs |

## 84. Quiz (10 questions + réponses)

**Q1.** Pourquoi l'authentification par clé est-elle structurellement plus sûre
qu'un mot de passe, même robuste ?
> **R.** La clé privée ne voyage jamais : le client signe un défi unique que le
> serveur vérifie avec la clé publique. Un mot de passe, lui, est transmis
> (dans le tunnel chiffré, certes) et peut être rejoué s'il est intercepté ou
> deviné. La clé résiste au phishing et au brute-force en ligne.

**Q2.** Que signifie `StrictHostKeyChecking=accept-new`, et pourquoi est-il
préférable à `no` dans les scripts ?
> **R.** Les hôtes inconnus sont ajoutés automatiquement, mais tout
> **changement** d'empreinte reste bloquant. `no` désactive toute vérification
> et expose aux attaques homme-du-milieu.

**Q3.** Dans `~/.ssh/config`, quelle règle de priorité s'applique entre deux
blocs `Host` qui définissent la même option ?
> **R.** La **première** valeur obtenue gagne. D'où : blocs spécifiques avant
> le bloc `Host *` générique, placé en dernier.

**Q4.** Cite trois dangers de l'agent forwarding (`ssh -A`).
> **R.** 1) Root sur le serveur distant peut utiliser ton agent (signer avec
> tes clés) tant que tu es connecté. 2) Le socket peut survivre à la
> déconnexion. 3) Il est inutile dans la plupart des cas : `ProxyJump`
> authentifie depuis ton poste sans exposer l'agent.

**Q5.** Quelle différence entre `ssh -L` et `ssh -D` ?
> **R.** `-L` redirige un port local fixe vers **une** destination fixée à
> l'ouverture ; `-D` ouvre un proxy **SOCKS** dont la destination est choisie
> connexion par connexion par l'application cliente.

**Q6.** À quoi sert `command=` dans `authorized_keys`, et avec quelles options
l'accompagner ?
> **R.** À forcer l'exécution d'une commande précise en ignorant celle du
> client (clés de service : backup, CI). À accompagner de `no-pty`,
> `no-agent-forwarding`, `no-X11-forwarding`, `no-port-forwarding` et
> idéalement `from=`.

**Q7.** Pourquoi une connexion SSH peut-elle mettre 20 secondes à s'établir
sur un réseau sain ? Deux causes et leurs remèdes.
> **R.** 1) `UseDNS yes` côté serveur : résolution inverse lente →
> `UseDNS no`. 2) `GSSAPIAuthentication yes` côté client : tentative Kerberos
> qui timeout → `GSSAPIAuthentication no`.

**Q8.** Que faire quand `ssh` répond `Too many authentication failures` ?
> **R.** Le client propose plus de clés que `MaxAuthTries` : forcer la bonne
> avec `-o IdentitiesOnly=yes -i <clé>`, puis figer ça dans le bloc `Host`
> (`IdentityFile` + `IdentitiesOnly yes`).

**Q9.** Quelles permissions doivent avoir `~/.ssh`, la clé privée, et
`authorized_keys` côté serveur (avec home) ?
> **R.** `~/.ssh` : `700` ; clé privée : `600` ; `~/.ssh/authorized_keys` :
> `600` ; home : `755` maximum (sinon `StrictModes` fait refuser la clé).

**Q10.** En une phrase : quel est l'apport principal des certificats SSH par
rapport aux `authorized_keys` ?
> **R.** La CA signe les clés au lieu de les déployer sur chaque serveur :
> onboarding/offboarding instantanés, expiration intégrée, révocation
> centralisée, et fin du TOFU pour les clés d'hôte.

## 85. Pour aller plus loin

- **Docs officielles** : `man ssh`, `man ssh_config`, `man sshd`,
  `man sshd_config`, `man ssh-keygen` — la référence absolue, à jour avec ta
  version (`man` > n'importe quel tuto).
- **Protocole** : RFC 4251 (architecture), 4252 (auth), 4253 (transport),
  4254 (connexion).
- **Mozilla Infosec** : « OpenSSH security guidelines » — recommandations de
  configuration durcies, mises à jour régulièrement.
- **SSH Certificates** : la doc de `ssh-keygen` (options `-s`, `-I`, `-n`,
  `-V`, `-z`) + les articles de Facebook/Meta et Netflix sur le SSH à grande
  échelle (bastions + certificats éphémères).
- **Outils** : `autossh` (tunnels persistants), `mosh` (shell sur liens
  instables — pas un remplacement de SSH, un complément), `sshuttle` (VPN
  paresseux sur SSH), `ansible` (déploiement d'`authorized_keys` et de
  `sshd_config` à l'échelle).
- **Pratique** : monte un labo (3 VM : poste, bastion, cible), applique les
  sections 42-48, casse volontairement la config et dépanne avec la méthode
  de la section 61. C'est en réparant qu'on apprend.
- **Veille** : suis les annonces OpenSSH (nouvelles versions = nouveaux
  algos, retraits d'anciens) et les CVE ; teste chaque montée de version
  avec `sshd -T` et tes cas limites (section 60).

---

*Fin du guide. 85 sections. Prochaine étape suggérée : versionner ton
`~/.ssh/config.d/` et ton `sshd_config.d/` dans le dépôt d'exploitation de
l'équipe, puis planifier l'inventaire trimestriel des `authorized_keys`
(section 59).*
