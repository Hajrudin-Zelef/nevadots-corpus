---
id: collect-261121-veille-ia/cisco/clamav-antivirus-open-source-linux-10-etapes-2026
title: "ClamAV : Antivirus Open Source Linux, 10 Étapes [2026]"
domain: cisco
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["clamav", "antivirus", "fichiers", "mail", "open", "source", "linux", "cisco"]
source: https://tech-insider.org/fr/clamav-antivirus-open-source-linux-2026/
source_anchor: ""
source_lines: [1, 414]
section: "Muse"
---
Le 7 août 2026, l’équipe de Cisco Talos publiait coup sur coup deux correctifs de sécurité pour ClamAV : les versions 1.5.4 et 1.4.6 LTS. Deux failles corrigées d’un coup, CVE-2026-20337 et CVE-2026-20345, toutes deux capables de provoquer une corruption mémoire lors de l’analyse d’archives ZIP ou d’images disque au format GPT. Pour un outil que beaucoup d’administrateurs considèrent comme acquis, tourné en tâche de fond sur leur passerelle mail depuis des années, c’est un rappel utile : ClamAV reste un logiciel vivant, activement maintenu, et qui mérite une configuration sérieuse plutôt qu’une installation par défaut oubliée dans un coin.

Ce tutoriel vous guide du premier `apt install` jusqu’à un déploiement complet de ClamAV en passerelle antivirus pour un serveur mail Postfix, avec surveillance, quarantaine et automatisation. Le contexte réglementaire y ajoute une urgence supplémentaire : selon le rapport annuel 2026 du ministère de l’Intérieur sur la cybercriminalité, la France a enregistré 453 200 cyberattaques en 2025, soit une hausse de 87 % en cinq ans. Pour les PME concernées par la directive NIS2, disposer d’un antivirus documenté sur les points d’entrée critiques (serveurs de mail, partages de fichiers, passerelles d’upload) n’est plus une option confortable mais une exigence de conformité.

ClamAV coche une case particulière : c’est un moteur antivirus open source, gratuit, maintenu par Cisco Talos, et intégré nativement dans la quasi-totalité des distributions Linux. Il n’a pas vocation à remplacer un EDR sur les postes de travail, mais sur un serveur mail, un NAS ou une passerelle de dépôt de fichiers, c’est une brique de défense en profondeur qui coûte zéro euro de licence. Voici comment l’installer, le configurer et l’exploiter correctement en 2026.

## Qu’est-ce que ClamAV et pourquoi l’installer en 2026

ClamAV est un moteur antivirus open source né à la fin des années 1990, aujourd’hui développé et maintenu par Cisco Talos, l’équipe de renseignement sur les menaces de Cisco. Contrairement aux suites commerciales grand public comme Bitdefender ou Kaspersky, ClamAV cible en priorité les serveurs : passerelles de messagerie, serveurs de fichiers, systèmes de dépôt de documents, NAS d’entreprise. Son fonctionnement repose sur une analyse par signatures, complétée par des heuristiques et la détection de format de fichiers malveillants (PDF piégés, macros Office, archives contenant des exécutables suspects).

Trois raisons expliquent pourquoi ClamAV reste pertinent en 2026 plutôt que d’être une relique technique. D’abord, le rythme de maintenance ne faiblit pas : entre juin 2025 et août 2026, l’équipe a publié au moins quatre vagues de correctifs de sécurité (1.4.3/1.0.9 en juin 2025, 1.5.3/1.4.5 début juillet 2026, puis 1.5.4/1.4.6 le 7 août 2026), corrigeant des vulnérabilités dans les parseurs PDF, UDF, PE et ZIP. Ensuite, le projet a entamé en décembre 2025 un effort de réduction de la taille des fichiers de signatures (`main.cvd` et `daily.cvd`), ce qui allège la consommation disque et RAM sur les machines modestes. Enfin, l’intégration native dans Debian, Ubuntu, RHEL et leurs dérivés en fait un choix quasi gratuit à déployer, sans négociation de licence ni portail client à gérer.

ClamAV n’est pas un remplaçant direct d’une solution EDR ou d’un antivirus commercial pour poste de travail. Son terrain de jeu naturel reste le scan côté serveur : filtrage des pièces jointes avant remise en boîte mail, analyse des fichiers déposés sur un partage Samba ou NFS, vérification des uploads sur une application web. Utilisé à cet endroit précis, dans une architecture de défense en profondeur, il rend un vrai service, gratuitement et sans dépendance à un éditeur tiers.

## Prérequis : versions, matériel et compétences requises

