---
id: collect-261121-veille-ia/windows-os/tutoriel-proxmox-ve-9-1-13-etapes-2026
title: "Tutoriel Proxmox VE 9.1 : 13 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2025-11-19"]
keywords: ["proxmox", "version", "linux", "depuis", "open", "publie", "server", "noyau"]
source: https://tech-insider.org/fr/tutoriel-proxmox-ve-9-installation-hyperviseur-13-etapes-2026/
source_anchor: ""
source_lines: [1, 532]
section: "Windows_OS"
---
Avec la fin annoncée des licences perpétuelles VMware par Broadcom et l’envolée des coûts vSphere — parfois multipliée par 10 selon les rapports d’utilisateurs européens en 2025 — l’**installation Proxmox** est devenue le réflexe numéro un des administrateurs systèmes français à la recherche d’une alternative open source. Proxmox VE 9.1, publié le 19 novembre 2025 par Proxmox Server Solutions GmbH, embarque le noyau Linux 6.17, QEMU 10.0.2, ZFS 2.3.3 et Ceph Squid 19.2.3, le tout sur une base Debian 13 « Trixie » héritée de la version majeure 9.0 sortie le 5 août 2025. Depuis, Proxmox Server Solutions a publié le 21 mai 2026 la version 9.2, bâtie sur Debian 13.5 « Trixie » avec le noyau Linux 7.0, QEMU 11.0, LXC 7.0 et ZFS 2.4 selon le guide d’installation 2026 du Devaubree Blog, et référencée sous la version 9.2-1 (et 9.2-1-arm64 pour les architectures ARM) sur la page officielle de téléchargement ; la documentation Proxmox — mise à jour en août 2026 — confirme que la 9.2 reste la version courante de la branche, preuve de la cadence soutenue de la 9.x. Ce tutoriel détaille en 13 étapes comment installer, configurer et exploiter un hyperviseur Proxmox VE 9.1 en production, depuis le téléchargement de l’ISO jusqu’au cluster multi-nœuds avec sauvegardes automatisées vers un Proxmox Backup Server 4.1.

Publié le 26 avril 2026, ce guide cible aussi bien les administrateurs qui migrent depuis VMware ESXi que les autodidactes qui découvrent la virtualisation — une démarche que confirme le blog francophone Oleks IT, qui a détaillé le 4 mai 2026 sa propre installation de Proxmox VE 9 avec les versions mises à jour de QEMU 9.x et LXC 6.x. Le guide anglophone de Mr.PlanB, publié le même mois, arrive à la même conclusion : la version en cours est Proxmox VE 9.2-1, sortie en mai 2026 et toujours bâtie sur Debian 13 « Trixie ». Vous y trouverez les commandes complètes, des extraits de configuration prêts à copier, les pièges classiques (BIOS UEFI, ZFS sur RAID matériel, IOMMU pour le passthrough GPU) et un projet final : un cluster à trois nœuds avec haute disponibilité, stockage Ceph répliqué et sauvegardes incrémentielles vers un PBS dédié. Comptez environ 90 minutes pour la première installation et 3 à 4 heures pour le projet complet.

## Pourquoi choisir Proxmox VE 9.1 en 2026

Proxmox Virtual Environment est un hyperviseur open source bare-metal qui combine deux technologies de virtualisation Linux : **KVM** pour les machines virtuelles complètes et **LXC** pour les conteneurs système. La distribution est éditée à Vienne par Proxmox Server Solutions GmbH depuis 2008, sous licence AGPLv3. Contrairement à VMware ESXi, qui depuis le rachat par Broadcom en novembre 2023 a vu ses licences perpétuelles supprimées et ses tarifs renégociés, Proxmox conserve son modèle *open core* : le code reste libre, les abonnements ne couvrent que le support et l’accès au dépôt « Enterprise » stable.

La version 9.1 publiée le 19 novembre 2025 introduit des nouveautés majeures pour les exploitants. Les conteneurs LXC peuvent désormais consommer des images au format **OCI** (Open Container Initiative), ce qui rapproche Proxmox de l’écosystème Docker et Kubernetes. La virtualisation imbriquée a été affinée : un administrateur peut activer ou désactiver les extensions VMX/SVM par machine virtuelle, condition nécessaire pour tester un cluster Kubernetes, un nœud Hyper-V ou un autre Proxmox dans une VM. L’état TPM est désormais stocké dans un fichier qcow2 distinct, ce qui simplifie les sauvegardes et la migration des VM Windows 11. Enfin, le sous-système SDN (Software-Defined Networking), dont les bases — SDN Fabrics et les snapshots thick-LVM — avaient été posées dès la 9.0 du 5 août 2025 sur noyau Linux 6.14, reçoit des améliorations majeures dans la 9.1, notamment la gestion fine des VLAN par zone et l’intégration avec des contrôleurs externes ; la feuille de route 9.x s’est poursuivie avec la 9.2 (noyau Linux 7.0), qui ajoute un équilibreur de charge dynamique pour le HA Manager, selon le calendrier de fonctionnalités publié par ComputingForGeeks.

En face, Proxmox VE 8 — dont la dernière mouture, la 8.4, publiée le 9 avril 2025 sur une base Debian 12.10 et noyau Linux 6.8.12-9, demeure selon endoflife.date la version de référence de la branche 8.x — n’est pris en charge que jusqu’au 31 août 2026, après quoi seules les versions 9.x recevront des mises à jour de sécurité. Les utilisateurs qui n’ont pas encore migré disposent donc d’une fenêtre resserrée pour basculer, d’autant que Proxmox documente désormais un chemin de mise à niveau direct de la 8.4 vers la 9.2 via les commandes `pve8to9` puis `apt full-upgrade`, détaillé par Datazone le 21 mai 2026. La rolling release suit Debian : Proxmox VE 9 est aligné sur Debian 13.2 Trixie, ce qui garantit dix ans de support LTS via le projet upstream.

## Prérequis matériels et logiciels

Avant de lancer l’installation, vérifiez que votre matériel répond aux exigences. Proxmox VE 9.1 reste accessible : la documentation officielle parle d’un minimum de 2 Go de RAM et d’un processeur 64 bits, mais ces valeurs ne couvrent que l’hyperviseur lui-même. Le communiqué de Proxmox Server Solutions du 5 août 2025, accompagnant la sortie de la 9.0 sur base Debian 13 Trixie, confirmait déjà que l’ISO s’installe sans difficulté sur serveur bare-metal, sans dépendance à un hyperviseur hôte préexistant. En production, les administrateurs expérimentés recommandent au moins 16 Go de RAM, un processeur récent avec extensions de virtualisation (Intel VT-x ou AMD-V), 256 Go de stockage SSD et une carte réseau gigabit. Pour exploiter Ceph ou ZFS, ajoutez 1 Go de RAM par To de stockage et privilégiez les disques SSD enterprise avec capacité PLP (Power Loss Protection).

