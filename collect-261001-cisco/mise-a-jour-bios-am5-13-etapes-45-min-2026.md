Ouvrir son PC pour la première fois après avoir changé de processeur, et tomber sur un écran noir. Ça arrive à des milliers d’utilisateurs AM5 chaque mois : un Ryzen 9000X3D flambant neuf posé sur une carte mère B650 achetée en 2023, dont le firmware date d’avant la sortie de la puce. Résultat, aucun signal vidéo, aucun bip, et souvent un retour au magasin qui aurait pu être évité en dix minutes.

La mise à jour du BIOS, ou plus précisément de l’UEFI qui l’a techniquement remplacé, reste l’étape la plus survolée d’un montage PC. Elle conditionne pourtant la compatibilité avec les nouveaux processeurs, la stabilité de la RAM DDR5 et parfois la sécurité de la machine. Sur le socket AM5, une plateforme qu’AMD a positionnée pour durer plusieurs générations comme nous le détaillions dans notre analyse de Zen 6 et de la longévité de l’AM5, cette opération devient même récurrente : chaque nouvelle génération de Ryzen, chaque kit mémoire un peu exotique, chaque correctif de sécurité repasse par une mise à jour du firmware.

Ce tutoriel détaille les 13 étapes pour flasher le BIOS d’une carte mère AM5 en toute sécurité, avec ou sans processeur installé, en couvrant les quatre grandes marques du marché (ASUS, MSI, Gigabyte, ASRock) ainsi que les erreurs qui transforment une opération de routine en carte mère hors service. Comptez environ 45 minutes, marge de dépannage comprise.

## Pourquoi mettre à jour le BIOS de votre carte mère AM5 en 2026

Le BIOS, pour Basic Input/Output System, est le tout premier logiciel exécuté au démarrage d’un PC, avant même le système d’exploitation. Sur les cartes mères modernes, il a été remplacé techniquement par l’UEFI (Unified Extensible Firmware Interface), plus rapide et plus riche en fonctionnalités, mais l’appellation BIOS a survécu dans le langage courant. C’est d’ailleurs celle que gardent la plupart des fabricants dans leurs propres menus et documentations.

Trois raisons poussent concrètement les utilisateurs d’AM5 à mettre à jour leur firmware en 2026.

La première est la compatibilité processeur. AMD a lancé plusieurs générations de Ryzen sur le même socket AM5 depuis 2022, et chaque nouvelle puce nécessite un microcode que les cartes mères plus anciennes n’embarquent pas d’origine. Gigabyte a par exemple publié la révision **AGESA 1.1.7.0 Patch A** pour rendre ses cartes X670, B650 et A620 compatibles avec le démarrage des Ryzen 9000, un cas typique où l’absence de mise à jour empêche purement et simplement le PC de démarrer, sans le moindre message d’erreur pour orienter l’utilisateur.

La deuxième raison est la compatibilité mémoire. La DDR5 reste plus exigeante que la DDR4 sur la validation des profils EXPO et XMP, et les fabricants publient régulièrement des révisions de firmware qui élargissent la liste des kits pris en charge ou stabilisent les fréquences élevées. Dans un contexte où les prix de la RAM ont explosé, comme nous le rapportions dans notre couverture de la pénurie et de la hausse de +171 % sur les modules DDR5, faire fonctionner correctement un kit déjà acheté compte plus que jamais plutôt que d’en racheter un neuf en cas d’incompatibilité.

La troisième raison, moins visible mais tout aussi réelle, concerne la sécurité. Un bulletin ASUS relayé par la communauté fin 2025 mentionnait des correctifs visant une vulnérabilité de dépassement de mémoire dans le composant PeCoffLoader, ainsi qu’une faille de vérification de signature de microcode AMD, corrigées via la révision **AGESA ComboAM5 PI 1.2.0.3**.

Pour un socket dont la longévité annoncée dépasse largement celle de la génération précédente, la mise à jour BIOS n’est donc plus une opération ponctuelle réservée aux passionnés d’overclocking. Elle devient un geste de maintenance normal, au même titre qu’une mise à jour Windows.

## Prérequis : ce qu’il vous faut avant de commencer