Avant de commencer, vérifiez que votre environnement correspond aux versions et ressources recommandées. ClamAV tourne sur la quasi-totalité des distributions Linux, ainsi que sur FreeBSD, macOS et Windows, mais ce tutoriel se concentre sur un déploiement Linux serveur, le cas d’usage le plus courant en entreprise.

| Élément | Recommandation 2026 | Remarque | 
|---|---|---|
| Distribution | Debian 12/13, Ubuntu 22.04/24.04, RHEL/Rocky 9 | Paquets à jour dans les dépôts officiels | 
| Version ClamAV | 1.5.4 (branche stable) ou 1.4.6 LTS | 1.4 LTS supportée jusqu’au 15 août 2027 | 
| RAM | 2 Go minimum, 4 Go recommandés | La base de signatures chargée en mémoire par clamd pèse plusieurs centaines de Mo | 
| Espace disque | 3 Go libres | Base virale + logs + quarantaine | 
| Accès réseau | Sortant HTTPS vers les miroirs de signatures | freshclam a besoin d’accéder aux CDN ClamAV | 
| Droits | Accès root ou sudo | Installation de paquets et services systemd | 
| Compétences | Ligne de commande Linux de base | Aucune connaissance de programmation requise | 

Pour la partie intégration Postfix (l’étape « projet complet » de ce tutoriel), vous aurez besoin d’un serveur mail Postfix déjà fonctionnel, avec accès aux fichiers `main.cf` et `master.cf`. Si vous partez de zéro, installez d’abord Postfix seul, vérifiez qu’il route correctement le courrier, puis revenez à cette section. Comptez environ 60 minutes pour l’ensemble du tutoriel, installation, configuration, tests et intégration mail comprises.

## Comprendre l’architecture de ClamAV (clamd, freshclam, clamscan, clamonacc)

ClamAV n’est pas un binaire unique mais un ensemble de quatre composants qui travaillent ensemble. Comprendre leur rôle respectif évite bien des erreurs de configuration par la suite.

### Le démon clamd et le scanner clamscan

**clamd** est le démon de scan qui tourne en arrière-plan. Il charge la base de signatures en mémoire une seule fois au démarrage, puis répond aux demandes de scan via un socket Unix ou TCP, ce qui le rend nettement plus rapide qu’un scan à froid pour chaque fichier. C’est le composant que vous connecterez à Postfix via Milter, ou que vous interrogerez depuis vos propres scripts.

**clamscan**, à l’inverse, est un outil en ligne de commande autonome qui recharge la base de signatures à chaque exécution. Plus lent sur de gros volumes, il reste idéal pour un scan ponctuel, un test rapide ou une tâche cron peu fréquente sur un petit répertoire.

### freshclam et la base de signatures

**freshclam** est le service de mise à jour automatique des signatures. Il interroge les miroirs officiels ClamAV et télécharge les fichiers `main.cvd`, `daily.cvd` et `bytecode.cvd`. Depuis le 16 décembre 2025, le projet a entamé un effort de réduction de la taille de ces fichiers, notamment via un retrait progressif des signatures obsolètes, ce qui allège la charge disque et RAM sur les petites machines.

Un cinquième composant, **clamonacc**, mérite d’être cité séparément : il s’appuie sur les mécanismes fanotify et inotify du noyau Linux pour scanner les fichiers au moment même où ils sont créés ou modifiés sur le disque, une forme de protection en temps réel proche de ce que proposent les antivirus grand public.

## Étape 1 : installer ClamAV sur Debian, Ubuntu et RHEL/Fedora

L’installation passe par le gestionnaire de paquets natif de votre distribution. Sur les systèmes basés Debian, un seul appel apt installe le moteur, le démon et l’outil de mise à jour :

```
# Debian / Ubuntu
sudo apt update
sudo apt install clamav clamav-daemon clamav-freshclam clamav-milter -y
# RHEL / Rocky Linux / AlmaLinux (dépôt EPEL requis)
sudo dnf install epel-release -y
sudo dnf install clamav clamav-update clamd clamav-milter -y
# Fedora
sudo dnf install clamav clamav-update clamd clamav-milter -y
```
Sur Debian et Ubuntu, l’installation du paquet `clamav-daemon` déclenche automatiquement un premier téléchargement de la base de signatures et tente de démarrer le service `clamav-freshclam`. Sur RHEL et dérivés, cette étape est manuelle : il faudra lancer freshclam une première fois vous-même, ce que nous verrons à l’étape suivante. Comptez entre 5 et 15 minutes pour cette première installation, l’essentiel du temps étant consommé par le téléchargement initial de la base virale, qui pèse plusieurs centaines de mégaoctets.

## Étape 2 : vérifier la version installée et le statut du cycle de vie