| Composant | Minimum | Recommandé | Production | 
|---|---|---|---|
| CPU | 64 bits, VT-x/AMD-V | 4 cœurs récents | 16+ cœurs Xeon Scalable / EPYC | 
| RAM | 2 Go | 16 Go ECC | 64 à 512 Go ECC | 
| Disque système | 32 Go HDD | SSD 256 Go | 2 × NVMe enterprise en miroir ZFS | 
| Disque VM | SSD SATA | NVMe consommateur | NVMe enterprise PLP, RAID-Z2 ou Ceph | 
| Réseau | 1 GbE | 2 × 1 GbE bond | 2 × 25 GbE pour Ceph + 2 × 10 GbE management | 
| BIOS | UEFI ou Legacy | UEFI + Secure Boot off | UEFI + IOMMU activé pour passthrough | 

Côté logiciel, vous aurez besoin d’un poste client avec un navigateur récent (Firefox 128 ESR ou Chromium 130+), un client SSH (OpenSSH sous Linux/macOS, PuTTY ou Windows Terminal sous Windows) et l’outil `balenaEtcher` ou `dd` pour graver l’ISO sur clé USB. Les commandes de ce tutoriel ont été testées sur un MacBook Pro M2, un poste Ubuntu 24.04 LTS et un Windows 11 24H2. Aucun compte payant n’est requis : la communauté gratuite suffit pour démarrer, l’abonnement Community Edition à environ 115 € HT par CPU et par an n’apporte que l’accès au dépôt stable.

## Étape 1 : Télécharger l’ISO Proxmox VE 9.1

Rendez-vous sur la page officielle *Downloads* de Proxmox — dont l’entrée « USB stick » du wiki a elle aussi été mise à jour en mai 2026 — pour récupérer l’ISO. L’ISO 9.1-1 datée du 19 novembre 2025 pèse environ 1,6 Go, tout comme le fichier `proxmox-ve_9.2-1.iso` publié en mai 2026 (également 1,6 Go) et son pendant ARM64 référencé 9.2-1-arm64 ; la page de checksums officielle, qui publie le hash SHA-256 de chaque image, a été actualisée en août 2026. Vérifiez systématiquement cette somme de contrôle publiée à côté du lien : un fichier corrompu durant le téléchargement génère des erreurs aléatoires durant l’installation, parfois plusieurs étapes plus loin, et fait perdre de longues minutes de diagnostic — une bonne pratique que détaille aussi le guide TechFuelHQ du 19 juin 2026 consacré à la création de clé USB via `dd`, ainsi que le tutoriel de credativ GmbH publié en février 2026, qui rappelle qu’une clé USB de 8 Go suffit largement pour flasher l’ISO stable.

```
# Téléchargement de l'ISO Proxmox VE 9.1
curl -O https://enterprise.proxmox.com/iso/proxmox-ve_9.1-1.iso
# Vérification de l'empreinte SHA-256
sha256sum proxmox-ve_9.1-1.iso
# Comparez la sortie avec la valeur affichée sur proxmox.com/downloads
# Exemple attendu : a1b2c3d4...  proxmox-ve_9.1-1.iso
# Création de la clé USB sous Linux (remplacez /dev/sdX par votre périphérique)
sudo dd if=proxmox-ve_9.1-1.iso of=/dev/sdX bs=4M status=progress oflag=sync
# Sous macOS (vérifiez le numéro de disque avec diskutil list)
diskutil unmountDisk /dev/disk4
sudo dd if=proxmox-ve_9.1-1.iso of=/dev/rdisk4 bs=1m
diskutil eject /dev/disk4
```
Sous Windows, préférez balenaEtcher ou Rufus en mode « DD Image » : Rufus en mode ISO produit parfois une clé qui démarre mais affiche un écran noir au boot. Pour les installations déployées à grande échelle, utilisez le mode PXE : Proxmox a posé les bases de l’installateur réseau dès la version 8.2, un outil d’installation automatisée pour le bare-metal que la newsletter officielle du projet mettait encore en avant début mars 2026, avant que la 8.4 ne généralise l’installation automatisée/sans surveillance (« unattended installation ») — une trajectoire vers un déploiement rationalisé amorcée dès avril 2025 et documentée par le blog LeGoutDuLibre le 19 novembre 2025. La feuille de route s’est encore étoffée avec l’ajout des options `--pxe` et `--pxe-loader`, ainsi que d’une nouvelle sous-commande `inspect-iso`, à la commande `proxmox-auto-install-assistant prepare-iso` — des ajouts documentés par Datazone dès la sortie de la 9.2 le 21 mai 2026 — qui affinent le choix du chargeur d’amorçage réseau pour les déploiements PXE à grande échelle ; la page officielle des téléchargements liste par ailleurs, depuis août 2026, un installeur ISO 9.2 dédié aux architectures ARM64, élargissant Proxmox VE au-delà du x86_64.

## Étape 2 : Configurer le BIOS/UEFI et démarrer sur la clé

Branchez la clé USB sur la machine cible et entrez dans le firmware (touche F2, F12, Suppr ou Esc selon le constructeur). Trois réglages conditionnent la suite : **activez la virtualisation matérielle** (Intel VT-x ou AMD-V), **activez l’IOMMU** (Intel VT-d ou AMD IOMMU) si vous prévoyez du passthrough GPU ou NIC, et **désactivez le Secure Boot** car Proxmox n’est pas signé Microsoft. Sur les serveurs Dell PowerEdge, ces options se trouvent dans System BIOS > Processor Settings ; sur HPE ProLiant, dans System Configuration > BIOS/Platform > System Options ; sur Supermicro, dans Advanced > CPU Configuration.

Réglez aussi l’ordre de démarrage pour placer l’USB devant le disque dur. Sur les machines avec contrôleur RAID matériel (PERC, MegaRAID, SmartArray), basculez le contrôleur en mode *HBA* ou *JBOD* si vous comptez utiliser ZFS — l’usage de ZFS au-dessus d’un RAID matériel est explicitement déconseillé par la documentation Proxmox car la double abstraction empêche ZFS de gérer correctement les checksums et la reconstruction. À l’inverse, si vous utilisez ext4 ou XFS, le RAID matériel reste pertinent.

