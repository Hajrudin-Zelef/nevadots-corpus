---
id: collect-261121-veille-ia/nvidia-hardware/display-driver-uninstaller-ddu-guide-12-etapes-2026
title: "Display Driver Uninstaller (DDU) : Guide 12 Étapes 2026"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["display", "driver", "uninstaller", "pilotes", "pilote", "nvidia", "guide", "jour"]
source: https://tech-insider.org/fr/display-driver-uninstaller-ddu-pilotes-gpu-2026/
source_anchor: ""
source_lines: [1, 349]
section: "NVIDIA_Hardware"
---
Un écran noir au démarrage, des micro-saccades qui apparaissent après une mise à jour, une carte graphique qui refuse de tenir son overclock : neuf fois sur dix, le coupable n’est pas le matériel mais un pilote mal désinstallé. **Display Driver Uninstaller**, plus connu sous l’acronyme **DDU**, est l’outil de référence pour effacer la moindre trace d’un pilote graphique NVIDIA, AMD ou Intel avant d’en installer un neuf. Ce tutoriel vous guide pas à pas, de la sauvegarde jusqu’à la réinstallation propre, avec les commandes PowerShell, les commutateurs en ligne de commande et les pièges spécifiques à Windows 11 24H2.

Comptez environ 30 minutes pour un nettoyage complet en mode sans échec. À l’heure où le prix des cartes graphiques flambe en Europe, savoir entretenir proprement la vôtre vaut largement l’investissement en temps. Toutes les versions et données de ce guide ont été vérifiées au 17 août 2026, en tenant compte de la sortie de **DDU 18.1.5.6** le 17 juillet 2026 sur le site officiel Wagnardsoft, désormais la référence à jour.

## Qu’est-ce que Display Driver Uninstaller (DDU) ?

**Display Driver Uninstaller** est un utilitaire gratuit développé par Wagnard (studio Wagnardsoft) qui supprime intégralement les pilotes graphiques et leurs résidus : entrées de registre, fichiers, services, dossiers temporaires et clés de configuration que le désinstalleur classique de Windows laisse derrière lui. Là où le Panneau de configuration retire le paquet principal, DDU racle jusqu’aux dernières traces, ce qui en fait la méthode la plus fiable pour repartir d’une base saine.

La version stable au moment de la rédaction est **DDU 18.1.5.6**, publiée le 17 juillet 2026 sur le site officiel Wagnardsoft, qui a succédé coup sur coup à la 18.1.5.3 (10 mai 2026) puis à la 18.1.5.4 (29-30 mai 2026). Il s’agit d’une mise à jour de maintenance : amélioration de l’arrêt et de la suppression des services, corrections sur le programme d’installation et ajout de nouveaux commutateurs en ligne de commande (`-cleanrealtek` et `-cleansoundblaster`) pour la partie audio. L’outil reste extrêmement léger – environ 1,2 Mo pour la version portable et 1,6 Mo pour l’installateur – et ne nécessite aucune installation : il se lance directement depuis son dossier.

DDU prend en charge les trois fabricants de GPU : **NVIDIA**, **AMD** et **Intel** (y compris les cartes Intel Arc). Il sait aussi nettoyer certains pilotes audio (Realtek, Sound Blaster) souvent installés en même temps que les pilotes HDMI/DisplayPort. Côté système, il fonctionne de Windows 7 à Windows 11. Téléchargeable depuis le site officiel Wagnardsoft et son miroir Guru3D, son code source est consultable sur GitHub.

## Pourquoi nettoyer ses pilotes GPU en 2026 ?

Réinstaller par-dessus l’ancien pilote suffit dans la majorité des cas, mais plusieurs situations imposent un nettoyage complet avec **Display Driver Uninstaller**. La première est le **changement de fabricant** : passer d’une carte NVIDIA à une Radeon AMD, ou inversement, laisse cohabiter deux jeux de pilotes qui se disputent les mêmes fichiers système. Sans DDU, les conflits sont quasi garantis (écran noir, code 43, pilote qui plante).

La deuxième situation concerne les **pilotes corrompus**. Après une mise à jour ratée, une coupure de courant en pleine installation ou une infection nettoyée à la hâte, le pilote graphique peut se retrouver dans un état instable. Les symptômes typiques : artefacts à l’écran, plantages dans les jeux, écrans bleus pointant vers `nvlddmkm.sys` (NVIDIA) ou `amdkmdag.sys` (AMD), ou encore l’application NVIDIA/Adrenalin qui refuse de s’ouvrir.

Troisième cas, fréquent chez les joueurs : le **retour à une version antérieure**. Quand un nouveau pilote dégrade les performances ou casse la compatibilité d’un jeu précis, revenir proprement à la version précédente exige d’effacer la nouvelle d’abord. Enfin, si vous prévoyez d’overclocker votre carte, partir de pilotes propres élimine une variable parasite dans le diagnostic d’instabilité. Pour la suite, notre guide MSI Afterburner pour overclocker son GPU prend logiquement le relais une fois vos pilotes réinstallés.

