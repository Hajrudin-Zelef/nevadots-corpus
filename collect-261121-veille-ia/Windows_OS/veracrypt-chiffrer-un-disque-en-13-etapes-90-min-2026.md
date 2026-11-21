---
id: collect-261121-veille-ia/windows-os/veracrypt-chiffrer-un-disque-en-13-etapes-90-min-2026
title: "VeraCrypt : Chiffrer un Disque en 13 Étapes, 90 Min [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["veracrypt", "disque", "windows", "chiffrement", "chiffrer", "macos", "fichiers", "truecrypt"]
source: https://tech-insider.org/fr/veracrypt-chiffrement-disque-2026/
source_anchor: ""
source_lines: [1, 301]
section: "Windows_OS"
---
Un ordinateur portable volé dans un train, un disque externe oublié dans un taxi, un serveur de sauvegarde qui finit sur le marché de l’occasion sans avoir été effacé correctement : dans chacun de ces cas, un simple mot de passe Windows ou macOS ne protège rien. Il suffit de démonter le disque et de le brancher ailleurs pour lire les fichiers en clair. Le chiffrement de disque change complètement la donne, et VeraCrypt reste en 2026 l’outil open source de référence pour le faire gratuitement, sur Windows, macOS et Linux. Ce tutoriel vous accompagne pas à pas, de l’installation jusqu’à la création d’un volume caché et d’un script de sauvegarde chiffrée automatisé.

## Qu’est-ce que VeraCrypt, et pourquoi chiffrer son disque en 2026 ?

VeraCrypt est un logiciel gratuit et open source de chiffrement de disque, disponible pour Windows, macOS et Linux. Le projet est développé par IDRIX et s’appuie sur le code de TrueCrypt 7.1a, le célèbre outil de chiffrement abandonné en 2014 dans des circonstances jamais totalement élucidées. Plutôt que de repartir de zéro, les développeurs de VeraCrypt ont repris cette base et corrigé les faiblesses identifiées lors de l’audit indépendant mené entre 2013 et 2016 par l’Open Crypto Audit Project (OSTIF) et le cabinet QuarksLab. Cet audit n’a pas révélé de porte dérobée volontaire, mais a mis au jour plusieurs vulnérabilités que VeraCrypt a corrigées au fil des versions.

Concrètement, VeraCrypt permet trois choses : créer un conteneur chiffré (un fichier qui se comporte comme un disque virtuel une fois monté), chiffrer une partition ou un disque entier de données, ou chiffrer le disque système lui-même, y compris Windows et ses fichiers temporaires. Un renforcement technique notable distingue VeraCrypt de TrueCrypt : le dérivé de clé pour les partitions système utilise désormais 200 000 itérations, contre seulement 1 000 sur TrueCrypt, ce qui complique nettement les attaques par force brute sur le mot de passe, au prix d’un temps de déverrouillage légèrement plus long.

Pourquoi ce sujet redevient-il d’actualité en France et en Europe en 2026 ? D’abord parce que le règlement général sur la protection des données (RGPD) cite explicitement le chiffrement à l’article 32 comme l’une des mesures techniques envisageables pour protéger les données personnelles, aux côtés de la pseudonymisation. Ensuite parce que la directive européenne NIS2 place elle aussi la cryptographie parmi les mesures de gestion des risques que les entités concernées doivent envisager, une liste qui touche en France plusieurs milliers d’organisations une fois la transposition pleinement appliquée. Ni le RGPD ni NIS2 n’imposent VeraCrypt spécifiquement ni même le chiffrement en toutes circonstances : les deux textes raisonnent par une approche fondée sur le risque, où le chiffrement figure comme une mesure « appropriée » plutôt qu’une obligation absolue et universelle. Mais dans les faits, un disque non chiffré rend la question du respect de ces textes beaucoup plus difficile à défendre en cas de contrôle ou d’incident.

## Prérequis : versions, configuration et éléments à préparer

Avant de vous lancer, vérifiez que votre configuration correspond à ces prérequis. Ce tutoriel s’appuie sur la version stable la plus récente de VeraCrypt au moment de la rédaction, la 1.26.29, dont les fichiers d’installation sont apparus sur la plateforme de distribution SourceForge le 13 juin 2026, succédant à la 1.26.24 qui y était référencée depuis le 26 avril 2026.

- **VeraCrypt** : version 1.26.29 (publiée le 9 juin 2026), qui ajoute la prise en charge d’Argon2id pour les volumes non-système et corrige deux failles de sécurité
- **Windows** : Windows 11 ou Windows 10 à partir de la version 1809 ; Windows Server 2019 ou ultérieur. Les versions antérieures de Windows nécessitent VeraCrypt 1.26.15 (dernière à supporter le 32 bits et Windows Server 2016) ou 1.25.9 (dernière à supporter XP, Vista, 7, 8 et 8.1)
- **macOS** : macOS 12 Monterey ou ultérieur (pour macOS 10.9 à 11 Big Sur, utilisez VeraCrypt 1.25.9)
- **Linux** : architectures x86, x86-64 et ARM64, la plupart des distributions récentes (Debian, Ubuntu, Fedora, Arch)
- **Autres systèmes** : FreeBSD x86-64 à partir de la version 14, OpenBSD x86-64 à partir de la version 7.8, Raspberry Pi OS en 32 et 64 bits
- **Espace disque libre** : au minimum l’équivalent de la taille du conteneur que vous souhaitez créer, plus 10 % de marge
- **Une clé USB vierge** d’au moins 100 Mo, dédiée au disque de secours (Rescue Disk) et, si vous le souhaitez, à un fichier-clé
- **Une sauvegarde complète** de vos données importantes sur un support externe déconnecté, avant toute opération de chiffrement du disque système
- **L’ordinateur portable branché au secteur** pendant toute la durée du chiffrement initial, qui peut prendre de quelques minutes à plusieurs heures selon la taille et la vitesse du disque

