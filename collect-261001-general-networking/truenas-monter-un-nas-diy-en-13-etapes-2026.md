Les prix des disques durs ont grimpé de 46 % en quatre mois selon Club386, une carte QNAP 4 baies diskless dépasse maintenant les 695 € chez les revendeurs allemands, et les inquiétudes sur la confidentialité des données hébergées chez des tiers ne faiblissent pas. Résultat : de plus en plus de particuliers et de petites structures en France et en Europe se tournent vers le NAS DIY (fait maison) plutôt que vers un boîtier Synology ou QNAP tout prêt. Le logiciel qui domine cette scène s’appelle TrueNAS, et son nom a changé en 2026 : ce qu’on appelait TrueNAS SCALE est devenu **TrueNAS Community Edition**. Signe de cette popularité grandissante, la branche TrueNAS 25.10 a franchi la barre des 100 000 déploiements actifs dès février 2026, selon les chiffres communiqués par iXsystems. Ce guide vous montre, en 13 étapes concrètes, comment transformer un PC ou un mini-ITX en serveur de stockage ZFS complet avec TrueNAS Community Edition 25.10.6 « Goldeye », la version stable au 22 août 2026.

On y couvre le choix du matériel, l’installation, la création du pool ZFS, les partages réseau, les snapshots, les applications Docker comme Plex ou Nextcloud, la sécurisation de l’accès et un vrai plan de sauvegarde. On termine avec les pièges classiques, un dépannage en 8 points et un comparatif de coûts face à Synology et QNAP.

## Qu’est-ce que TrueNAS Community Edition en 2026 ?

Avant de commencer, il faut clarifier un point qui perd beaucoup de monde en 2026 : TrueNAS SCALE n’existe plus sous ce nom. Avec la sortie de TrueNAS 25.04 « Fangtooth » en avril 2025, iXsystems a fusionné ses gammes et rebaptisé SCALE en **TrueNAS Community Edition (CE)**. Cette branche a ensuite reçu une première mise à jour de maintenance, la 25.04.1, dès mai 2025, tandis que l’ancienne branche 24.10 « Electric Eel » recevait sa dernière mouture terminale, la 24.10.2.4, en août 2025, avant d’être définitivement abandonnée au profit de la nouvelle génération. La page produit officielle le confirme noir sur blanc : « TrueNAS SCALE is now officially known as TrueNAS Community Edition. » L’ancien nom SCALE ne survit plus que dans un contexte historique, y compris sur les forums où d’anciens utilisateurs demandent encore pourquoi leur installation « SCALE » s’affiche désormais comme « Community Edition ».

Concrètement, TrueNAS Community Edition reste un système d’exploitation basé sur Debian Linux, gratuit et open source, conçu pour transformer n’importe quel PC x86-64 en serveur de stockage ZFS. Il coexiste avec TrueNAS Enterprise, la version payante vendue avec le support commercial d’iXsystems et destinée aux entreprises. Les deux partagent le même socle logiciel : Community Edition sert de base gratuite pour les particuliers, les homelabs et les petites structures, tandis qu’Enterprise ajoute le support et des fonctionnalités de scalabilité.

La branche majeure 25.10 « Goldeye » est passée en disponibilité générale dès octobre 2025, avant de recevoir une mise à jour 25.10.1 en décembre 2025, puis une 25.10.2 en février 2026 qui a corrigé plus de 100 bugs selon iXsystems. La version stable au moment de la rédaction est **TrueNAS 25.10.6 « Goldeye »**, sortie le 12 août 2026. Cette mise à jour de maintenance corrige des failles de sécurité dans le noyau Linux et le pilote NVIDIA GPU, selon les notes de version officielles. Une branche 26.0.0 existe déjà en version bêta (BETA.3 au 20 août 2026), mais elle n’est pas destinée à la production : pour ce tutoriel, on utilise exclusivement la branche 25.10 stable.

## Pourquoi construire un NAS DIY plutôt qu’acheter un Synology ou QNAP ?

La réponse courte : le contrôle et le rapport performance/prix. Un NAS DIY sous TrueNAS Community Edition tourne sur du matériel PC standard, ce qui veut dire que vous choisissez vous-même le processeur, la quantité de RAM, le nombre de baies et le type de réseau (2,5 GbE ou 10 GbE) sans être limité aux specs figées d’un boîtier fermé. Sur les forums TrueNAS, le blogueur homelab Brian Moses a documenté en détail sa propre construction dans un billet largement cité dans la communauté : « With drives, my out of pocket cost was a little over $1,750 and without drives the price comes in at right around $617 », précise-t-il à propos de sa configuration compacte à 5 baies avec réseau 10GbE.