Avec la pénurie qui touche aussi bien la mémoire que les cartes graphiques – voir notre dossier sur les prix de la RAM en 2026 – conserver son GPU actuel en parfait état logiciel est devenu un réflexe d’économie. Un pilote propre, c’est quelques images par seconde gagnées sans dépenser un euro.

## DDU face aux autres méthodes de désinstallation

Pourquoi recourir à **Display Driver Uninstaller** alors que Windows et les fabricants proposent leurs propres outils ? Parce qu’aucun d’eux ne va aussi loin. Le désinstalleur de Windows (Paramètres > Applications) retire le paquet principal mais laisse intacts des dizaines d’entrées de registre, de services et de fichiers. Le Gestionnaire de périphériques, avec sa case « Supprimer le pilote », fait à peine mieux. Quant aux options « installation propre » intégrées aux installeurs NVIDIA et AMD, elles ne nettoient que les paramètres de leur propre marque, en mode normal, et ratent les résidus profonds.

| Méthode | Fabricants couverts | Profondeur de nettoyage | Mode sans échec | Coût | 
|---|---|---|---|---|
| Désinstalleur Windows | Tous (paquet principal) | Faible – laisse registre et fichiers | Non | Gratuit, intégré | 
| Gestionnaire de périphériques | Tous (partiel) | Faible à moyenne | Non | Gratuit, intégré | 
| Installation propre NVIDIA | NVIDIA uniquement | Moyenne (paramètres NVIDIA) | Non | Gratuit | 
| AMD Cleanup Utility | AMD uniquement | Élevée pour AMD | Recommandé | Gratuit | 
| **Display Driver Uninstaller** | **NVIDIA, AMD, Intel (+ audio)** | **Maximale – registre, fichiers, services** | **Recommandé** | **Gratuit** | 

Le verdict est sans appel : pour un nettoyage réellement complet, multi-fabricants et automatisable en ligne de commande, **DDU** reste l’outil le plus exhaustif. L’AMD Cleanup Utility constitue une bonne alternative si vous restez exclusivement chez AMD, mais dès que vous changez de marque ou que vous voulez effacer un pilote audio HDMI récalcitrant, Display Driver Uninstaller s’impose. C’est aussi le seul à offrir un mode silencieux scriptable, ce qui en fait le favori des techniciens et des ateliers de réparation.

## Prérequis : versions, téléchargements et sauvegardes

Avant de lancer **DDU**, rassemblez le nécessaire. La règle d’or : **téléchargez vos nouveaux pilotes GPU AVANT de nettoyer les anciens**. Une fois DDU passé, votre carte tournera sur le pilote d’affichage basique de Microsoft (résolution réduite, pas d’accélération), et il vaut mieux ne pas dépendre du réseau à ce moment-là. Vérifiez aussi que **.NET Framework 4.8** ou une version supérieure est installé : Wagnardsoft l’exige depuis la version 18.1.5.2 du 11 avril 2026, faute de quoi l’exécutable refuse de se lancer. Le tableau suivant récapitule les éléments à préparer.

| Élément | Version / source | Remarque | 
|---|---|---|
| Display Driver Uninstaller | 18.1.5.4 (29 mai 2026) | Site Wagnardsoft ou miroir Guru3D uniquement | 
| Système d’exploitation | Windows 7 à Windows 11 (24H2) | Compte administrateur requis | 
| Décompresseur | 7-Zip 24.x ou WinRAR | L’archive DDU est auto-extractible (7-Zip SFX) | 
| Pilote NVIDIA | GeForce / Studio (le plus récent) | Téléchargé depuis le site officiel NVIDIA | 
| Pilote AMD | Adrenalin Edition | Téléchargé depuis le site officiel AMD | 
| Pilote Intel | Arc & Graphics | Pour iGPU et cartes Intel Arc | 
| Espace disque | ~1,5 Go libre | Pour DDU + paquet pilote décompressé | 

Deux précautions complètent la liste. D’abord, **installez et exécutez DDU depuis un disque local** (C: ou D:) : l’outil refuse de fonctionner depuis un lecteur réseau. Ensuite, prévoyez de quoi vous connecter sans le code PIN : sur certaines configurations Windows 11 24H2, l’authentification Windows Hello est indisponible en mode sans échec (voir la section dédiée plus bas). Ayez votre mot de passe de compte Microsoft sous la main.

## Étape 1 – Télécharger DDU depuis une source officielle