Avant d’aller plus loin, vérifiez quelle version vient d’être installée. Les dépôts des distributions LTS (Debian stable, Ubuntu LTS) livrent parfois une version plus ancienne que la dernière release amont.

```
clamscan --version
clamd --version
```
Exemple de sortie attendue avec une installation à jour :

`ClamAV 1.4.6/27671/Mon Aug 10 08:12:33 2026`
Le premier nombre après la version (ici 27671) correspond au numéro de la base de signatures, et la date qui suit indique quand cette base a été téléchargée pour la dernière fois. Si votre distribution propose une version antérieure à 1.4, comparez-la à la table du cycle de vie plus bas dans cet article : certaines branches comme 0.103 ou 1.2 ont déjà atteint leur fin de support de sécurité, et il devient urgent de migrer.

Si votre paquet système est trop ancien, deux options s’offrent à vous : activer un dépôt tiers plus récent (backports Debian, dépôt EPEL à jour pour RHEL) ou compiler ClamAV depuis les sources publiées sur le dépôt GitHub Cisco-Talos. Pour la majorité des déploiements en PME, s’en tenir aux paquets de la distribution avec des mises à jour régulières du système suffit amplement.

## Étape 3 : configurer freshclam pour la mise à jour des signatures

Un antivirus sans base de signatures à jour ne sert à rien. freshclam se configure via le fichier `/etc/clamav/freshclam.conf` (Debian/Ubuntu) ou `/etc/freshclam.conf` (RHEL). Ouvrez-le et vérifiez les paramètres suivants :

```
# /etc/clamav/freshclam.conf
DatabaseMirror database.clamav.net
DatabaseDirectory /var/lib/clamav
UpdateLogFile /var/log/clamav/freshclam.log
LogFileMaxSize 5M
LogTime yes
Checks 12
NotifyClamd /etc/clamav/clamd.conf
```
La directive `Checks 12` indique à freshclam de vérifier les mises à jour douze fois par jour (soit toutes les deux heures), un rythme raisonnable pour ne pas manquer les signatures publiées en urgence tout en restant respectueux des miroirs officiels. La ligne `NotifyClamd` est cruciale : elle indique à freshclam de signaler à clamd qu’il doit recharger la base en mémoire dès qu’une nouvelle version est téléchargée, sans quoi le démon continuerait à tourner avec une base obsolète jusqu’à son prochain redémarrage.

Lancez ensuite une première synchronisation manuelle pour vérifier que tout fonctionne :

```
sudo systemctl stop clamav-freshclam
sudo freshclam
sudo systemctl start clamav-freshclam
sudo systemctl enable clamav-freshclam
```
Une exécution réussie affiche une sortie proche de celle-ci, avec le téléchargement séquentiel des trois fichiers de la base virale :

```
ClamAV update process started at Mon Sep 3 09:14:02 2026
main.cvd database is up to date (version: 62, sigs: 6647427)
daily.cvd updated (version: 27671, sigs: 2043112)
bytecode.cvd updated (version: 335, sigs: 92)
Database updated (8690631 signatures) from database.clamav.net
```
Si la commande échoue avec un message de type `ERROR: Can't connect to port 443`, le souci vient presque toujours d’un pare-feu sortant trop restrictif ou d’un proxy d’entreprise non déclaré. Nous détaillerons ce cas précis dans la section dépannage.

## Étape 4 : configurer et démarrer le démon clamd

Le fichier de configuration principal de clamd se trouve dans `/etc/clamav/clamd.conf`. Les paramètres par défaut conviennent pour un premier test, mais quelques ajustements s’imposent avant une mise en production.

```
# /etc/clamav/clamd.conf
LocalSocket /var/run/clamav/clamd.ctl
FixStaleSocket yes
User clamav
MaxThreads 12
MaxConnectionQueueLength 200
StreamMaxLength 100M
MaxScanSize 400M
MaxFileSize 100M
ScanArchive yes
ArchiveBlockEncrypted no
ExcludePath ^/proc
ExcludePath ^/sys
LogFile /var/log/clamav/clamd.log
LogFileMaxSize 10M
LogTime yes
LogRotate yes
```
Deux paramètres méritent une attention particulière en production. `MaxThreads` contrôle le nombre de scans simultanés : sur un serveur mail à fort trafic, une valeur trop basse crée une file d’attente qui ralentit la livraison des messages, tandis qu’une valeur trop haute peut saturer la RAM sur une petite instance. `StreamMaxLength` et `MaxFileSize` définissent respectivement la taille maximale d’un flux reçu via le socket et la taille maximale d’un fichier scanné : au-delà, ClamAV ignore le fichier plutôt que de risquer une consommation mémoire excessive, un compromis à connaître si votre organisation échange régulièrement de gros documents.

Démarrez et activez le service :

