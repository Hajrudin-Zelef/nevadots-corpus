---
id: collect-261121-veille-ia/windows-os/nas-synology-configurer-dsm-en-12-etapes-2026
title: "NAS Synology : Configurer DSM en 12 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["synology", "disques", "systeme", "sauvegarde", "donnees", "configurer", "cloud", "baies"]
source: https://tech-insider.org/fr/nas-synology-dsm-configuration-2026/
source_anchor: ""
source_lines: [1, 272]
section: "Windows_OS"
---
Un NAS Synology reste allumé 24 heures sur 24 dans un coin du salon ou du bureau, souvent pendant des années sans qu’on y touche vraiment après l’installation initiale. C’est justement cette phase de démarrage qui coince le plus : entre le choix du système de fichiers, la configuration du volume, le paramétrage de QuickConnect et la mise en place d’une vraie stratégie de sauvegarde, il y a une bonne douzaine de décisions à prendre correctement dès le premier jour. Se tromper sur le système de fichiers ou sur le type de RAID, par exemple, oblige à tout reformater plus tard, ce qui veut dire réinjecter des téraoctets de données depuis zéro.

Ce tutoriel couvre la configuration complète d’un NAS Synology sous DSM, de la sortie de la boîte jusqu’à un système de sauvegarde automatisé et sécurisé, avec les pièges à éviter et les réglages qui comptent vraiment pour un usage domestique ou pour un petit bureau en France. Nous utilisons DSM 7.4.1‑90080 (la version générale la plus récente au 20 août 2026), point de mise à jour de la branche DSM 7.4 lancée le 16 juin 2026 avec l’assistant piloté par IA **DSM Agent** et la déduplication/compression a posteriori des volumes HDD, tandis que la branche de maintenance longue durée DSM 7.2.2‑72806 reste mentionnée à titre de repère pour les modèles plus anciens.

## Pourquoi configurer un NAS Synology en 2026

La question du stockage local revient en force depuis deux ans, portée par trois choses : la hausse des abonnements cloud grand public, les inquiétudes sur la localisation des données personnelles, et l’explosion des volumes de photos et vidéos 4K produits par les smartphones. Un NAS 2 baies comme le **Synology DS224+** se trouve autour de **400 à 430 € TTC** en France mi‑2026 (comparateurs 123Comparer et Labonneconfig, prix relevés en août 2026), disques durs non compris. Un modèle 4 baies comme le **DS423+** tourne plutôt entre **430 et 600 € TTC** pour le boîtier seul, jusqu’à plus de 900 € en kit avec disques.

Sur le papier, ça paraît cher face à 200 Go de stockage cloud à quelques euros par mois. Mais un NAS bien configuré remplace en même temps un service de sauvegarde photo, un serveur de fichiers partagé, un lecteur multimédia pour la télévision et, avec Synology Drive, une alternative à Google Drive ou OneDrive hébergée chez soi. Pour les foyers ou petites structures qui veulent garder la main sur leurs données plutôt que de dépendre d’un cloud tiers, l’angle rejoint directement les problématiques de souveraineté numérique face au cloud public déjà traitées sur ce site, et fait écho aux recommandations générales de la CNIL sur la minimisation des données confiées à des tiers.

Le NAS Synology occupe une place particulière dans ce paysage : DSM (DiskStation Manager) est une interface web complète, proche d’un vrai système d’exploitation, avec un catalogue d’applications (Paquets) qui couvre la sauvegarde, la synchronisation, la surveillance vidéo, le VPN ou même un serveur mail. C’est ce qui distingue Synology d’un NAS plus brut ou d’une solution auto-hébergée : l’installation initiale est guidée de bout en bout, ce qui réduit fortement le risque d’erreur pour un premier NAS.

## Prérequis matériels et logiciels

Avant de commencer, voici ce qu’il faut avoir sous la main. La configuration matérielle influence directement les performances et les options disponibles dans DSM, donc mieux vaut vérifier chaque point avant de sortir le tournevis.

| Élément | Recommandation | Détail | 
|---|---|---|
| Modèle NAS | DS224+ (2 baies) ou DS423+/DS923+ (4 baies) | DS923+ : CPU AMD Ryzen R1600, RAM ECC 4 Go extensible à 32 Go | 
| Disques durs | 2 minimum (idéalement identiques) | Disques NAS dédiés, pas des disques de bureau grand public | 
| Version DSM | DSM 7.4.1‑90080 ou plus récent | Publiée le 23 juillet 2026, branche générale | 
| Réseau | Câble Ethernet Cat 6 minimum | Le Wi-Fi fonctionne mais ralentit fortement les premiers transferts | 
| Ordinateur d’installation | Windows, macOS ou Linux | Navigateur à jour (Chrome, Edge ou Firefox) | 
| Compte Synology | Gratuit, créé pendant l’installation | Nécessaire pour QuickConnect et les notifications | 
| Onduleur (optionnel mais conseillé) | 400 VA minimum | Protège contre les coupures qui corrompent les volumes Btrfs | 