À titre de comparaison, un NAS Synology 4 baies diskless démarre autour de 659 € chez les revendeurs allemands (idealo.de, août 2026), et un QNAP TS-464-8G équivalent se négocie à partir de 695,26 €, disques non inclus dans les deux cas. Le DIY ne gagne pas systématiquement sur le prix pur (une carte mère mini-ITX + boîtier + alimentation peut coûter cher aussi), mais il gagne sur la flexibilité : upgrade de RAM à volonté, choix libre des disques, accès root complet, et surtout la possibilité de faire tourner des containers Docker sans les limitations d’un NAS propriétaire.

Il y a aussi un argument de résilience aux prix. Selon Club386, les prix des disques durs ont grimpé de 46 % en seulement quatre mois début 2026, un Toshiba MG11ACA 24 To passant de 386,09 € à 602,00 €. Avec un NAS DIY, vous pouvez ajouter des disques progressivement, au fil des baisses de prix, plutôt que d’acheter un pack complet imposé par le constructeur.

Pour les foyers et les indépendants en France, il existe un troisième argument, plus discret mais tout aussi réel : la question de la localisation des données. Un NAS DIY reste physiquement chez vous, sous votre propre toit, ce qui évite de dépendre des conditions générales d’un cloud tiers pour vos photos de famille, vos documents administratifs ou vos sauvegardes professionnelles. Ce n’est pas un argument juridique en soi, mais c’est une garantie pratique que personne d’autre ne peut modifier unilatéralement les conditions d’accès à vos fichiers du jour au lendemain.

Enfin, un NAS DIY sous TrueNAS s’adapte à des usages que la plupart des boîtiers grand public ne couvrent pas nativement : héberger un serveur Plex qui transcode plusieurs flux 4K en simultané, faire tourner un petit cluster de VM pour tester des configurations, ou servir de cible de sauvegarde pour tout un parc de PC via Time Machine (macOS) ou Veeam Community Edition (Windows). Ces usages restent possibles sur un Synology ou un QNAP haut de gamme, mais à un coût d’achat souvent double, pour une puissance CPU comparable à une carte mini-ITX à 150 €.

## Prérequis : matériel, logiciel et budget avant de commencer

Avant de sortir le tournevis, voici ce qu’il vous faut réellement. Cette liste correspond aux exigences officielles du guide matériel de TrueNAS, complétées par les recommandations pratiques de la communauté homelab.

- Un processeur x86-64 (Intel ou AMD), 2 cœurs minimum selon la doc officielle, mais 4 cœurs recommandés si vous comptez lancer des applications Docker
- 8 Go de RAM minimum pour une installation basique jusqu’à 8 disques (+1 Go par disque supplémentaire selon iXsystems), 16 Go pour un usage réel, 32 Go ou plus si vous prévoyez des VM ou plusieurs containers
- Un support d’amorçage dédié : SSD ou clé USB de 16 à 20 Go minimum (un SSD SATA ou NVMe de 120 Go est largement suffisant et plus fiable dans la durée)
- Au moins deux disques identiques pour créer un premier pool de stockage ZFS (HDD ou SSD selon l’usage)
- Une carte mère avec suffisamment de ports SATA ou un contrôleur HBA en mode passthrough (IT mode) pour plus de baies
- Une clé USB de 8 Go minimum pour créer le support d’installation
- Un deuxième ordinateur pour télécharger l’image ISO et accéder à l’interface web de TrueNAS pendant la configuration
- Une connexion Ethernet filaire (le Wi-Fi n’est pas recommandé ni officiellement supporté pour le stockage principal)

Côté budget, comptez large. D’après les relevés de prix idealo.de et Amazon.fr/de au 15 août 2026, un disque dur NAS/entreprise coûte en moyenne 48 à 50 €/To, avec quelques promotions ponctuelles descendant vers 25 €/To sur de gros volumes externes (comme un WD Elements 22 To à 549 €). Une carte mère mini-ITX avec processeur Intel N100 intégré, 6 ports SATA et réseau 2,5 GbE se trouve entre 130 € et 210 € selon la marque (ASRock N100DC-ITX autour de 130 $, cartes Topton/SZBOX entre 139 € et 206 €). Les modèles à base d’Intel N150, plus récents, montent plutôt entre 180 € et 280 €.

| Composant | Option d’entrée de gamme | Option recommandée 2026 | Prix approximatif | 
|---|---|---|---|
| Carte mère + CPU | Intel N100 mini-ITX (ASRock N100DC-ITX) | Intel N150 mini-ITX 6 baies SATA (Topton N18) | 130 € – 280 € | 
| RAM | 8 Go DDR4/DDR5 | 32 Go DDR5 (pour ARC et apps Docker) | 35 € – 110 € | 
| Disque de démarrage | Clé USB 32 Go | SSD SATA 120-256 Go | 15 € – 25 € | 
| Stockage principal (par To) | HDD grand public | HDD NAS/entreprise (WD Red Pro, Seagate IronWolf Pro) | 48 € – 50 €/To | 
| Boîtier | Boîtier mini-ITX 4 baies | Boîtier NAS 6-8 baies avec ventilation dédiée | 70 € – 180 € | 
| Alimentation | SFX 300-450W 80+ Bronze | SFX 450-550W 80+ Gold | 50 € – 100 € | 