Ne téléchargez jamais **Display Driver Uninstaller** depuis un site de partage inconnu : les versions repackagées sont un vecteur classique d’adware. Deux sources font autorité : le site de l’éditeur Wagnardsoft et le miroir historique Guru3D. Les deux distribuent désormais une archive auto-extractible nommée `DDU_v18.1.5.6.exe`, mise en ligne le 17 juillet 2026, pour un poids d’environ 1,6 Mo une fois l’installateur récupéré.

Une fois le fichier récupéré, vérifiez son intégrité avant de l’ouvrir. PowerShell calcule l’empreinte SHA-256 en une commande, que vous comparerez à celle publiée sur la page de téléchargement :

```
# Calculer l'empreinte SHA-256 de l'archive (PowerShell)
Get-FileHash "$env:USERPROFILE\Downloads\DDU_v18.1.5.4.exe" -Algorithm SHA256
# Sortie attendue (extrait) :
# Algorithm       Hash
# ---------       ----
# SHA256          9F3C...     C:\Users\vous\Downloads\DDU_v18.1.5.4.exe
```
Si l’empreinte correspond, vous tenez bien la version officielle. Conservez l’archive dans un dossier facile d’accès : nous la décompresserons à l’étape suivante. Inutile de la lancer maintenant – nous voulons d’abord cartographier votre configuration graphique.

## Étape 2 – Extraire l’archive dans un dossier dédié

L’exécutable de DDU est une archive 7-Zip auto-extractible. En le lançant, il propose de décompresser son contenu dans un sous-dossier. Choisissez un emplacement court et sans espace, par exemple `C:\DDU` : cela simplifiera les commandes en ligne de commande de l’étape 12. Vous pouvez aussi automatiser l’extraction en silence :

```
:: Extraire DDU dans C:\DDU sans interaction (invite de commandes)
"%USERPROFILE%\Downloads\DDU_v18.1.5.4.exe" -o"C:\DDU" -y
:: Vérifier que l'exécutable principal est bien présent
dir "C:\DDU\Display Driver Uninstaller.exe"
```
Le dossier obtenu contient l’exécutable `Display Driver Uninstaller.exe`, un fichier `ReadMe` et quelques ressources. Ouvrez le ReadMe : il liste les nouveautés de la version et confirme les commutateurs en ligne de commande disponibles. Ne lancez pas encore DDU en mode normal : pour un nettoyage optimal, nous passerons par le mode sans échec.

## Étape 3 – Identifier votre GPU et vos pilotes actuels

Avant tout nettoyage, notez précisément quelle carte vous possédez et quelle version de pilote elle utilise. Cette information sert de point de comparaison après réinstallation et évite de télécharger le mauvais paquet. PowerShell interroge directement le contrôleur vidéo :

```
# Lister les cartes graphiques, leur pilote et leur memoire (PowerShell)
Get-CimInstance Win32_VideoController |
  Select-Object Name, DriverVersion, @{N='VRAM_Go';E={[math]::Round($_.AdapterRAM/1GB,1)}} |
  Format-Table -AutoSize
# Detail des pilotes d'affichage et de leur etat
Get-PnpDevice -Class Display | Select-Object FriendlyName, Status, InstanceId
```
La sortie ressemble à ceci pour une carte NVIDIA :

```
Name                          DriverVersion   VRAM_Go
----                          -------------   -------
NVIDIA GeForce RTX 4070        32.0.15.7652      12,0
```
Vous pouvez aussi taper `dxdiag` dans le menu Démarrer pour ouvrir l’outil de diagnostic DirectX, dont l’onglet « Affichage » récapitule le modèle, le fabricant et la version du pilote. Notez ces valeurs. Si vous hésitez encore sur le choix de votre prochaine carte, notre comparatif RX 9070 XT vs RTX 5070 détaille le rapport performances/prix actuel.

## Étape 4 – Télécharger à l’avance les nouveaux pilotes

C’est l’étape que la plupart des tutoriels négligent, et c’est pourtant la plus importante. Une fois **DDU** passé et le redémarrage effectué, Windows bascule sur un pilote d’affichage générique : résolution limitée, pas de multi-écran fluide, pas d’accélération matérielle. Récupérer un pilote dans ces conditions est pénible. Téléchargez donc dès maintenant le paquet adapté à votre carte :

- **NVIDIA** : pilote GeForce Game Ready ou Studio depuis la page officielle NVIDIA.
- **AMD** : suite Adrenalin Edition depuis le centre de téléchargement AMD.
- **Intel** : pilote Arc & Graphics pour les GPU intégrés et les cartes Intel Arc.

Placez le fichier d’installation sur le Bureau ou dans un dossier facile à retrouver, idéalement sur le même disque que DDU. Si vous changez de marque de carte, téléchargez le pilote de la *nouvelle* carte, pas de l’ancienne. Gardez aussi sous la main votre pilote de chipset (carte mère) : il arrive qu’un nettoyage agressif touche des composants audio HDMI partagés.