Un point souvent négligé : les disques durs. Un disque grand public conçu pour tourner quelques heures par jour dans un PC de bureau n’est pas fait pour fonctionner 24h/24 dans un NAS. Les fabricants proposent des gammes dédiées, plus tolérantes à la vibration et calibrées pour un fonctionnement continu. Bonne nouvelle pour les propriétaires de modèles récents : DSM 7.3, sorti en octobre 2025, a restauré la prise en charge des disques SATA tiers sur les DiskStation de cette génération, après les restrictions imposées un temps par Synology. Cela dit, utiliser des disques de bureau dans un NAS Synology fonctionne à court terme, mais les taux de panne grimpent nettement après 12 à 18 mois d’usage continu selon les retours d’expérience de la communauté Synology.

## Étape 1 : Préparer le matériel et insérer les disques

Éteignez le NAS avant toute manipulation, même si la plupart des modèles Synology récents supportent l’insertion à chaud. Sur un DS224+ ou un DS423+, les tiroirs de disques se retirent sans outil : un clic sur la languette latérale suffit à les libérer. Insérez le disque dans le tiroir, vissez-le sur les côtés (4 vis fournies) si vous utilisez un modèle sans fixation tool-less pour les baies 3,5 pouces, puis remettez le tiroir en place jusqu’au clic.

Sur les modèles Plus récents comme le DS423+, deux emplacements M.2 NVMe supplémentaires sont disponibles sous le boîtier, séparés des baies principales. Ces emplacements servent presque exclusivement au cache SSD (voir étape 9) et non à créer un volume de stockage principal — Synology limite volontairement cet usage sur l’entrée de gamme Plus. Si vous comptez utiliser un NVMe comme stockage de volume complet, vérifiez la compatibilité exacte du modèle avant achat, car cette fonctionnalité n’est pas activée sur tous les DS Plus.

Une fois les disques en place, branchez le câble Ethernet sur le port LAN1 (ou sur les deux ports si votre routeur supporte l’agrégation de liens), puis le câble d’alimentation. Appuyez sur le bouton d’alimentation pendant une seconde. Le voyant d’état clignote en bleu pendant l’amorçage, ce qui prend généralement 90 secondes à 3 minutes selon le modèle.

## Étape 2 : Trouver le NAS sur le réseau avec Synology Assistant

Depuis un ordinateur connecté au même réseau local, deux méthodes permettent de localiser le NAS fraîchement démarré. La première passe par le site **find.synology.com**, qui lance le Web Assistant directement dans le navigateur : il scanne le réseau local et affiche les NAS Synology détectés avec leur statut (“Non installé” pour un appareil neuf).

La seconde méthode utilise **Synology Assistant**, un utilitaire de bureau téléchargeable depuis le site Synology, qui scanne le réseau local en dehors du navigateur et permet ensuite de gérer plusieurs NAS depuis une interface centralisée. C’est l’outil à privilégier si vous gérez plusieurs appareils Synology ou si le Web Assistant ne détecte rien (pare-feu d’entreprise, réseau segmenté en VLAN, etc.).

**Point de sécurité important** : Synology a publié l’avis **Synology-SA-26:12** le 3 août 2026, concernant une faille CVE-2026-4793 dans Synology Assistant pour Windows, qui permettait à un utilisateur local de lire ou écrire des fichiers arbitraires et de provoquer un déni de service. Avant de télécharger l’outil, vérifiez que vous récupérez bien la version 7.0.7-50095 ou plus récente, uniquement depuis le site officiel Synology.

Si aucun outil n’est disponible, vous pouvez aussi accéder directement au NAS via son adresse IP locale, port 5000 en HTTP ou 5001 en HTTPS, par exemple :

```
http://192.168.1.100:5000
https://192.168.1.100:5001
```
L’adresse IP exacte se trouve dans l’interface d’administration de votre routeur (liste des appareils connectés), ou directement dans les résultats du scan Synology Assistant.

## Étape 3 : Installer DSM 7.4

