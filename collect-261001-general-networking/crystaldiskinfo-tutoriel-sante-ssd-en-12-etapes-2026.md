Un SSD ou un disque dur ne prévient presque jamais avant de rendre l’âme : un matin, la machine ne démarre plus, et des années de données s’évaporent. Pourtant, la quasi-totalité des disques modernes tiennent un journal de bord interne – la technologie **S.M.A.R.T.** – qui accumule des dizaines d’indicateurs sur leur usure réelle. Le problème, c’est que ces valeurs restent invisibles tant qu’un outil ne va pas les lire. C’est précisément le rôle de **CrystalDiskInfo**, l’utilitaire gratuit et open source qui traduit ces compteurs bruts en un verdict clair : *Bon*, *Attention* ou *Mauvais*.

Ce tutoriel vous guide pas à pas, en 12 étapes et une trentaine de minutes, pour installer **CrystalDiskInfo** dans sa version 9.9.2, publiée le 25 juillet 2026 par Crystal Dew World, décrypter les attributs S.M.A.R.T. qui comptent vraiment, configurer des alertes automatiques par e-mail et mettre en place un pipeline de surveillance qui vous prévient avant la panne. Que vous administriez un parc de PME en France, un homelab sous Proxmox ou simplement votre PC de bureau, vous saurez à la fin repérer un disque agonisant des semaines avant qu’il ne lâche.

## Qu’est-ce que CrystalDiskInfo et pourquoi surveiller la santé de vos disques ?

**CrystalDiskInfo** est un logiciel de diagnostic de stockage développé par le studio japonais Crystal Dew World (le développeur connu sous le pseudonyme hiyohiyo). Il lit les données S.M.A.R.T. (*Self-Monitoring, Analysis and Reporting Technology*) directement dans le micrologiciel de chaque disque, puis les présente dans une interface lisible. Contrairement à un simple gestionnaire de fichiers, il ne s’intéresse pas à ce que contient le disque, mais à son état physique : nombre de secteurs défectueux, heures de fonctionnement, cycles d’écriture, température, taux d’erreur. Le code source est publié sur GitHub, ce qui en fait l’un des rares outils de ce type entièrement auditable.

La question qui revient toujours : *pourquoi ne pas attendre que Windows signale un problème ?* Parce que Windows n’affiche un avertissement S.M.A.R.T. que lorsque le disque a déjà franchi un seuil critique – souvent trop tard pour sauvegarder sereinement. Les grandes études de fiabilité le confirment : dans son rapport Drive Stats, l’hébergeur Backblaze, qui surveille plus de 300 000 disques, a identifié cinq attributs S.M.A.R.T. particulièrement corrélés à une panne imminente : les secteurs réalloués (ID 05), les erreurs non corrigibles signalées (187), les délais de commande dépassés (188), les secteurs en attente (197) et les secteurs non corrigibles hors ligne (198). Quand l’un de ces compteurs commence à grimper, la panne n’est plus une question de « si », mais de « quand ».

Surveiller ces indicateurs avec **CrystalDiskInfo** transforme une panne subie en panne anticipée. Vous gagnez la fenêtre de quelques jours à quelques semaines nécessaire pour cloner le disque, commander un remplacement ou activer votre garantie constructeur – le tout avant la perte de données. Pour une surveillance de l’ensemble du système (CPU, GPU, cartes mères, ventilateurs), reportez-vous à notre tutoriel HWiNFO ; CrystalDiskInfo, lui, se concentre exclusivement et en profondeur sur le stockage.

## CrystalDiskInfo en 2026 : version 9.9.1, éditions et licence

La version stable la plus récente est **CrystalDiskInfo 9.9.2**, mise en ligne le 25 juillet 2026 par Crystal Dew World sous la forme d’un installeur EXE de 6,2 Mo et d’une archive ZIP portable de 8,5 Mo (chiffres SourceForge ; build EXE daté du 29 juillet sur Uptodown, où l’outil affiche une note de 4,5/5), succédant à la 9.9.1 du 23 mai 2026 – déjà créditée de 159 438 téléchargements hebdomadaires sur SourceForge pour son seul installeur EXE –, à la 9.9.0 du 18-19 mai 2026 et à la 9.8.0 du 16 février 2026 (distribuée en archive ZIP de 8,1 Mo). Le logiciel pèse ainsi à peine quelques mégaoctets, se lance en une seconde et reste entièrement gratuit – un modèle rare pour un outil aussi complet. Il ne faut pas le confondre avec **CrystalDiskMark**, son outil frère du même développeur : ce dernier mesure les *débits* (vitesses de lecture/écriture) du disque, tandis que CrystalDiskInfo mesure sa *santé*. Les deux sont complémentaires, mais répondent à des questions différentes.

Un point déroute souvent les nouveaux venus : CrystalDiskInfo se décline en quatre « éditions » – Standard, Shizuku, Kurei Kei et Aoi. Rassurez-vous, elles sont **fonctionnellement identiques**. Seule change l’habillage graphique : les éditions Shizuku, Kurei Kei et Aoi embarquent des thèmes illustrés par des mascottes de l’univers du logiciel, ce qui alourdit nettement le téléchargement – la build Aoi de la 9.9.2, datée elle aussi du 25 juillet 2026, pèse 65,0 Mo en EXE et 67,0 Mo en ZIP sur SourceForge, contre seulement 6,2 Mo et 8,5 Mo pour l’édition Standard équivalente. Pour une utilisation professionnelle ou un déploiement en entreprise, l’édition **Standard** est le choix évident : plus légère, sobre, sans illustration superflue.