## Étape 5 – Créer un point de restauration système

**DDU modifie le registre en profondeur.** Même si l’outil est mûr et fiable, un point de restauration vous offre un filet de sécurité en cas de mauvaise manipulation (mauvais fabricant sélectionné, par exemple). Sur les machines récentes, la protection système est parfois désactivée par défaut : activez-la avant de créer le point.

```
# A executer dans une console PowerShell ouverte EN ADMINISTRATEUR
# 1. Activer la protection systeme sur le disque C: (si necessaire)
Enable-ComputerRestore -Drive "C:\"
# 2. Creer le point de restauration avant d'utiliser DDU
Checkpoint-Computer -Description "Avant nettoyage DDU" -RestorePointType "MODIFY_SETTINGS"
# 3. Verifier que le point a bien ete cree
Get-ComputerRestorePoint | Select-Object SequenceNumber, Description, CreationTime
```
Si la commande `Checkpoint-Computer` renvoie une erreur de fréquence (Windows limite la création à un point toutes les 24 heures), créez le point manuellement via « Créer un point de restauration » dans les propriétés système, ou réglez temporairement la valeur de registre `SystemRestorePointCreationFrequency` sur 0. Notez le numéro de séquence affiché : il vous servira si vous devez revenir en arrière.

## Étape 6 – Empêcher Windows Update de réinstaller les pilotes

Voici le piège qui ruine la moitié des nettoyages : à peine DDU terminé, Windows Update détecte une carte « sans pilote » et réinstalle automatiquement une version générique, souvent ancienne. Résultat : vos résidus reviennent et votre nettoyage est annulé. Il faut donc **bloquer le téléchargement automatique des pilotes** avant de redémarrer. Deux clés de registre suffisent :

```
:: Invite de commandes ouverte EN ADMINISTRATEUR
:: 1. Ne plus rechercher les pilotes via Windows Update
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\DriverSearching" /v SearchOrderConfig /t REG_DWORD /d 0 /f
:: 2. Exclure les pilotes des mises a jour qualite de Windows Update
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v ExcludeWUDriversInQualityUpdate /t REG_DWORD /d 1 /f
```
DDU propose lui-même une option « Empêcher les téléchargements de pilotes via Windows Update » dans son menu Options : cochez-la également, ceinture et bretelles. La méthode la plus radicale reste de **débrancher le câble réseau (ou couper le Wi-Fi)** pendant toute la durée de l’opération, jusqu’à la réinstallation manuelle de votre pilote. Une fois vos pilotes réinstallés, vous pourrez réactiver la recherche en repassant les valeurs de registre à 1, ou en décochant l’option dans DDU.

## Étape 7 – Démarrer Windows en mode sans échec

Wagnard recommande explicitement d’utiliser **Display Driver Uninstaller** en mode sans échec. La raison est simple : en mode normal, Windows maintient le pilote graphique chargé en mémoire et verrouille certains fichiers, ce qui empêche une suppression totale. En mode sans échec, seul le pilote d’affichage minimal est actif, et DDU peut tout effacer sans interférence.

La méthode la plus fiable consiste à forcer le démarrage en mode sans échec via `bcdedit`, puis à annuler ce réglage après l’opération :

```
:: Invite de commandes EN ADMINISTRATEUR
:: Forcer le prochain demarrage en mode sans echec minimal
bcdedit /set {current} safeboot minimal
:: Redemarrer immediatement
shutdown /r /t 0
:: --- APRES le nettoyage et la reinstallation, ANNULER le mode sans echec ---
bcdedit /deletevalue {current} safeboot
```
Vous pouvez aussi laisser DDU gérer le redémarrage : son interface propose un bouton « Lancer en mode sans échec » qui prépare le boot automatiquement. Autre voie, plus visuelle, via l’environnement de récupération : exécutez `shutdown /r /o /f /t 0`, puis suivez *Dépannage > Options avancées > Paramètres de démarrage > Redémarrer*, et appuyez sur 4 ou F4 pour le mode sans échec. Quelle que soit la méthode, n’oubliez pas votre mot de passe de connexion.

## Étape 8 – Lancer DDU et régler les options

Une fois en mode sans échec, ouvrez le dossier `C:\DDU` et lancez `Display Driver Uninstaller.exe` par un clic droit puis *Exécuter en tant qu’administrateur*. Au premier démarrage, une fenêtre d’options s’affiche. Les réglages par défaut conviennent à la plupart des utilisateurs, mais vérifiez les points suivants :

- **Empêcher les téléchargements de pilotes via Windows Update** : à cocher, en complément des clés de registre de l’étape 6.
- **Supprimer les dossiers de pilotes** (C:\AMD, C:\NVIDIA, C:\Intel) : utile pour un nettoyage total.
- **Supprimer les moniteurs enregistrés** : à activer si vous changez d’écran ou si vous rencontrez des soucis d’EDID/résolution.
- **Créer un point de restauration** : laissez coché si vous n’en avez pas créé à l’étape 5.