Une fois le NAS détecté, cliquez sur “Configurer” ou “Installer” en suivant les mêmes étapes que le guide officiel d’installation de DSM. L’assistant propose de télécharger automatiquement la dernière version stable de DSM — en août 2026, il s’agit de **DSM 7.4.1-90080**, publiée le 23 juillet 2026. Le téléchargement et l’installation prennent entre 5 et 15 minutes selon la vitesse de votre connexion internet, le NAS redémarrant une ou deux fois pendant le processus.

Pendant cette phase, ne débranchez jamais le NAS et ne fermez pas la fenêtre du navigateur. Une coupure d’alimentation pendant l’écriture du firmware peut rendre le NAS inutilisable et nécessiter une réinstallation via un mode de récupération plus contraignant.

Si vous préférez rester sur une branche de maintenance longue durée plutôt que sur la dernière version générale, sachez que Synology a basculé le support LTS sur **DSM 7.3** depuis octobre 2025, avec une maintenance prévue jusqu’en octobre 2027 et une fin de vie annoncée pour octobre 2028 ; l’ancienne branche **DSM 7.2**, disponible depuis juin 2023, voit de son côté sa maintenance s’arrêter en décembre 2025, même si des correctifs ponctuels comme **DSM 7.2.2-72806 Update 9** (30 juin 2026) ou le plus ancien **DSM 7.2-64570 Update 4** (11 février 2025) restent disponibles pour les modèles qui ne supportent pas toujours DSM 7.4. Vérifiez la compatibilité de votre modèle exact sur la page officielle des notes de version DSM avant de choisir.

## Étape 4 : Créer le compte administrateur et sécuriser l’accès

DSM demande ensuite de nommer le serveur (par exemple “NAS-Maison” ou “NAS-Bureau”) et de créer le compte administrateur. Trois règles à respecter absolument à cette étape :

- Ne gardez jamais le compte “admin” par défaut actif après l’installation — créez un compte administrateur avec un nom personnalisé, puis désactivez le compte “admin” générique dans Panneau de configuration > Utilisateur.
- Utilisez un mot de passe d’au moins 12 caractères avec majuscules, chiffres et symboles ; DSM refuse les mots de passe trop simples mais un gestionnaire de mots de passe reste préférable pour en générer un vraiment robuste.
- Activez l’authentification à deux facteurs (2FA) dès cette étape via Panneau de configuration > Compte utilisateur > Authentification à 2 facteurs, avec une application comme Google Authenticator ou Microsoft Authenticator.

Ces réglages ne sont pas cosmétiques : les NAS exposés sur internet sans 2FA ni changement du compte admin par défaut restent une cible privilégiée pour les campagnes de ransomware ciblant spécifiquement les NAS grand public, un vecteur d’attaque documenté à plusieurs reprises ces dernières années. L’ANSSI, l’agence française de cybersécurité, recommande systématiquement l’activation du 2FA et la suppression des comptes génériques sur tout équipement exposé, une règle qui s’applique aussi bien en entreprise qu’à la maison.

## Étape 5 : Choisir entre Btrfs et ext4

C’est la décision la plus structurante du tutoriel, car elle ne se change pas sans tout reformater. DSM 7.4 propose deux systèmes de fichiers pour le volume principal :

| Critère | Btrfs | ext4 | 
|---|---|---|
| Snapshots natifs | Oui, quasi instantanés | Non | 
| Checksums de données | Oui (détecte la corruption silencieuse) | Non | 
| Versionning Synology Drive | Plus économe en espace disque | Nécessite un espace de stockage double pour les métadonnées de version | 
| Quotas par dossier partagé | Oui | Non | 
| Compatibilité LUN iSCSI sur disque externe | Non pour Hyper Backup | Requis pour la sauvegarde de LUN iSCSI vers disque externe | 
| Charge CPU | Légèrement supérieure | Plus légère, utile sur NAS d’entrée de gamme | 

Pour la quasi-totalité des usages domestiques ou de petit bureau, **Btrfs est le choix recommandé** en 2026 : les snapshots protègent contre les erreurs de manipulation et les débuts de ransomware, et les checksums repèrent la corruption de données avant qu’elle ne devienne un vrai problème. Depuis DSM 7.4, l’outil Storage Efficiency affiche en plus, dossier partagé par dossier partagé, le gain réel obtenu par la déduplication et la compression a posteriori des volumes HDD, exprimé directement en pourcentage d’espace récupéré. ext4 garde un intérêt pour les configurations avec CPU limité (vieux modèles d’entrée de gamme) ou pour des besoins spécifiques de compatibilité iSCSI avec Hyper Backup vers un disque externe.