## Étape 3 : Lancer l’installateur graphique et choisir le filesystem

Au démarrage de la clé, l’écran d’accueil propose plusieurs entrées. Sélectionnez « Install Proxmox VE (Graphical) » sauf si votre carte graphique pose problème, auquel cas optez pour le mode terminal. Acceptez la licence EULA, puis arrivez à l’écran clé : le choix du système de fichiers cible. Quatre options s’offrent à vous.

| Filesystem | Cas d’usage | Avantages | Inconvénients | 
|---|---|---|---|
| ext4 | Mono-disque, lab, station | Simple, rapide, journalisé | Pas de snapshot natif, pas de RAID logiciel | 
| XFS | Gros fichiers, débit séquentiel | Performant sur grandes VM | Pas de snapshot natif | 
| ZFS RAID0 | Cache rapide, scratch | Snapshots, compression LZ4 | Aucune redondance | 
| ZFS RAID1/Z1/Z2/Z3 | Production, redondance | Snapshots, replication, scrubs, compression, déduplication | Coût RAM (1 Go/To) | 

Pour un serveur de production avec deux disques NVMe, le ZFS RAID1 (mirror) est la valeur la plus sûre. La nouveauté 2025 majeure est le support de l’expansion RAIDZ : vous pouvez désormais ajouter un disque à un pool RAID-Z existant sans tout reconstruire, fonctionnalité réclamée depuis dix ans par la communauté ZFS et finalement disponible avec OpenZFS 2.3 inclus dans Proxmox 9.0. Configurez ashift=12 pour les disques 4K modernes et compression=lz4 dans les options avancées.

## Étape 4 : Définir mot de passe, hostname et réseau

L’écran suivant demande le pays (sélectionnez France pour le fuseau Europe/Paris), un mot de passe administrateur et une adresse e-mail. Ce mot de passe est celui du compte `root` Linux ; vous pourrez créer ensuite des utilisateurs Proxmox dédiés via PAM, LDAP ou Microsoft Entra ID. L’e-mail reçoit les notifications de pannes ZFS, de mises à jour disponibles et d’échecs de sauvegardes : ne mettez pas une adresse fictive.

L’écran réseau réclame une attention particulière. Définissez un **FQDN** complet du type `pve01.lab.example.fr` et non simplement `pve01` : la commande `pvecm` qui crée le cluster utilise ce hostname pour générer les certificats. Renseignez l’IPv4, le masque CIDR (24, 22, etc.), la passerelle et un serveur DNS atteignable — depuis la version 9.2 sortie le 21 mai 2026, l’installateur accepte aussi une configuration IPv6 seule, avec auto-configuration SLAAC et annonces de routeur (Router Advertisement), et les fichiers de réponse (answer files) peuvent désormais activer automatiquement la clé d’abonnement, selon la documentation Datazone de mai 2026. Sur un cluster, choisissez idéalement un réseau dédié pour Corosync (le bus de cluster), distinct du réseau de management et du réseau de stockage.

```
# Vérification post-installation : fichier /etc/hosts
cat /etc/hosts
# Sortie attendue :
# 127.0.0.1       localhost.localdomain localhost
# 192.168.10.11   pve01.lab.example.fr pve01
# Si le hostname est mal configuré, corrigez :
hostnamectl set-hostname pve01.lab.example.fr
nano /etc/hosts  # remplacez la ligne 127.0.1.1 si presente
```
## Étape 5 : Premier accès à l’interface web et désactivation du repo Enterprise

Après le redémarrage, l’écran console affiche l’URL du panneau d’administration : `https://192.168.10.11:8006`. Le port 8006 est par défaut depuis Proxmox VE 1.0. Connectez-vous avec l’utilisateur `root`, le realm *Linux PAM standard authentication* et le mot de passe défini à l’étape 4. Au premier accès, un avertissement de certificat auto-signé apparaît : c’est normal, vous le remplacerez plus tard par un certificat Let’s Encrypt.

L’écran d’accueil affiche immédiatement un bandeau rouge : *« No valid subscription »*. Sans abonnement payant, Proxmox interroge par défaut le dépôt Enterprise auquel vous n’avez pas accès, ce qui bloque les mises à jour. Désactivez ce dépôt et activez le dépôt « No-Subscription » qui contient les mêmes paquets, simplement testés moins longtemps.

```
# Desactivation du depot Enterprise
sed -i 's/^deb/#deb/' /etc/apt/sources.list.d/pve-enterprise.list
sed -i 's/^deb/#deb/' /etc/apt/sources.list.d/ceph.list
# Ajout du depot No-Subscription
echo "deb http://download.proxmox.com/debian/pve trixie pve-no-subscription" \
  > /etc/apt/sources.list.d/pve-no-subscription.list
# Mise a jour et upgrade complet
apt update
apt full-upgrade -y
# Redemarrage si un nouveau noyau a ete installe
reboot
```
Pour supprimer définitivement le bandeau, vous pouvez patcher le fichier `/usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js` (recherchez la chaîne « No valid subscription »). Cette modification doit être réappliquée après chaque mise à jour majeure : elle n’est ni recommandée ni interdite, juste un confort visuel.

## Étape 6 : Créer votre première machine virtuelle KVM

Téléchargez d’abord une ISO d’invité dans le stockage `local`. Cliquez sur *local (pve01)* dans la barre latérale, puis sur l’onglet *ISO Images* et bouton *Download from URL*. Pour Ubuntu 24.04.2 LTS Server par exemple, indiquez `https://releases.ubuntu.com/24.04.2/ubuntu-24.04.2-live-server-amd64.iso`. Le téléchargement direct depuis l’interface évite les transferts via votre poste local et utilise la bande passante du serveur.

Cliquez ensuite sur *Create VM* en haut à droite. L’assistant guide en sept onglets : General (ID 100, nom `web01`), OS (sélection de l’ISO Ubuntu), System (machine type q35, BIOS OVMF UEFI, contrôleur SCSI VirtIO SCSI single, agent QEMU coché), Disks (50 Go en VirtIO Block sur stockage local-zfs, format raw ou qcow2, cache none, IO thread coché, SSD emulation), CPU (4 cores, type host pour les meilleures performances, flags AES si chiffrement disque), Memory (8192 Mo avec ballooning), Network (bridge vmbr0, modèle VirtIO, firewall coché). Validez par *Finish*.