Avant de lancer la moindre mise à jour, réunissez les éléments suivants. Un flash BIOS interrompu reste la cause numéro un de carte mère rendue inutilisable, et la majorité des cas se résume à un prérequis oublié plutôt qu’à un vrai problème matériel.

**Côté matériel :** une clé USB de 4 à 32 Go formatée exclusivement en FAT32 (le format exFAT ou NTFS n’est reconnu par aucune interface de flash BIOS grand public), une alimentation électrique stable, et si possible un second appareil comme un smartphone pour consulter les instructions pendant l’opération, l’écran principal étant indisponible une bonne partie du temps.

**Côté logiciel :** Windows 10 ou 11 à jour pour identifier votre matériel, l’utilitaire CPU-Z en dernière version disponible pour confirmer le modèle exact de carte mère, et l’invite de commandes PowerShell intégrée à Windows, en version 5.1 ou supérieure et préinstallée depuis Windows 10. Aucune installation tierce n’est nécessaire pour l’essentiel de la procédure.

**Côté informations :** notez précisément la référence complète de votre carte mère, révision comprise. Les fabricants publient parfois des firmwares distincts pour une même gamme selon la révision de carte (Rev 1.0, Rev 1.1…), et flasher le mauvais fichier figure parmi les erreurs les plus fréquentes détaillées plus bas. Vérifiez aussi l’état de la pile CMOS si votre carte mère a plus de deux ans, une pile faible pendant un flash augmentant le risque de coupure inopinée.

Enfin, prévoyez 45 minutes sans interruption. Le flash proprement dit ne dure que 3 à 10 minutes selon les fabricants, mais l’identification du matériel, le téléchargement, la vérification et la reconfiguration post-update représentent l’essentiel du temps réel passé sur l’opération.

## Étape 1 : identifier le modèle exact de votre carte mère

La méthode la plus fiable ne consiste pas à se fier de mémoire au nom inscrit sur la boîte, mais à lire l’information directement via un outil logiciel. Si le PC démarre encore normalement, ouvrez une invite PowerShell en administrateur et exécutez la commande suivante.

`Get-CimInstance Win32_BaseBoard | Select-Object Manufacturer, Product, Version`
Exemple de sortie obtenue sur une carte B650 :

```
Manufacturer                         Product                    Version
------------                         -------                    -------
Micro-Star International Co., Ltd.   MAG B650 TOMAHAWK WIFI     1.0
```
Ce résultat donne le fabricant, le nom commercial exact et la révision matérielle, les trois informations nécessaires pour télécharger le bon firmware. Si Windows ne démarre pas, l’alternative consiste à ouvrir le boîtier et lire l’étiquette sérigraphiée directement sur le circuit imprimé, généralement située entre les slots PCIe et les ports SATA, ou à photographier la référence avec un smartphone avant de refermer le boîtier.

Attention aux variantes de nommage. Une même carte mère peut exister en version simple, WiFi ou « Plus », avec des firmwares non interchangeables. Un ASUS ROG Strix B650E-F Gaming WiFi et sa version sans le suffixe WiFi ne partagent pas nécessairement le même fichier BIOS, même si les deux cartes se ressemblent visuellement.

## Étape 2 : repérer la version BIOS actuellement installée

Une fois le modèle confirmé, il faut connaître la version de firmware déjà en place, pour savoir si une mise à jour est réellement nécessaire et éviter de flasher une version identique ou plus ancienne. Toujours en PowerShell :

`Get-CimInstance Win32_BIOS | Select-Object Manufacturer, SMBIOSBIOSVersion, ReleaseDate`
Vous pouvez aussi consulter cette information directement dans le BIOS lui-même, en appuyant sur Suppr ou F2 au démarrage (la touche exacte s’affiche brièvement à l’écran pendant les deux premières secondes du POST), puis en repérant le numéro de version, presque toujours situé en haut à droite de l’écran principal ou dans un onglet « Main ».

Comparez ensuite ce numéro à la dernière version disponible sur le site du fabricant. Les conventions de nommage varient énormément d’une marque à l’autre : MSI utilise des suites comme « 7D78v1C », ASUS des formats numériques comme « 3421 », Gigabyte des versions comme « F23 ». Ne vous fiez jamais à un simple tri alphabétique ou numérique, référez-vous systématiquement à la date de publication indiquée sur la page de support officielle.

## Étape 3 : vérifier la compatibilité CPU et RAM sur la liste QVL