Comptez environ 90 minutes pour suivre l’intégralité de ce tutoriel en 13 étapes, en incluant le temps d’attente du chiffrement initial d’un petit volume de test. Le chiffrement complet d’un disque système de plusieurs centaines de gigaoctets se poursuivra en tâche de fond bien après la fin de la lecture.

## VeraCrypt face à BitLocker et FileVault : quelles différences réelles ?

BitLocker (Windows) et FileVault (macOS) sont déjà intégrés à votre système d’exploitation, ce qui les rend plus simples à activer. Mais ce sont des solutions propriétaires : leur code source n’est pas public, et leur fonctionnement interne ne peut être vérifié que par les éditeurs eux-mêmes ou des chercheurs travaillant par rétro-ingénierie. VeraCrypt, à l’inverse, publie l’intégralité de son code source, ce qui permet un audit communautaire indépendant, comme celui mené sur la base TrueCrypt. Le tableau suivant résume les différences pratiques entre les trois solutions.

| Critère | VeraCrypt | BitLocker | FileVault | 
|---|---|---|---|
| Licence | Open source, gratuit | Propriétaire, inclus dans Windows Pro/Enterprise/Education | Propriétaire, inclus dans macOS | 
| Plateformes | Windows, macOS, Linux, FreeBSD, OpenBSD, Raspberry Pi OS | Windows uniquement | macOS uniquement | 
| Algorithme principal | AES, Serpent, Twofish, Camellia, Kuznyechik (cascades possibles) | AES-CBC ou AES-XTS, 128 ou 256 bits | XTS-AES 128 bits | 
| Volumes cachés / déni plausible | Oui | Non | Non | 
| Dépendance à une puce TPM | Non requise | Recommandée, quasi obligatoire pour le déverrouillage automatique | Non requise (clé dérivée du mot de passe) | 
| Auditabilité du code | Code source public, audit communautaire possible | Code fermé | Code fermé | 
| Conteneurs de fichiers chiffrés | Oui, indépendamment du disque système | Non (détournement via VHD chiffré possible) | Non nativement | 

Aucune de ces solutions n’est meilleure dans l’absolu : BitLocker s’intègre mieux dans un parc Windows géré via Active Directory et Intune, FileVault est le choix par défaut cohérent sur un Mac. VeraCrypt s’impose dès que vous avez besoin de portabilité entre systèmes, de conteneurs chiffrés indépendants du disque système, ou de la fonctionnalité de volume caché qu’aucune des deux alternatives propriétaires ne propose.

## Comprendre les algorithmes de chiffrement et de hachage de VeraCrypt

VeraCrypt propose plusieurs algorithmes de chiffrement, combinables en cascade avec un ou deux autres pour ajouter des couches de protection supplémentaires, au prix d’une perte de performance. Le mode de fonctionnement utilisé est XTS, spécifiquement conçu pour le chiffrement de disque. La fonction de dérivation Argon2id a fait ses débuts dans la version 1.26.27 du 20 septembre 2025, pour le hachage du mot de passe et des montages plus rapides, avant d’être étendue aux volumes non-système avec la 1.26.29. Le tableau ci-dessous liste les algorithmes disponibles dans cette version.

| Type | Options disponibles | Recommandation | 
|---|---|---|
| Chiffrement (simple) | AES, Serpent, Twofish, Camellia, Kuznyechik | AES pour la majorité des usages (accélération matérielle AES-NI) | 
| Chiffrement (cascade) | AES(Twofish), Serpent(AES), Twofish(Serpent), AES(Twofish(Serpent)), et autres combinaisons | À réserver aux données très sensibles, impact sur la vitesse | 
| Fonctions de hachage | SHA-256, SHA-512, Streebog, Whirlpool, BLAKE2s-256 | SHA-512 par défaut, solide et rapide | 
| Dérivation de clé (nouveauté 1.26.29) | Argon2id (volumes non-système), PBKDF2 historique | Argon2id quand disponible : plus résistant au craquage par GPU/ASIC | 
| Mode de fonctionnement | XTS | Seul mode proposé, standard pour le chiffrement de disque | 

Pour un usage courant, l’algorithme AES normalisé par le NIST américain, combiné à SHA-512, constitue le meilleur compromis entre sécurité et performance, grâce à l’accélération matérielle AES-NI présente sur la quasi-totalité des processeurs Intel et AMD depuis plus d’une décennie. Depuis la version 1.26.20 du 3 février 2025, cette logique d’accélération matérielle s’étend aussi aux processeurs ARM64 pour la fonction de hachage SHA-256, un gain notable sur les Mac Apple Silicon et les machines Linux ARM. Les cascades comme AES(Twofish(Serpent)) ajoutent une marge de sécurité théorique contre la découverte future d’une faiblesse sur un algorithme unique, mais elles ralentissent sensiblement les opérations de lecture et d’écriture, sans bénéfice pratique pour l’utilisateur grand public.