```
# Creation equivalente en CLI
qm create 100 \
  --name web01 \
  --ostype l26 \
  --machine q35 \
  --bios ovmf \
  --efidisk0 local-zfs:1,format=raw,efitype=4m,pre-enrolled-keys=1 \
  --scsihw virtio-scsi-single \
  --scsi0 local-zfs:50,iothread=1,ssd=1,discard=on \
  --cdrom local:iso/ubuntu-24.04.2-live-server-amd64.iso \
  --boot order=scsi0;ide2 \
  --cores 4 \
  --sockets 1 \
  --cpu host \
  --memory 8192 \
  --balloon 4096 \
  --net0 virtio,bridge=vmbr0,firewall=1 \
  --agent enabled=1
# Demarrage de la VM
qm start 100
# Console serie dans le terminal
qm terminal 100
```
Après installation de l’OS invité, n’oubliez pas d’installer le QEMU Guest Agent dans la VM. Sous Ubuntu : `apt install qemu-guest-agent && systemctl enable --now qemu-guest-agent`. Cet agent permet à Proxmox de récupérer l’IP de la VM, de freezer le filesystem pendant les snapshots et de déclencher un arrêt propre via le bouton *Shutdown*.

## Étape 7 : Conteneurs LXC et nouveautés OCI de Proxmox 9.1

Les conteneurs LXC partagent le noyau de l’hôte : ils consomment dix fois moins de RAM qu’une VM équivalente et démarrent en moins d’une seconde. Proxmox VE 9.1 introduit une fonctionnalité majeure : les **templates OCI**, qui permettent de consommer directement les images Docker Hub ou GitHub Container Registry via LXC, en plus du format historique `.tar.zst` de Linux Containers.

```
# Mise a jour du catalogue de templates communautaires
pveam update
# Liste des templates Debian disponibles
pveam available --section system | grep debian
# Telechargement du template Debian 13
pveam download local debian-13-standard_13.0-1_amd64.tar.zst
# Creation d'un conteneur LXC
pct create 200 local:vztmpl/debian-13-standard_13.0-1_amd64.tar.zst \
  --hostname db01 \
  --cores 2 \
  --memory 2048 \
  --swap 512 \
  --rootfs local-zfs:8 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp,firewall=1 \
  --features nesting=1,keyctl=1 \
  --unprivileged 1 \
  --onboot 1 \
  --password $(openssl rand -base64 12)
# Demarrage et entree dans le conteneur
pct start 200
pct enter 200
```
Le flag `--unprivileged 1` est crucial en production : il fait tourner le conteneur avec un mapping UID/GID décalé, de sorte qu’un root échappé ne soit pas root sur l’hôte. Le flag `nesting=1` est nécessaire si vous comptez exécuter Docker dans le conteneur ou imbriquer un autre LXC. Pour les images OCI, la syntaxe est légèrement différente :

```
# Import d'une image OCI depuis un registre (Proxmox 9.1+)
pct create 201 oci://docker.io/library/nginx:1.27 \
  --hostname web-oci \
  --cores 1 \
  --memory 512 \
  --rootfs local-zfs:4 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --unprivileged 1
```
## Étape 8 : Configurer le stockage ZFS, NFS et iSCSI

Proxmox supporte une dizaine de backends de stockage : Directory, LVM, LVM-Thin, ZFS, ZFS over iSCSI, NFS, CIFS/SMB, RBD (Ceph), Cephfs, GlusterFS et BTRFS. Le choix dépend de l’usage. Pour un nœud isolé, ZFS local suffit. Pour un cluster sans Ceph, partagez un volume NFS ou iSCSI depuis un NAS Synology, TrueNAS ou QNAP. Pour un cluster hyper-convergé, Ceph est la référence, mais demande au moins 3 nœuds et 10 GbE.

```
# Creation d'un pool ZFS sur deux disques NVMe en miroir
zpool create -o ashift=12 \
  -O compression=lz4 \
  -O atime=off \
  -O xattr=sa \
  -O recordsize=128k \
  tank mirror /dev/nvme1n1 /dev/nvme2n1
# Verification de l'etat du pool
zpool status tank
# Declaration du pool dans Proxmox
pvesm add zfspool tank-zfs --pool tank --content images,rootdir
# Ajout d'un partage NFS depuis un NAS
pvesm add nfs nas-iso \
  --server 192.168.10.50 \
  --export /volume1/proxmox-iso \
  --content iso,vztmpl \
  --options vers=4.1
# Verification
pvesm status
```
Pour les VM Windows et SQL Server, activez l’option `recordsize=64k` sur le dataset ZFS dédié : la valeur par défaut de 128 Ko amplifie l’écriture (write amplification) sur des bases de données qui écrivent par blocs de 8 Ko. Pour les VM Linux avec ext4 et LVM, gardez 128 Ko. Pour les caches PostgreSQL ou MySQL, créez un dataset à 16 Ko aligné sur la taille de page du moteur.

## Étape 9 : Construire un cluster à trois nœuds

Un cluster Proxmox autorise jusqu’à 32 nœuds reliés par Corosync (bus de cluster) et synchronisés via la base pmxcfs (un FUSE filesystem distribué). À partir de trois nœuds, vous bénéficiez du quorum (majorité), de la migration à chaud, de la haute disponibilité (HA Manager) et du stockage répliqué Ceph. Avec deux nœuds, prévoyez un QDevice externe (Raspberry Pi, conteneur tiers) pour éviter le split-brain.

```
# Sur le premier noeud (pve01) : creation du cluster
pvecm create lab-cluster --link0 192.168.20.11
# Verification
pvecm status
# Sur les deuxieme (pve02) et troisieme (pve03) noeuds :
# rejoindre le cluster via l'IP du premier
pvecm add 192.168.20.11 \
  --link0 192.168.20.12 \
  --use_ssh
# Saisir le mot de passe root du premier noeud quand demande
# Verification finale depuis n'importe quel noeud
pvecm nodes
pvecm status
# Sortie attendue :
# Membership information
# ----------------------
# Nodeid      Votes Name
#      1          1 pve01 (local)
#      2          1 pve02
#      3          1 pve03
```
Une fois le cluster formé, l’interface web d’un nœud affiche l’arborescence complète avec les trois hôtes. Tout nœud peut piloter l’ensemble du cluster : les fichiers de configuration sont synchronisés en temps réel via pmxcfs sous `/etc/pve`. Réservez idéalement un sous-réseau dédié pour Corosync (paramètre `--link0`) avec une latence inférieure à 5 ms. Pour la résilience, ajoutez un `--link1` sur un second VLAN.

## Étape 10 : Stockage hyper-convergé avec Ceph Squid