C’est l’étape que la majorité des utilisateurs sautent, et c’est justement celle qui évite la panne la plus fréquente : le nouveau processeur acheté qui ne démarre pas une fois installé.

Chaque fabricant publie deux documents distincts sur la page produit de sa carte mère : la liste de support CPU, souvent nommée « CPU Support List », et la liste de qualification mémoire, la fameuse QVL pour Qualified Vendor List. La première indique, pour chaque processeur, la version BIOS minimale requise pour démarrer. La seconde fait de même pour les kits de RAM testés en usine à une fréquence donnée.

Le cas le plus documenté sur AM5 concerne justement les Ryzen 9000. D’après un guide technique consacré à la question, la révision **AGESA ComboAM5 PI 1.1.7.0** constitue le minimum pour qu’un Ryzen 9000 termine simplement sa séquence de démarrage, tandis que la révision **1.3.0.1**, publiée en 2025, ajoute la prise en charge des variantes X3D de cette même génération. Sans cette information, un utilisateur qui installe un Ryzen 9000X3D sur une carte flashée en usine avant la sortie de la puce se retrouve face à un écran noir.

Pour la RAM, consultez la QVL même si votre kit affiche fièrement un profil EXPO. Un kit non testé peut fonctionner normalement, tourner en dessous de sa fréquence annoncée, ou plus rarement empêcher le démarrage. Notre couverture de la validation par MSI de modules DDR5 chinois CXMT à 8 200 MT/s sur AM5 illustre bien à quel point ces listes de compatibilité évoluent au fil des mises à jour BIOS, parfois plusieurs mois après la sortie commerciale d’un kit mémoire.

## Étapes 4 à 6 : télécharger, vérifier et préparer le fichier BIOS

Une fois le modèle, la version actuelle et la compatibilité confirmés, la préparation du fichier se déroule en trois temps.

**Étape 4, le téléchargement.** Rendez-vous exclusivement sur le site officiel du fabricant, jamais sur un site tiers ou un forum même si le fichier semble identique. Recherchez votre référence exacte, en incluant la révision matérielle notée à l’étape 1, puis téléchargez la version la plus récente listée dans l’onglet BIOS de la page support.

**Étape 5, la vérification d’intégrité.** Un fichier corrompu pendant le téléchargement peut passer inaperçu jusqu’au moment du flash, où il provoque un échec en plein milieu de l’opération. Si le fabricant publie un hash de contrôle SHA-256 sur sa page de téléchargement, vérifiez-le avant d’aller plus loin :

`certutil -hashfile "C:\Downloads\B650-TOMAHAWK-WIFI.zip" SHA256`
Comparez la chaîne obtenue avec celle publiée par le fabricant, caractère par caractère. Si elles ne correspondent pas, retéléchargez le fichier plutôt que de tenter le flash avec un doute.

**Étape 6, la préparation de la clé USB.** Formatez-la en FAT32 via l’invite de commandes, la méthode la plus fiable pour éviter les échecs liés à l’explorateur Windows sur les clés de plus de 32 Go :

```
diskpart
list disk
select disk 1
clean
create partition primary
format fs=fat32 quick
assign
exit
```
Remplacez « disk 1 » par le numéro exact de votre clé USB, visible dans la liste affichée par la commande « list disk ». Une erreur de sélection à cette étape efface le mauvais disque, vérifiez donc deux fois la taille affichée avant de valider quoi que ce soit.

Décompressez ensuite le fichier téléchargé et copiez le fichier .bin ou .cap directement à la racine de la clé, sans le placer dans un sous-dossier. Certaines méthodes de flash, comme le M-Flash de MSI, exigent en plus un renommage précis documenté dans le manuel de la carte mère.

## Étapes 7 à 9 : flasher sans CPU installé grâce au BIOS FlashBack

L’avantage majeur des cartes mères AM5 récentes tient dans leur capacité à flasher le BIOS sans processeur, sans RAM et sans carte graphique installés. Une aubaine quand le CPU tout juste acheté n’est justement pas reconnu par le firmware d’origine, ce qui serait sinon un problème difficile à résoudre sans emprunter un processeur compatible.