## Étape 1 : Choisir le processeur et la carte mère

Le choix du processeur détermine tout le reste de la construction. Pour un usage NAS pur (stockage, partages SMB/NFS, sauvegardes), un Intel N100 suffit largement : il consomme peu (6W de TDP), reste silencieux avec un simple dissipateur passif et gère sans problème le chiffrement AES-NI nécessaire au chiffrement des pools ZFS. Si vous prévoyez de transcoder de la vidéo avec Plex ou Jellyfin, ou de faire tourner plusieurs containers Docker en parallèle (Nextcloud, Immich, Home Assistant), montez plutôt vers un Intel N150 ou un Core i3-N305, qui offre 8 cœurs contre 4 pour le N100.

Sur le choix de la carte mère, privilégiez les modèles mini-ITX orientés NAS qui intègrent directement 6 à 8 ports SATA III, comme les cartes Topton N18 ou les modèles ASRock équivalents. Elles évitent d’avoir à acheter une carte contrôleur HBA séparée. Vérifiez systématiquement trois points avant l’achat : la présence d’un port réseau 2,5 GbE minimum (le 10 GbE reste un vrai plus si votre switch le supporte), un slot M.2 NVMe pour le cache ou le disque de démarrage, et la compatibilité mémoire DDR5 si vous visez 32 Go ou plus.

## Étape 2 : Dimensionner la RAM et le stockage de démarrage

Le guide matériel officiel de TrueNAS fixe la barre basse à 8 Go de RAM pour une installation basique avec jusqu’à 8 disques, avec 1 Go supplémentaire par disque au-delà. C’est un minimum technique pour que le système démarre et fonctionne, pas une recommandation pour un usage confortable. En pratique, la communauté homelab et plusieurs guides matériels 2026 convergent vers 16 Go comme plancher réaliste pour un NAS familial, et 32 Go ou plus si vous comptez sauvegarder des VM via iSCSI ou faire tourner plusieurs applications Docker simultanément.

### Le rôle du cache ARC dans ZFS

ZFS, le système de fichiers utilisé par TrueNAS, exploite la RAM disponible comme cache de lecture appelé ARC (Adaptive Replacement Cache). Plus vous avez de RAM, plus ZFS peut garder de données fréquemment consultées en mémoire, ce qui réduit les accès disque et accélère les lectures. Ce n’est pas un luxe : sur un NAS avec seulement 8 Go, l’ARC dispose de très peu de marge une fois le système d’exploitation et les services lancés, ce qui dégrade sensiblement les performances dès que plusieurs utilisateurs accèdent au NAS en même temps.

Pour le disque de démarrage, TrueNAS impose un minimum de 16 à 20 Go selon la version. Évitez la clé USB en usage permanent : elle fonctionne pour l’installation, mais sa durée de vie en écriture continue est nettement inférieure à celle d’un SSD SATA ou NVMe bon marché, qui coûte aujourd’hui entre 15 € et 25 € pour 120 à 256 Go. C’est un des postes de dépense les plus rentables de toute la construction.

## Étape 3 : Choisir la topologie ZFS – RAIDZ1, RAIDZ2 ou Mirror

C’est la décision la plus structurante de tout le projet, car elle détermine votre capacité utile, votre tolérance aux pannes et vos possibilités d’extension future. ZFS propose trois topologies de vdev principales, chacune avec un nombre minimum de disques imposé par la documentation officielle.

| Topologie | Disques minimum | Tolérance de panne | Capacité utile (avec 4 disques de 4 To) | Cas d’usage recommandé | 
|---|---|---|---|---|
| Mirror (miroir) | 2 | 1 disque par paire | 8 To (sur 2 paires) | Lectures aléatoires rapides, bases de données, VM | 
| RAIDZ1 | 3 | 1 disque | 12 To | Stockage familial, budget serré, disques ≤ 8 To | 
| RAIDZ2 | 4 | 2 disques | 8 To | Données critiques, disques > 8 To, plus de 6 disques | 

La documentation ZFS officielle est claire sur la hiérarchie de fiabilité : RAIDZ2 offre une meilleure disponibilité des données et un MTTDL (temps moyen avant perte de données) nettement supérieur à RAIDZ1, car il tolère la perte simultanée de deux disques. En contrepartie, un mirror reste la topologie la plus performante pour les charges de lecture aléatoire, en particulier sur des workloads qu’on ne peut pas mettre en cache. Le livre blanc TrueNAS sur les layouts ZFS recommande de rester entre 3 et 9 disques par vdev, et déconseille formellement de dépasser 12 disques dans un seul vdev, quelle que soit la topologie choisie.