## Étape 1 : Télécharger VeraCrypt et vérifier son authenticité

Téléchargez toujours VeraCrypt depuis le site officiel du projet, veracrypt.io (l’ancien domaine veracrypt.fr redirige désormais vers cette adresse). Un logiciel de chiffrement téléchargé depuis un site tiers ou un miroir non officiel peut avoir été modifié pour intégrer une porte dérobée : c’est précisément le scénario que l’outil est censé empêcher. Une fois le fichier récupéré, vérifiez sa signature PGP ou, a minima, son empreinte SHA-256, publiée sur la page officielle des téléchargements. Ce n’est pas une précaution superflue : en janvier 2025, le site gHacks a documenté la correction de deux failles de sécurité, référencées CVE-2024-54187 et CVE-2025-23021, avec la sortie de la version 1.26.18 pour Linux et macOS, un rappel que mieux vaut toujours installer la dernière version disponible plutôt qu’un exécutable oublié dans un dossier de téléchargements.

Sous Windows ou macOS, ouvrez un terminal (PowerShell ou Terminal.app) et calculez l’empreinte du fichier téléchargé :

```
# Windows (PowerShell)
Get-FileHash .\VeraCrypt_Setup_1.26.29.exe -Algorithm SHA256
# macOS / Linux
shasum -a 256 VeraCrypt_1.26.29.dmg
```
Comparez la sortie obtenue caractère par caractère avec l’empreinte publiée sur le site officiel. Si les deux ne correspondent pas, ne procédez pas à l’installation : supprimez le fichier et retéléchargez-le depuis une connexion différente pour écarter un problème de proxy d’entreprise ou de cache DNS corrompu.

## Étape 2 : Installer VeraCrypt sous Windows, macOS et Linux

Sous Windows, lancez l’exécutable téléchargé, acceptez la licence et laissez les options par défaut sauf besoin spécifique (langue de l’interface, dossier d’installation). Depuis la version 1.26.24, publiée le 30 mai 2025, l’installateur Windows embarque aussi une protection d’écran supplémentaire contre la capture de mot de passe, activable dans les options avancées. Sous macOS, montez l’image .dmg puis suivez l’assistant d’installation ; macOS demandera d’autoriser l’extension système de VeraCrypt dans Réglages Système > Confidentialité et sécurité, une étape facile à manquer et qui bloque silencieusement le montage de volumes si elle est ignorée.

### Installation sous Linux en ligne de commande

Sous Linux, VeraCrypt est distribué sous forme d’un script d’installation autonome (console ou GUI) plutôt que via les dépôts officiels des distributions. Depuis la version 1.26.24 du 30 mai 2025, un format AppImage est également proposé en alternative, pratique si vous préférez éviter le script d’installation classique. Téléchargez l’archive correspondant à votre architecture, puis exécutez le script d’installation graphique :

```
tar xjf veracrypt-1.26.29-setup.tar.bz2
./veracrypt-1.26.29-setup-gui-x64
# Vérifier l'installation
veracrypt --text --version
```
Sur les distributions qui appliquent AppArmor ou SELinux de façon stricte (Fedora, openSUSE), il arrive que le module FUSE nécessaire au montage des volumes soit bloqué par défaut. Si le montage échoue à l’étape suivante, installez le paquet `fuse` ou `fuse3` via le gestionnaire de paquets de votre distribution avant de continuer.

## Étape 3 : Créer votre premier conteneur chiffré (volume standard)

Lancez VeraCrypt et cliquez sur **Create Volume** (Créer un volume). Choisissez la première option, « Create an encrypted file container », pour créer un conteneur : un simple fichier qui se comportera comme un disque virtuel une fois monté. C’est l’option la plus flexible pour débuter, car elle ne touche à aucune partition existante et peut être supprimée à tout moment en effaçant simplement le fichier.

- Sélectionnez **Standard VeraCrypt volume** pour ce premier essai (le volume caché sera abordé à l’étape 9)
- Choisissez l’emplacement et le nom du fichier conteneur, par exemple `coffre.hc`
- Sélectionnez l’algorithme de chiffrement (AES par défaut convient à la majorité des cas) et la fonction de hachage (SHA-512)
- Indiquez la taille du volume : commencez par un petit conteneur de test, par exemple 500 Mo, avant de créer vos volumes définitifs
- Choisissez un mot de passe long et unique, idéalement une phrase de passe de 20 caractères ou plus. VeraCrypt affiche un avertissement si le mot de passe est jugé trop court
- Bougez la souris de façon aléatoire dans la fenêtre pendant la génération des clés : ce mouvement alimente le générateur de nombres aléatoires
- Choisissez un système de fichiers (exFAT pour un usage multiplateforme Windows/macOS/Linux, NTFS si le volume reste sur Windows)

Cliquez sur **Format** pour lancer la création. Pour un conteneur de 500 Mo, l’opération prend généralement moins d’une minute.

## Étape 4 : Monter et démonter un volume au quotidien