```
sudo systemctl start clamav-daemon
sudo systemctl enable clamav-daemon
sudo systemctl status clamav-daemon
```
Le premier démarrage peut prendre 30 à 60 secondes, le temps que clamd charge intégralement la base de signatures en mémoire. Sur une machine avec 2 Go de RAM, surveillez la consommation mémoire à ce moment précis : c’est le pic le plus élevé du cycle de vie normal du service.

## Étape 5 : lancer un premier scan avec clamscan et le fichier de test EICAR

Avant de connecter ClamAV à quoi que ce soit de sensible, validez que la détection fonctionne réellement. Le standard de l’industrie pour ce test est le fichier EICAR, une chaîne de caractères inoffensive reconnue par tous les moteurs antivirus du marché comme un signal de test, sans être un vrai malware.

```
curl -o /tmp/eicar.com https://secure.eicar.org/eicar.com
clamscan /tmp/eicar.com
```
Une détection réussie affiche :

```
/tmp/eicar.com: Win.Test.EICAR_HDB-1 FOUND
----------- SCAN SUMMARY -----------
Known viruses: 8690631
Engine version: 1.4.6
Scanned directories: 0
Scanned files: 1
Infected files: 1
Data scanned: 0.00 MB
Data read: 0.00 MB (ratio 0.00:1)
Time: 8.234 sec (0 m 8 s)
Start Date: 2026:09:03 09:22:14
End Date:   2026:09:03 09:22:22
```
Si `Infected files: 1` apparaît, votre installation détecte correctement. Testez ensuite un scan récursif sur un répertoire réel, par exemple un dossier de documents partagés, pour valider les performances sur un volume représentatif :

`clamscan -r --infected --remove=no /srv/documents/`
L’option `--infected` limite l’affichage aux fichiers infectés (utile sur un gros volume), et `--remove=no` vous protège d’une suppression accidentelle tant que vous validez encore votre configuration. Supprimez immédiatement le fichier EICAR de test une fois vos vérifications terminées.

## Étape 6 : activer la surveillance en temps réel avec clamonacc

clamscan et clamd via socket fonctionnent en mode « à la demande » : quelqu’un ou quelque chose doit déclencher le scan. Pour une protection continue d’un répertoire critique, comme un partage Samba de dépôt de fichiers, clamonacc s’appuie sur fanotify pour intercepter chaque création ou modification de fichier et déclencher un scan automatique.

Activez d’abord le support on-access dans `clamd.conf` :

```
# Ajouts dans /etc/clamav/clamd.conf
OnAccessIncludePath /srv/documents
OnAccessExcludePath /srv/documents/tmp
OnAccessMaxFileSize 50M
OnAccessPrevention no
OnAccessExtraScanning yes
```
Redémarrez clamd, puis lancez clamonacc en tant que service dédié :

```
sudo systemctl restart clamav-daemon
sudo clamonacc --fdpass --log=/var/log/clamav/clamonacc.log
```
`OnAccessPrevention no` signifie que ClamAV journalise et alerte sur les fichiers infectés sans bloquer leur écriture. Passer cette valeur à `yes` active le blocage préventif, mais attention : cela nécessite le module noyau fanotify avec les permissions adéquates, et une mauvaise configuration peut ralentir sensiblement les écritures sur le volume surveillé. Sur un serveur de production à fort trafic disque, testez d’abord en mode journalisation seule pendant plusieurs jours avant d’activer le blocage.

## Étape 7 : projet complet — sécuriser un serveur mail Postfix avec ClamAV Milter

Voici le cas d’usage le plus répandu de ClamAV en entreprise : filtrer les pièces jointes malveillantes avant qu’elles n’atteignent la boîte de réception. L’intégration passe par le protocole Milter, une interface standard supportée par Postfix pour déléguer le filtrage à un service tiers.

Configurez d’abord clamav-milter dans `/etc/clamav/clamav-milter.conf` :

```
# /etc/clamav/clamav-milter.conf
MilterSocket inet:[email protected]
MilterSocketGroup clamav
MilterSocketMode 660
User clamav
ClamdSocket unix:/var/run/clamav/clamd.ctl
OnInfected Reject
RejectMsg "Message rejeté : contenu malveillant détecté par ClamAV"
LogFile /var/log/clamav/clamav-milter.log
LogTime yes
MaxFileSize 100M
```
Puis déclarez le milter dans la configuration Postfix, en modifiant `/etc/postfix/main.cf` :

```
# Ajouts dans /etc/postfix/main.cf
milter_default_action = accept
milter_protocol = 6
smtpd_milters = inet:127.0.0.1:7357
non_smtpd_milters = inet:127.0.0.1:7357
```
Redémarrez les deux services dans l’ordre : clamav-milter d’abord, puis Postfix, sans quoi Postfix tenterait de se connecter à un socket Milter encore inexistant.