Pour un premier NAS familial avec 4 à 6 disques de 8 à 16 To, RAIDZ2 reste le choix le plus raisonnable en 2026 : avec la hausse des capacités unitaires, le temps de reconstruction (resilver) après panne s’allonge, et RAIDZ1 devient risqué puisqu’une seconde panne pendant la reconstruction entraîne une perte totale du pool.

## Étape 4 : Préparer la clé USB et installer TrueNAS 25.10.6 Goldeye

Téléchargez l’image ISO officielle depuis la page de téléchargement TrueNAS (25.10.6 pour la branche stable, évitez la 26.0.0-BETA.3 tant qu’elle reste en bêta). Si vous partez d’une installation existante encore sur l’ancienne branche 25.04, sachez qu’elle a reçu deux dernières mises à jour de maintenance, la 25.04.2.4 en septembre 2025 puis la 25.04.2.6 en octobre 2025, avant qu’iXsystems ne recommande officiellement de migrer vers la branche 25.10.x. Utilisez un outil comme Balena Etcher ou Rufus pour flasher l’ISO sur une clé USB de 8 Go minimum. En ligne de commande sous Linux ou macOS, la méthode `dd` reste la plus directe :

```
# Identifier la clé USB (attention à bien cibler le bon disque)
lsblk
# Flasher l'image ISO sur la clé USB (remplacez /dev/sdX par votre clé)
sudo dd if=TrueNAS-25.10.6.iso of=/dev/sdX bs=4M status=progress conv=fsync
# Vérifier le checksum SHA256 de l'ISO avant de flasher
sha256sum TrueNAS-25.10.6.iso
```
Une fois la clé prête, branchez-la sur votre futur NAS, entrez dans le BIOS/UEFI (touche Suppr ou F2 selon la carte mère), désactivez le Secure Boot s’il pose problème au démarrage, et sélectionnez la clé USB comme périphérique de boot. L’installeur TrueNAS se lance en mode texte : choisissez le disque de démarrage (jamais un disque que vous comptez utiliser pour le stockage de données), définissez le mot de passe root, et laissez l’installation se terminer. Redémarrez ensuite en retirant la clé USB.

## Étape 5 : Configuration réseau et première connexion à l’interface web

Au premier démarrage, la console texte de TrueNAS affiche l’adresse IP attribuée par DHCP sur votre réseau local. Notez-la, puis ouvrez un navigateur depuis un autre PC connecté au même réseau et rendez-vous sur cette adresse en HTTPS. Le certificat sera auto-signé au départ, votre navigateur affichera donc un avertissement de sécurité : c’est normal, acceptez l’exception temporairement (on réglera un vrai certificat à l’étape sécurité).

Connectez-vous avec le compte root et le mot de passe défini pendant l’installation. Dans le menu Network, configurez une adresse IP statique plutôt que le DHCP par défaut : un NAS qui change d’adresse IP à chaque redémarrage du routeur casse tous vos partages réseau et vos scripts de sauvegarde. Réservez l’adresse via votre box/routeur (réservation DHCP par adresse MAC) plutôt que de la figer en dur dans TrueNAS, ce qui évite les conflits en cas de changement de réseau.

## Étape 6 : Créer votre premier pool de stockage ZFS

Rendez-vous dans Storage puis Pools et cliquez sur Create Pool. L’assistant graphique liste tous les disques physiques détectés. Sélectionnez ceux que vous voulez inclure (jamais le disque de démarrage), choisissez la topologie décidée à l’étape 3 (RAIDZ2 dans notre exemple), nommez votre pool (par exemple « tank », convention historique héritée de ZFS/Solaris) et validez. TrueNAS initialise le pool en quelques secondes, contrairement à un RAID matériel classique qui peut prendre des heures.

Une fois le pool créé, planifiez immédiatement un scrub périodique (Data Protection > Scrub Tasks). Le scrub relit l’intégralité des données du pool et corrige les erreurs de checksum détectées grâce à la redondance ZFS, un peu comme un contrôle technique régulier. La fréquence recommandée par la documentation communautaire tourne autour d’une fois par mois pour un usage domestique, et toutes les deux semaines pour un pool qui stocke des données professionnelles critiques. Un scrub sur un pool de 16 To avec des disques mécaniques classiques prend généralement entre 6 et 12 heures, pendant lesquelles les performances du NAS baissent légèrement mais restent utilisables.

Avant de charger vos données, lancez un test de santé sur chaque disque neuf avec `badblocks` ou un test S.M.A.R.T. long via l’interface TrueNAS (Storage > Disks > sélectionner le disque > Manual Test > Long). Cette étape prend plusieurs heures sur des disques de 10 To et plus, mais elle révèle les disques défectueux avant qu’ils ne contiennent des données critiques.