Chaque fabricant a sa propre appellation et sa propre procédure, mais le principe reste identique : brancher l’alimentation sans démarrer le PC, insérer la clé USB dans un port spécifique du panneau arrière, puis maintenir un bouton dédié jusqu’à ce qu’une diode commence à clignoter.

### ASUS BIOS FlashBack

Sur les cartes ASUS, branchez le câble d’alimentation 24 broches et le connecteur CPU 8 broches sans allumer le PC. Insérez la clé USB dans le port identifié « BIOS » à l’arrière du boîtier, jamais un port USB classique. Maintenez le bouton BIOS FlashBack environ trois secondes jusqu’à ce que la diode commence à clignoter. Le clignotement indique que le flash est en cours, son arrêt signale la fin de l’opération. La procédure complète est détaillée dans le guide officiel ASUS pour les cartes AM5.

### MSI Flash BIOS Button

La procédure MSI suit la même logique : fichier à la racine d’une clé FAT32 insérée dans le port BIOS dédié, alimentation branchée, PC éteint, puis pression sur le bouton Flash BIOS à l’arrière du boîtier. Selon la documentation officielle MSI pour les cartes AM5, l’opération dure généralement entre 5 et 10 minutes, la diode clignotante confirmant sa progression jusqu’à extinction complète.

### Gigabyte Q-Flash Plus

Gigabyte utilise le même principe sous le nom Q-Flash Plus, disponible en particulier sur les cartes X670, B650 et A620 compatibles avec la révision AGESA 1.1.7.0 Patch A mentionnée plus haut. Le fichier BIOS doit être renommé « GIGABYTE.bin » avant d’être copié sur la clé, une exigence propre à cette marque qui fait échouer la procédure si elle n’est pas respectée à la lettre.

### ASRock BIOS Flashback

ASRock propose une fonction équivalente sur ses cartes AM5 haut et moyen de gamme, avec un bouton dédié et un port USB identifié. La documentation exacte varie sensiblement d’un modèle à l’autre chez ce fabricant, il est donc recommandé de consulter le manuel PDF spécifique à votre référence plutôt que de suivre une procédure générique trouvée en ligne.

Dans les quatre cas, ne débranchez jamais l’alimentation et ne retirez jamais la clé USB tant que la diode clignote. Une interruption à ce stade est la cause la plus documentée de carte mère rendue inutilisable après une tentative de mise à jour.

## Étapes 10 à 13 : flash classique depuis le BIOS existant et reconfiguration

Si votre PC démarre déjà normalement avec l’ancien firmware, la méthode « classique » depuis l’interface UEFI reste plus rapide que le FlashBack.

**Étape 10**, entrez dans le BIOS en appuyant sur Suppr ou F2 au démarrage, puis basculez en mode avancé si votre carte affiche par défaut une interface simplifiée, ce qui est le cas de l’EZ Mode chez ASUS et Gigabyte.

**Étape 11**, lancez l’utilitaire de flash intégré : M-Flash chez MSI, EZ Flash chez ASUS, Q-Flash chez Gigabyte, Instant Flash chez ASRock. Chacun se trouve dans un onglet Outils ou Tool du menu principal. Sélectionnez votre clé USB, puis le fichier BIOS copié à l’étape 6.

**Étape 12**, validez et patientez. L’écran reste noir ou affiche une barre de progression selon les fabricants, pendant 3 à 8 minutes. Le PC redémarre automatiquement une ou deux fois à la fin de l’opération, ce comportement est normal et ne doit surtout pas être interrompu.

**Étape 13**, reconfigurez votre BIOS. C’est l’étape la plus souvent oubliée : une mise à jour réinitialise la quasi-totalité des réglages à leurs valeurs par défaut. Il faut donc systématiquement :

- réactiver le profil mémoire EXPO ou XMP, sans quoi la RAM retombe à sa fréquence JEDEC de base, généralement autour de 4 800 à 5 200 MT/s au lieu des 6 000 MT/s ou plus annoncés sur la boîte
- vérifier l’ordre de démarrage (Boot Order), en particulier si vous utilisez plusieurs disques
- réactiver le Resizable BAR si vous l’aviez configuré pour votre carte graphique
- reconfigurer vos courbes de ventilateurs personnalisées, remises à un profil silencieux par défaut