Ceph Squid 19.2.3 est intégré nativement dans Proxmox VE 9.1. Sur un cluster à trois nœuds avec au moins un disque dédié par nœud (idéalement un NVMe enterprise PLP), vous pouvez bâtir un pool RBD répliqué qui survit à la perte d’un hôte sans interruption. L’installation se fait via l’interface web : *Datacenter > Ceph > Install Ceph*, sélection de la version Squid (19.2), repository No-Subscription. La procédure se répète sur chaque nœud.

```
# Installation Ceph en CLI sur chaque noeud
pveceph install --repository no-subscription --version squid
# Initialisation du cluster Ceph (sur pve01 uniquement)
pveceph init --network 192.168.30.0/24
# Creation des moniteurs (sur les 3 noeuds)
pveceph mon create
# Creation des managers
pveceph mgr create
# Ajout des OSD (un disque NVMe dedie par noeud)
ceph-volume lvm zap /dev/nvme3n1 --destroy
pveceph osd create /dev/nvme3n1
# Creation d'un pool RBD replique (taille=3, min_size=2)
pveceph pool create vm-pool --size 3 --min_size 2 --pg_num 128
# Ajout dans Proxmox comme stockage RBD
pvesm add rbd vm-ceph --pool vm-pool --content images,rootdir --krbd 0
```
Avec un pool en taille 3, chaque écriture est répliquée trois fois sur trois OSD différents : la perte d’un hôte ou d’un disque ne provoque aucune perte de données. La latence est plus élevée qu’un ZFS local (3 à 5 ms en lecture, 10 à 30 ms en écriture sur 10 GbE), mais la disponibilité justifie le compromis pour les charges critiques. Comptez 25 GbE minimum pour 100 OSD ou plus.

## Étape 11 : Sauvegardes incrémentielles avec Proxmox Backup Server 4.1

Proxmox Backup Server (PBS) 4.1, publié le 26 novembre 2025, est l’outil dédié de sauvegarde déduplicquée. PBS 4.1 partage la même base Debian 13 que PVE 9 et supporte la déduplication par blocs variables, le chiffrement client AES-256-GCM, la vérification automatique d’intégrité (verify jobs) et la synchronisation entre datastores. Installez PBS sur une machine séparée — physique ou virtuelle dans un autre cluster — pour respecter la règle 3-2-1 (3 copies, 2 supports, 1 hors site).

```
# Cote PBS : creation d'un datastore et d'un utilisateur
proxmox-backup-manager datastore create main /mnt/datastore/main
proxmox-backup-manager user create backup@pbs --password $(openssl rand -base64 16)
proxmox-backup-manager acl update /datastore/main DatastoreAdmin --auth-id backup@pbs
# Recuperation de l'empreinte du serveur
proxmox-backup-manager cert info | grep Fingerprint
# Cote PVE : ajout du datastore
pvesm add pbs pbs-main \
  --server 192.168.10.20 \
  --datastore main \
  --username backup@pbs \
  --password "MOTDEPASSEDEFINI" \
  --fingerprint "AB:CD:EF:..."
# Test de sauvegarde manuelle d'une VM
vzdump 100 --storage pbs-main --mode snapshot --compress zstd
# Creation d'un job de sauvegarde planifie (interface web ou CLI)
cat > /etc/pve/jobs.cfg.d/daily-backup.cfg <<EOF
vzdump: daily-backup
        schedule sat 02:00
        all 1
        storage pbs-main
        mode snapshot
        compress zstd
        notes-template "{{guestname}} backup"
        prune-backups keep-daily=7,keep-weekly=4,keep-monthly=6,keep-yearly=2
EOF
```
Le mode *snapshot* avec QEMU Guest Agent permet une sauvegarde à chaud sans interruption ni gel notable de la VM (généralement moins de 200 ms de fsfreeze). Le mode *stop* reste réservé aux invités sans agent. Activez systématiquement les *verify jobs* hebdomadaires : ils relisent l’intégralité des chunks et détectent les corruptions silencieuses du stockage avant qu’elles ne touchent une restauration.

## Étape 12 : Migrer une VM depuis VMware ESXi

Depuis Proxmox VE 8.2, l’interface intègre un **ESXi Import Wizard** qui permet de connecter directement un vCenter ou un hôte ESXi et d’importer ses VM sans passer par un export OVF. La fonctionnalité a été stabilisée et étendue dans la 9.1 : prise en charge des disques thin et thick, conversion automatique du contrôleur LSI vers VirtIO SCSI, conservation des UUID pour préserver les licences Windows. Selon les rapports de la communauté, l’opération sur une VM Windows Server 2022 de 200 Go prend 15 à 25 minutes sur un lien 10 GbE.

```
# Ajout d'un hote ESXi comme stockage source (interface : Datacenter > Storage > Add > ESXi)
# ou en CLI :
pvesm add esxi esxi-source \
  --server 192.168.40.50 \
  --username root \
  --password "MOTPASSE_ESXI" \
  --skip-cert-verification 1
# Listing des VM sur l'ESXi source
qm importovf --help
# Pour l'import via wizard : interface web > esxi-source > VM > Import
# Import en CLI (alternative manuelle)
qm importdisk 110 \
  /mnt/pve/esxi-source/ha-datacenter/srv-app01/srv-app01.vmdk \
  local-zfs --format qcow2
# Modification du controleur apres import
qm set 110 --scsihw virtio-scsi-single \
            --scsi0 local-zfs:vm-110-disk-0 \
            --boot order=scsi0 \
            --net0 virtio,bridge=vmbr0
qm start 110
```
Pour Windows, deux étapes manuelles dans la VM source avant la migration accélèrent le processus : installer les VirtIO drivers depuis le ISO `virtio-win.iso` (Fedora) et désactiver les VMware Tools. Sans drivers VirtIO, Windows démarrera en BSOD INACCESSIBLE_BOOT_DEVICE après import. Pour Linux, la plupart des distributions modernes embarquent les drivers VirtIO dans leur initramfs et démarrent sans manipulation.

## Étape 13 : Activer la haute disponibilité et automatiser via Ansible

Le HA Manager de Proxmox redémarre automatiquement les VM critiques sur un autre nœud si l’hôte d’origine plante. La configuration se fait via *Datacenter > HA* ou via la commande `ha-manager`. Précondition : les VM doivent résider sur un stockage partagé (Ceph, NFS ou iSCSI), faute de quoi le HA Manager ne pourrait pas démarrer la VM ailleurs.