```
# Vérifier l'état du pool ZFS après création
zpool status tank
# Exemple de sortie attendue pour un pool RAIDZ2 sain
  pool: tank
 state: ONLINE
config:
        NAME        STATE     READ WRITE CKSUM
        tank        ONLINE       0     0     0
          raidz2-0  ONLINE       0     0     0
            sda     ONLINE       0     0     0
            sdb     ONLINE       0     0     0
            sdc     ONLINE       0     0     0
            sdd     ONLINE       0     0     0
errors: No known data errors
```
## Étape 7 : Configurer les partages SMB, NFS et les utilisateurs

Avec le pool créé, il faut maintenant définir des datasets (des sous-volumes du pool) et les partager sur le réseau. Dans Datasets, créez-en un par usage : « documents », « medias », « backups », par exemple. Chaque dataset peut avoir ses propres réglages de compression, quotas et permissions, ce qui évite qu’un usage sature tout le pool.

Pour partager un dataset avec des machines Windows ou macOS, utilisez SMB (menu Shares > Windows Shares SMB). Pour des machines Linux ou des NAS Synology/QNAP en réplication, NFS reste plus léger et plus rapide. Créez d’abord des comptes utilisateurs dédiés dans Credentials > Local Users : évitez d’utiliser le compte root pour l’accès quotidien aux partages, une mauvaise pratique de sécurité classique.

```
# Monter un partage NFS TrueNAS depuis un client Linux
sudo mount -t nfs 192.168.1.50:/mnt/tank/medias /mnt/nas-medias
# Ajouter le montage de façon permanente dans /etc/fstab
192.168.1.50:/mnt/tank/medias  /mnt/nas-medias  nfs  defaults  0  0
```
## Étape 8 : Automatiser les snapshots et la réplication

Un des plus gros avantages de ZFS face à un système de fichiers classique, ce sont les snapshots quasi instantanés et sans coût de performance notable. Dans Data Protection > Periodic Snapshot Tasks, planifiez une tâche par dataset important, avec une rétention adaptée : par exemple des snapshots horaires conservés 24h, des snapshots journaliers conservés 30 jours, et des snapshots mensuels conservés un an pour les données sensibles.

Les snapshots protègent contre la suppression accidentelle ou le ransomware, mais pas contre une panne matérielle totale du NAS lui-même : ils vivent sur le même pool. Pour une vraie protection, configurez une réplication ZFS (Data Protection > Replication Tasks) vers un second TrueNAS, un NAS distant, ou un service cloud compatible S3. C’est le seul mécanisme qui vous protège si le boîtier entier tombe en panne, brûle ou se fait voler.

## Étape 9 : Installer des applications Docker (Plex, Nextcloud, Immich)

TrueNAS Community Edition intègre depuis la version 24.10 un nouveau backend d’applications basé sur Docker, qui a remplacé l’ancien système Kubernetes des premières versions de SCALE. La migration s’est faite automatiquement pour les utilisateurs qui mettaient à jour depuis une version antérieure. La documentation officielle décrit ce système comme l’« Applications Market », accessible depuis le menu Apps, avec un catalogue par défaut baptisé « TRUENAS » qui regroupe des applications validées par iXsystems : Plex, Nextcloud, MinIO, Immich, Home Assistant et bien d’autres.

Comme le résume l’article de Computing for Geeks consacré aux options de NAS DIY : « TrueNAS is the free Community Edition, formerly called SCALE, is built on ZFS », une distinction utile à garder en tête face à des concurrents comme Unraid, qui n’utilisent pas ZFS en natif.

### Le catalogue TrueNAS Apps

Pour installer une application, allez dans Apps > Discover Apps, choisissez par exemple Nextcloud, et configurez le chemin de stockage vers un dataset dédié (créez un dataset « apps » séparé de vos données personnelles). L’interface graphique gère le déploiement du container, l’attribution du port réseau et les volumes de stockage sans passer par la ligne de commande. Pour des besoins plus personnalisés, TrueNAS accepte aussi l’import de fichiers docker-compose classiques :

```
# Exemple simplifié d'un fichier compose pour une app custom sur TrueNAS
version: "3.9"
services:
  monapp:
    image: monimage/exemple:latest
    ports:
      - "8080:80"
    volumes:
      - /mnt/tank/apps/monapp:/config
    restart: unless-stopped
```
## Étape 10 : Sécuriser l’accès à votre NAS

Un NAS mal sécurisé exposé sur Internet est une cible facile. Trois réglages sont non négociables. D’abord, activez l’authentification à deux facteurs (2FA) sur le compte root et tous les comptes admin, via System Settings > General > Two-Factor Authentication. Ensuite, remplacez le certificat auto-signé par un certificat valide : TrueNAS supporte l’intégration ACME (Let’s Encrypt) directement dans System Settings > Certificates, à condition d’avoir un nom de domaine pointant vers votre NAS.