Côté couverture matérielle, la 9.9.2 reconnaît les plateformes récentes – SSD NVMe PCIe 5.0, contrôleurs Intel les plus récents, disques SATA de grande capacité – et lit aussi bien les disques internes que la plupart des boîtiers externes USB. Elle prend en charge les architectures x86, x64 et ARM64, ce qui la rend utilisable jusque sur les PC Windows on ARM. Cette régularité de mises à jour – de la 9.6.0 du 25 février 2025 à la 9.6.3 du 12 mars 2025, puis la 9.7.0 des 16-17 juin 2025, la 9.7.1 du 28 juillet 2025 et la 9.7.2 du 1er septembre 2025, avant les 9.8.0 du 16 février 2026, les 9.9.0 et 9.9.1 – dont l’archive ZIP portable cumulait à elle seule 64 338 téléchargements hebdomadaires sur SourceForge fin mai 2026 – puis la 9.9.2 – est un gage de fiabilité : le support des nouveaux disques suit de près leur sortie.

## Prérequis et configuration système requise

Avant de commencer, vérifiez que votre environnement coche les cases suivantes. CrystalDiskInfo est peu gourmand, mais quelques prérequis conditionnent l’accès à certaines fonctions avancées (lecture NVMe, alertes par e-mail).

| Élément | Exigence minimale | Recommandé pour ce tutoriel | 
|---|---|---|
| Système d’exploitation | Windows 7 / 8 / 8.1 / 10 / 11, Server 2003 à 2025 | Windows 10 22H2 ou Windows 11 24H2 | 
| Architecture | x86, x64 ou ARM64 | x64 | 
| Lecture des disques NVMe | Windows 10 / Server 2016 ou ultérieur | Windows 11 | 
| .NET Framework | Non requis pour l’usage de base | .NET Framework 4.8+ (obligatoire pour les alertes e-mail) | 
| Privilèges | Compte administrateur (accès S.M.A.R.T. bas niveau) | Administrateur local | 
| Espace disque | ~15 Mo | ~15 Mo (version portable possible) | 
| Version du logiciel | CrystalDiskInfo 9.9.1 (édition Standard) | CrystalDiskInfo 9.9.1 Standard | 

Deux limites techniques méritent votre attention. D’abord, les **ponts USB** : tous ne relaient pas les commandes S.M.A.R.T. Un même SSD affichera ses attributs en interne (SATA/NVMe) mais restera muet dans un boîtier externe bon marché dont le contrôleur ne transmet pas le protocole. Ensuite, les **configurations RAID** : CrystalDiskInfo lit une partie des grappes Intel RST, mais pas tous les contrôleurs RAID matériels, qui masquent les disques physiques derrière un volume logique. Ces deux cas de figure ne sont pas des bugs, mais des limitations du chemin d’accès matériel.

## Étape 1 – Télécharger CrystalDiskInfo depuis la source officielle

La règle d’or : ne téléchargez **jamais** CrystalDiskInfo depuis un portail de téléchargement tiers. Ces sites empaquettent régulièrement l’installeur avec des logiciels indésirables (barres d’outils, « optimiseurs » douteux). Rendez-vous exclusivement sur le site officiel Crystal Dew World, sur le dépôt SourceForge du projet, ou via le Microsoft Store (édition sans publicité, mise à jour automatiquement).

Sur la page de téléchargement, vous rencontrerez deux décisions. La première : quelle **édition** – choisissez « CrystalDiskInfo » (Standard), sans suffixe. La seconde : **installeur ou version portable**. L’installeur (fichier `.exe`) s’intègre au menu Démarrer et gère les mises à jour ; la version portable (archive `.zip`) s’extrait dans un dossier et se lance sans installation – idéale pour une clé USB de dépannage ou un poste verrouillé, et plébiscitée par les utilisateurs (la build Portable 9.9.0.0 du 19 mai 2026 affiche une note de 5/5 sur Uptodown, avec 1 343 téléchargements hebdomadaires sur SourceForge contre 2 376 pour l’installeur EXE équivalent de la même version). Notez que les installeurs récents incluent parfois une offre sponsorisée : la build « 9.9.1 Ads » du 5 juin 2026 pèse par exemple 15,9 Mo – contre 6,2 Mo pour l’EXE classique – et totalisait pourtant 57 963 téléchargements hebdomadaires sur SourceForge. Si vous voulez un exécutable strictement sans publicité, préférez plutôt la dernière archive ZIP portable, entièrement propre.

Vérifiez toujours l’empreinte du fichier lorsque la source la publie. Sous PowerShell, une commande suffit à confirmer que le binaire téléchargé n’a pas été altéré :

```
# Calculer l'empreinte SHA-256 de l'installeur téléchargé
Get-FileHash -Algorithm SHA256 "$env:USERPROFILE\Downloads\CrystalDiskInfo9_9_1.exe"
# Sortie attendue (exemple) :
# Algorithm  Hash                              Path
# ---------  ----                              ----
# SHA256     3F9A1C...E7B2                     C:\Users\vous\Downloads\CrystalDiskInfo9_9_1.exe
```
## Étape 2 – Installer CrystalDiskInfo (ou utiliser la version portable)

Si vous avez choisi l’installeur, double-cliquez sur le `.exe` et acceptez l’invite du Contrôle de compte d’utilisateur (UAC) – cette élévation est indispensable, car la lecture S.M.A.R.T. exige un accès matériel de bas niveau. Suivez l’assistant : acceptez la licence, conservez le dossier d’installation par défaut, et surveillez chaque écran. **Décochez toute offre optionnelle** (logiciel additionnel, page d’accueil, extension de navigateur) qui pourrait apparaître. L’installation se termine en quelques secondes.