Un redémarrage complet suivi d’une vérification dans l’utilitaire HWiNFO, que nous détaillons dans notre tutoriel dédié à la surveillance PC avec HWiNFO, permet de confirmer que les fréquences RAM et les températures sont revenues à la normale après la mise à jour.

## Comprendre les versions AGESA : quel firmware pour quel Ryzen

AGESA, pour AMD Generic Encapsulated Software Architecture, est le socle logiciel qu’AMD fournit à tous les fabricants de cartes mères, à charge pour chacun de l’intégrer dans son propre BIOS. C’est ce numéro de version, bien plus que le numéro de BIOS propre au fabricant, qui détermine réellement la compatibilité CPU et RAM d’une carte mère AM5.

| Révision AGESA | Apport principal | CPU ou usage concerné | 
|---|---|---|
| ComboAM5 PI 1.1.7.0 | Version minimale pour démarrer (POST) | Ryzen 9000 non-X3D | 
| ComboAM5 PI 1.1.7.0 Patch A | Compatibilité de démarrage élargie sur X670, B650 et A620 | Ryzen 9000 (selon Gigabyte) | 
| ComboAM5 PI 1.2.0.2 | Amélioration rapportée de la compatibilité mémoire (non confirmée par une source officielle) | Kits DDR5 haute fréquence | 
| ComboAM5 PI 1.2.0.3 | Correctifs de sécurité : dépassement mémoire PeCoffLoader et vérification de signature microcode | Tous CPU AM5 | 
| ComboAM5 PI 1.3.0.1 (2025) | Ajout de la prise en charge | Ryzen 9000 séries X3D | 

Le point important à retenir : deux cartes mères de marques différentes portant des numéros de BIOS totalement distincts peuvent embarquer la même révision AGESA, et donc offrir exactement le même niveau de compatibilité. Inversement, deux versions BIOS consécutives chez un même fabricant peuvent conserver la même révision AGESA si la mise à jour ne portait que sur un réglage mineur sans rapport avec la compatibilité CPU ou RAM.

Ces informations évoluent vite, et certains apports listés ci-dessus proviennent de guides techniques tiers plutôt que de bulletins officiels AMD. La meilleure pratique reste donc de toujours vérifier la liste de compatibilité officielle de votre référence exacte via le site chipset AM5 d’AMD, plutôt que de se fier uniquement à un tableau générique, aussi à jour soit-il.

## Tableau comparatif des méthodes de flash par marque

Avant de choisir votre méthode, ce tableau récapitule les différences pratiques entre le flash sans CPU et le flash classique depuis le BIOS, marque par marque.

| Marque / méthode | CPU requis ? | Emplacement | Format du fichier | Durée approximative | 
|---|---|---|---|---|
| ASUS BIOS FlashBack | Non | Port USB dédié « BIOS » + bouton arrière | Nom précis selon le modèle, à la racine de la clé | Quelques minutes, LED clignotante | 
| MSI Flash BIOS Button | Non | Port USB dédié + bouton Flash BIOS | Fichier à la racine, FAT32 | 5 à 10 minutes | 
| Gigabyte Q-Flash Plus | Non | Port USB dédié + bouton Q-Flash | Renommé « GIGABYTE.bin » | Quelques minutes, LED clignotante | 
| ASRock BIOS Flashback | Non | Port USB dédié + bouton (variable selon modèle) | Voir manuel du modèle exact | Variable selon modèle | 
| Méthode classique (M-Flash / EZ Flash / Q-Flash / Instant Flash) | Oui, CPU fonctionnel requis | Depuis le menu BIOS/UEFI | Fichier à la racine, FAT32 | 3 à 8 minutes | 

Pour un premier montage ou un changement de processeur qui empêche déjà le démarrage, la méthode sans CPU reste la seule option viable. Pour une mise à jour de routine sur un système qui fonctionne déjà, la méthode classique depuis le BIOS est généralement plus rapide, car elle évite d’avoir à démonter le radiateur du processeur ou à débrancher les câbles d’alimentation.

## Les 7 erreurs les plus fréquentes lors d’une mise à jour BIOS

La plupart des cartes mères rendues inutilisables après un flash ne le sont pas à cause d’un défaut matériel, mais d’une de ces sept erreurs, toutes évitables.