Enfin, ne jamais exposer directement l’interface web de TrueNAS sur Internet en redirigeant un port depuis votre box. Si vous avez besoin d’un accès distant, passez par un VPN vers votre réseau local (WireGuard, disponible directement via une app TrueNAS, ou un routeur compatible), ou par un reverse proxy avec authentification renforcée. Vérifiez également que le pare-feu de votre routeur bloque bien tout trafic entrant non sollicité vers l’IP du NAS.

## Étape 11 : Sauvegardes 3-2-1 et test de restauration

Un pool RAIDZ2 protège contre la panne de disque, pas contre un incendie, un vol, une erreur humaine ou un ransomware qui chiffre vos fichiers avant que le snapshot suivant ne se déclenche. La règle 3-2-1 reste la référence : 3 copies de vos données, sur 2 supports différents, dont 1 copie hors site. Concrètement pour un NAS TrueNAS : les données originales sur le pool ZFS, une réplication vers un second NAS ou un disque externe, et une copie chiffrée envoyée vers un stockage cloud compatible S3 (Backblaze B2, Wasabi, ou équivalent).

```
# Exemple de synchronisation manuelle vers un stockage distant compatible S3 avec rclone
rclone sync /mnt/tank/documents remote-backup:mon-bucket/documents \
  --transfers 4 --checksum --log-file=/var/log/rclone-backup.log
# Vérifier la dernière exécution planifiée dans les logs
tail -n 50 /var/log/rclone-backup.log
```
Le point que presque tout le monde néglige : personne ne teste jamais la restauration avant d’en avoir réellement besoin. Planifiez au moins une fois par trimestre une restauration test d’un snapshot ou d’une sauvegarde distante vers un dossier temporaire, pour vérifier que les fichiers restaurés sont bien intacts et lisibles. Une sauvegarde jamais testée n’est qu’une hypothèse.

## Étape 12 : Cas d’usage concrets pour exploiter votre NAS

Une fois l’infrastructure de base en place (pool, partages, snapshots, sauvegardes), l’intérêt réel d’un NAS DIY se révèle dans les usages qu’il permet d’empiler sur la même machine. Voici quatre scénarios concrets, testés par la communauté homelab, qui montrent pourquoi TrueNAS dépasse largement le simple stockage de fichiers.

- **Serveur multimédia centralisé.** Installez Plex ou Jellyfin depuis le catalogue d’applications, pointez la bibliothèque vers un dataset dédié, et diffusez votre collection de films et séries vers vos téléviseurs, consoles et smartphones sans dépendre d’un abonnement de streaming. Sur un Intel N150, le transcodage logiciel gère confortablement un flux 1080p, mais un flux 4K HDR demande plutôt un CPU avec un iGPU capable de décodage matériel (Quick Sync), à vérifier avant l’achat de la carte mère.
- **Sauvegarde centralisée du parc familial ou professionnel.** Configurez Time Machine sur les Mac (TrueNAS expose nativement un partage SMB compatible Time Machine) et un outil comme Veeam Community Edition ou Duplicati sur les PC Windows, tous pointant vers des datasets séparés sur le même pool. Un seul NAS peut ainsi absorber les sauvegardes de toute une maisonnée ou d’une petite équipe.
- **Photothèque privée avec reconnaissance IA.** Immich, disponible dans le catalogue TrueNAS Apps, reproduit l’essentiel des fonctionnalités de Google Photos (tri automatique, reconnaissance de visages, carte des lieux) mais garde toutes les photos sur votre pool ZFS local plutôt que sur un cloud tiers.
- **Bac à sable pour l’apprentissage DevOps.** TrueNAS Community Edition supporte les machines virtuelles en plus des containers Docker. C’est un terrain d’entraînement réaliste pour tester des déploiements Kubernetes, des bases de données ou des pipelines CI/CD sans payer d’instance cloud à l’heure.

## 5 erreurs courantes à éviter lors d’un montage NAS DIY

- **Mélanger des disques de tailles ou d’âges différents dans un même vdev.** ZFS aligne la capacité utile sur le plus petit disque du vdev, et des disques achetés en même temps ont statistiquement plus de risques de tomber en panne au même moment (défaut de lot).
- **Utiliser le disque de démarrage comme espace de stockage de données.** C’est une erreur fréquente chez les débutants qui veulent économiser un port SATA : en cas de panne du disque système, vous perdez la configuration ET les données au lieu de perdre uniquement la configuration.
- **Sous-dimensionner la RAM en pensant que 8 Go officiels suffisent à un usage réel.** C’est un minimum de démarrage, pas une recommandation de confort, surtout dès que vous ajoutez des apps Docker.
- **Négliger le test S.M.A.R.T. long des disques neufs avant mise en production.** Un disque neuf défectueux se révèle souvent dans les 48 premières heures d’utilisation intensive (courbe de mortalité infantile), autant le savoir avant d’y stocker des données importantes.
- **Confondre snapshots et sauvegardes.** Un snapshot protège contre l’erreur humaine sur le même pool, pas contre la perte du NAS entier. Sans réplication ou copie hors site, vous n’avez toujours qu’une seule copie physique de vos données.