Pour la version portable, extrayez simplement l’archive ZIP dans un dossier de votre choix, par exemple `C:\Outils\CrystalDiskInfo\`. Le dossier contient plusieurs exécutables : choisissez celui adapté à votre système. La convention de nommage est la suivante :

```
CrystalDiskInfo\
├── DiskInfo.exe        # Version 32 bits (x86)
├── DiskInfo64.exe      # Version 64 bits (x64) – a privilegier
├── DiskInfoA64.exe     # Version ARM64 (Windows on ARM)
├── DiskInfo.ini        # Fichier de configuration (cree au 1er lancement)
└── Language\           # Fichiers de langue (dont Francais.lang)
```
Lancez `DiskInfo64.exe` sur un PC 64 bits moderne. Astuce pour les administrateurs : la version portable est parfaite pour un déploiement en masse via GPO ou script, puisqu’elle ne laisse aucune trace dans le registre et se pilote entièrement par son fichier `DiskInfo.ini` – nous exploiterons cette propriété à l’étape 10.

## Étape 3 – Premier lancement et vue d’ensemble de l’interface

Au premier démarrage, CrystalDiskInfo interroge tous les disques détectés et affiche sa fenêtre principale. Prenez le temps de la décoder, car chaque zone a un rôle précis :

- **La barre d’onglets supérieure** : un onglet par disque détecté. Cliquez pour basculer d’un disque à l’autre. La couleur de l’onglet reflète déjà l’état de santé.
- **Le cartouche « État de santé »** (à gauche) : le verdict global – Bon, Attention ou Mauvais – accompagné d’un pourcentage (surtout pertinent pour les SSD, où il reflète l’usure restante).
- **Le cartouche « Température »** : la température instantanée du disque, en °C.
- **Le bandeau d’informations** : modèle, micrologiciel (firmware), numéro de série, capacité, interface (Serial ATA, NVM Express…), mode de transfert négocié, lettre de lecteur et fonctionnalités supportées (S.M.A.R.T., APM, AAM, TRIM…).
- **Le tableau S.M.A.R.T.** (partie basse) : la liste complète des attributs, cœur du diagnostic, que nous détaillons à l’étape 5.

Première action recommandée : passer l’interface en français. Ouvrez le menu `Language` (ou `言語`) et sélectionnez **Français**. Toute l’interface, y compris les libellés d’attributs, bascule alors dans la langue de Molière. Ensuite, si vous gérez plusieurs disques, notez que l’ordre des onglets suit l’énumération matérielle, pas les lettres de lecteur : le disque « (1) » n’est pas forcément votre lecteur C:.

## Étape 4 – Lire et interpréter l’état de santé (Bon / Attention / Mauvais)

Le cartouche « État de santé » est la synthèse que 90 % des utilisateurs consulteront. CrystalDiskInfo agrège l’ensemble des attributs S.M.A.R.T. et les compare à leurs seuils constructeur pour produire un verdict codé par couleur. Voici la grille de lecture complète :

| État | Couleur | Signification | Action recommandée | 
|---|---|---|---|
| Bon | Bleu (ou vert) | Tous les attributs sont dans les limites normales | Aucune ; surveillance périodique | 
| Attention | Jaune | Un attribut a franchi un seuil d’alerte (secteurs réalloués, en attente, usure élevée…) | Sauvegarder immédiatement, planifier le remplacement | 
| Mauvais | Rouge | Un attribut critique a dépassé le seuil de panne | Cloner d’urgence, cesser tout usage important | 
| Inconnu | Gris | Le S.M.A.R.T. n’est pas lisible (pont USB, RAID, disque virtuel) | Brancher en SATA/NVMe direct pour diagnostiquer | 

Le point le plus mal compris concerne l’état **Attention** (jaune). Il ne signifie pas « panne imminente dans l’heure », mais « un compteur a commencé à se dégrader ». Sur un disque dur, quelques secteurs réalloués peuvent rester stables des années ; en revanche, si le compteur *augmente* à chaque analyse, la spirale est enclenchée. La règle pratique : ce n’est pas la valeur absolue qui compte, mais sa **tendance**. Un état Attention stable se surveille ; un état Attention qui progresse impose le remplacement.

Sur les SSD, le pourcentage de santé mérite une lecture nuancée. Il reflète l’usure des cellules NAND (endurance d’écriture), pas un défaut mécanique. Un SSD à 90 % après trois ans est parfaitement sain ; il ne tombera « Attention » que lorsque l’usure approchera de la limite garantie (le fameux TBW, *Total Bytes Written*). Nous y revenons en détail à l’étape 11.

## Étape 5 – Décrypter les attributs S.M.A.R.T. essentiels

Le tableau S.M.A.R.T. affiche pour chaque attribut : un identifiant (ID en hexadécimal), les valeurs *Actuelle* / *Pire* / *Seuil* (normalisées de 1 à 253, où plus haut = mieux) et une colonne **Valeurs brutes** (RawValues) – c’est cette dernière, en clair, qui vous intéresse vraiment. La norme S.M.A.R.T. définit des dizaines d’attributs, mais une poignée suffit à diagnostiquer 95 % des cas. Pour un disque dur mécanique :

| ID | Attribut | Ce qu’il révèle | Valeur brute idéale | 
|---|---|---|---|
| 05 | Secteurs réalloués | Secteurs défectueux remplacés par des secteurs de réserve | 0 (toute hausse est un signal d’alerte) | 
| C5 | Secteurs en attente (Current Pending) | Secteurs instables en cours de surveillance | 0 | 
| C6 | Secteurs non corrigibles (Uncorrectable) | Secteurs définitivement illisibles | 0 | 
| C4 | Événements de réallocation | Nombre de tentatives de réallocation | 0 | 
| 0A | Nouvelles tentatives de rotation (Spin Retry) | Difficulté du moteur à atteindre sa vitesse | 0 | 
| C2 | Température | Température du disque en °C | < 45 °C | 
| 09 | Heures de fonctionnement | Durée d’utilisation cumulée | Indicatif (usure globale) | 

Les attributs 05, C5 et C6 forment le **trio de la mort** pour un disque dur : dès que l’un d’eux quitte le zéro et progresse, planifiez le remplacement. À l’inverse, un compteur « Heures de fonctionnement » élevé n’est pas alarmant en soi – un disque peut tourner 60 000 heures sans un seul secteur réalloué. Pour un SSD (SATA ou NVMe), la grille change complètement, car il n’y a plus de pièces mobiles ; c’est l’endurance d’écriture qui prime :

| Attribut (SSD / NVMe) | Ce qu’il révèle | À surveiller | 
|---|---|---|
| Percentage Used (NVMe) | Usure estimée par le contrôleur (0 % = neuf, 100 % = fin de garantie d’endurance) | Approche de 100 % | 
| Total Host Writes / TBW | Volume total écrit depuis l’origine | Comparer au TBW garanti du constructeur | 
| Available Spare (NVMe) | Réserve de blocs de secours restante | Chute sous le « Spare Threshold » | 
| Wear Leveling Count (SSD SATA) | Cycles d’effacement moyens des cellules | Décroît vers son seuil | 
| Unsafe Shutdowns | Coupures d’alimentation brutales | Hausse anormale (alim/onduleur) | 
| Media and Data Integrity Errors | Erreurs d’intégrité des données | Toute valeur > 0 | 

## Étape 6 – Surveiller la température de vos disques

La température est un tueur silencieux de stockage, en particulier pour les SSD NVMe M.2 logés sous une carte graphique ou dans un ordinateur portable exigu. CrystalDiskInfo affiche la température instantanée dans son cartouche dédié, et l’attribut C2 (ou « Composite Temperature » en NVMe) en conserve l’historique haut/bas. Les plages à retenir : en dessous de 40 °C, tout va bien ; entre 45 et 55 °C, c’est acceptable sous charge ; au-delà de 60-70 °C, un SSD NVMe déclenche généralement une *limitation thermique* (throttling) qui réduit ses performances pour se protéger.

CrystalDiskInfo peut afficher la température en permanence dans la **zone de notification** de Windows (barre des tâches), sous forme d’icône numérique colorée. Activez cette option depuis `Fonction → Notification → Icône de température`. Vous obtenez alors une surveillance passive : un coup d’œil à la barre des tâches suffit à repérer un disque qui chauffe anormalement. Si vous constatez des pics récurrents, la solution passe souvent par un dissipateur M.2 (radiateur) ou une meilleure ventilation du boîtier – pas par le logiciel.

Petite subtilité d’affichage : certains disques rapportent deux capteurs de température (par exemple le contrôleur et la NAND sur un NVMe). CrystalDiskInfo affiche alors la valeur la plus élevée ou les deux selon le modèle. Ce n’est pas une anomalie ; c’est la richesse du télémétrage du disque qui transparaît.

## Étape 7 – Régler les seuils d’alerte de santé et de température

Par défaut, CrystalDiskInfo se contente d’afficher l’état. Pour qu’il vous *alerte* activement, configurez les seuils. Ouvrez `Fonction → Paramètres avancés` : vous y trouverez la définition des seuils de santé et de température par disque. Vous pouvez par exemple décider qu’un disque doit passer en « Attention » dès le premier secteur réalloué, ou qu’une alarme se déclenche au-delà de 55 °C.

Ces seuils personnalisés sont particulièrement utiles pour un parc hétérogène. Sur un serveur de fichiers, vous serez plus strict (alerte à 50 °C, tolérance zéro sur les secteurs en attente) ; sur un vieux PC secondaire, vous pourrez lever le pied. La documentation complète de ces réglages figure dans le manuel des fonctionnalités avancées de Crystal Dew World. Une fois les seuils définis, associez-leur une réaction : une alarme sonore, une icône, ou – le plus utile – une notification par e-mail, que nous configurons à l’étape suivante.

## Étape 8 – Activer les alertes par e-mail (Alert Mail)

C’est la fonction qui transforme CrystalDiskInfo d’un outil de consultation en véritable sentinelle. La fonction **Alert Mail** envoie un courriel automatique dès qu’un seuil est franchi. Prérequis impératif : **.NET Framework 4.8 ou supérieur** doit être installé, faute de quoi l’option reste grisée. Rendez-vous dans `Fonction → Paramètres avancés → Notification par e-mail` et renseignez les paramètres SMTP de votre fournisseur.

Voici un exemple de configuration SMTP fonctionnelle, ici avec un serveur classique en TLS sur le port 587. Adaptez-la à votre fournisseur (Gmail exige un mot de passe d’application, OVH ou Infomaniak utilisent leurs propres hôtes SMTP) :

```
Serveur SMTP    : smtp.votre-domaine.fr
Port            : 587 (STARTTLS) ou 465 (SSL)
Authentification: activee
Utilisateur     : [email protected]
Mot de passe    : ******** (mot de passe d'application si Gmail/Outlook)
Expediteur      : [email protected]
Destinataire    : [email protected]
Objet           : [CrystalDiskInfo] Alerte disque sur POSTE-01
```
Après la saisie, utilisez impérativement le bouton **« Envoyer un e-mail de test »**. Il valide en direct la connexion SMTP et vous évite de découvrir, le jour d’une vraie panne, que l’authentification échouait silencieusement. Une fois le test reçu, votre poste enverra automatiquement un courriel à chaque passage en « Attention » ou « Mauvais ». Pour un administrateur gérant plusieurs sites en France ou en Europe, centraliser ces alertes sur une boîte dédiée (par exemple `stockage-alertes@`) crée un tableau de bord de fait, sans aucun serveur de supervision supplémentaire.

## Étape 9 – Activer le mode Résident et le lancement au démarrage

Une alerte e-mail ne sert à rien si le logiciel n’est pas ouvert. Le mode **Résident** résout ce problème : CrystalDiskInfo se réduit dans la zone de notification et continue de surveiller les disques en arrière-plan, à intervalle régulier. Activez-le via `Fonction → Résident`. Combinez-le avec `Fonction → Démarrage` pour que l’outil se lance automatiquement à l’ouverture de session Windows.

Réglez ensuite la fréquence de vérification via `Fonction → Paramètres avancés → Actualisation automatique`. Un intervalle de 10 minutes constitue un bon compromis pour un poste de travail ; sur un serveur, 30 à 60 minutes suffisent et allègent l’accès disque. Attention : une actualisation trop fréquente (toutes les minutes) réveille en permanence les disques durs mécaniques qui tentent de se mettre en veille, ce qui use inutilement le mécanisme. Sur SSD, l’impact est négligeable.

En version portable, ces trois réglages (Résident, Démarrage, Actualisation) s’inscrivent dans le fichier `DiskInfo.ini`. Voici à quoi ressemble une configuration type prête pour un déploiement automatisé – vous pouvez la distribuer telle quelle sur un parc :

```
[Setting]
Language=French
AutoRefresh=10          ; actualisation toutes les 10 minutes
Startup=1               ; lancement au demarrage de Windows
ResidentMinimize=1      ; demarrer reduit dans la zone de notification
AlarmTemperature=55     ; seuil d'alarme temperature (deg C)
AlarmHealthStatus=1     ; alarme si l'etat passe sous "Bon"
MailAlert=1             ; activer l'envoi d'e-mail sur alerte
TrayTemperature=1       ; afficher la temperature dans la barre des taches
```
Les noms de clés exacts peuvent varier légèrement selon la version ; le plus fiable reste de configurer une machine « modèle » via l’interface graphique, puis de récupérer son `DiskInfo.ini` comme gabarit à répliquer.

## Étape 10 – Automatiser l’export des rapports S.M.A.R.T.

Pour l’administration d’un parc, l’idéal est de collecter périodiquement l’état de tous les disques sans intervention. CrystalDiskInfo expose pour cela des **options en ligne de commande** officielles, particulièrement pratiques pour le scripting :

- `/Copy` – met à jour les données puis écrit le résultat de « Édition → Copier » dans un fichier`DiskInfo.txt` .
- `/CopyExit` – identique, mais ferme l’application ensuite (parfait pour une tâche planifiée).
- `/Exit` – met à jour les valeurs S.M.A.R.T. et l’état AAM/APM, puis quitte.

Créons une tâche planifiée Windows qui exporte l’état de tous les disques chaque nuit. Le script batch suivant lance CrystalDiskInfo en mode `/CopyExit`, puis archive le rapport horodaté :

```
@echo off
REM export-smart.bat -- export nocturne du rapport S.M.A.R.T.
set "CDI=C:\Outils\CrystalDiskInfo\DiskInfo64.exe"
set "LOGDIR=C:\Logs\SMART"
if not exist "%LOGDIR%" mkdir "%LOGDIR%"
REM Genere DiskInfo.txt dans le dossier du programme, puis quitte
"%CDI%" /CopyExit
REM Horodate et archive le rapport
set "STAMP=%date:~-4%%date:~3,2%%date:~0,2%"
copy /Y "C:\Outils\CrystalDiskInfo\DiskInfo.txt" "%LOGDIR%\smart-%COMPUTERNAME%-%STAMP%.txt"
```
Planifiez ensuite ce script via le Planificateur de tâches Windows, en une seule ligne PowerShell exécutée en administrateur :

```
# Cree une tache quotidienne a 03h00 qui exporte l'etat S.M.A.R.T.
$action  = New-ScheduledTaskAction -Execute "C:\Outils\CrystalDiskInfo\export-smart.bat"
$trigger = New-ScheduledTaskTrigger -Daily -At 3am
$princ   = New-ScheduledTaskPrincipal -UserId "SYSTEM" -RunLevel Highest
Register-ScheduledTask -TaskName "Export-SMART-CrystalDiskInfo" `
  -Action $action -Trigger $trigger -Principal $princ `
  -Description "Export nocturne des rapports S.M.A.R.T. via CrystalDiskInfo"
```
Vous disposez désormais d’un historique daté, poste par poste, exploitable pour repérer les tendances (un secteur réalloué qui apparaît, une température qui grimpe semaine après semaine). Cette approche par ligne de commande est la même que celle documentée par la communauté DevOps, notamment sur CyberDrain pour l’intégrer à un outil de supervision RMM.

## Étape 11 – Surveiller spécifiquement les SSD NVMe (TBW et endurance)

Les SSD NVMe imposent une lecture différente. Ici, pas de secteurs mécaniques : le risque n’est pas la panne brutale mais l’**épuisement de l’endurance d’écriture**. Trois indicateurs concentrent l’attention. Le *Percentage Used* exprime, en pourcentage, l’usure estimée par le contrôleur : à 100 %, le disque a consommé l’endurance garantie (il continue souvent de fonctionner, mais hors garantie). L’*Available Spare* indique la réserve de blocs de secours ; s’il chute sous son seuil (« Spare Threshold »), le SSD est en fin de vie. Enfin, le *Total Host Writes* (ou *Data Units Written*) totalise les octets écrits, à comparer au TBW inscrit sur la fiche technique.

Un exemple concret : un SSD NVMe de 2 To garanti pour 1 200 TBW. Si CrystalDiskInfo affiche 150 To écrits après deux ans, vous avez consommé 12,5 % de l’endurance – le disque tiendra largement au-delà de sa garantie de cinq ans à ce rythme. Voici un extrait de rapport `DiskInfo.txt` réel pour un tel SSD, tel qu’exporté à l’étape 10 :

```
----------------------------------------------------------------------------
 (1) Samsung SSD 990 PRO 2TB
----------------------------------------------------------------------------
           Modele : Samsung SSD 990 PRO 2TB
    Micrologiciel : 4B2QJXD7
        Interface : NVM Express
   Mode transfert : PCIe 4.0 x4 | PCIe 4.0 x4
   Lettre lecteur : C:
    Etat de sante : Bon (94 %)
      Temperature : 43 C (109 F)
   Heures fonct.  : 6821 heures
    Nb demarrages : 742
  Total ecritures : 150331 GB
   Total lectures : 214887 GB
-- S.M.A.R.T. --------------------------------------------------------------
ID Actuel Pire Seuil  Valeurs brutes   Nom de l'attribut
01  100   100   __0   000000000000     Critical Warning
05  100   100   __0   000000000006     Percentage Used  (6 %)
07  100   100   __0   000000000064     Available Spare  (100 %)
0C  100   100   __0   00000002C7A1     Unsafe Shutdowns
```
Pour recouper ces valeurs, Windows propose un complément natif via PowerShell. Les compteurs de fiabilité de stockage exposent une partie des mêmes données, utile pour un contrôle croisé ou une supervision sans installer de logiciel :

```
# Lire l'usure et la temperature via PowerShell (complement a CrystalDiskInfo)
Get-PhysicalDisk | Get-StorageReliabilityCounter |
  Select-Object DeviceId, Wear, Temperature, ReadErrorsTotal, PowerOnHours |
  Format-Table -AutoSize
# Afficher l'etat de sante global vu par Windows
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, HealthStatus, OperationalStatus
```
La colonne `Wear` renvoyée par Windows correspond au *Percentage Used* de CrystalDiskInfo. Si les deux outils divergent nettement, faites confiance à CrystalDiskInfo, qui lit directement le journal S.M.A.R.T. du contrôleur plutôt qu’une abstraction du système.

## Étape 12 – Gérer l’AAM/APM et exporter un diagnostic complet

Dernière étape, souvent méconnue : CrystalDiskInfo peut aussi *agir* sur certains disques via `Fonction → Paramètres avancés → Contrôle AAM/APM`. L’**AAM** (Automatic Acoustic Management) ajuste le compromis bruit/performance de la tête de lecture d’un disque dur ; l’**APM** (Advanced Power Management) règle son agressivité de mise en veille. Ces réglages ne concernent que les disques durs mécaniques compatibles – la plupart des SSD les ignorent. Sur un vieux disque bruyant, réduire l’AAM peut sensiblement diminuer le cliquetis, au prix d’un léger recul de performance.

Pour clore votre diagnostic, générez un rapport complet à archiver ou à joindre à une demande de garantie (RMA). Le menu `Édition → Copier` place l’intégralité du rapport dans le presse-papiers ; collez-le dans un fichier texte. Ce rapport contient le modèle, le numéro de série, le micrologiciel et l’ensemble des attributs S.M.A.R.T. – exactement ce qu’un service après-vente demandera pour valider un remplacement sous garantie. Conservez-le : en cas de litige avec un vendeur, un rapport S.M.A.R.T. daté fait office de preuve technique objective.

## Projet complet : un pipeline de surveillance automatisée

Assemblons maintenant les briques précédentes en un système de surveillance autonome. L’objectif : un poste qui exporte son état S.M.A.R.T. chaque nuit (étape 10), qu’un script analyse pour détecter tout état « Attention » ou « Mauvais » et vous alerte immédiatement. Le script PowerShell suivant lit le dernier rapport `DiskInfo.txt`, en extrait chaque disque et son état de santé, et envoie un courriel si un problème est détecté :

```
# watchdog-smart.ps1 -- analyse le rapport CrystalDiskInfo et alerte
$rapport = "C:\Outils\CrystalDiskInfo\DiskInfo.txt"
$contenu = Get-Content $rapport -Encoding UTF8
# Extraire les lignes "Etat de sante" de chaque disque
$etats = $contenu | Select-String -Pattern "Etat de sante|Health Status"
$alertes = @()
foreach ($ligne in $etats) {
    if ($ligne -match "Attention|Caution|Mauvais|Bad") {
        $alertes += $ligne.Line.Trim()
    }
}
if ($alertes.Count -gt 0) {
    $corps = "Disque(s) a risque sur $env:COMPUTERNAME :`n`n" + ($alertes -join "`n")
    Send-MailMessage -SmtpServer "smtp.votre-domaine.fr" -Port 587 -UseSsl `
        -From "[email protected]" -To "[email protected]" `
        -Subject "[SMART] Alerte disque sur $env:COMPUTERNAME" -Body $corps `
        -Credential (Import-Clixml "C:\Outils\smtp-cred.xml")
    Write-Host "ALERTE envoyee : $($alertes.Count) disque(s) concerne(s)."
} else {
    Write-Host "Tous les disques sont en bon etat."
}
```
Enchaînez ce watchdog juste après l’export nocturne (une seconde tâche planifiée à 03h10, par exemple). Vous obtenez alors une supervision de bout en bout, gratuite et sans serveur : CrystalDiskInfo lit le matériel, le batch exporte, PowerShell analyse et alerte. Pour sécuriser les identifiants SMTP, stockez-les chiffrés avec `Get-Credential | Export-Clixml "C:\Outils\smtp-cred.xml"` – le fichier n’est déchiffrable que par le compte qui l’a créé. Ce montage, éprouvé, se déploie identiquement sur dix ou mille postes.

## Erreurs fréquentes à éviter avec CrystalDiskInfo

Même un outil aussi simple réserve des pièges. Voici les cinq erreurs qui reviennent le plus souvent, et comment les contourner :

- **Confondre valeur brute et valeur normalisée.** La colonne normalisée (Actuel/Pire/Seuil) est trompeuse : « 100 » n’y veut pas dire « 100 % de santé ». Fiez-vous à la colonne*Valeurs brutes* , en clair, pour interpréter un attribut.
- **Paniquer devant un état « Attention » stable.** Quelques secteurs réalloués figés depuis des mois ne justifient pas un remplacement immédiat. C’est la*progression* du compteur qui compte, pas sa simple présence.
- **Télécharger depuis un site tiers.** Les portails de téléchargement injectent des logiciels indésirables. Passez toujours par la source officielle ou le Microsoft Store.
- **Diagnostiquer via un boîtier USB.** Beaucoup de ponts USB masquent le S.M.A.R.T. et affichent « Inconnu ». Pour un vrai diagnostic, branchez le disque en SATA ou NVMe direct.
- **Oublier .NET Framework 4.8 pour les e-mails.** Sans lui, la fonction Alert Mail reste grisée. Installez-le avant de configurer les alertes.

Une sixième erreur, plus insidieuse, mérite d’être citée : croire que CrystalDiskInfo *répare* un disque. Il ne fait que *lire* et *diagnostiquer*. Il n’effectue ni analyse de surface, ni correction d’erreurs, ni mise à jour de micrologiciel. Un état « Mauvais » ne se « corrige » pas dans le logiciel : il impose une sauvegarde et un remplacement.

## Dépannage : 8 problèmes courants et leurs solutions

Voici les huit situations les plus signalées par les utilisateurs de CrystalDiskInfo, avec la marche à suivre pour chacune.

| Problème | Cause probable | Solution | 
|---|---|---|
| État « Inconnu » (gris) | Pont USB ou RAID masquant le S.M.A.R.T. | Brancher en SATA/NVMe direct ; tester un autre boîtier | 
| Le disque n’apparaît pas du tout | Manque de privilèges administrateur | Relancer en tant qu’administrateur (clic droit) | 
| Aucun SSD NVMe détecté | Windows antérieur à la version 10 | Mettre à jour vers Windows 10/11 | 
| Option Alert Mail grisée | .NET Framework 4.8 absent | Installer .NET Framework 4.8 ou supérieur | 
| E-mail de test qui échoue | Port/SSL ou mot de passe SMTP incorrect | Vérifier port 587/465 ; mot de passe d’application | 
| Température affichée aberrante | Capteur mal interprété (vieux disque/pont) | Recouper avec PowerShell ; ignorer si valeur figée | 
| Attributs vendeur illisibles | Noms d’attributs propriétaires | Consulter la fiche du constructeur ; c’est normal | 
| L’outil ne se lance pas au démarrage | Option « Démarrage » non cochée / bloquée par l’antivirus | Activer Fonction → Démarrage ; ajouter une exception | 

Cas particulier récurrent : après une mise à jour majeure de Windows, l’icône de température peut disparaître de la barre des tâches. Il suffit de rouvrir `Fonction → Notification → Icône de température` pour la restaurer. De même, si vous migrez d’un poste à l’autre, copiez le fichier `DiskInfo.ini` pour retrouver instantanément tous vos seuils et réglages.

## Astuces avancées pour les administrateurs et passionnés

Une fois les bases maîtrisées, plusieurs techniques élèvent votre pratique de la surveillance de stockage :

- **Croiser avec les outils constructeur.** Samsung Magician, WD Dashboard ou Crucial Storage Executive offrent des fonctions (mise à jour de micrologiciel, Over-Provisioning) absentes de CrystalDiskInfo. Utilisez-les en complément, pas en remplacement, du diagnostic S.M.A.R.T.
- **Cloner avant qu’il ne soit trop tard.** Dès qu’un disque passe « Attention » avec un compteur en progression, clonez-le immédiatement (Macrium Reflect, Clonezilla) tant qu’il est encore lisible. Attendre l’état « Mauvais », c’est parier sur la chance.
- **Documenter le TBW à l’achat.** Notez le*Total Host Writes* le jour de l’installation d’un SSD. Vous saurez ensuite précisément votre rythme d’écriture quotidien et pourrez extrapoler la durée de vie réelle.
- **Surveiller les « Unsafe Shutdowns ».** Une hausse rapide de ce compteur trahit un problème d’alimentation ou l’absence d’onduleur – un risque pour l’intégrité des données bien plus immédiat que l’usure.
- **Coupler à une supervision globale.** Sur un homelab, exportez les rapports vers un serveur central. Notre tutoriel Proxmox VE montre comment bâtir l’infrastructure de stockage qui héberge ce type de collecte.

Enfin, gardez à l’esprit la complémentarité des outils. CrystalDiskInfo excelle sur la santé des disques, mais pour le reste du matériel – cartes graphiques, processeurs, tensions – un utilitaire comme HWiNFO prend le relais, et pour nettoyer proprement des pilotes GPU récalcitrants, notre guide DDU reste la référence. La surveillance d’un PC fiable repose sur une boîte à outils, pas sur un logiciel unique.

## CrystalDiskInfo face aux alternatives

CrystalDiskInfo n’est pas seul sur ce créneau. Comprendre ses forces et ses limites face aux alternatives vous aide à choisir le bon outil selon le contexte.

| Outil | Type | Points forts | Limites | 
|---|---|---|---|
| CrystalDiskInfo 9.9.1 | Gratuit, open source | S.M.A.R.T. détaillé, alertes e-mail, résident, CLI | Disques uniquement ; pas de réparation | 
| HWiNFO | Gratuit | Surveillance de tout le système (CPU/GPU/disques) | Moins détaillé sur l’endurance SSD | 
| smartmontools (smartctl) | Gratuit, open source | Multiplateforme, scriptable, très complet | Ligne de commande, courbe d’apprentissage | 
| Outils constructeur (Magician, WD Dashboard…) | Gratuit | Mise à jour firmware, Over-Provisioning | Limités à une seule marque | 
| Hard Disk Sentinel | Payant (version gratuite bridée) | Prédiction de durée de vie, historique | Fonctions clés payantes | 

Le verdict : pour la lecture S.M.A.R.T. sous Windows, CrystalDiskInfo offre le meilleur rapport simplicité/profondeur, entièrement gratuit et sans marque imposée. Les administrateurs Linux ou multiplateformes préféreront `smartctl` (de la suite smartmontools), plus scriptable. Et pour une prédiction de durée de vie plus élaborée avec historique graphique, Hard Disk Sentinel reste une option, moyennant licence. Dans la majorité des cas – poste de travail, PME, homelab – CrystalDiskInfo couvre 100 % du besoin de diagnostic.

### À lire également sur Tech-Insider

## Foire aux questions (FAQ)

### CrystalDiskInfo est-il vraiment gratuit et sûr ?

Oui. CrystalDiskInfo est un logiciel open source et entièrement gratuit, développé par Crystal Dew World, dont le code est publié sur GitHub. Il est sûr à condition de le télécharger depuis la source officielle, SourceForge ou le Microsoft Store. Méfiez-vous uniquement des portails tiers qui empaquettent parfois l’installeur avec des logiciels indésirables.

### Quelle est la dernière version de CrystalDiskInfo en 2026 ?

La version stable la plus récente est CrystalDiskInfo 9.9.2, publiée le 25 juillet 2026 sous la forme d’un installeur de 6,2 Mo et d’une archive portable de 8,5 Mo (build daté du 29 juillet sur Uptodown, noté 4,5/5). Elle succède à la 9.9.1 du 23 mai 2026 – déjà à 159 438 téléchargements hebdomadaires sur SourceForge –, à la 9.9.0 de la mi-mai 2026 et à la 9.8.0 du 16 février 2026 (archive ZIP de 8,1 Mo) – qui elle-même prolongeait la lignée 9.7.x lancée à l’été 2025. L’édition Standard est recommandée pour la plupart des usages ; les éditions Shizuku, Kurei Kei et Aoi, nettement plus lourdes (jusqu’à 65-67 Mo pour l’édition Aoi), n’ajoutent qu’un habillage graphique.

### Que signifie l’état « Attention » (jaune) ?

Il signale qu’un attribut S.M.A.R.T. a franchi un seuil d’alerte – le plus souvent des secteurs réalloués ou en attente sur un disque dur, ou une usure élevée sur un SSD. Ce n’est pas une panne immédiate, mais un avertissement. Surveillez si le compteur progresse : une valeur stable se tolère, une valeur qui augmente impose de sauvegarder et de remplacer le disque.

### CrystalDiskInfo peut-il réparer un disque défectueux ?

Non. CrystalDiskInfo est un outil de diagnostic : il lit et interprète les données S.M.A.R.T., mais n’effectue aucune réparation, analyse de surface ou correction d’erreurs. Un état « Mauvais » impose une sauvegarde immédiate et le remplacement du disque, pas une tentative de réparation logicielle.

### Pourquoi mon SSD affiche-t-il « Inconnu » ou aucune donnée ?

Deux causes principales. Soit vous consultez le disque via un boîtier USB dont le pont ne relaie pas les commandes S.M.A.R.T. – branchez-le alors en SATA ou NVMe direct. Soit vous n’avez pas lancé le logiciel en administrateur, ce qui bloque l’accès matériel de bas niveau. Pour les SSD NVMe, vérifiez aussi que vous êtes sous Windows 10 ou ultérieur.

### Comment surveiller la santé d’un SSD sur le long terme ?

Activez le mode Résident et le lancement au démarrage pour une surveillance continue, réglez une actualisation automatique (10 minutes convient), et configurez des alertes par e-mail. Pour un parc, automatisez l’export via l’option en ligne de commande `/CopyExit` et une tâche planifiée, puis analysez les rapports avec un script PowerShell comme celui présenté dans ce tutoriel.

### Quelle température est dangereuse pour un disque ?

En dessous de 40 °C, tout va bien. Entre 45 et 55 °C, c’est acceptable sous charge. Au-delà de 60-70 °C, un SSD NVMe déclenche une limitation thermique qui réduit ses performances, et une exposition prolongée à ces températures accélère l’usure. Si vous constatez des pics récurrents, ajoutez un dissipateur M.2 ou améliorez la ventilation du boîtier.

### CrystalDiskInfo et CrystalDiskMark, est-ce la même chose ?

Non, ce sont deux outils distincts du même développeur. CrystalDiskInfo mesure la *santé* du disque (données S.M.A.R.T., température, usure). CrystalDiskMark mesure ses *performances* (vitesses de lecture et d’écriture). Ils sont complémentaires : l’un vous dit si le disque va bien, l’autre s’il est rapide.

*Article publié le 03 juin 2026. Les versions logicielles et données techniques citées reflètent les informations disponibles à cette date. Vérifiez toujours l’état de vos sauvegardes avant toute manipulation sur un disque signalé en « Attention » ou « Mauvais ».*