Dans la fenêtre principale de VeraCrypt, sélectionnez une lettre de lecteur libre (Windows) ou un point de montage (macOS/Linux), cliquez sur **Select File** pour pointer vers votre conteneur, puis sur **Mount**. Saisissez votre mot de passe : le volume apparaît alors comme un disque normal dans l’explorateur de fichiers, avec sa propre lettre ou son propre point de montage.

Une fois le travail terminé, ne vous contentez jamais d’éjecter le volume comme une simple clé USB : utilisez le bouton **Dismount** (rebaptisé **Unmount All** pour l’option globale depuis la mise à jour 1.26.24 du 30 mai 2025, qui a aussi actualisé les traductions chinoise et russe de l’interface) dans VeraCrypt. Cette étape force l’écriture de toutes les données en attente et referme proprement le volume chiffré. Un démontage brutal (extinction de l’ordinateur, débranchement d’un disque externe) pendant qu’un volume reste monté peut corrompre les données qu’il contient, exactement comme pour n’importe quel système de fichiers.

## Étape 5 : Chiffrer une clé USB ou un disque externe entier

Plutôt qu’un simple conteneur, vous pouvez chiffrer un périphérique complet : une clé USB, un disque dur externe ou une carte SD. Dans l’assistant de création, choisissez cette fois **Encrypt a non-system partition/drive**, puis sélectionnez le périphérique cible via **Select Device** plutôt que **Select File**.

Soyez particulièrement vigilant sur le choix du périphérique à cette étape : VeraCrypt formate entièrement le support sélectionné, effaçant toutes les données déjà présentes. Vérifiez deux fois la lettre de lecteur ou l’identifiant du périphérique avant de valider, surtout si plusieurs disques externes sont branchés simultanément. Une fois le chiffrement terminé, le périphérique doit être monté via VeraCrypt à chaque branchement : il n’apparaîtra pas comme un disque lisible normalement dans l’explorateur de fichiers d’un ordinateur ne disposant pas de VeraCrypt.

## Étape 6 : Chiffrer le disque système complet (Windows)

Le chiffrement du disque système protège l’intégralité de Windows, y compris le fichier d’échange, les fichiers temporaires et les données de mise en veille prolongée, qui peuvent tous contenir des fragments de données sensibles. Dans l’assistant, choisissez **Encrypt the system partition or entire system drive**, puis **Normal** (le mode caché est traité à l’étape 9).

- Sélectionnez **Encrypt the whole drive** si votre disque ne contient qu’une seule partition Windows, ou**Encrypt the Windows system partition** pour ne cibler que la partition système en cas de multi-boot
- Sur un poste avec un seul système d’exploitation installé, répondez **Single-boot** à la question posée par l’assistant
- Choisissez l’algorithme de chiffrement et la fonction de hachage, comme pour un conteneur classique
- Définissez un mot de passe de pré-démarrage (pre-boot authentication password) : c’est ce mot de passe qui sera demandé avant même le chargement de Windows
- Laissez VeraCrypt collecter de l’entropie en bougeant la souris, puis passez à la génération des clés

## Étape 7 : Créer le disque de secours (Rescue Disk), l’étape qu’il ne faut jamais sauter

Avant de chiffrer quoi que ce soit sur le disque système, VeraCrypt oblige à créer un **VeraCrypt Rescue Disk**, sous la forme d’une image ISO à graver ou à copier sur une clé USB bootable. Ce disque de secours n’est pas une option accessoire : si le chargeur de démarrage est un jour endommagé (mise à jour de firmware ratée, secteur de démarrage corrompu, panne de courant pendant une opération sensible), c’est l’unique moyen de redémarrer le système ou de déchiffrer le disque en cas de problème durable.

Une fois l’image générée, VeraCrypt propose de vérifier automatiquement que le disque de secours démarre correctement avant de continuer. Ne sautez jamais cette vérification : un disque de secours corrompu ou mal gravé ne sert à rien le jour où vous en avez réellement besoin, et ce jour arrive toujours au pire moment. Conservez ce support dans un endroit différent de l’ordinateur lui-même, par exemple avec vos documents importants, plutôt que dans le même sac ou le même tiroir.

## Étape 8 : Test de pré-chiffrement et lancement du chiffrement système

Avant de chiffrer réellement le disque, VeraCrypt propose un **test de pré-chiffrement** (pretest) : l’ordinateur redémarre, affiche l’écran de mot de passe de pré-démarrage, puis revient sous Windows sans qu’aucune donnée n’ait encore été modifiée. Ce test confirme que l’écran d’authentification fonctionne correctement sur votre configuration matérielle précise (clavier, résolution d’écran, pilotes de démarrage) avant que le chiffrement irréversible ne commence.

Si le test réussit, cliquez sur **Encrypt** pour lancer le chiffrement effectif. Une barre de progression affiche l’avancement en pourcentage et en Mo restants. Vous pouvez continuer à utiliser l’ordinateur normalement pendant le chiffrement : l’opération se déroule en tâche de fond, avec un léger ralentissement des accès disque. Il est possible de mettre le chiffrement en pause via VeraCrypt et de le reprendre plus tard, ce qui est utile sur un ordinateur portable si la batterie devient faible malgré la recommandation de rester branché au secteur.