```
sudo systemctl restart clamav-milter
sudo systemctl restart postfix
sudo systemctl status clamav-milter postfix
```
Testez l’ensemble de la chaîne en envoyant un mail contenant le fichier EICAR en pièce jointe vers une boîte de test. Le message doit être rejeté à l’étape SMTP, avec le motif `Message rejeté : contenu malveillant détecté par ClamAV` visible dans les logs Postfix (`/var/log/mail.log`) et dans `/var/log/clamav/clamav-milter.log`. Si le mail passe malgré le fichier de test, vérifiez d’abord que `smtpd_milters` pointe bien vers le bon port, puis que le service clamav-milter écoute effectivement sur ce port avec `ss -tlnp | grep 7357`.

Pour les organisations qui préfèrent mettre en quarantaine plutôt que rejeter purement et simplement, remplacez `OnInfected Reject` par `OnInfected Quarantine` et définissez un dossier de quarantaine dédié via `QuarantineDir`. Ce choix évite les faux positifs bloquants pour l’expéditeur tout en isolant le contenu suspect pour analyse manuelle ultérieure.

## Étape 8 : automatiser les scans planifiés avec systemd timers

Même avec clamonacc actif sur les répertoires critiques, un scan complet planifié reste utile pour détecter les menaces dormantes ou les fichiers déposés avant l’activation de la surveillance temps réel. Plutôt qu’une entrée cron classique, un timer systemd offre un meilleur suivi des exécutions et de leurs échecs.

```
# /etc/systemd/system/clamav-full-scan.service
[Unit]
Description=Scan ClamAV complet planifie
After=clamav-daemon.service
[Service]
Type=oneshot
ExecStart=/usr/bin/clamdscan --multiscan --fdpass -r /srv/documents /home
Nice=15
IOSchedulingClass=idle
# /etc/systemd/system/clamav-full-scan.timer
[Unit]
Description=Execution quotidienne du scan ClamAV complet
[Timer]
OnCalendar=*-*-* 02:30:00
Persistent=true
[Install]
WantedBy=timers.target
```
Activez le timer :

```
sudo systemctl daemon-reload
sudo systemctl enable --now clamav-full-scan.timer
systemctl list-timers | grep clamav
```
Le choix de `clamdscan` plutôt que `clamscan` n’est pas anodin : clamdscan délègue le scan au démon clamd déjà actif en mémoire, ce qui le rend nettement plus rapide sur de gros volumes qu’un clamscan classique qui recharge la base à chaque lancement. Les paramètres `Nice=15` et `IOSchedulingClass=idle` évitent que le scan nocturne ne monopolise les ressources si d’autres tâches de maintenance tournent au même moment.

## Étape 9 : gérer la quarantaine et les faux positifs

Aucun moteur antivirus, ClamAV inclus, n’atteint zéro faux positif. Un document PDF avec une structure inhabituelle ou un script légitime utilisant des techniques d’obscurcissement peut parfois déclencher une alerte. Prévoir un processus de quarantaine et de révision limite l’impact opérationnel de ces cas.

Créez un répertoire de quarantaine dédié avec des permissions restreintes :

```
sudo mkdir -p /var/quarantine/clamav
sudo chown clamav:clamav /var/quarantine/clamav
sudo chmod 700 /var/quarantine/clamav
```
Pour les scans planifiés, remplacez la suppression automatique par un déplacement en quarantaine :

`clamdscan --multiscan --move=/var/quarantine/clamav -r /srv/documents`
Si un fichier légitime se retrouve mis en quarantaine, ne le restaurez jamais aveuglément. Vérifiez d’abord le nom de la signature déclenchée (visible dans les logs, sous la forme `NomDeSignature FOUND`), recherchez ce nom sur la base de connaissances ClamAV ou sur un service comme VirusTotal pour confirmer qu’il s’agit bien d’un faux positif, puis signalez-le à l’équipe ClamAV via leur processus officiel de soumission de faux positifs avant de restaurer le fichier. Cette étape de vérification, souvent sautée par manque de temps, est justement celle qui évite qu’un vrai malware ne soit restauré par excès de confiance.

## Étape 10 : superviser, journaliser et alerter

Un antivirus qui tourne sans supervision est un antivirus dont vous ne saurez jamais s’il fonctionne encore. Configurez au minimum une rotation de logs et une alerte sur les détections.

```
# /etc/logrotate.d/clamav
/var/log/clamav/*.log {
    weekly
    rotate 8
    compress
    missingok
    notifempty
    create 640 clamav adm
}
```
Pour une alerte simple par mail à chaque détection dans les logs clamd, un script surveillé par un timer systemd supplémentaire suffit dans la plupart des PME :