## Dépannage : 8 problèmes fréquents et leurs solutions

1. **L’interface web reste inaccessible après l’installation.** Vérifiez que le câble Ethernet est bien branché avant le démarrage (TrueNAS ne détecte pas toujours une interface branchée à chaud) et confirmez l’adresse IP via la console physique du serveur.
2. **Le pool ZFS affiche l’état DEGRADED.** Un disque a été marqué défaillant. Identifiez-le avec `zpool status -v`, remplacez-le physiquement, puis lancez `zpool replace tank ancien-disque nouveau-disque` pour déclencher le resilver.
3. **Les transferts SMB sont anormalement lents.** Testez d’abord la bande passante réseau brute avec iperf3 entre le client et le NAS avant de blâmer TrueNAS : un câble Cat5e ou un switch limité à 1 GbE plafonne tout, quelle que soit la config logicielle.
4. **Une application Docker refuse de démarrer après une mise à jour.** Consultez les logs du container depuis Apps > sélectionner l’app > Logs, et vérifiez en priorité les chemins de volumes : une mise à jour de TrueNAS peut parfois modifier les points de montage attendus.
5. **Le pool ne monte plus après un redémarrage soudain (coupure de courant).** ZFS est journalisé (copy-on-write) et résiste bien aux coupures brutales, mais lancez un `zpool scrub tank` dès que possible pour vérifier l’intégrité et corriger d’éventuelles erreurs de checksum.
6. **Le certificat ACME/Let’s Encrypt ne se renouvelle pas automatiquement.** Vérifiez que le port 80 (validation HTTP-01) reste bien accessible depuis Internet vers votre NAS au moment du renouvellement, ou basculez vers une validation DNS-01 si votre registrar le supporte.
7. **Le NAS surchauffe ou les ventilateurs tournent à plein régime en permanence.** Vérifiez le mapping des courbes de ventilation dans le BIOS et la propreté des filtres à poussière : un boîtier NAS mal ventilé fait grimper la température des disques au-delà de 40°C, ce qui réduit leur durée de vie.
8. **Impossible de se connecter en SSH après activation du firewall intégré.** Vérifiez dans System Settings > Services que le service SSH est bien démarré ET que la règle de firewall autorise le port 22 depuis votre plage d’IP locale, les deux réglages sont indépendants.

## Astuces avancées pour optimiser les performances de votre NAS

Une fois le NAS stable, plusieurs réglages font une vraie différence sur la durée. Activez la compression LZ4 par défaut sur tous vos datasets sauf ceux contenant déjà des fichiers compressés (vidéos, archives) : LZ4 est quasiment gratuit en CPU et réduit l’espace occupé par les documents, logs et bases de données de 20 à 50 % selon le type de fichier. Pour les datasets contenant des VM ou des bases de données avec beaucoup d’écritures aléatoires, envisagez un cache SLOG (ZFS Intent Log) sur un SSD NVMe rapide et à faible latence, séparé du pool principal.

### Consommation électrique et undervolting

Un NAS tourne en général 24h/24, ce qui rend chaque watt économisé significatif sur la facture annuelle. Sur une plateforme Intel N100/N150, activez les états d’économie d’énergie C-states dans le BIOS et vérifiez que le spin-down des disques inactifs est configuré dans Storage > Disks (mais évitez un spin-down trop agressif sur un pool RAIDZ, qui use prématurément les têtes de lecture par des cycles marche/arrêt répétés). Un serveur bien réglé à base de N100 consomme généralement entre 15 et 30 watts au repos, disques inclus, ce qui représente une facture annuelle très raisonnable comparée à un NAS 8 baies haut de gamme qui peut dépasser 60 watts en usage courant.

Enfin, si votre carte mère et votre switch le supportent, passez au réseau 2,5 GbE minimum, voire 10 GbE si votre budget le permet. Un lien Gigabit standard plafonne à environ 118 Mo/s en pratique, ce qui devient le vrai goulot d’étranglement dès que votre pool RAIDZ2 est capable de débits séquentiels supérieurs à 300-400 Mo/s avec des disques mécaniques modernes.

Si votre carte mère embarque plusieurs ports Ethernet mais pas de 10 GbE natif, l’agrégation de liens (LACP) reste une alternative économique : en combinant deux ports 2,5 GbE dans Network > Link Aggregation, vous approchez un débit cumulé plus élevé pour les transferts simultanés de plusieurs clients, à condition que votre switch supporte lui aussi le LACP sur les mêmes ports. Ce n’est pas équivalent à un vrai 10 GbE pour un transfert unique volumineux, mais ça aide beaucoup en usage multi-utilisateurs.

## TrueNAS DIY vs Synology vs QNAP : le vrai coût en 2026