Dans le menu déroulant en haut à droite, sélectionnez d’abord le **type de périphérique « GPU »**, puis le **fabricant correspondant à votre carte** : NVIDIA, AMD ou Intel. C’est l’étape critique : choisir le mauvais fabricant ne supprimera rien d’utile. Si vous avez à la fois un iGPU Intel et une carte dédiée, traitez chaque fabricant l’un après l’autre. L’interface est traduite et indique en clair la carte détectée.

## Étape 9 – Nettoyer les pilotes (Clean and Restart)

DDU propose trois actions principales. Le tableau ci-dessous les compare pour vous aider à choisir. Dans la grande majorité des cas, **« Nettoyer et redémarrer »** est le bon choix : il efface les pilotes puis relance Windows proprement.

| Bouton DDU | Action | Quand l’utiliser | 
|---|---|---|
| Nettoyer et redémarrer | Supprime les pilotes puis redémarre le PC | Cas standard : réinstallation ou changement de carte | 
| Nettoyer sans redémarrer | Supprime les pilotes, reste en session | Pour enchaîner sur un second fabricant avant de redémarrer | 
| Nettoyer et éteindre | Supprime les pilotes puis arrête le PC | Lorsque vous remplacez physiquement la carte graphique | 

Cliquez sur le bouton voulu et laissez DDU travailler : une barre de progression et un journal détaillé défilent. L’opération dure de 30 secondes à 2 minutes. L’écran peut clignoter ou changer de résolution : c’est normal, le pilote est en train d’être retiré. Ne touchez à rien jusqu’au redémarrage automatique.

Si vous nettoyez en mode normal plutôt qu’en mode sans échec (déconseillé), répétez le cycle **Nettoyer → Redémarrer → Nettoyer → Redémarrer** deux fois pour un résultat équivalent. En mode sans échec, un seul passage suffit. À ce stade, votre système redémarre sans pilote graphique propriétaire : place à la vérification.

## Étape 10 – Vérifier la suppression avec pnputil

Comment être sûr que DDU n’a rien oublié ? L’utilitaire Windows **pnputil** liste tous les paquets de pilotes encore enregistrés dans le magasin de pilotes. Après un nettoyage réussi, la classe « Affichage » ne doit plus contenir que l’adaptateur de base de Microsoft (ou rien du tout). Ouvrez une invite de commandes en administrateur :

```
:: Lister les paquets de pilotes d'affichage encore presents
pnputil /enum-drivers /class Display
:: Variante compatible toutes versions (filtre la sortie)
pnputil /enum-drivers | findstr /i "Display Affichage nv_disp amd ig"
```
Avant nettoyage, la sortie ressemble à ceci (un pilote NVIDIA tiers encore enregistré) :

```
Utilitaire Microsoft PnP
Nom publie :        oem42.inf
Nom d'origine :     nv_dispi.inf
Fournisseur :       NVIDIA
Classe :            Cartes graphiques
Version :           06/12/2026 32.0.15.7652
Nom signataire :    Microsoft Windows Hardware Compatibility Publisher
```
Après un nettoyage DDU réussi, la même commande renvoie « Aucun élément trouvé » pour la classe Affichage, ou uniquement `Microsoft Basic Display Adapter`. S’il subsiste un paquet tiers récalcitrant, supprimez-le manuellement (remplacez `oem42.inf` par le nom réel) :

```
:: Supprimer un paquet pilote residuel et le desinstaller du systeme
pnputil /delete-driver oem42.inf /uninstall /force
```
La documentation complète des commutateurs est disponible dans la référence officielle pnputil de Microsoft. Cet outil est précieux pour diagnostiquer les pilotes fantômes que même DDU peut, rarement, laisser passer.

## Étape 11 – Réinstaller des pilotes propres (NVIDIA, AMD, Intel Arc)

Votre système est désormais vierge de tout pilote graphique propriétaire. Lancez le paquet d’installation téléchargé à l’étape 4. Quelques recommandations par fabricant :

### NVIDIA

Choisissez l’option **« Personnalisée »** puis cochez **« Effectuer une installation propre »**. Depuis 2024, l’installeur NVIDIA est modulaire : vous pouvez décocher GeForce Experience / l’application NVIDIA si vous ne l’utilisez pas. Pour la majorité des joueurs, le pilote Game Ready suffit ; les créateurs préféreront la branche Studio, plus stable pour les applications professionnelles.

### AMD et Intel