Si vous avez déjà un volume ext4 existant et voulez migrer vers Btrfs, Synology ne propose pas de conversion en place : il faut sauvegarder les données avec Hyper Backup, supprimer le volume ext4, créer un nouveau volume Btrfs, puis restaurer depuis la sauvegarde. Prévoyez le temps et l’espace de stockage nécessaires avant de vous lancer.

## Étape 6 : Créer le pool de stockage et choisir le type de RAID

Direction Gestionnaire de stockage > Stockage > Créer. DSM propose plusieurs schémas de redondance selon le nombre de disques installés :

- **SHR (Synology Hybrid RAID)** : le format maison de Synology, qui permet de mélanger des disques de tailles différentes sans perdre trop d’espace. SHR-1 tolère la panne d’un disque (équivalent RAID 5), SHR-2 tolère deux pannes simultanées (équivalent RAID 6) mais demande au moins 4 disques.
- **RAID 1** : miroir simple, recommandé sur un NAS 2 baies pour une protection basique contre la panne d’un disque.
- **RAID 5** : nécessite 3 disques minimum, tolère une panne, bon compromis capacité/sécurité sur 4 baies.
- **RAID 6** : nécessite 4 disques minimum, tolère deux pannes simultanées, recommandé si le NAS stocke des données critiques sans sauvegarde externe immédiate.

Pour un premier NAS 2 baies, SHR-1 ou RAID 1 sont les deux options les plus sûres. La différence pratique : RAID 1 exige deux disques strictement identiques en capacité, alors que SHR-1 accepte un disque de 4 To et un disque de 6 To en n’utilisant “que” 4 To exploitables sur le second, ce qui laisse une marge d’évolution si vous rachetez un disque plus gros plus tard.

## Étape 7 : Configurer QuickConnect et l’accès distant

QuickConnect est le service de relais et de DNS dynamique de Synology, qui permet d’accéder à DSM et aux applications (Drive, Photos, etc.) depuis l’extérieur du réseau local sans avoir à configurer manuellement une redirection de port sur la box internet. Le paramétrage se fait dans Panneau de configuration > QuickConnect : connectez-vous avec votre compte Synology, choisissez un identifiant QuickConnect unique, et activez les applications que vous souhaitez rendre accessibles.

DSM 7.2 et 7.4 ont renforcé la sécurité de ce service : Auto Block bannit désormais automatiquement les adresses IP après plusieurs tentatives de connexion échouées sur les serveurs QuickConnect, en plus de la protection déjà existante sur l’accès local. Ce point mérite d’être vérifié manuellement dans Panneau de configuration > Sécurité > Protection du compte, car le seuil par défaut (généralement 5 tentatives) peut être resserré pour un NAS exposé publiquement.

Alternative plus contrôlée : configurer un accès VPN vers le réseau local (via le paquet VPN Server de Synology ou un routeur compatible WireGuard) plutôt que d’exposer DSM directement sur internet. Cette approche rejoint la logique de l’architecture Zero Trust appliquée à un contexte domestique : on limite la surface d’attaque en n’exposant que ce qui est strictement nécessaire.

## Étape 8 : Mettre en place la sauvegarde avec Hyper Backup

Un NAS n’est pas une sauvegarde en soi — c’est un poste de stockage. Sans copie externe, un incendie, un vol ou un ransomware qui chiffre le volume local emporte tout. Synology propose **Hyper Backup**, un paquet gratuit qui gère la sauvegarde par blocs vers plusieurs types de destinations : un autre NAS Synology, un disque USB externe, ou le service cloud Synology C2 Storage.

Installation depuis le Centre de paquets, puis création d’une tâche de sauvegarde qui cible les dossiers partagés et les paramètres système à protéger. Un point technique à connaître : Hyper Backup fonctionne avec les volumes Btrfs et ext4, mais si la destination est un disque externe utilisé pour sauvegarder des LUN iSCSI, ce disque externe doit obligatoirement être formaté en ext4.

```
# Exemple de planification de tâche Hyper Backup (via l'interface DSM)
Destination : Synology C2 Storage ou NAS distant
Fréquence : quotidienne, 03:00
Rétention : rotation par versions (ex. 7 quotidiennes, 4 hebdomadaires, 6 mensuelles)
Compression : activée
Chiffrement : activé avec phrase de passe séparée
```
Sur la tarification cloud, Synology C2 Storage propose en 2026 des paliers Basic en Europe autour de **9,99 € par an pour 100 Go**, **24,99 € par an pour 300 Go** et **59,99 € par an pour 1 To** (hors TVA), tandis que C2 Backup pour particuliers démarre autour de **34,99 $/an pour 500 Go** avec 15 Go offerts au départ. Pour une structure professionnelle, C2 Backup Business se facture environ **11,99 € par To et par mois** en Europe.