```
#!/bin/bash
# /usr/local/bin/clamav-alert.sh
LOGFILE="/var/log/clamav/clamd.log"
FOUND=$(grep "FOUND" "$LOGFILE" | tail -n 20)
if [ -n "$FOUND" ]; then
    echo "$FOUND" | mail -s "Alerte ClamAV : detection sur $(hostname)" [email protected]
fi
```
Si votre organisation dispose déjà d’un SIEM ou d’un outil de centralisation de logs comme Graylog ou Wazuh, préférez y faire remonter directement les fichiers de log ClamAV plutôt qu’un script mail isolé : la corrélation avec d’autres événements de sécurité (tentatives de connexion, alertes réseau) donne une vision bien plus complète qu’une alerte antivirus prise isolément.

## Cycle de vie des versions ClamAV : LTS, non-LTS et dates de fin de support

ClamAV distingue deux types de releases. Les branches LTS (Long Term Support) bénéficient d’au moins trois ans de support de sécurité à compter de leur publication initiale. Les branches non-LTS reçoivent un support plus court, généralement jusqu’à quatre mois après la publication de la release suivante. Connaître ces échéances évite de faire tourner une version qui ne reçoit plus de correctifs de sécurité, un vrai risque sur un serveur exposé à des pièces jointes venant de l’extérieur.

| Branche | Type | Date de sortie | Fin de support sécurité | Dernier patch connu | 
|---|---|---|---|---|
| 0.103 | LTS | 14 septembre 2020 | 14 septembre 2024 (signatures jusqu’au 14/09/2025) | Fin de vie atteinte | 
| 1.0 | LTS | 28 novembre 2022 | 28 novembre 2025 | 1.0.9 (18 juin 2025) | 
| 1.2 | Non-LTS | 27 août 2023 | 15 décembre 2024 | Fin de vie atteinte | 
| 1.3 | Non-LTS | 7 février 2024 | 7 février 2026 | 1.3.2 (4 septembre 2024) | 
| 1.4 | LTS | 15 août 2024 | 15 août 2027 | 1.4.6 (7 août 2026) | 
| 1.5 | Stable / latest | 2025 | En cours | 1.5.4 (7 août 2026) | 

Pour un déploiement en production, deux stratégies raisonnables se dégagent de ce tableau. Si la stabilité et un cycle de mises à jour prévisible priment, choisissez la branche 1.4 LTS, supportée jusqu’en août 2027. Si vous préférez bénéficier des dernières fonctionnalités et de la réduction de taille des signatures amorcée fin 2025, la branche 1.5 (taguée `latest` et `stable` par l’équipe ClamAV) est le choix par défaut. Dans les deux cas, planifiez dès maintenant la migration hors des branches 0.103, 1.0 et 1.2 si elles tournent encore chez vous : elles ne reçoivent plus de correctifs de sécurité.

## Erreurs courantes à éviter avec ClamAV

Cinq erreurs reviennent systématiquement dans les déploiements ClamAV mal préparés, souvent découvertes trop tard, au moment d’un incident.

- **Oublier NotifyClamd dans freshclam.conf** — sans cette directive, clamd continue de tourner avec une base de signatures obsolète après chaque mise à jour de freshclam, jusqu’au prochain redémarrage manuel du démon.
- **Scanner uniquement à l’arrivée, jamais en continu** — un scan ponctuel au moment de l’upload ne protège pas contre un fichier déposé avant l’activation de ClamAV, ou contre une signature ajoutée après coup pour un malware déjà présent sur le disque. Un scan planifié régulier reste indispensable en complément.
- **Confondre clamscan et clamdscan sur de gros volumes** — utiliser clamscan pour un scan récurrent sur des dizaines de milliers de fichiers recharge la base à chaque exécution et peut multiplier le temps de traitement par dix par rapport à clamdscan, qui délègue au démon déjà en mémoire.
- **Négliger la mise à jour du système hôte** — un ClamAV à jour sur un OS non patché reste vulnérable aux failles du noyau ou des bibliothèques système que le malware analysé pourrait exploiter une fois exécuté, indépendamment de la détection antivirus.
- **Ignorer les dates de fin de vie des branches** — faire tourner une version 0.103 ou 1.2 sans plan de migration expose à des vulnérabilités connues et déjà publiées, comme celles corrigées par les patchs 1.4.6 et 1.5.4 d’août 2026.

## Dépannage : les problèmes ClamAV les plus fréquents

Voici les incidents les plus fréquemment rencontrés en production, avec leur cause probable et la marche à suivre.