Côté AMD, l’installeur Adrenalin propose lui aussi une **« Installation usine »** qui repart d’une base propre – pratique, mais inutile ici puisque DDU a déjà fait le ménage ; une installation standard convient. Pour les cartes **Intel Arc**, installez le pilote Arc & Graphics le plus récent : les gains de performances pilote après pilote restent significatifs sur cette gamme. Après installation, redémarrez et n’oubliez pas d’annuler le mode sans échec (`bcdedit /deletevalue {current} safeboot`) si vous l’aviez forcé.

Vérifiez enfin que la bonne version est active en relançant la commande `Get-CimInstance Win32_VideoController` de l’étape 3 : le numéro de pilote doit correspondre à celui que vous venez d’installer. Si vous suivez l’actualité des GPU, notre comparatif RX 9070 XT vs RTX 5070 Ti et notre analyse de la stratégie NVIDIA DSX complètent ce tutoriel.

Une fois le pilote en place, validez la stabilité avant de conclure. Lancez un jeu exigeant ou un test de charge (3DMark, Superposition, ou simplement une boucle de benchmark intégré) pendant une dizaine de minutes et surveillez les températures, la fréquence et l’absence d’artefacts. Réactivez ensuite les fonctionnalités que vous utilisez : G-SYNC/FreeSync, résolution et taux de rafraîchissement natifs de l’écran, profils de couleur. Si tout est stable, n’oubliez pas de rétablir la recherche de pilotes via Windows Update si vous comptez laisser le système se mettre à jour à l’avenir – en gardant à l’esprit qu’un pilote installé manuellement reste prioritaire tant qu’il est plus récent.

## Étape 12 – Automatiser DDU en ligne de commande

Pour les techniciens qui gèrent plusieurs machines, ou simplement pour gagner du temps, DDU s’exécute en mode silencieux via des commutateurs en ligne de commande. C’est notre « projet complet » : un script batch qui nettoie les pilotes NVIDIA et redémarre, sans la moindre interaction. À lancer depuis le mode sans échec, en administrateur.

```
@echo off
:: ddu-auto.bat – Nettoyage automatise des pilotes GPU avec DDU 18.1.5.4
:: A executer EN ADMINISTRATEUR, de preference en mode sans echec
set "DDU=C:\DDU\Display Driver Uninstaller.exe"
if not exist "%DDU%" (
  echo Erreur : DDU introuvable dans C:\DDU
  pause
  exit /b 1
)
echo Nettoyage des pilotes NVIDIA en cours...
"%DDU%" -silent -cleannvidia -removemonitors -restart
:: Variantes :
::   AMD   -^>  "%DDU%" -silent -cleanamd -restart
::   Intel -^>  "%DDU%" -silent -cleanintel -restart
::   Audio -^>  "%DDU%" -silent -cleanrealtek
```
Le tableau suivant récapitule les principaux commutateurs reconnus par DDU 18.1.5.4. Notez que l’option « supprimer PhysX » n’est disponible que dans l’interface graphique, pas en ligne de commande.

| Commutateur | Effet | 
|---|---|
| `-Silent` | Exécution silencieuse, sans interface graphique | 
| `-CleanNvidia` | Nettoie les pilotes NVIDIA | 
| `-CleanAMD` | Nettoie les pilotes AMD | 
| `-CleanIntel` | Nettoie les pilotes Intel (y compris Arc) | 
| `-RemoveMonitors` | Supprime les données EDID / moniteurs enregistrés | 
| `-CleanRealtek` | Nettoie les pilotes audio Realtek (ajout 18.1.5.x) | 
| `-CleanSoundblaster` | Nettoie les pilotes audio Sound Blaster | 
| `-Restart` | Redémarre le PC après le nettoyage | 

Enregistrez le script sous `ddu-auto.bat`, faites un clic droit puis *Exécuter en tant qu’administrateur*. Pour un déploiement de masse, vous pouvez combiner ce batch avec un démarrage forcé en mode sans échec (`bcdedit`) orchestré par GPO ou par un outil de gestion de parc. Pensez toujours à rétablir le mode normal en fin de script.

## Windows 11 24H2 : le piège du code PIN en mode sans échec

Voici le problème le plus signalé en 2026. Sur certaines configurations **Windows 11 24H2**, la connexion par **code PIN (Windows Hello) ne fonctionne pas en mode sans échec**. Vous arrivez à l’écran de connexion, vous saisissez votre PIN… et rien ne se passe, ou un message indique que l’option est indisponible. Comme DDU s’utilise précisément en mode sans échec, l’utilisateur se retrouve bloqué hors de sa session, parfois sans se souvenir de son mot de passe de compte Microsoft.

La cause : en mode sans échec, le service de gestion des identités (et certains composants de Windows Hello) n’est pas toujours chargé. Trois parades fiables :