La règle à retenir pour une vraie stratégie de sauvegarde robuste reste la méthode 3-2-1-1-0 : trois copies des données, sur deux supports différents, dont une hors site, avec une copie hors ligne ou immuable, et zéro erreur vérifiée régulièrement. Un NAS Synology avec Hyper Backup vers C2 Storage coche déjà trois de ces cinq cases dès la configuration initiale.

## Étape 9 : Ajouter un cache SSD pour accélérer les accès fréquents

Sur les modèles équipés d’emplacements M.2 NVMe (DS423+, DS923+ et modèles supérieurs), il est possible de configurer un cache en lecture ou en lecture/écriture. Ce cache accélère l’accès aux fichiers consultés fréquemment (miniatures Synology Photos, indexation Synology Drive, bases de données de petites applications) sans passer systématiquement par les disques durs mécaniques, plus lents en accès aléatoire.

Configuration dans Gestionnaire de stockage > SSD Cache > Créer. Le cache en lecture seule est sans risque (une panne du SSD n’affecte pas l’intégrité des données), tandis que le cache en lecture/écriture améliore aussi les performances d’écriture mais nécessite deux SSD en miroir pour éviter la perte de données en cas de panne du cache lui-même. Pour un usage domestique standard, le cache en lecture seule suffit largement et reste la configuration la plus simple à mettre en place.

## Étape 10 : Installer Synology Drive et Synology Photos

Une fois le stockage et la sauvegarde en place, les deux applications les plus utilisées au quotidien sont Synology Drive (synchronisation de fichiers façon Dropbox, avec historique de versions) et Synology Photos (galerie photo avec reconnaissance faciale et albums partagés, la suite de Moments sur les anciennes versions de DSM).

Installation via le Centre de paquets, puis configuration des dossiers à synchroniser depuis Panneau de configuration au sein de chaque application. Sur un volume Btrfs, l’historique de versions de Synology Drive consomme sensiblement moins d’espace disque que sur ext4, grâce à la copie sur écriture (copy-on-write) qui évite de dupliquer intégralement les métadonnées à chaque nouvelle version d’un fichier.

Les applications clientes existent pour Windows, macOS, Linux, Android et iOS, avec synchronisation automatique en arrière-plan. Pour un usage professionnel, cette combinaison représente une alternative crédible et auto-hébergée aux abonnements Google Workspace ou Microsoft 365 pour la partie stockage de fichiers, dans la même logique que Nextcloud en cloud souverain.

## Étape 11 : Optimiser le réseau pour de meilleurs débits

La plupart des NAS Synology d’entrée et milieu de gamme embarquent deux ports Ethernet 1 GbE, qui peuvent être agrégés (link aggregation, 802.3ad) pour atteindre un débit cumulé plus élevé — à condition que le routeur ou switch le supporte également. Configuration dans Panneau de configuration > Réseau > Interface réseau > Créer > Créer une liaison agrégée.

Si votre installation réseau domestique reste en Wi-Fi 6 ou passe au Wi-Fi 7, le NAS lui-même reste presque toujours raccordé en filaire pour des raisons de fiabilité et de débit soutenu — voir notre comparatif Wi-Fi 7 vs Wi-Fi 6 pour la partie réseau sans fil du reste de la maison. Un transfert initial de plusieurs centaines de gigaoctets vers un NAS neuf se fait presque toujours plus vite et de façon plus fiable via câble Ethernet direct plutôt qu’en Wi-Fi, où les micro-coupures peuvent interrompre de longs transferts.

## Étape 12 : Sécuriser durablement le NAS et suivre les mises à jour

Synology publie des avis de sécurité réguliers, et 2026 n’a pas fait exception. Le CERT-FR avait d’ailleurs déjà alerté en mai 2025 sur des vulnérabilités touchant DSM 7.2.1 dans toutes les versions antérieures à 7.2.1-69057-7, un rappel utile que ces failles ne datent pas seulement de cette année. Plusieurs correctifs importants sont sortis en 2026, avec un rythme moyen d’une à deux publications par mois sur le second trimestre :