## Étape 9 : Créer un volume caché pour le déni plausible

La fonctionnalité de volume caché (hidden volume) permet de dissimuler un second volume chiffré à l’intérieur de l’espace libre d’un premier volume, dit « volume extérieur ». Sans connaître le mot de passe du volume caché, il est mathématiquement impossible de prouver qu’il existe : l’espace qu’il occupe apparaît comme de l’espace libre non utilisé du volume extérieur. C’est la base du concept de **déni plausible** : si quelqu’un vous contraint à révéler un mot de passe, vous pouvez communiquer celui du volume extérieur, qui contient des données plausibles mais non sensibles, sans révéler l’existence du volume caché.

Pour créer un volume caché, choisissez **Hidden VeraCrypt volume** dans l’assistant de création plutôt que **Standard**. VeraCrypt vous demande de créer d’abord le volume extérieur, de le remplir avec des fichiers crédibles (jamais un volume vide, ce qui éveillerait immédiatement les soupçons), puis de définir la taille et le mot de passe du volume caché à l’intérieur. Un point technique important, confirmé par les notes de version de VeraCrypt 1.26.29 : la structure actuelle prend en charge **un volume caché à l’intérieur d’un volume extérieur**, et non un empilement de plusieurs couches cachées imbriquées les unes dans les autres. Si vous avez créé des volumes cachés avec une version antérieure à 1.26.6, ou entre 1.26.6 et 1.26.29, consultez les notes de version officielles : la base nationale américaine de vulnérabilités (NIST NVD) a confirmé le 21 août 2026 que ces versions intermédiaires étaient exposées à la faille CVE-2026-54073, susceptible d’affecter la déniabilité plausible et corrigée depuis dans la 1.26.29 ; les volumes créés dans cet intervalle doivent être recréés par précaution.

Attention à un piège technique classique : si vous montez le volume extérieur et y écrivez de nouveaux fichiers sans protection particulière, vous risquez d’écraser silencieusement le volume caché qu’il contient. Lorsque vous devez modifier le volume extérieur après coup, montez-le en cochant l’option **Protect hidden volume** et en indiquant le mot de passe du volume caché : VeraCrypt empêchera alors toute écriture qui risquerait d’endommager les données cachées.

## Étape 10 : Renforcer l’authentification avec des fichiers-clés (keyfiles)

Un fichier-clé (keyfile) est un fichier quelconque (image, document, fichier audio) utilisé en complément ou à la place d’un mot de passe pour déverrouiller un volume. Sans ce fichier précis, le volume reste indéchiffrable même avec le bon mot de passe. Dans la fenêtre de montage, cliquez sur **Use keyfile**, ajoutez un ou plusieurs fichiers depuis **Add Files**, puis cochez l’option correspondante lors du montage.

L’usage le plus courant consiste à stocker le fichier-clé sur une clé USB distincte de l’ordinateur : sans cette clé physiquement branchée, impossible de monter le volume, même en connaissant le mot de passe par cœur. C’est une forme simple d’authentification à deux facteurs « fait maison ». La contrepartie est évidente : perdre ce fichier-clé sans en avoir conservé de copie de sauvegarde équivaut à perdre l’accès aux données chiffrées de façon définitive, aussi certainement qu’en oubliant le mot de passe. Conservez toujours une copie du fichier-clé sur un support de sauvegarde séparé et hors ligne.

## Étape 11 : Automatiser le montage en ligne de commande

VeraCrypt propose une interface en ligne de commande complète, utile pour scripter le montage et le démontage plutôt que de passer par l’interface graphique à chaque fois. Voici la syntaxe de base sous Linux et macOS :

```
# Monter un volume avec mot de passe et fichier-clé
veracrypt --text --keyfiles="/media/usb/cle.key" --pim=0 \
  --mount coffre.hc /mnt/coffre
# VeraCrypt demandera alors :
Enter password for coffre.hc:
# Démonter proprement
veracrypt --text --dismount /mnt/coffre
# Lister les volumes actuellement montés
veracrypt --text --list
```
Le paramètre `--pim` (Personal Iterations Multiplier) permet d’augmenter manuellement le nombre d’itérations de dérivation de clé au-delà de la valeur par défaut, pour renforcer la résistance à la force brute au prix d’un déverrouillage plus lent. Sur un script automatisé destiné à tourner sans surveillance, ajoutez l’option `--non-interactive` associée à un mot de passe fourni par une variable d’environnement plutôt qu’en clair dans le script, afin d’éviter qu’il n’apparaisse dans l’historique des commandes ou dans les journaux système.

## Étape 12 : Sauvegarder l’en-tête de volume pour éviter la perte de données

Chaque volume VeraCrypt commence par un en-tête chiffré qui contient les clés nécessaires pour déchiffrer le reste du volume. Si cet en-tête est endommagé, par exemple à cause de secteurs défectueux ou d’une coupure de courant pendant une écriture, l’intégralité du volume peut devenir illisible, même avec le bon mot de passe. VeraCrypt propose justement un outil de sauvegarde d’en-tête, accessible via **Tools > Backup Volume Header** dans l’interface graphique.