1. **Couper l’alimentation pendant le flash.** Même une coupure de quelques secondes peut laisser le firmware dans un état incomplet et rendre la carte muette au démarrage suivant.
2. **Flasher le mauvais fichier.** Confondre une révision de carte, une variante WiFi et non-WiFi, ou un modèle proche dans la même gamme reste la deuxième cause la plus fréquente de blocage.
3. **Utiliser une clé USB mal formatée.** Une clé en NTFS ou exFAT n’est tout simplement pas lue par la plupart des ports BIOS dédiés, sans message d’erreur explicite.
4. **Télécharger depuis une source non officielle.** Un fichier modifié ou corrompu récupéré sur un forum peut sembler fonctionner au moment de la copie, puis échouer en plein flash.
5. **Ignorer une pile CMOS fatiguée.** Sur une carte mère de plus de deux ou trois ans, une pile faible augmente sensiblement le risque de coupure inopinée pendant l’opération.
6. **Oublier de reconfigurer après le flash.** Repartir sur une RAM au ralenti pendant des semaines parce que le profil EXPO n’a pas été réactivé reste étonnamment courant.
7. **Flasher une version plus ancienne alors qu’un CPU récent est déjà installé.** Un downgrade peut faire disparaître la prise en charge du processeur en place, provoquant un blocage qui ne peut alors être résolu que via la méthode sans CPU si la carte la propose.

Le guide de Grosbill sur la mise à jour du BIOS sans plantage recense des précautions similaires côté PC portable, où la marge d’erreur est encore plus faible en l’absence de fonction FlashBack.

## Dépannage : 10 problèmes courants et leurs solutions

Si malgré ces précautions quelque chose se passe mal, voici les symptômes les plus signalés et la marche à suivre pour chacun.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Écran noir, aucun signal après le flash | RAM ou GPU mal réinsérée, ou reconfiguration nécessaire | Réinitialiser le CMOS via le jumper ou le bouton Clear CMOS, réinsérer RAM et carte graphique | 
| Aucun démarrage, aucun bip, aucune LED | Alimentation coupée pendant un flash précédent | Tenter un nouveau flash via FlashBack si disponible, sinon contacter le support du fabricant | 
| Boucle de redémarrage continue | Profil mémoire incompatible réactivé automatiquement | Réinitialiser le BIOS aux valeurs par défaut, réactiver EXPO/XMP seulement après un démarrage stable | 
| LED FlashBack qui clignote sans jamais s’arrêter | Fichier BIOS mal nommé ou corrompu | Revérifier le nom exact attendu par le fabricant et le hash SHA-256 du fichier | 
| Clé USB non détectée par le port BIOS | Clé formatée en NTFS/exFAT, ou branchée sur le mauvais port | Reformater en FAT32, utiliser exclusivement le port identifié « BIOS » | 
| Message « fichier BIOS invalide » | Fichier destiné à un autre modèle ou une autre révision de carte | Retélécharger le fichier exact pour la référence et la révision identifiées à l’étape 1 | 
| RAM non reconnue à la fréquence annoncée après la mise à jour | Profil EXPO/XMP désactivé par la réinitialisation du flash | Réactiver le profil dans l’onglet mémoire du BIOS | 
| Nouveau CPU toujours non reconnu malgré le flash | Version AGESA insuffisante malgré la mise à jour, ou flash resté incomplet | Vérifier le numéro de version affiché après redémarrage, recommencer le flash si nécessaire | 
| Courbes de ventilateurs remises à un profil bruyant | Réinitialisation complète des réglages personnalisés | Reconfigurer manuellement dans l’onglet dédié (Smart Fan, Fan Xpert…) | 
| Message « CMOS checksum error » au premier redémarrage | Comportement normal après un flash, ou pile CMOS faible | Entrer dans le BIOS, charger les réglages par défaut, remplacer la pile si elle a plus de trois ans | 

Le guide de JustGeek sur la mise à jour du BIOS propose une check-list complémentaire utile si vous préférez repartir de zéro sur un système qui refuse obstinément de redémarrer normalement.

## Script de vérification complet après mise à jour

Pour clore proprement l’opération, voici un script PowerShell qui rassemble en un seul passage tout ce que ce tutoriel vous a appris à vérifier manuellement : version BIOS, modèle de carte mère, processeur reconnu et vitesse réelle de la RAM. Enregistrez-le sous le nom `verification-bios-am5.ps1` et exécutez-le après chaque mise à jour.