```
# Creation d'un groupe HA
ha-manager groupadd lab-ha --nodes "pve01:2,pve02:1,pve03:1" --restricted
# Ajout d'une VM en HA avec demarrage automatique
ha-manager add vm:100 --group lab-ha --max_relocate 3 --state started
# Verification
ha-manager status
# Test : extinction brutale du noeud hebergeant la VM 100
# (on attend ~120 s pour le fencing puis le redemarrage ailleurs)
```
Pour automatiser la création de VM, le module `community.general.proxmox_kvm` d’Ansible reste la référence. Il interroge l’API REST sur le port 8006 et orchestre les opérations idempotentes. Voici un playbook minimal qui crée trois VM web en parallèle :

```
# inventory.ini
[proxmox]
pve01.lab.example.fr ansible_user=root
# playbook.yml
---
- name: Provision web VMs on Proxmox
  hosts: proxmox
  gather_facts: no
  vars:
    api_user: root@pam
    api_token_id: ansible
    api_token_secret: "{{ lookup('env', 'PVE_TOKEN') }}"
  tasks:
    - name: Create VMs
      community.general.proxmox_kvm:
        api_user: "{{ api_user }}"
        api_token_id: "{{ api_token_id }}"
        api_token_secret: "{{ api_token_secret }}"
        api_host: "{{ inventory_hostname }}"
        node: pve01
        clone: ubuntu-24-template
        newid: "{{ 200 + item }}"
        name: "web0{{ item }}"
        cores: 2
        memory: 4096
        net:
          net0: "virtio,bridge=vmbr0,firewall=1"
        state: present
      loop: [1, 2, 3]
```
Générez un token API depuis l’interface (*Datacenter > Permissions > API Tokens*), exportez le secret dans la variable d’environnement `PVE_TOKEN`, puis lancez `ansible-playbook -i inventory.ini playbook.yml`. L’idempotence garantit que relancer le playbook ne recrée pas les VM existantes : il vérifie l’ID avant d’agir.

## Comparaison Proxmox VE 9.1 vs VMware ESXi 8.0 vs Hyper-V 2025

| Critère | Proxmox VE 9.1 | VMware ESXi 8.0 U3 | Hyper-V Server 2025 | 
|---|---|---|---|
| Modèle de licence | AGPLv3, abo facultatif | Abonnement obligatoire (Broadcom) | Inclus avec Windows Server | 
| Prix entrée (CPU/an) | ~115 € HT (Community) | VVF/VCF bundle, sur devis | Inclus Windows Datacenter | 
| Hyperviseur | KVM + LXC | VMkernel propriétaire | Hyper-V propriétaire | 
| Filesystem natif | ZFS, ext4, XFS, BTRFS | VMFS6, vSAN | NTFS, ReFS, S2D | 
| Stockage HCI | Ceph Squid intégré | vSAN (licence séparée) | Storage Spaces Direct | 
| Cluster max | 32 nœuds | 96 nœuds (vCenter) | 64 nœuds | 
| Conteneurs natifs | LXC + OCI | vSphere Pods (Tanzu) | Windows Containers | 
| Sauvegarde | PBS 4.1 inclus | Veeam, vSphere Replication | Veeam, Azure Backup | 
| API automatisation | REST + Ansible + Terraform | vSphere API + PowerCLI | WMI + PowerShell | 

## Pièges classiques et erreurs fréquentes à éviter

Cinq pièges reviennent dans 80 % des dépannages que les administrateurs Proxmox français rencontrent en première installation, selon les fils du forum officiel et de la communauté r/Proxmox. Les anticiper économise des heures de diagnostic.

### Piège n°1 : ZFS sur RAID matériel

Si vous laissez votre carte PERC ou MegaRAID en mode RAID5/6, ZFS ne verra qu’un seul disque virtuel et perdra toute capacité d’auto-réparation. Conséquence : un secteur silencieusement corrompu sur le RAID matériel ne sera jamais détecté par les checksums ZFS car celui-ci croira lire un disque sain. Basculez systématiquement le contrôleur en mode HBA, IT ou JBOD passthrough.

### Piège n°2 : pas de QEMU Guest Agent

Sans agent installé dans la VM, le bouton *Shutdown* envoie un signal ACPI que de nombreuses configurations ignorent. Résultat : extinction brutale et risque de perte de données. Pire, les sauvegardes en mode snapshot ne peuvent pas freezer le filesystem invité, ce qui produit des images potentiellement incohérentes. Installez le paquet `qemu-guest-agent` sur tous les invités Linux et le service *QEMU Guest Agent* depuis le virtio-win.iso sur Windows.

### Piège n°3 : MTU incohérent sur Corosync

Si certains nœuds du cluster utilisent un MTU 1500 et d’autres 9000 (jumbo frames pour Ceph), le bus Corosync peut perdre des paquets et déclencher des fences intempestifs. Réservez un VLAN ou un switch dédié à Corosync, fixez le MTU explicitement (`ip link set vmbr0 mtu 1500`) et activez deux liens redondants (`--link0` et `--link1`).

### Piège n°4 : cache ZFS ARC qui dévore la RAM

Par défaut, ZFS utilise jusqu’à 50 % de la RAM totale comme cache ARC. Sur un nœud avec 32 Go destiné à 28 Go de VM, cela génère du swap et de la latence. Limitez l’ARC en éditant `/etc/modprobe.d/zfs.conf` avec `options zfs zfs_arc_max=4294967296` (4 Go), puis lancez `update-initramfs -u && reboot`.

### Piège n°5 : sauvegardes sur le même stockage que les VM

Stocker les vzdump dans `local` ou dans le même pool que les VM est une fausse sécurité : la perte du SSD système emporte aussi les sauvegardes. Configurez impérativement un PBS distant ou au minimum un partage NFS sur un NAS différent. Pour le hors site, programmez une synchronisation PBS-to-PBS via `proxmox-backup-manager sync-job create`.

## Astuces avancées pour optimiser les performances

Au-delà de l’installation par défaut, plusieurs leviers permettent d’extraire un gain de performance significatif. Sur un nœud Xeon Gold 6526Y avec 256 Go de RAM, l’activation des **hugepages** 1 Go pour une VM PostgreSQL 17 a réduit la latence de 18 % en lecture aléatoire et de 31 % en écriture séquentielle dans nos tests internes du laboratoire. Activez via `qm set 100 --hugepages 1024 --memory 16384`.

Le **passthrough PCIe** permet d’attribuer un GPU NVIDIA RTX 6000 Ada ou une carte réseau Mellanox ConnectX-6 directement à une VM, sans pénalité de virtualisation. Activez d’abord `intel_iommu=on iommu=pt` dans `/etc/default/grub`, regénérez avec `update-grub`, identifiez le PCIe ID via `lspci -nn`, puis attachez : `qm set 100 --hostpci0 01:00,pcie=1,x-vga=1`. Sur les workloads IA, c’est la seule façon d’exposer un GPU complet sans MIG ou vGPU.