Générez cette sauvegarde juste après la création de chaque volume important, et conservez le fichier résultant sur un support totalement distinct du volume lui-même : un en-tête de secours stocké sur le même disque que le volume qu’il protège ne sert à rien en cas de panne physique du support. En cas de corruption, la fonction **Restore Volume Header** permet de restaurer l’accès à partir de cette sauvegarde, à condition de connaître toujours le mot de passe d’origine.

## Étape 13 : Mesurer les performances avec le benchmark intégré

VeraCrypt intègre un outil de benchmark accessible via **Tools > Benchmark**, qui mesure la vitesse de chiffrement et de déchiffrement de chaque algorithme directement sur votre matériel. Des tests communautaires partagés sur le forum SourceForge du projet donnent un ordre de grandeur utile, à titre indicatif puisque les résultats varient selon le processeur et la charge du système au moment du test.

| Algorithme | Débit observé (MB/s) | Remarque | 
|---|---|---|
| AES (simple) | Environ 650 à 1 000+ | Le plus rapide, accélération matérielle AES-NI quasi systématique | 
| Serpent (simple) | Environ 390 à 560 | Pas d’accélération matérielle dédiée | 
| Twofish (simple) | Environ 380 | Performance proche de Serpent | 
| Serpent(AES) [cascade] | Environ 350 | Cascade à deux algorithmes | 
| Twofish(Serpent) [cascade] | Environ 290 | Cascade à deux algorithmes | 
| Serpent(Twofish(AES)) [cascade] | Environ 280 | Triple cascade, sécurité maximale théorique | 

Sur un SSD NVMe récent avec AES-NI actif, la différence de vitesse ressentie au quotidien entre un disque chiffré avec AES et un disque non chiffré reste minime pour un usage bureautique courant : l’accélération matérielle absorbe l’essentiel du surcoût de calcul. L’écart se creuse nettement dès que vous activez une cascade à deux ou trois algorithmes, ou sur du matériel plus ancien dépourvu d’AES-NI, où le chiffrement logiciel pur peut diviser le débit par cinq ou plus.

## Projet complet : un coffre-fort numérique avec sauvegarde automatisée

Pour mettre en pratique l’ensemble des étapes précédentes, voici un projet complet et fonctionnel : un conteneur VeraCrypt dédié aux documents confidentiels, déverrouillé par un mot de passe combiné à un fichier-clé stocké sur une clé USB, avec un script qui monte le volume, synchronise un dossier source vers le coffre chiffré, puis démonte proprement le tout. Ce script peut ensuite être appelé automatiquement par une tâche planifiée (cron sous Linux/macOS, Planificateur de tâches sous Windows avec WSL).

```
#!/bin/bash
# backup-chiffre.sh — synchronise un dossier vers un coffre VeraCrypt chiffré
set -euo pipefail
VOLUME="/home/user/coffre.hc"
MOUNT_POINT="/mnt/coffre"
KEYFILE="/media/usb/cle.key"
SOURCE_DIR="/home/user/Documents/confidentiel"
LOG_FILE="/home/user/backup-chiffre.log"
if [ ! -f "$KEYFILE" ]; then
  echo "$(date '+%F %T') - Fichier-cle introuvable, abandon" >> "$LOG_FILE"
  exit 1
fi
mkdir -p "$MOUNT_POINT"
veracrypt --text --keyfiles="$KEYFILE" --pim=0 --protect-hidden=no \
  --mount "$VOLUME" "$MOUNT_POINT" --non-interactive \
  --password="$VERACRYPT_PASSWORD"
if mountpoint -q "$MOUNT_POINT"; then
  rsync -av --delete "$SOURCE_DIR/" "$MOUNT_POINT/"
  sync
  veracrypt --text --dismount "$MOUNT_POINT"
  echo "$(date '+%F %T') - Sauvegarde chiffree terminee avec succes" >> "$LOG_FILE"
else
  echo "$(date '+%F %T') - Echec du montage du volume VeraCrypt" >> "$LOG_FILE"
  exit 1
fi
```
Le mot de passe est ici lu depuis la variable d’environnement `VERACRYPT_PASSWORD`, définie séparément (par exemple dans un fichier lu uniquement par root, avec des permissions restreintes à 600) plutôt qu’écrit en clair dans le script. Pour planifier ce script toutes les nuits à 2h du matin via cron, ajoutez une ligne à la crontab de l’utilisateur concerné :

```
# crontab -e
0 2 * * * VERACRYPT_PASSWORD="$(cat /root/.backup-pass)" /home/user/backup-chiffre.sh
```
Ce projet combine les briques essentielles vues dans ce tutoriel : un conteneur chiffré (étape 3), une authentification renforcée par fichier-clé (étape 10), et l’automatisation en ligne de commande (étape 11). Vous pouvez l’étendre en ajoutant une rotation de sauvegardes horodatées, ou en synchronisant le résultat vers un second support physique conservé hors site.

## 5 erreurs fréquentes à éviter avec VeraCrypt