| Avis | Date | Composant | Correctif | 
|---|---|---|---|
| Synology-SA-26:03 | 19 mars 2026 | telnetd DSM (GNU inetutils) | CVE-2026-32746, exécution de commande à distance jugée critique | 
| Synology-SA-26:06/07 | 15 avril 2026 | DSM (plusieurs modules) | Mise à jour vers 7.3-81180 ou 7.2.2-72806-7 | 
| Synology-SA-26:01 | 27 mai 2026 | Storage Manager | CVE-2026-2237, mise à jour vers 1.0.1-1100 | 
| Synology-SA-26:10 | 26 mai 2026 | Chat Server | Mise à jour vers 2.4.5-22148 | 
| Synology-SA-26:11 | 26 juin 2026 | MailPlus Server | Mise à jour vers 4.0.1-31663 | 
| Synology-SA-26:12 | 3 août 2026 | Synology Assistant (Windows) | CVE-2026-4793, mise à jour vers 7.0.7-50095 | 

Ce rythme de correctifs n’a rien d’inhabituel pour un système exposé à internet, mais il impose une discipline simple : activer les mises à jour automatiques de sécurité dans Panneau de configuration > Mise à jour et restauration, et vérifier manuellement la page des avis de sécurité Synology une fois par trimestre si le NAS gère des données sensibles. La liste complète des avis reste consultable sur la page officielle des avis de sécurité Synology.

## Projet complet : monter un NAS familial avec sauvegarde automatisée

Pour rendre ce tutoriel concret, voici la configuration complète telle qu’assemblée de bout en bout sur un DS224+, pensée pour un foyer de 3 à 4 personnes avec photos, documents et un peu de partage multimédia :

- Boîtier : Synology DS224+, deux disques 4 To dédiés NAS en SHR-1 (soit environ 3,6 To exploitables après formatage)
- Système de fichiers : Btrfs, pour les snapshots et la protection contre la corruption silencieuse
- Sécurité : compte admin renommé, 2FA activée, Auto Block resserré à 3 tentatives
- Accès distant : QuickConnect activé uniquement pour Synology Photos et Drive, DSM lui-même accessible seulement via VPN local
- Sauvegarde : Hyper Backup quotidien vers Synology C2 Storage (palier 1 To, chiffré, rétention 7/4/6)
- Applications : Synology Drive pour les documents partagés, Synology Photos avec albums familiaux partagés
- Snapshots locaux : toutes les 4 heures sur le dossier “Documents”, conservés 30 jours, via le paquet Snapshot Replication

Cette configuration coûte, matériel compris (NAS + 2 disques 4 To), environ 650 à 700 € à l’achat, plus 60 € par an pour la sauvegarde cloud C2. Sur 5 ans, cela reste comparable au coût cumulé d’un abonnement cloud premium à 2 To, mais avec un contrôle total sur les données et des fonctionnalités de partage familial bien plus riches.

## Résultats attendus et exemples de sortie

Une fois la configuration terminée, voici ce que l’on doit observer dans le tableau de bord DSM (Panneau de contrôle > Ressource système) :

```
État du volume : Sain (Btrfs, SHR-1)
Espace utilisé : 2 % (installation initiale)
Statut Hyper Backup : Dernière sauvegarde réussie, il y a 2 heures
QuickConnect : Connecté (ID : nas-famille-xxxx)
2FA : Activée sur le compte administrateur
Mises à jour DSM : À jour (7.4.1-90080)
Ventilateurs : Normal, 45 % de charge
Température disques : 32-36°C
```
Sur un transfert initial de 500 Go via câble Ethernet Gigabit, comptez environ 1h15 à 1h30 en conditions réelles (débit soutenu autour de 90-100 Mo/s), contre plusieurs heures voire une journée entière en Wi-Fi selon la qualité du signal.

## 5 pièges fréquents à éviter

**1. Utiliser des disques de bureau grand public plutôt que des disques NAS.** Le manque de tolérance aux vibrations et l’absence d’optimisation pour un fonctionnement 24/7 réduisent significativement la durée de vie dans un boîtier multi-disques.

**2. Choisir ext4 sans réfléchir, puis regretter l’absence de snapshots.** La conversion a posteriori demande une sauvegarde complète, une suppression du volume et une restauration : autant bien choisir dès le départ.

**3. Confondre RAID et sauvegarde.** Un RAID protège contre la panne d’un disque, pas contre un vol, un incendie, une erreur de manipulation ou un ransomware qui chiffre le volume entier. Sans copie externe via Hyper Backup, les données restent exposées à un point de défaillance unique.

**4. Exposer DSM directement sur internet sans 2FA ni restriction d’accès.** Les campagnes de ransomware ciblant spécifiquement les NAS grand public exploitent en priorité les comptes admin par défaut et l’absence d’authentification à deux facteurs.