1. **Utilisez votre mot de passe** de compte Microsoft (ou local) plutôt que le PIN. Sur l’écran de connexion, cliquez sur « Options de connexion » puis choisissez l’icône de mot de passe. C’est la solution la plus simple – d’où l’importance de le connaître avant de commencer.
2. **Démarrez en « Mode sans échec avec prise en charge réseau »** (touche 5 ou F5 dans les Paramètres de démarrage) : davantage de services sont chargés, ce qui rétablit parfois l’authentification.
3. **Basculez temporairement sur un compte local** avant l’opération (Paramètres > Comptes), avec un mot de passe simple, puis rebasculez sur votre compte Microsoft ensuite.

Par sécurité, réinitialisez ou mémorisez votre mot de passe de compte Microsoft *avant* de redémarrer en mode sans échec. C’est l’unique précaution qui vous évitera une mauvaise surprise sur 24H2.

## Pièges courants à éviter avec DDU

Même bien conçu, **Display Driver Uninstaller** punit l’imprudence. Voici les erreurs qui reviennent le plus souvent et comment les contourner :

- **Laisser Windows Update réinstaller le pilote.** C’est l’erreur n°1. Sans le blocage de l’étape 6 ni la déconnexion réseau, Windows réinjecte un pilote générique dans les minutes qui suivent le nettoyage, et tout est à refaire.
- **Oublier de télécharger le nouveau pilote avant.** Se retrouver sur l’affichage basique Microsoft sans paquet d’installation sous la main transforme une tâche de 30 minutes en galère.
- **Lancer DDU en mode normal.** Cela fonctionne, mais laisse des résidus. Le mode sans échec reste la recommandation officielle de Wagnard.
- **Sélectionner le mauvais fabricant.** Choisir AMD alors qu’on a une carte NVIDIA ne supprime rien d’utile et fait perdre un cycle complet de redémarrage.
- **Exécuter DDU depuis un lecteur réseau ou une clé USB lente.** L’outil exige un disque local ; sur un support réseau, il refuse de démarrer ou se comporte de façon erratique.
- **Télécharger DDU depuis une source douteuse.** Les versions repackagées embarquent parfois des logiciels indésirables. Tenez-vous-en à Wagnardsoft et Guru3D.

## Dépannage : 8 problèmes fréquents et leurs solutions

Voici les incidents les plus rapportés après un passage de **DDU**, avec leur cause probable et la marche à suivre.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Écran noir après nettoyage | Aucun pilote chargé, sortie vidéo sur le mauvais port | Branchez l’écran sur la sortie de la carte dédiée, réinstallez le pilote | 
| Résolution bloquée en 1024×768 | Pilote d’affichage basique Microsoft actif | Installez le pilote GPU téléchargé à l’étape 4 | 
| Windows réinstalle un vieux pilote | Recherche Windows Update non bloquée | Appliquez les clés de registre de l’étape 6, restez hors ligne | 
| Code PIN refusé en mode sans échec | Windows Hello indisponible (Win11 24H2) | Connectez-vous par mot de passe, ou mode sans échec avec réseau | 
| DDU refuse de se lancer | Exécution depuis un lecteur réseau, ou pas d’admin | Copiez DDU sur C:, lancez en administrateur | 
| Carte non détectée après réinstallation | Connecteur PCIe ou alimentation mal branchés | Vérifiez le branchement, testez un autre port PCIe | 
| Application NVIDIA/Adrenalin ne s’ouvre pas | Composant logiciel non installé | Réinstallez le pilote complet (option personnalisée) | 
| Paquet pilote résiduel après nettoyage | INF tiers verrouillé par le système | Supprimez-le via `pnputil /delete-driver` | 

Si malgré tout l’instabilité persiste, revenez au point de restauration créé à l’étape 5, puis recommencez le nettoyage en respectant scrupuleusement le mode sans échec et le blocage de Windows Update.

## Astuces avancées pour aller plus loin

Quelques techniques pour les utilisateurs aguerris. D’abord, le **journal de DDU** : chaque session génère un fichier texte dans le sous-dossier `Logs`. En cas de problème récurrent, il documente précisément ce qui a été supprimé et les éventuelles erreurs – indispensable pour un diagnostic sérieux ou un signalement sur le forum Wagnardsoft.

Ensuite, l’option **« Nettoyer le cache des shaders » seule**. Inutile de tout effacer pour régler un problème de saccades dans un jeu précis : DDU peut purger uniquement le cache de shaders DirectX/Vulkan, souvent responsable de stutters après une mise à jour de pilote. C’est un nettoyage non destructif, sans redémarrage.

Enfin, pensez à **conserver une copie de DDU sur une clé USB** avec votre dernier pilote stable. En cas d’écran noir total empêchant tout accès au réseau, vous pourrez nettoyer et réinstaller hors ligne depuis le mode sans échec. Pour les passionnés de réglage fin, l’enchaînement logique reste : pilotes propres avec DDU, puis overclocking et undervolting – un terrain que nous couvrons dans notre guide dédié à MSI Afterburner.