Pour trancher objectivement, voici une comparaison à capacité équivalente, environ 16 To utiles en RAIDZ2/RAID équivalent, basée sur les prix relevés mi-août 2026.

| Solution | Prix boîtier/matos (diskless) | CPU | RAM incluse | Apps Docker natives | Garantie/support | 
|---|---|---|---|---|---|
| NAS DIY + TrueNAS CE | ~350 € – 500 € (carte, boîtier, alim, RAM) | Au choix (N100 à N305) | Au choix, 8 à 64 Go | Oui, catalogue complet | Communauté / forums | 
| Synology 4 baies (ex. DS925+) | À partir de 659 € | AMD Ryzen V1500B (selon modèle) | 4 Go (extensible) | Limité, écosystème propriétaire | Garantie constructeur 2-3 ans | 
| QNAP TS-464-8G | À partir de 695,26 € | Intel (selon modèle) | 8 Go | Oui via Container Station | Garantie constructeur 2-3 ans | 

Le DIY reste généralement moins cher à specs équivalentes, surtout si vous montez au-delà de 16 Go de RAM (où les boîtiers propriétaires facturent cher chaque upgrade). Mais Synology et QNAP gagnent nettement sur la simplicité de prise en main et le support constructeur direct en cas de panne matérielle. Le DIY suppose d’accepter de gérer soi-même le dépannage, avec l’aide des forums communautaires TrueNAS, très actifs (plusieurs centaines de messages par semaine sur les annonces de version selon les statistiques publiques du forum officiel).

## Foire aux questions

**TrueNAS Community Edition est-il vraiment gratuit ?**

Oui, entièrement. C’est un logiciel open source sous licence GPL-3.0 pour la partie communautaire. Seule la version Enterprise, vendue avec le support commercial d’iXsystems et des appliances matérielles dédiées, est payante.

**Faut-il connaître Linux pour installer TrueNAS ?**

Non. L’installation se fait via un assistant texte guidé, et toute la configuration quotidienne (pools, partages, apps, snapshots) passe par l’interface web graphique. Des bases en ligne de commande aident pour le dépannage avancé, mais ne sont pas indispensables au démarrage.

**Peut-on utiliser un vieux PC pour faire tourner TrueNAS ?**

Oui, tant qu’il dispose d’un processeur 64 bits, d’au moins 8 Go de RAM et de ports SATA suffisants. Attention toutefois à la consommation électrique d’un vieux PC de bureau, souvent bien supérieure à celle d’une carte mini-ITX moderne à base de N100, ce qui pèse sur la facture d’électricité vu qu’un NAS tourne en continu.

**Quelle est la différence entre TrueNAS CORE et TrueNAS Community Edition ?**

TrueNAS CORE était la branche historique basée sur FreeBSD. Depuis la fusion des gammes en 2025, TrueNAS Community Edition (basé sur Debian Linux, anciennement SCALE) est devenu la version phare recommandée pour toute nouvelle installation, notamment parce qu’elle supporte nativement les containers Docker et les VM, contrairement à CORE.

**Combien de temps prend l’installation complète ?**

Comptez environ 30 minutes pour l’installation du système lui-même, et 90 à 120 minutes au total en intégrant le montage physique du matériel, la création du pool, les premiers partages et les tests S.M.A.R.T. de base (hors test long complet, qui tourne en arrière-plan pendant plusieurs heures).

**RAIDZ1 est-il obsolète en 2026 ?**

Pas obsolète, mais de moins en moins recommandé à mesure que les capacités de disques augmentent. Avec des disques de 12 To et plus, le temps de reconstruction (resilver) après une panne s’allonge, augmentant le risque qu’une seconde panne survienne avant la fin du processus. RAIDZ2 ou le mirror restent plus sûrs pour les configurations avec de gros disques.

**Peut-on migrer facilement de TrueNAS CORE vers Community Edition ?**

iXsystems propose un chemin de migration documenté, mais il implique de réinstaller le système sur la branche Linux et d’importer le pool ZFS existant (les pools ZFS restent compatibles entre les deux branches). Sauvegardez systématiquement votre configuration avant toute migration.

**TrueNAS fonctionne-t-il correctement en Wi-Fi ?**

Ce n’est ni recommandé ni officiellement supporté pour un usage NAS sérieux. Le stockage réseau dépend d’une latence stable et d’un débit soutenu que le Wi-Fi peine à garantir, sans compter les micro-coupures qui peuvent corrompre des transferts en cours. Une connexion Ethernet filaire reste la seule option fiable.

### Contenus associés

Pour suivre l’actualité complète du matériel PC, des puces IA et des tendances hardware 2026, consultez notre dossier hardware et puces IA.

Sources officielles : notes de version TrueNAS 25.10.6, guide matériel officiel TrueNAS, page de téléchargement TrueNAS, fiche Wikipédia TrueNAS, et l’analyse Club386 sur la hausse des prix des disques durs.