**5. Ignorer les avis de sécurité Synology une fois le NAS configuré.** Un NAS “qui marche” n’est pas un NAS “à jour” : sans mises à jour automatiques activées, un appareil peut rester vulnérable à des failles corrigées depuis des mois.

## Conseils avancés pour aller plus loin

Une fois la base solide, plusieurs réglages permettent de tirer davantage parti d’un NAS Synology. Le paquet **Snapshot Replication** permet de répliquer en temps quasi réel les snapshots vers un second NAS distant, ce qui offre une redondance géographique sans passer par le cloud pour les structures qui disposent de deux sites. Le paquet **Active Backup for Business**, gratuit, permet de sauvegarder des postes Windows, des machines virtuelles VMware/Hyper-V et même des instances Microsoft 365 directement sur le NAS, sans licence supplémentaire — une option souvent ignorée alors qu’elle remplace des solutions de sauvegarde d’entreprise payantes pour les petites structures. Pour les structures qui pensent déjà à la suite, Synology a présenté à Computex **DSM Enterprise 1.0**, avec un support annoncé à partir de mai 2026 et une maintenance prévue jusqu’en mai 2028, ainsi qu’une feuille de route dévoilée le 4 juin 2026 autour d’agents d’IA privés et d’un **Cluster Manager** destinés à orchestrer de l’IA on-premise gouvernée sur plusieurs nœuds NAS.

Pour les utilisateurs qui gèrent aussi des SSD ou des disques dans d’autres machines (PC de jeu, PS5), surveiller leur état de santé reste tout aussi important que pour les disques du NAS ; notre tutoriel sur CrystalDiskInfo détaille comment lire les indicateurs SMART qui annoncent une panne avant qu’elle ne survienne. Côté extension de stockage, l’installation d’un SSD M.2 NVMe suit une logique similaire à ce que nous avions documenté pour l’installation d’un SSD M.2 sur PS5, avec les mêmes précautions de manipulation antistatique.

Enfin, pensez à documenter votre configuration : notez le modèle de NAS, la disposition RAID, les identifiants QuickConnect et la procédure de restauration Hyper Backup dans un endroit accessible même si le NAS tombe en panne. C’est le genre de détail qu’on néglige au moment de l’installation et qu’on regrette amèrement le jour où il faut restaurer une sauvegarde en urgence, sous pression, sans savoir par où commencer.

## Dépannage : 8 problèmes courants et leurs solutions

**Le NAS n’apparaît pas dans Synology Assistant ou sur find.synology.com.** Vérifiez que l’ordinateur et le NAS sont bien sur le même sous-réseau, désactivez temporairement le pare-feu du PC, et essayez de brancher le câble Ethernet sur un autre port du routeur. Sur un réseau avec VLAN, la découverte automatique échoue souvent : utilisez alors l’adresse IP directe.

**L’installation de DSM échoue ou reste bloquée.** Un téléchargement de firmware interrompu est la cause la plus fréquente. Retirez et réinsérez les disques, redémarrez le NAS, et relancez l’installation en vérifiant la stabilité de la connexion internet.

**La création du volume échoue avec une erreur de disque.** Un des disques est probablement défectueux ou mal reconnu. Vérifiez son état dans Gestionnaire de stockage > HDD/SSD, et testez-le individuellement si le message d’erreur persiste.

**QuickConnect affiche “Non connecté” ou une erreur de relais.** Vérifiez que le NAS a bien accès à internet, que l’horloge système est synchronisée (Panneau de configuration > Heure), et réessayez après quelques minutes : les serveurs de relais Synology peuvent être temporairement surchargés.

**Hyper Backup échoue avec une erreur “Chiffrement de LUN” vers un disque externe.** Rappel du point technique de l’étape 8 : les LUN iSCSI ne peuvent être sauvegardés que vers un disque externe formaté en ext4, pas en Btrfs ni en NTFS/exFAT.

**Les transferts de fichiers sont anormalement lents.** Vérifiez d’abord le câble Ethernet (un câble Cat 5 limite le débit bien en dessous du Gigabit), puis le statut de la liaison agrégée si configurée, et enfin l’activité en arrière-plan (indexation Photos, scrub du volume) qui peut temporairement consommer les ressources disque.

**Le NAS chauffe ou les ventilateurs tournent en continu à pleine vitesse.** Vérifiez l’aération autour du boîtier (au moins 10 cm d’espace libre à l’arrière), dépoussiérez les grilles de ventilation, et consultez le journal des températures dans Panneau de contrôle > Ressource système.