- **freshclam échoue avec « Can’t connect to port 443 »** — un pare-feu sortant bloque l’accès HTTPS vers les miroirs de signatures. Vérifiez avec`curl -v https://database.clamav.net` et ouvrez le port 443 en sortant si nécessaire, ou déclarez un proxy via`HTTPProxyServer` dans freshclam.conf.
- **clamd refuse de démarrer avec « ERROR: Can’t open/parse the config file »** — une erreur de syntaxe s’est glissée dans clamd.conf, souvent une ligne dupliquée ou un paramètre mal orthographié. Validez avec`clamd --config-file=/etc/clamav/clamd.conf --debug` qui affiche la ligne fautive.
- **Consommation mémoire excessive au démarrage** — normal sur une machine à 2 Go de RAM au chargement initial de la base ; si le service est tué par l’OOM killer, augmentez la RAM disponible ou limitez`MaxThreads` pour réduire l’empreinte mémoire globale.
- **Le milter ne bloque aucun mail malgré une détection dans les logs** — vérifiez que`OnInfected` est bien réglé sur`Reject` ou`Quarantine` et non`Pass` , et que Postfix pointe vers le bon port dans`smtpd_milters` .
- **clamonacc ne détecte aucun fichier créé** — le noyau doit exposer fanotify ; vérifiez avec`zgrep CONFIG_FANOTIFY /proc/config.gz` ou consultez la documentation de votre distribution pour l’activer.
- **Faux positif récurrent sur un type de fichier interne** — créez une exception locale via un fichier de signature d’exclusion personnalisé (`local.ign2` ) plutôt que de désactiver globalement une catégorie de détection.
- **Scan anormalement lent sur des archives volumineuses** — vérifiez les paramètres`MaxScanSize` ,`MaxFileSize` et`ArchiveBlockEncrypted` , qui peuvent forcer une analyse exhaustive d’archives imbriquées consommant beaucoup de CPU.
- **Erreur « ERROR: Malformed database » après une mise à jour** — le téléchargement d’une base de signatures a été interrompu ou corrompu. Supprimez les fichiers`.cvd` dans`/var/lib/clamav` et relancez`freshclam` pour forcer un téléchargement complet.
- **Socket clamd introuvable (« Could not connect to clamd »)** — vérifiez que le service clamav-daemon est bien actif avec`systemctl status clamav-daemon` , et que le chemin du socket dans clamdscan.conf correspond bien à celui déclaré dans clamd.conf.

## ClamAV face aux solutions commerciales : Sophos, Bitdefender GravityZone, ESET

ClamAV ne participe pas régulièrement aux tests indépendants publiés par AV-Test ou AV-Comparatives, contrairement aux suites commerciales grand public. Il n’existe donc pas de score de détection comparable et vérifiable à opposer directement à Sophos, Bitdefender GravityZone ou ESET. La comparaison pertinente n’est pas tant « qui détecte le plus de menaces » que « quel outil pour quel usage ».

| Critère | ClamAV | Solutions commerciales (Sophos, Bitdefender GravityZone, ESET) | 
|---|---|---|
| Coût de licence | Gratuit, open source | Abonnement par poste/serveur | 
| Cas d’usage principal | Serveurs, passerelles mail, scan de fichiers | Postes de travail, serveurs, EDR intégré | 
| Console centralisée | Aucune nativement, à construire soi-même | Console cloud ou on-premise incluse | 
| Sandboxing / ML avancé | Non | Souvent inclus dans les offres premium | 
| Tests indépendants AV-Test/AV-Comparatives | Non régulièrement évalué | Évalué mensuellement | 
| Support éditeur | Communautaire + Cisco Talos (maintenance) | Support contractuel SLA | 
| Intégration Postfix/Milter native | Oui, clamav-milter dédié | Variable selon l’éditeur | 

En pratique, beaucoup d’organisations combinent les deux approches : ClamAV en première ligne sur la passerelle mail pour filtrer le volume brut à coût nul, et une solution commerciale avec EDR sur les postes de travail pour la détection comportementale avancée et la réponse aux incidents. Ce n’est pas l’un ou l’autre, mais une architecture en couches où chaque outil couvre un périmètre différent.

## ClamAV et conformité NIS2 pour les PME françaises

La directive NIS2 impose aux entités essentielles et importantes des mesures techniques de détection et de prévention des logiciels malveillants, y compris sur les serveurs de messagerie et de partage de fichiers. ClamAV, déployé sur une passerelle mail Postfix comme décrit dans ce tutoriel, contribue directement à cette exigence, à condition d’être intégré dans une démarche documentée plutôt que laissé en configuration par défaut.