1. **Sauter la création du disque de secours.** C’est de loin l’erreur la plus coûteuse : sans Rescue Disk fonctionnel, un problème de chargeur de démarrage après le chiffrement système peut rendre l’ordinateur totalement inutilisable, données comprises.
2. **Choisir un mot de passe court ou réutilisé.** VeraCrypt ne protège que ce que le mot de passe protège réellement : un mot de passe de 6 caractères rend le chiffrement AES 256 bits presque décoratif face à une attaque par dictionnaire ciblée.
3. **Chiffrer un ordinateur portable sur batterie.** Une coupure d’alimentation pendant le chiffrement initial du disque système peut corrompre la conversion en cours. Restez toujours branché au secteur pendant toute la durée de l’opération.
4. **Oublier de désactiver l’hibernation et le démarrage rapide de Windows** avant de chiffrer le disque système. Ces fonctionnalités écrivent des données mémoire directement sur le disque en dehors du contrôle habituel de VeraCrypt, ce qui peut provoquer des erreurs de démarrage après activation du chiffrement.
5. **Perdre le fichier-clé sans sauvegarde.** Un keyfile est aussi critique qu’un mot de passe : sa perte définitive équivaut à perdre l’accès aux données, sans aucun recours possible, même pour les développeurs de VeraCrypt eux-mêmes.
6. **Écrire dans le volume extérieur sans protéger le volume caché.** Monter le volume extérieur sans cocher**Protect hidden volume** avant d’y ajouter des fichiers risque d’écraser silencieusement les données du volume caché qu’il contient.

## VeraCrypt, RGPD et NIS2 : ce que dit la réglementation en 2026

Le RGPD ne mentionne aucun logiciel par son nom. L’article 32 impose aux responsables de traitement et à leurs sous-traitants de mettre en œuvre « les mesures techniques et organisationnelles appropriées » pour garantir un niveau de sécurité adapté au risque, en citant explicitement « la pseudonymisation et le chiffrement des données à caractère personnel » comme exemple de mesure envisageable. Le texte reste donc fondé sur le risque : le chiffrement n’est pas obligatoire dans l’absolu, mais devient difficile à justifier de ne pas y recourir dès lors que le poste de travail ou le support amovible contient des données personnelles sensibles.

La directive NIS2 suit une logique comparable côté cybersécurité des entités essentielles et importantes : elle inclut la cryptographie et le chiffrement parmi les mesures de gestion des risques que ces organisations doivent envisager de mettre en place, aux côtés de la gestion des incidents, de la sécurité de la chaîne d’approvisionnement et du contrôle des accès. Une fois la transposition pleinement appliquée en France, le périmètre concerné couvrirait plusieurs milliers d’entités supplémentaires par rapport au régime antérieur, bien au-delà des seuls grands groupes déjà soumis à des obligations de cybersécurité. En France, c’est l’Agence nationale de la sécurité des systèmes d’information (ANSSI) qui pilote l’application opérationnelle de ces textes et publie les référentiels techniques associés.

Dans ce contexte réglementaire, un outil comme VeraCrypt ne suffit pas à lui seul à assurer une conformité complète : il ne remplace ni une politique de sécurité documentée, ni la gestion des accès, ni la journalisation des événements. Il constitue en revanche une mesure technique concrète et vérifiable, facile à documenter dans un registre de traitement ou une analyse de risques, pour la partie « protection des données au repos » que ces textes appellent de leurs vœux sans en faire une case à cocher unique.

## Dépannage : 8 problèmes courants et leurs solutions

**1. « Incorrect password or not a VeraCrypt volume » alors que le mot de passe est correct.** Vérifiez la disposition du clavier active au moment de la saisie : un mot de passe créé en AZERTY puis saisi en QWERTY (ou inversement, par exemple depuis l’écran de pré-démarrage qui peut charger une disposition différente) ne produira jamais la même chaîne de caractères.

**2. L’ordinateur ne démarre plus après le chiffrement du disque système.** Démarrez depuis le Rescue Disk créé à l’étape 7, puis utilisez l’option de réparation du chargeur de démarrage proposée dans son menu. C’est exactement le scénario pour lequel ce disque a été créé.

**3. Le volume se monte, mais en lecture seule.** Vérifiez que le système de fichiers n’a pas été démonté de façon incorrecte lors de la session précédente. Sous Linux, un montage forcé en lecture seule après un arrêt brutal indique souvent qu’une vérification du système de fichiers (fsck/CHKDSK) est nécessaire avant de rouvrir le volume en écriture.

**4. Le chiffrement du disque système semble bloqué à un pourcentage fixe.** Un ralentissement marqué peut signaler un secteur défectueux sur le disque. Laissez l’opération se poursuivre sans interruption brutale ; si elle reste bloquée plusieurs heures sans progression, consultez l’observateur d’événements Windows pour repérer une erreur disque sous-jacente avant de forcer un arrêt.

**5. VeraCrypt affiche une erreur liée à FUSE sous Linux.** Installez le paquet `fuse` ou `fuse3` selon votre distribution, puis vérifiez que votre utilisateur appartient bien au groupe autorisé à utiliser ce module (souvent le groupe `fuse` ou `disk` selon la distribution).

**6. macOS refuse de monter un volume après une mise à jour système.** Retournez dans Réglages Système > Confidentialité et sécurité et vérifiez que l’extension système de VeraCrypt est toujours autorisée : les mises à jour majeures de macOS réinitialisent parfois ces autorisations.

**7. Les performances s’effondrent après activation d’une cascade d’algorithmes.** C’est un comportement attendu, pas un dysfonctionnement : chaque algorithme supplémentaire dans la cascade s’exécute séquentiellement. Si la vitesse devient gênante au quotidien, repassez sur AES seul, qui bénéficie de l’accélération matérielle AES-NI.