```
# verification-bios-am5.ps1
Write-Host "=== Verification post mise a jour BIOS AM5 ===" -ForegroundColor Cyan
$bios = Get-CimInstance Win32_BIOS
Write-Host "`nBIOS actuel :"
Write-Host "  Fabricant   : $($bios.Manufacturer)"
Write-Host "  Version     : $($bios.SMBIOSBIOSVersion)"
Write-Host "  Date        : $($bios.ReleaseDate)"
$board = Get-CimInstance Win32_BaseBoard
Write-Host "`nCarte mere :"
Write-Host "  Modele      : $($board.Product)"
Write-Host "  Revision    : $($board.Version)"
$cpu = Get-CimInstance Win32_Processor
Write-Host "`nProcesseur :"
Write-Host "  Nom         : $($cpu.Name)"
Write-Host "  Coeurs      : $($cpu.NumberOfCores)"
$ram = Get-CimInstance Win32_PhysicalMemory
$vitesseMoyenne = ($ram | Measure-Object -Property Speed -Average).Average
Write-Host "`nMemoire :"
Write-Host "  Modules     : $($ram.Count)"
Write-Host "  Vitesse moy.: $vitesseMoyenne MT/s (comparez au profil EXPO/XMP attendu)"
Write-Host "`n=== Fin de la verification ===" -ForegroundColor Cyan
```
Exemple de sortie sur un système correctement reconfiguré après le flash :

```
=== Verification post mise a jour BIOS AM5 ===
BIOS actuel :
  Fabricant   : American Megatrends International, LLC.
  Version     : 7D78v1C
  Date        : 20260214000000.000000+000
Carte mere :
  Modele      : MAG B650 TOMAHAWK WIFI
  Revision    : 1.0
Processeur :
  Nom         : AMD Ryzen 9 9800X3D
  Coeurs      : 8
Memoire :
  Modules     : 2
  Vitesse moy.: 6000 MT/s (comparez au profil EXPO/XMP attendu)