Pour les configurations à double GPU (iGPU Intel ou AMD intégré + carte dédiée), un dernier conseil : nettoyez et réinstallez les pilotes dans l’ordre, du plus simple au plus complexe. Traitez d’abord le GPU intégré, redémarrez, puis la carte dédiée. Des outils tiers comme GPU-Z ou HWiNFO confirment, après coup, que seul le bon pilote est chargé et que la carte fonctionne à pleine vitesse PCIe (et non en mode dégradé x1). Cette rigueur évite les conflits de pilotes hybrides qui provoquent des micro-saccades difficiles à diagnostiquer, en particulier sur les ordinateurs portables gamer où le commutateur graphique (Optimus, par exemple) ajoute une couche de complexité.

### Related Coverage

## FAQ : Display Driver Uninstaller (DDU)

### DDU est-il sûr et gratuit ?

Oui. Display Driver Uninstaller est entièrement gratuit, développé par Wagnard depuis plus de dix ans, et largement recommandé par les communautés matérielles. Téléchargé depuis le site officiel Wagnardsoft ou le miroir Guru3D, il ne contient aucun logiciel indésirable. Comme il modifie le registre, créez un point de restauration au préalable, par simple prudence.

### Faut-il obligatoirement utiliser le mode sans échec ?

Ce n’est pas obligatoire, mais c’est fortement recommandé. En mode normal, Windows verrouille des fichiers du pilote, ce qui empêche une suppression à 100 %. Le mode sans échec garantit un nettoyage complet en un seul passage. En mode normal, il faudrait répéter le cycle nettoyer/redémarrer deux fois pour un résultat comparable.

### DDU fonctionne-t-il avec les cartes Intel Arc ?

Oui. Display Driver Uninstaller prend en charge les pilotes NVIDIA, AMD et Intel, y compris les cartes graphiques dédiées Intel Arc et les iGPU Intel. Sélectionnez simplement « Intel » comme fabricant dans le menu déroulant, puis installez ensuite le pilote Arc & Graphics le plus récent.

### Pourquoi mon code PIN ne fonctionne-t-il pas en mode sans échec ?

Sur Windows 11 24H2, Windows Hello (et donc le code PIN) peut être indisponible en mode sans échec, car certains services d’identité ne sont pas chargés. Connectez-vous avec votre mot de passe de compte Microsoft via « Options de connexion », ou démarrez en mode sans échec avec prise en charge réseau. Préparez toujours votre mot de passe avant l’opération.

### DDU supprime-t-il aussi les pilotes audio ?

Partiellement. DDU peut nettoyer certains pilotes audio liés au GPU (audio HDMI/DisplayPort) et, depuis les versions 18.1.5.x, les pilotes Realtek et Sound Blaster via les commutateurs `-cleanrealtek` et `-cleansoundblaster`. Il ne remplace toutefois pas un désinstalleur audio dédié pour les cartes son complexes.

### Dois-je débrancher Internet pendant l’opération ?

C’est la méthode la plus sûre. Tant que vous n’avez pas réinstallé votre pilote, Windows Update tentera de télécharger un pilote générique automatiquement. Couper le réseau (ou appliquer les clés de registre de l’étape 6) empêche cette réinstallation parasite qui annulerait votre nettoyage.

### Quelle est la dernière version de DDU en 2026 ?

Au 17 août 2026, la version stable est DDU 18.1.5.6, publiée le 17 juillet 2026 par Wagnardsoft. Elle succède à un rythme soutenu de mises à jour depuis la 18.1.3.7 d’octobre 2025 (couverte par iTechGuides), en passant par la 18.1.3.9 de décembre 2025 signalée par Guru3D, la 18.1.4.0 puis la 18.1.4.1 de janvier 2026, la 18.1.4.2 de février 2026, la 18.1.5.2 d’avril 2026 (qui a introduit l’exigence de .NET Framework 4.8) et la 18.1.5.3 de mai 2026. Il s’agit d’une mise à jour de maintenance améliorant la suppression des services, le programme d’installation et la prise en charge audio en ligne de commande. Vérifiez toujours la page Wagnardsoft ou Guru3D pour la toute dernière révision.

### À quelle fréquence faut-il utiliser DDU ?

Pas systématiquement. La mise à jour « par-dessus » convient pour les pilotes courants. Réservez DDU aux situations problématiques : changement de fabricant, pilote corrompu, plantages persistants, retour à une version antérieure, ou avant une session sérieuse d’overclocking. L’utiliser à chaque mise à jour est inutile et fait perdre du temps.

*Tutoriel publié le 29 juin 2026. Versions logicielles et procédures vérifiées à cette date ; reportez-vous aux sites officiels des éditeurs pour les dernières révisions.*