**8. Un volume caché semble endommagé après une opération de défragmentation ou d’optimisation de disque.** Ne défragmentez jamais un disque contenant un volume caché : l’outil de défragmentation ne sait pas distinguer l’espace « libre » du volume extérieur de l’espace occupé par le volume caché, et peut écrire par-dessus ce dernier. Désactivez toute optimisation planifiée sur les disques concernés.

## Astuces avancées pour aller plus loin

- **Augmentez le PIM** (Personal Iterations Multiplier) sur les volumes contenant des données très sensibles pour renforcer la résistance au brute-force, en acceptant un temps de déverrouillage plus long à chaque montage.
- **Passez à Argon2id** pour les nouveaux volumes non-système créés avec VeraCrypt 1.26.29 : cette fonction de dérivation de clé résiste mieux aux attaques accélérées par GPU ou circuits ASIC que l’ancien PBKDF2.
- **Combinez volume caché et système d’exploitation caché** (hidden OS) pour un déni plausible complet au niveau du système d’exploitation lui-même, et non plus seulement d’un conteneur de fichiers.
- **Scriptez la vérification d’intégrité** de vos sauvegardes chiffrées en comparant des sommes de contrôle SHA-256 avant et après chaque cycle de synchronisation, en plus du script de montage/démontage présenté plus haut.
- **Testez régulièrement votre disque de secours** , y compris plusieurs mois après sa création : un support USB ou un CD peut se dégrader physiquement avec le temps, rendant l’image illisible au moment précis où vous en avez besoin.

## Foire aux questions

### VeraCrypt est-il légal à utiliser en France ?

Oui. L’utilisation d’un logiciel de chiffrement comme VeraCrypt pour protéger ses propres données personnelles ou professionnelles est parfaitement légale en France. Le chiffrement est même explicitement recommandé par plusieurs textes européens, dont le RGPD, comme mesure de protection des données.

### VeraCrypt est-il vraiment sûr, sans backdoor cachée ?

Son code source est entièrement public et repose sur la base de TrueCrypt, qui a fait l’objet d’un audit indépendant complet entre 2013 et 2016 par l’Open Crypto Audit Project et QuarksLab. Cet audit n’a révélé aucune porte dérobée intentionnelle, même s’il a permis d’identifier des vulnérabilités depuis corrigées. Le caractère open source du projet signifie que n’importe quel chercheur en sécurité peut, encore aujourd’hui, examiner le code et signaler un problème.

### Que se passe-t-il si j’oublie mon mot de passe VeraCrypt ?

Il n’existe aucune méthode de récupération intégrée, aucune porte dérobée, aucun contact support capable de déchiffrer un volume sans le mot de passe ou le fichier-clé correspondant. C’est une conséquence directe et voulue de la conception du chiffrement : si une porte de récupération existait, elle constituerait elle-même une faille exploitable. Notez le mot de passe dans un gestionnaire de mots de passe fiable plutôt que de compter sur votre seule mémoire.

### VeraCrypt ralentit-il vraiment l’ordinateur au quotidien ?

Sur un processeur récent équipé d’AES-NI, la différence reste peu perceptible pour un usage bureautique avec l’algorithme AES seul. L’impact devient plus net avec les cascades d’algorithmes ou sur du matériel ancien dépourvu d’accélération matérielle dédiée au chiffrement.

### Peut-on utiliser VeraCrypt sur un NAS Synology ou QNAP ?

VeraCrypt lui-même ne s’installe pas nativement sur les systèmes d’exploitation propriétaires de ces NAS (DSM, QTS). En revanche, un conteneur VeraCrypt créé sur un ordinateur Windows, macOS ou Linux peut être stocké en tant que fichier sur un partage réseau hébergé par le NAS, puis monté depuis un ordinateur client qui exécute VeraCrypt.

### Quelle est la différence entre chiffrer un conteneur et chiffrer le disque système entier ?

Un conteneur protège uniquement les fichiers que vous y placez volontairement. Le chiffrement du disque système protège tout : le système d’exploitation, les fichiers temporaires, la mémoire virtuelle et les données de mise en veille prolongée, qui peuvent involontairement contenir des fragments de données sensibles jamais explicitement enregistrés par l’utilisateur.

### VeraCrypt fonctionne-t-il sur les Chromebook ?

Pas nativement sur ChromeOS. Un Chromebook capable de faire tourner un environnement Linux complet (via Crostini ou le mode développeur) peut en revanche installer la version Linux de VeraCrypt à l’intérieur de cet environnement, avec les limitations habituelles de ces conteneurs Linux sur ChromeOS.

### Faut-il chiffrer un SSD différemment d’un disque dur mécanique ?

VeraCrypt fonctionne sur les deux types de support, mais gardez à l’esprit une limite propre aux SSD : leurs algorithmes internes de répartition de l’usure (wear leveling) peuvent laisser subsister d’anciennes copies de données sur des cellules mémoire marquées comme réutilisables, en dehors du contrôle direct du logiciel de chiffrement. Chiffrer un SSD dès sa mise en service, avant d’y stocker des données sensibles en clair, reste la pratique la plus sûre.