=== Fin de la verification ===
```
Si la vitesse mémoire affichée reste proche de 4 800 ou 5 200 MT/s alors que votre kit est annoncé à 6 000 MT/s ou plus, c’est le signe que le profil EXPO ou XMP n’a pas été réactivé après le flash, exactement le scénario décrit à l’étape 13. Ce script devient ainsi votre projet de référence, réutilisable à chaque future mise à jour, y compris lors de l’installation d’un prochain processeur.

## Astuces avancées pour les utilisateurs expérimentés

Une fois la procédure de base maîtrisée, quelques réglages supplémentaires méritent d’être connus.

**Le double BIOS.** Certaines cartes haut de gamme embarquent deux puces de firmware distinctes, avec un interrupteur physique pour basculer de l’une à l’autre. En cas de flash raté sur la puce principale, basculer sur la seconde permet souvent de redémarrer normalement puis de reprendre la mise à jour dans de meilleures conditions.

**Le downgrade réfléchi.** Revenir à une version antérieure reste possible sur la plupart des cartes via la même procédure de flash, en sélectionnant simplement un fichier plus ancien. Cette option a du sens si une version récente a introduit une instabilité mémoire chez vous spécifiquement, mais elle redevient risquée si un CPU récent, non supporté par l’ancienne révision AGESA, est déjà installé.

**Les canaux bêta.** Plusieurs fabricants publient des BIOS bêta en avance sur leur page de support ou sur des forums officiels, en général pour corriger un bug précis avant la sortie d’une version stable. Réservez ces versions aux cas où vous rencontrez déjà le bug documenté, jamais en usage préventif sur une machine de production.

**Le flash sans carte graphique dédiée.** Sur un processeur doté de graphismes intégrés, la méthode classique depuis le BIOS fonctionne sans GPU externe. Pour un CPU sans partie graphique, seule la méthode FlashBack, qui ne nécessite aucun affichage, reste utilisable en cas de panne de la carte graphique principale.

**L’automatisation du contrôle.** Le script de vérification présenté plus haut peut être programmé via le Planificateur de tâches Windows pour s’exécuter à chaque démarrage pendant une semaine suivant une mise à jour, une façon simple de détecter une régression de fréquence RAM ou un capteur qui serait passé inaperçu.

## BIOS AM5 vs Intel LGA1851 : ce qui diffère

La philosophie de mise à jour n’est pas identique d’une plateforme à l’autre, et cela influence directement la fréquence à laquelle un utilisateur doit s’en soucier.

AMD a fait de la longévité du socket AM5 un argument commercial explicite, avec plusieurs générations de Ryzen prévues sur la même base physique. Cette approche multiplie mécaniquement les occasions de mise à jour BIOS, puisque chaque nouvelle génération de processeurs doit être intégrée au firmware de cartes mères parfois vendues deux ou trois ans plus tôt. C’est tout l’enjeu détaillé dans notre article sur Zen 6 et le maintien de l’AM5 jusqu’en 2029.

Intel, avec le socket LGA1851, a historiquement suivi un cycle de renouvellement plus court, un nouveau socket accompagnant plus fréquemment chaque nouvelle génération de processeurs. Les utilisateurs Intel se retrouvent donc moins souvent dans la situation « nouveau CPU sur ancienne carte mère », mais changent plus régulièrement de carte mère dans son ensemble plutôt que de se contenter d’un flash BIOS.

Concrètement, pour un utilisateur qui prévoit de faire évoluer uniquement son processeur dans les prochaines années sans changer sa carte mère, la plateforme AM5 reste la mieux positionnée, à condition de maîtriser précisément la procédure de mise à jour décrite dans ce tutoriel.

## Foire aux questions

### Faut-il un processeur installé pour flasher le BIOS d’une carte AM5 ?

Non, à condition que votre carte mère dispose d’une fonction de flash sans CPU (BIOS FlashBack, Q-Flash Plus, Flash BIOS Button ou équivalent ASRock), présente sur la quasi-totalité des cartes AM5 vendues depuis 2022. Seule une alimentation électrique connectée est nécessaire.

### Combien de temps dure une mise à jour de BIOS sur AM5 ?

Le flash lui-même prend entre 3 et 10 minutes selon le fabricant et la méthode choisie. En comptant l’identification du matériel, le téléchargement, la vérification du fichier et la reconfiguration des réglages après redémarrage, prévoyez environ 45 minutes au total.

### Que se passe-t-il si je coupe l’alimentation pendant le flash ?

C’est le scénario qui rend le plus souvent une carte mère inutilisable. Le firmware reste dans un état incomplet et la carte peut ne plus démarrer du tout. Certaines cartes haut de gamme disposent d’une puce BIOS de secours qui permet une récupération, mais cette fonction n’est pas systématique sur l’ensemble de la gamme AM5.

### Comment savoir si mon BIOS est déjà à jour ?

Comparez la version affichée par la commande `Get-CimInstance Win32_BIOS`, ou visible dans l’onglet Main du BIOS, à la dernière version publiée sur la page de support officielle de votre référence exacte, révision matérielle comprise.

### Faut-il mettre à jour le BIOS même si le PC fonctionne bien ?

Pas systématiquement. Si vous ne changez ni CPU ni RAM et que le système reste stable, une mise à jour n’apporte pas toujours un bénéfice visible. Elle reste recommandée en cas de correctif de sécurité documenté par le fabricant, ou avant l’installation d’un nouveau composant.

### Le socket AM5 reste-t-il un bon choix de plateforme en 2026 ?

Oui. AMD a positionné l’AM5 comme une plateforme de longue durée, un choix cohérent avec les informations que nous avions détaillées à propos de Zen 6. Investir dans une carte mère AM5 aujourd’hui reste pertinent pour qui prévoit de faire évoluer son processeur dans les prochaines années sans tout remplacer.

### Peut-on revenir à une version de BIOS antérieure ?

La plupart des fabricants le permettent via la même procédure de flash, en sélectionnant simplement un fichier plus ancien. Attention toutefois : un downgrade peut faire disparaître la prise en charge d’un CPU récent déjà installé, ce qui empêcherait le système de redémarrer.

### La mise à jour du BIOS efface-t-elle mes fichiers ou mes programmes ?

Non. Le BIOS est un firmware totalement indépendant de Windows et de vos données stockées sur le disque. Seuls les réglages internes du BIOS lui-même (profils mémoire, ordre de démarrage, courbes de ventilateurs) sont réinitialisés par l’opération.