Trois points de vigilance pour un dossier de conformité solide : documentez la politique de mise à jour des versions (branche LTS choisie, date de migration prévue avant l’échéance de fin de support), tracez la surveillance des vulnérabilités connues avec preuve d’application des correctifs (comme le passage aux versions 1.5.4 ou 1.4.6 d’août 2026), et conservez les logs de détection pendant une durée cohérente avec votre politique de rétention interne. Ces trois éléments, combinés à une authentification multifacteur sur les accès administratifs et une protection contre le brute-force via un outil comme Fail2ban, forment une base défendable devant un auditeur NIS2.

## Astuces avancées pour optimiser ClamAV en production

Une fois l’installation de base stabilisée, plusieurs réglages permettent d’affiner les performances et la couverture de détection sur un serveur à fort trafic.

Sur les serveurs multi-cœurs, augmentez progressivement `MaxThreads` par palier de deux en surveillant la charge CPU et mémoire avec `htop`, plutôt que de fixer une valeur arbitraire élevée d’emblée. Pour les environnements où le scan d’archives imbriquées ralentit le traitement, ajustez `MaxRecursion` dans clamd.conf pour limiter la profondeur d’analyse des archives contenues dans d’autres archives, un vecteur d’attaque connu sous le nom de « zip bomb ». Si votre infrastructure combine ClamAV avec une passerelle réseau plus large, envisagez également une association avec un pare-feu applicatif comme OPNsense pour filtrer le trafic en amont, réduisant d’autant le volume de fichiers que ClamAV doit traiter.

Pour les organisations exploitant plusieurs serveurs ClamAV, un miroir de signatures interne via `cvdupdate` évite de multiplier les requêtes sortantes vers les serveurs officiels ClamAV, un gain particulier sur les connexions internet limitées ou fortement filtrées. Enfin, si votre passerelle mail gère un volume important, envisagez de combiner ClamAV avec un système de protection contre les tentatives d’intrusion réseau comme CrowdSec, qui couvre un périmètre complémentaire (comportement réseau) à celui de ClamAV (contenu de fichiers).

## Questions fréquentes

**ClamAV est-il un antivirus complet pour poste de travail Windows ou macOS ?**

Non. ClamAV cible en priorité les serveurs et passerelles (mail, fichiers, uploads). Sur un poste de travail, il ne remplace pas une suite de sécurité complète avec protection en temps réel avancée et détection comportementale.

**Quelle est la différence entre clamscan et clamdscan ?**

clamscan recharge la base de signatures à chaque exécution, ce qui le rend lent sur de gros volumes. clamdscan délègue le scan au démon clamd déjà chargé en mémoire, donc nettement plus rapide pour les scans répétés ou planifiés.

**Faut-il choisir la branche 1.4 LTS ou la branche 1.5 stable ?**

La branche 1.4 LTS est supportée jusqu’au 15 août 2027 et convient aux environnements qui privilégient un cycle de mise à jour prévisible. La branche 1.5, taguée « latest » et « stable » par l’équipe ClamAV, apporte les dernières optimisations, notamment la réduction de taille des signatures amorcée fin 2025.

**ClamAV peut-il bloquer un mail contenant un virus avant qu’il n’arrive en boîte de réception ?**

Oui, via l’intégration Milter avec Postfix décrite dans ce tutoriel. En réglant `OnInfected` sur `Reject`, le message est rejeté au niveau SMTP avant toute remise en boîte.

**Combien de temps prend le premier téléchargement de la base de signatures ?**

Entre 2 et 10 minutes selon la bande passante disponible, la base virale complète pesant plusieurs centaines de mégaoctets. Les mises à jour suivantes sont incrémentales et beaucoup plus rapides.

**ClamAV consomme-t-il beaucoup de ressources en fonctionnement normal ?**

Le pic de consommation mémoire survient au démarrage de clamd, lors du chargement de la base en mémoire. En fonctionnement courant, la consommation CPU reste proportionnelle au volume de fichiers scannés ; ajustez `MaxThreads` selon les ressources disponibles.

**Que faire si ClamAV signale un faux positif sur un fichier légitime ?**

Ne restaurez jamais le fichier sans vérification. Identifiez le nom de la signature déclenchée dans les logs, croisez-le avec une seconde source (comme VirusTotal), signalez le cas à l’équipe ClamAV via leur processus officiel de soumission, puis restaurez uniquement après confirmation.

**ClamAV suffit-il à lui seul pour la conformité NIS2 ?**

Non. ClamAV répond à l’exigence de détection de logiciels malveillants sur un périmètre donné (mail, fichiers), mais la conformité NIS2 couvre un ensemble de mesures bien plus large : gestion des accès, chiffrement, plan de continuité, notification d’incidents. ClamAV est une brique parmi d’autres dans une architecture de défense en profondeur.