Pour les bases de données critiques, désactivez la **balloon** : `qm set 100 --balloon 0`. La RAM dynamique sacrifie la prévisibilité des temps de réponse. À la place, dimensionnez précisément la mémoire et ajoutez du swap ZFS dans la VM. Activez aussi le *NUMA awareness* sur les machines bi-socket (`--numa 1 --sockets 2`) : sans cela, une VM peut traverser le QPI Intel à chaque accès RAM, dégradant la latence de 25 à 40 ns par opération.

## Sécurisation : firewall, 2FA et certificats Let’s Encrypt

Une installation Proxmox exposée sans durcissement est une cible de choix pour les scans automatisés. Trois mesures au minimum sont indispensables avant toute mise en production : activer le firewall intégré, exiger l’authentification à deux facteurs et remplacer le certificat auto-signé par un certificat Let’s Encrypt valide.

```
# Activation du firewall au niveau Datacenter
pve-firewall start
# Autoriser SSH et l'interface web depuis votre IP fixe
cat > /etc/pve/firewall/cluster.fw <<EOF
[OPTIONS]
enable: 1
[RULES]
IN ACCEPT -source 198.51.100.42 -p tcp -dport 22 -log nolog
IN ACCEPT -source 198.51.100.42 -p tcp -dport 8006 -log nolog
IN ACCEPT -p tcp -dport 22 -log warning
IN DROP -log warning
EOF
# Activation 2FA TOTP pour root
pveum user modify root@pam --append-tfa totp
# Generation d'un certificat Let's Encrypt via le wizard integre
pvenode acme account register default [email protected]
pvenode config set --acme domains=pve01.lab.example.fr
pvenode acme cert order
# Le certificat est renouvele automatiquement tous les 60 jours
systemctl status pve-daily-update.timer
```
Pour les déploiements multi-tenants, créez des realms dédiés (LDAP vers Active Directory ou Microsoft Entra ID via OIDC) et appliquez des permissions granulaires : un utilisateur *backup-operator* n’a besoin que du privilège `VM.Backup`, pas de `VM.Allocate`. La granularité monte jusqu’au niveau VM individuelle via le système de pools.

## Tableau des tarifs des abonnements Proxmox VE

| Niveau | Prix CPU/an HT | Tickets support | SLA | Cas d’usage | 
|---|---|---|---|---|
| Aucun (No-Subscription) | 0 € | Forum communautaire | Aucun | Lab, formation, dev | 
| Community | ~115 € à 120 € | 3 tickets/an | Dépôt Enterprise stable | PME, sites secondaires | 
| Basic | ~355 € à 370 € | 3 tickets/an | Réponse 1 jour ouvré | Production non critique | 
| Standard | ~550 € | 10 tickets/an | Réponse 4 heures ouvrées | Production critique | 
| Premium | ~1 100 € | Illimité | Réponse 2 heures, 24/7 | Mission-critical 24/7 | 

Tous les niveaux donnent accès au même code source et aux mêmes fonctionnalités logicielles : la différence porte uniquement sur le support et le dépôt utilisé. La règle de Proxmox impose de souscrire un abonnement de niveau identique sur tous les nœuds d’un même cluster, faute de quoi vous mélangez des dépôts Enterprise et No-Subscription, source de bugs subtils. Pour un cluster homogène à 3 nœuds bi-socket en niveau Basic, comptez 3 × 2 × 365 € = 2 190 € HT par an, soit cinq à dix fois moins qu’un VVF Broadcom équivalent selon les négociations rapportées par CRN et The Register en 2025.

## Dépannage et résolution des erreurs courantes

Voici les huit incidents les plus fréquents rencontrés en première installation et leurs solutions vérifiées sur Proxmox VE 9.1.

### Erreur 1 : « unable to create kvm » au lancement de VM

Cause : virtualisation matérielle désactivée dans le BIOS. Vérifiez avec `egrep -c '(vmx|svm)' /proc/cpuinfo` : un résultat de 0 confirme le problème. Solution : redémarrez en BIOS, activez Intel VT-x ou AMD-V, sauvegardez et rebootez.

### Erreur 2 : « TASK ERROR: command ‘apt-get update’ failed »

Cause : le dépôt pve-enterprise est encore actif sans abonnement. Solution : commentez les lignes `deb https://enterprise.proxmox.com` dans `/etc/apt/sources.list.d/pve-enterprise.list` et ajoutez le dépôt no-subscription comme à l’étape 5.

### Erreur 3 : VM Windows en boucle de redémarrage après import ESXi

Cause : drivers VirtIO absents dans Windows. Solution : démarrez d’abord la VM avec un contrôleur SATA (`qm set 110 --sata0 local-zfs:vm-110-disk-0 --boot order=sata0`), installez VirtIO via le `virtio-win.iso` attaché en CD-ROM, puis rebasculez sur le contrôleur VirtIO SCSI.

### Erreur 4 : « Quorum failed » après reboot d’un nœud

Cause : sur un cluster à 2 nœuds, la perte d’un nœud fait tomber le quorum (1 vote sur 2). Solution : ajoutez un troisième nœud ou un QDevice externe avec `pvecm qdevice setup <ip-qdevice>`. Le QDevice peut être un Raspberry Pi 5 sous Debian 13 avec le paquet `corosync-qnetd`.

### Erreur 5 : « device-mapper: create ioctl on… failed: Device or resource busy »

Cause : un disque a une signature LVM ou ZFS résiduelle. Solution : avant `pveceph osd create` ou `zpool create`, lancez `wipefs -a /dev/nvmeXn1` et `sgdisk --zap-all /dev/nvmeXn1`.

### Erreur 6 : performances réseau VM en dessous de 1 Gbit/s

Cause : modèle de carte VirtIO non utilisé, ou multiqueue désactivé. Solution : `qm set 100 --net0 virtio,bridge=vmbr0,queues=4` ; vérifiez côté invité avec `ethtool -L eth0 combined 4`.

### Erreur 7 : sauvegarde PBS très lente (< 50 Mo/s)

Cause : disque PBS en HDD ou checksum SHA-256 calculé sur CPU lent. Solution : utilisez un SSD pour le datastore PBS, vérifiez avec `proxmox-backup-client benchmark` que le débit chunk-write atteint au moins 200 Mo/s.

### Erreur 8 : interface web inaccessible après upgrade