**Impossible de se connecter après une mise à jour DSM.** Videz le cache du navigateur et essayez en navigation privée avant de suspecter un vrai problème système ; DSM change parfois de port ou de certificat après une mise à jour majeure, ce qui déclenche des avertissements de sécurité du navigateur qu’il faut accepter manuellement une fois.

## Comparatif rapide : quel modèle Synology choisir en 2026

| Modèle | Baies | CPU / RAM | Prix France (boîtier seul) | 
|---|---|---|---|
| DS224+ | 2 | Intel Celeron J4125, 2 Go DDR4 (max 6 Go) | ~400-430 € | 
| DS423+ | 4 + 2 M.2 NVMe (cache) | Intel, 2 Go DDR4 (max 6 Go) | ~430-600 € | 
| DS923+ | 4 (extensible à 9) + 2 M.2 NVMe | AMD Ryzen R1600, 4 Go DDR4 ECC (max 32 Go) | ~600-750 € | 

Pour un premier NAS familial centré sur la sauvegarde photo et documents, le DS224+ suffit largement. Le DS423+ apporte de la marge pour du multimédia et davantage de baies. Le DS923+ vise plutôt les usages plus intensifs : machines virtuelles légères, bases de données, ou charge de travail qui bénéficie de la RAM ECC et du CPU plus puissant.

### Related Coverage

## Questions fréquentes

**Quelle est la dernière version de DSM disponible en août 2026 ?**

DSM 7.4.1-90080, publiée le 23 juillet 2026, est la version générale la plus récente ; elle fait suite à DSM 7.4, lancée le 16 juin 2026 avec l’assistant IA DSM Agent. Pour la maintenance longue durée, Synology a fait basculer le support LTS sur DSM 7.3 depuis octobre 2025 (maintenance jusqu’en octobre 2027), même si des correctifs comme DSM 7.2.2-72806 Update 9 (30 juin 2026) restent disponibles pour les modèles plus anciens.

**Faut-il choisir Btrfs ou ext4 pour un premier NAS Synology ?**

Btrfs, dans la grande majorité des cas, grâce aux snapshots, aux checksums qui détectent la corruption silencieuse et à un historique de versions plus économe en espace avec Synology Drive. ext4 reste pertinent seulement pour du matériel très limité en CPU ou des besoins spécifiques de compatibilité iSCSI.

**Un NAS Synology remplace-t-il une vraie sauvegarde ?**

Non. Un NAS avec RAID protège contre la panne d’un disque, pas contre un vol, un incendie ou un ransomware. Il faut ajouter Hyper Backup vers une destination externe (autre NAS, disque USB ou C2 Storage) pour obtenir une vraie protection selon la méthode 3-2-1-1-0.

**Combien coûte Synology C2 Storage en 2026 ?**

Les paliers Basic en Europe démarrent autour de 9,99 € par an pour 100 Go, 24,99 € par an pour 300 Go et 59,99 € par an pour 1 To, hors TVA. C2 Backup Business se facture environ 11,99 € par To et par mois pour les structures professionnelles.

**Peut-on utiliser des disques durs de PC classiques dans un NAS Synology ?**

Techniquement oui, mais ce n’est pas recommandé pour un usage continu 24/7. Les disques dédiés NAS sont conçus pour tolérer davantage de vibrations et fonctionner en permanence, avec des taux de panne nettement plus bas sur la durée que des disques de bureau grand public.

**QuickConnect est-il sûr pour accéder à son NAS depuis l’extérieur ?**

Avec 2FA activée et Auto Block configuré, QuickConnect offre un niveau de sécurité correct pour un usage domestique. Pour un usage professionnel ou des données sensibles, un accès VPN vers le réseau local reste préférable à une exposition directe de DSM sur internet.

**Quel modèle Synology choisir pour un premier NAS en 2026 ?**

Le DS224+ (2 baies, environ 400-430 € en France) couvre largement les besoins d’un foyer standard pour la sauvegarde photo et documents. Le DS423+ ou le DS923+ apportent plus de baies et de puissance pour du multimédia intensif ou des charges de travail plus lourdes.

**Comment savoir si mon NAS Synology doit être mis à jour pour des raisons de sécurité ?**

Consultez régulièrement la page officielle des avis de sécurité Synology et activez les mises à jour automatiques dans Panneau de configuration > Mise à jour et restauration. Six avis de sécurité au moins ont été publiés entre mars et août 2026, dont un jugé critique en mars concernant telnetd.