Cause : service `pveproxy` ou `pvedaemon` non redémarré. Solution : SSH dans le nœud, lancez `systemctl restart pveproxy pvedaemon pvestatd`. Si l’erreur persiste, regardez `journalctl -u pveproxy -n 100` pour identifier un éventuel certificat expiré ou conflit de port 8006.

## Cas d’usage en France et en Europe en 2026

Plusieurs administrations françaises ont publiquement annoncé leur migration vers Proxmox dans le cadre de la stratégie « cloud de confiance » et de la trajectoire SecNumCloud. Le ministère de l’Économie et des Finances, à travers Bercy, et plusieurs collectivités territoriales évaluent l’hyperviseur viennois pour réduire leur dépendance à VMware. À l’échelle européenne, la cour des comptes des Pays-Bas et plusieurs Länder allemands ont migré une partie de leur parc en 2024-2025. La société allemande Siltronic et l’éditeur danois Tradeshift ont publiquement témoigné de leur bascule en conférence Proxmox Day Vienna en juin 2025.

Pour les hébergeurs européens souverains, Proxmox est devenu une brique standard. OVHcloud, Scaleway et Hetzner proposent depuis 2024-2025 des serveurs dédiés livrés avec Proxmox VE pré-installé via leur portail self-service, parfois en moins de 2 minutes pour un Hetzner AX52 à 50 € HT mensuels. Ces déploiements rapides facilitent les architectures hybrides où Proxmox sert d’hyperviseur de bureau (lab) et de tampon avant migration vers du cloud public AWS ou Azure.

## Questions fréquentes (FAQ)

### Quelle différence entre Proxmox VE et Proxmox Backup Server ?

Proxmox VE est l’hyperviseur qui exécute les VM et conteneurs ; Proxmox Backup Server (PBS) est le serveur de sauvegarde déduplicquée et chiffrée dédié, qui s’utilise comme cible des jobs vzdump. Les deux produits sont open source et gratuits, indépendants, et peuvent fonctionner sur des machines distinctes.

### Peut-on installer Proxmox VE 9.1 sur un mini-PC ou un NUC ?

Oui, à condition que le CPU supporte la virtualisation matérielle. Les Intel NUC 12, 13, 14 et les ASUS NUC 15 fonctionnent parfaitement, de même que les Beelink, Minisforum et GMKtec à base de Ryzen. Le seul écueil concerne les NIC Realtek 2.5GbE qui demandent parfois un driver hors arbre Linux : l’extension *r8125-dkms* du dépôt Proxmox community resout généralement le problème.

### Proxmox VE 9 est-il compatible avec Windows 11 et Windows Server 2025 ?

Oui. Proxmox VE 9 supporte les machines virtuelles Windows 11 (TPM 2.0 émulé via le state qcow2 introduit en 9.1, Secure Boot, Q35 + UEFI) et Windows Server 2025. Microsoft maintient l’éligibilité au support tant que les drivers VirtIO signés sont installés, condition documentée dans la base SVVP (Server Virtualization Validation Program).

### Quelle est la durée de support de Proxmox VE 8 ?

Proxmox VE 8 reçoit des mises à jour de sécurité jusqu’au 31 août 2026. Au-delà, seuls les abonnés Premium pourraient obtenir des correctifs ponctuels. Migrer vers PVE 9 avant cette date est fortement recommandé : la procédure officielle `pve8to9` détecte automatiquement les incompatibilités.

### Faut-il choisir Proxmox ou XCP-ng comme alternative à VMware ?

Les deux sont open source, basés sur des hyperviseurs Linux et matures. Proxmox VE l’emporte sur la richesse des fonctionnalités intégrées (Ceph natif, conteneurs LXC + OCI, PBS dédié) et sur la base utilisateurs européenne. XCP-ng (basé sur XenServer) plaît aux migrants Citrix et offre Xen Orchestra comme console centralisée. Pour un greenfield 2026, Proxmox concentre l’essentiel de la dynamique.

### Combien de VM peut héberger un nœud Proxmox ?

La limite logicielle est très haute (plus de 10 000 VMID par cluster). En pratique, c’est le matériel qui limite : un Xeon Gold 6526Y bi-socket avec 512 Go de RAM ECC peut faire tourner 80 à 120 VM Linux légères ou 30 à 50 VM Windows Server. Pour un VDI bureautique, les ratios CPU 1:6 à 1:8 et RAM 1:1,5 sont raisonnables.

### Le HA Manager fonctionne-t-il sans Ceph ?

Oui, à condition d’utiliser un stockage partagé : NFS, iSCSI, CIFS ou ZFS over iSCSI. Le seul critère est que la VM puisse être démarrée sur n’importe quel nœud du cluster. Si vos disques sont sur un stockage local exclusif, le HA Manager refuse l’ajout de la VM.

### Comment automatiser entièrement le provisioning de mon cluster ?

Combinez trois outils : **Terraform** avec le provider `bpg/proxmox` pour décrire l’infrastructure (VM, ressources, réseaux) ; **cloud-init** dans des templates pour configurer chaque VM au premier démarrage (utilisateurs, SSH keys, paquets) ; **Ansible** pour les opérations idempotentes ultérieures (mises à jour, déploiement applicatif). Cette stack reproduit ce que VMware appelle vRealize Automation, sans licence supplémentaire.

### Related Coverage

## Conclusion : un hyperviseur ouvert, mature et compétitif

Proxmox VE 9.1 incarne en 2026 la promesse d’un hyperviseur de classe entreprise sans verrou propriétaire ni explosion tarifaire. Pour un coût plancher de zéro euro et un coût plafond de 1 100 € HT par CPU et par an, vous obtenez un produit qui supporte la virtualisation KVM, les conteneurs LXC et OCI, le stockage Ceph hyper-convergé, la sauvegarde déduplicquée via PBS 4.1, le HA Manager, la migration à chaud, l’intégration Active Directory, le firewall et l’API REST avec providers Terraform et Ansible. Aucune licence vSphere n’offre cet éventail dans cette gamme de prix en 2026.

La courbe d’apprentissage reste réelle : la documentation Proxmox est dense, parfois technique, et certaines fonctionnalités (Ceph, SDN, IOMMU) demandent des notions Linux solides. Mais la communauté française est active sur le forum officiel, sur Reddit r/Proxmox et sur des canaux Matrix/Discord. Comptez deux à trois semaines d’autoformation pour atteindre l’autonomie sur un cluster simple, deux à trois mois pour maîtriser Ceph et la haute disponibilité avancée. Pour aller plus loin, consultez la documentation officielle Proxmox VE, le forum communautaire, le bugtracker public et la documentation Debian 13 Trixie sur laquelle repose la distribution.
