*Dernière mise à jour : 04 juin 2026.* Votre PC portable gaming hurle, grimpe à 100 °C et perd des FPS dès que l’action s’intensifie ? Le coupable porte un nom : le bridage thermique, ou *thermal throttling*. La bonne nouvelle, c’est qu’un utilitaire gratuit d’à peine 1,71 Mo selon la fiche FileCroco mise à jour le 2 mai 2026, la version 9.7.3 (bêta) publiée en avril 2025 par TechPowerUp et qui ajoute, selon TechSpot, le réglage par cœur (per-core adjustment) pour les puces Intel de 12e génération et plus récentes – un outil déjà crédité de 207 962 téléchargements sur Uptodown au compteur d’août 2026 –, peut souvent régler le problème sans tournevis, sans démontage et sans pâte thermique. Ce tutoriel **ThrottleStop** vous montre, en 12 étapes et une trentaine de minutes, comment undervolter (sous-volter) le processeur Intel de votre portable pour gagner jusqu’à 15 °C, stopper le bridage et récupérer les images par seconde perdues.

Nous couvrons l’installation de **ThrottleStop** 9.7.3 (bêta, avril 2025), qui succède à la 9.7.2 bêta du 16 janvier 2025 – une version que le fil NotebookTalk situe à la même date et qui retire l’ancien code de multiplicateur maximal tout en corrigeant la sauvegarde de l’offset PROCHOT, en plus d’ajouter la prise en charge des puces Arrow Lake selon UltrabookReview –, elle-même publiée après la 9.7 du 26 décembre 2024 (référencée par Uptodown) et la 9.6 du 26 mai 2023. Cette version 9.7, toujours proposée par TechPowerUp au format ZIP de 2 Mo selon MajorGeeks, cumule déjà 103 488 téléchargements recensés en septembre 2026, et FileHorse la qualifiait encore de sortie « il y a 10 mois » en octobre 2025 – preuve qu’elle est restée la version de référence tout au long de 2025. Le guide 2026 d’UltrabookReview continue même de citer la 9.7 comme version stable de référence, avec un réglage FIVR affiné pour les puces HX/K de 10e génération, tandis que la page SourceForge du projet affiche une dernière mise à jour au 6 janvier 2025. Nous détaillons aussi la mesure de référence avec le TS Bench, l’undervolting du CPU Core et du CPU Cache via la fenêtre FIVR, le réglage des limites de puissance (TPL), la gestion du BD PROCHOT, puis la sauvegarde d’un profil qui se lance au démarrage de Windows. Vous trouverez aussi 8 pièges courants, 8 problèmes de dépannage et une FAQ complète. Ce guide s’adresse aux processeurs Intel : si vous avez un CPU AMD Ryzen, l’undervolt passe par Ryzen Master (voir plus bas).

## Qu’est-ce que ThrottleStop et pourquoi undervolter un CPU portable ?

**ThrottleStop** est un petit logiciel Windows gratuit créé par Kevin Glynn, plus connu sous le pseudonyme « UncleWebb », et distribué par TechPowerUp. Sa mission tient en une phrase : rendre le contrôle du processeur à l’utilisateur. Sur un ordinateur portable, le constructeur applique des réglages de puissance et de tension volontairement conservateurs pour garantir la stabilité de toutes les puces sorties d’usine, y compris les plus mauvaises. Résultat : la plupart des CPU reçoivent bien plus de tension qu’ils n’en ont réellement besoin, ce qui produit une chaleur inutile.

Cette chaleur excédentaire déclenche le *thermal throttling* : dès que le processeur atteint sa température maximale (le Tjmax, généralement 100 °C sur les puces Intel mobiles), il abaisse brutalement sa fréquence pour ne pas se détériorer. Vos FPS chutent, le portable devient bruyant et les temps de compilation ou d’export s’allongent. L’undervolting consiste à réduire la tension fournie au CPU tout en gardant les mêmes fréquences : le processeur consomme moins, chauffe moins, et peut donc rester à haute fréquence plus longtemps avant d’être bridé.

ThrottleStop agit sur les trois grands types de bridage identifiés par UncleWebb : le bridage thermique (température), le bridage par limite de puissance (Power Limit / TDP) et le bridage lié au régulateur de tension (VRM). Contrairement à un overclock qui augmente les fréquences et les risques, un undervolt bien mené ne réduit que la tension : il n’y a aucune usure supplémentaire, et le gain se lit immédiatement au thermomètre. C’est l’une des rares optimisations logicielles qui améliore à la fois les performances, les nuisances sonores et l’autonomie.

## Undervolt, offset de tension et bridage thermique : les concepts clés

Avant de plonger dans le tutoriel **ThrottleStop**, il faut maîtriser quatre notions. Elles reviennent dans toutes les étapes et évitent 90 % des erreurs de débutant.

### L’offset de tension (Offset Voltage)

Vous ne fixez pas une tension absolue : vous appliquez un *décalage négatif* (offset) par rapport à la courbe de tension d’origine. Un offset de -100 mV signifie « donne 100 millivolts de moins que d’habitude, à chaque fréquence ». La courbe garde sa forme, elle est simplement translatée vers le bas. C’est ce qui rend l’undervolt sûr : le CPU continue de demander plus de tension quand il monte en fréquence, mais toujours 100 mV en dessous de la valeur d’usine.

### FIVR, CPU Core et CPU Cache

La fenêtre FIVR (Fully Integrated Voltage Regulator) est le cœur de l’undervolt dans ThrottleStop. Sur la majorité des processeurs Intel mobiles, deux rails comptent : **CPU Core** (les cœurs) et **CPU Cache** (le cache et l’anneau interne). Sur la plupart des architectures, ces deux rails sont liés : il faut leur appliquer le *même* offset, sous peine d’instabilité. On peut aussi undervolter l’iGPU (Intel GPU) et le System Agent, mais ces gains sont marginaux et plus risqués – on les laisse de côté pour un premier réglage.

### PL1, PL2 et le TPL

La fenêtre TPL (Turbo Power Limits) définit combien de watts le CPU a le droit de consommer. PL1 est la limite soutenue (le TDP réel après quelques secondes), PL2 la limite en pic (turbo court). Beaucoup de portables imposent un PL1 très bas pour rester silencieux, ce qui bride les performances même quand la température le permettrait. Ajuster ces valeurs – avec prudence – complète parfaitement l’undervolt.

## ThrottleStop vs Intel XTU vs Ryzen Master : le bon outil selon votre CPU

ThrottleStop n’est pas le seul logiciel d’undervolt, mais c’est le plus léger et le plus complet pour les portables Intel. Point crucial : **ThrottleStop ne fonctionne que sur les processeurs Intel**. Si votre machine embarque un CPU AMD Ryzen, il faut passer par Ryzen Master (voir notre tutoriel dédié à l’undervolt du GPU avec MSI Afterburner pour la partie carte graphique). Le tableau ci-dessous résume les différences.

| Critère | ThrottleStop 9.7.3 | Intel XTU | AMD Ryzen Master | 
|---|---|---|---|
| Éditeur | UncleWebb / TechPowerUp | Intel | AMD | 
| CPU compatibles | Intel uniquement | Intel uniquement | AMD Ryzen uniquement | 
| Taille / poids | < 1 Mo, portable | Installeur lourd | Installeur lourd | 
| Undervolt (offset) | Oui (FIVR) | Oui | Oui (Curve Optimizer) | 
| Limites de puissance | Oui (TPL : PL1/PL2) | Oui | Oui (PPT/TDC/EDC) | 
| Benchmark intégré | Oui (TS Bench) | Oui | Non | 
| BD PROCHOT / C-States | Oui | Non | Non | 
| Prix | Gratuit | Gratuit | Gratuit | 

Intel XTU fait globalement le même travail que ThrottleStop côté undervolt, mais il est plus lourd, moins réactif et offre moins de contrôle sur le BD PROCHOT et les C-States. La plupart des passionnés de portables Intel préfèrent ThrottleStop pour sa légèreté et sa gestion fine des profils. Pour les tests de stabilité, XTU et ThrottleStop se complètent bien : on peut valider un offset dans XTU puis piloter le tout depuis ThrottleStop. Le test comparatif de Tom’s Hardware France sur l’undervolting d’un portable gaming confirme les gains thermiques mesurables des deux approches.

## Prérequis : versions, matériel compatible et sauvegardes

Réunissez les éléments suivants avant de commencer. La compatibilité du processeur est le point le plus important : les puces les plus récentes ont souvent l’undervolt verrouillé (voir la section Plundervolt).

| Élément | Version / recommandation | Rôle | 
|---|---|---|
| ThrottleStop | 9.7.3 (avril 2025) | Undervolt et gestion du throttling | 
| Système d’exploitation | Windows 10 ou Windows 11 (64 bits) | ThrottleStop est Windows uniquement | 
| Processeur | Intel Core mobile (4e à 14e gén.) | Undervolt via FIVR ; à vérifier au cas par cas | 
| HWiNFO (recommandé) | Dernière version | Surveillance fine des tensions et températures | 
| Outil de charge | Cinebench R23 ou TS Bench | Test de stabilité sous charge | 
| .NET Framework | 4.x (fourni avec Windows) | Dépendance d’exécution | 
| Point de restauration | À créer avant de commencer | Filet de sécurité en cas d’instabilité | 

Deux précautions valent de l’or. D’abord, créez un point de restauration Windows : un undervolt trop agressif peut provoquer un écran bleu (BSOD) au démarrage, et il est rassurant de pouvoir revenir en arrière. Ensuite, installez HWiNFO en suivant notre tutoriel : c’est le meilleur moyen de vérifier en temps réel que votre offset est bien appliqué et de surveiller la température, la fréquence et la puissance package pendant les tests.

## ThrottleStop est-il dangereux ? Plundervolt (CVE-2019-11157) et le verrouillage FIVR

C’est la question numéro un, et la réponse est nuancée. Un undervolt qui ne touche qu’à l’Offset Voltage ne présente **aucun risque physique** pour votre matériel : vous réduisez la tension, jamais vous ne l’augmentez au-delà des valeurs d’usine. Le pire scénario est un plantage ou un écran bleu si l’offset est trop agressif – sans conséquence durable, un simple redémarrage annule le réglage non sauvegardé.

Il existe toutefois une limite importante à connaître. En décembre 2019, des chercheurs ont révélé Plundervolt (référencée CVE-2019-11157), une attaque exploitant l’interface d’undervolting pour corrompre les enclaves sécurisées Intel SGX en manipulant la tension. Elle a touché les processeurs Intel Core de la 6e à la 10e génération. Pour la corriger, Intel a diffusé une mise à jour de microcode qui, combinée à une mise à jour du BIOS, permet de *verrouiller* l’interface d’undervolt : la tension est alors figée aux valeurs par défaut. Plus récemment, un avis publié le 6 août 2025 dans la GitHub Advisory Database a référencé une faille distincte, **CVE-2025-7771**, touchant cette fois le pilote ThrottleStop.sys en version 3.0.0.0 : une raison supplémentaire de toujours télécharger l’outil depuis la page officielle TechPowerUp plutôt qu’un miroir douteux, et de garder son antivirus à jour.

Conséquence concrète : sur de nombreux portables récents – souvent à partir de la 11e génération, ou après certaines mises à jour du BIOS – les curseurs de tension du FIVR sont grisés ou restent sans effet. Si vous appliquez un offset et que la température ne bouge pas d’un degré, c’est probablement que l’undervolt est verrouillé au niveau du microcode. Il n’existe pas de contournement logiciel fiable ; seul un BIOS déverrouillé (parfois proposé par le constructeur, ou via des menus cachés) peut rouvrir l’accès. Vous pouvez consulter la fiche Plundervolt sur Wikipédia pour le détail technique. Dans ce cas, tournez-vous vers les limites de puissance (TPL) et le BD PROCHOT, qui restent souvent accessibles.

## Étapes 1 à 4 : installer ThrottleStop et mesurer la référence

### Étape 1 – Télécharger et extraire ThrottleStop

Rendez-vous sur la page officielle TechPowerUp et téléchargez ThrottleStop 9.7.3, une archive ZIP mise en ligne en avril 2025 et compatible Windows 7 à 11 ; la page officielle affiche désormais une empreinte SHA256 (7B5E…7098A) que vous pouvez comparer après téléchargement pour vérifier que le fichier n’a pas été altéré. Si un miroir vous propose plutôt l’ancienne archive 9.7 du 26 décembre 2024, l’empreinte à contrôler change : SHA256 d83d…800d, selon les fiches TechPowerUp et Uptodown. C’est une archive ZIP : il n’y a pas d’installation. Extrayez-la dans un dossier permanent, par exemple *C:\ThrottleStop*. Évitez le Bureau ou le dossier Téléchargements, car vous pointerez plus tard le démarrage automatique vers ce chemin. Lancez *ThrottleStop.exe* ; au premier démarrage, un avertissement rappelle que l’outil est destiné aux utilisateurs avertis – acceptez pour continuer.

### Étape 2 – Comprendre l’interface principale

La fenêtre principale se lit en trois zones. À gauche, les cases de contrôle globales (Speed Shift EPP, Disable Turbo, BD PROCHOT). Au centre, les profils (numérotés de 1 à 4) et les boutons FIVR, TPL et Options. À droite, la lecture en direct de la fréquence, de la température (C0 %) et de l’état du throttling. En bas, la barre d’état affiche des drapeaux comme PROCHOT, POWER ou THERMAL quand un bridage se produit – c’est votre tableau de bord.

### Étape 3 – Mesurer la température de référence avec le TS Bench

Avant tout réglage, mesurez l’état actuel. Cliquez sur **TS Bench**, choisissez la taille 960M et lancez le test. Notez la température maximale, la fréquence moyenne et surtout la présence de drapeaux THERMAL ou POWER. C’est votre point de comparaison. Voici un exemple de relevé typique sur un portable gaming bridé thermiquement :

```
=== TS Bench 960M – AVANT undervolt ===
Processeur : Intel Core i7 (13e gen, TDP 45 W)
Duree du test        : 00:00:58
Temperature max      : 100 C  <-- Tjmax atteint
Frequence moyenne    : 3,12 GHz (bridee)
Puissance package    : 45 W (PL1)
Drapeaux actifs      : THERMAL, PROCHOT
Erreurs de calcul    : 0
Verdict              : bridage thermique confirme
```
### Étape 4 – Ouvrir la fenêtre FIVR

Cliquez sur le bouton **FIVR** pour ouvrir la fenêtre de contrôle de tension. Si les curseurs sont grisés ou inaccessibles, votre undervolt est verrouillé (relisez la section Plundervolt) : passez directement aux limites de puissance. S’ils sont actifs, félicitations, vous pouvez undervolter. Sélectionnez d’abord **CPU Core** dans la liste « FIVR Control », puis cochez *Unlock Adjustable Voltage* : c’est la case qui déverrouille le curseur d’offset.

## Étapes 5 à 8 : undervolter le CPU Core et le CPU Cache

### Étape 5 – Appliquer un premier offset sur le CPU Core

Toujours dans FIVR, avec CPU Core sélectionné et *Unlock Adjustable Voltage* coché, descendez le curseur **Offset Voltage** à -80 mV. C’est une valeur de départ prudente que la quasi-totalité des puces encaissent sans broncher. Ne partez jamais directement à -150 mV : vous risqueriez un plantage immédiat sans savoir à quel palier votre CPU décroche. La fenêtre FIVR doit ressembler à ceci :

```
FIVR Control : CPU Core
[x] Unlock Adjustable Voltage
Offset Voltage      : -80,1 mV
[x] CPU Core         (rail actif)
Default VID         : 1,250 V
Idle / Turbo VID    : suivent la courbe -80 mV
[ ] CPU Cache        (a regler a l'etape 6)
```
### Étape 6 – Appliquer le même offset sur le CPU Cache

Sélectionnez maintenant **CPU Cache** dans la liste FIVR, cochez *Unlock Adjustable Voltage* et appliquez exactement le même offset : -80 mV. Sur la plupart des processeurs Intel, les rails Core et Cache sont électriquement liés : un déséquilibre entre les deux est l’une des causes les plus fréquentes d’instabilité. Gardez-les toujours identiques, sauf indication contraire propre à votre modèle de puce.

### Étape 7 – Valider avec « OK – Save voltages immediately »

Cliquez sur **Apply** pour appliquer l’offset en direct, puis sur **OK – Save voltages immediately**. Cette option est capitale : elle réapplique automatiquement l’undervolt à chaque réveil de veille. Sans elle, de nombreux portables « oublient » l’offset après une mise en veille, et le CPU repart à pleine tension sans que vous vous en aperceviez. Le tableau ci-dessous propose des paliers d’undervolt indicatifs selon le profil de silicium.

| Palier | Offset Core + Cache | Profil de puce | Gain thermique typique | 
|---|---|---|---|
| Découverte | -50 mV | Toutes, ultra-sûr | -3 à -6 °C | 
| Prudent | -80 mV | Point de départ recommandé | -6 à -10 °C | 
| Standard | -100 à -125 mV | Bon silicium (fréquent) | -10 à -15 °C | 
| Agressif | -130 à -160 mV | Très bon silicium | -15 à -20 °C | 
| Extrême | > -180 mV | Loterie du silicium, rare | > -20 °C, souvent instable | 

### Étape 8 – Vérifier que l’offset est bien appliqué

Ouvrez HWiNFO en mode capteurs et cherchez la ligne « IA Offset » ou « Core VID ». La valeur doit refléter votre offset (-80 mV). Si elle reste à 0, l’undervolt n’est pas actif : soit il est verrouillé, soit vous avez oublié de cocher *Unlock Adjustable Voltage* sur les deux rails. Relancez ensuite un TS Bench court pour confirmer la baisse de température :

```
=== HWiNFO – verification de l'offset ===
CPU Core Voltage (SVID)   : 1,168 V (etait 1,248 V)
IA Offset                 : -80,1 mV  <-- offset applique
CPU Package Temperature   : 88 C (etait 100 C)
Core Clocks (moyenne)     : 3,58 GHz (etait 3,12 GHz)
Thermal Throttling        : NON
=> undervolt actif et stable
```
## Étapes 9 à 12 : tester la stabilité et sauvegarder le profil

### Étape 9 – Descendre par paliers de -10 mV

Votre -80 mV tient ? Descendez à -90 mV, appliquez, testez. Puis -100 mV, et ainsi de suite, par pas de -10 mV. À chaque palier, lancez un TS Bench 960M complet. L’objectif est de trouver le point de rupture, puis de remonter d’un cran pour la marge de sécurité. La règle d’or : un undervolt stable au TS Bench mais qui plante en jeu est un undervolt *trop agressif*. Gardez toujours 10 à 20 mV de marge sous votre plus bas offset stable.

### Étape 10 – Valider la stabilité en profondeur

Le TS Bench est un bon filtre rapide, mais la vraie validation demande une charge longue et variée. Enchaînez un Cinebench R23 en boucle 30 minutes, puis une vraie session de jeu de 1 à 2 heures. Surveillez trois signaux d’instabilité : les redémarrages ou BSOD (offset trop bas), les erreurs de calcul dans le TS Bench (supérieures à 0), et les avertissements WHEA dans l’Observateur d’événements de Windows, qui trahissent des corrections d’erreurs matérielles silencieuses.

```
# Verifier les erreurs materielles WHEA (PowerShell, admin)
Get-WinEvent -LogName System |
  Where-Object { $_.Id -eq 18 -or $_.Id -eq 19 } |
  Select-Object -First 10 TimeCreated, Id, LevelDisplayName
# Aucun evenement WHEA-Logger => undervolt stable
# Des evenements Id 18/19 apparaissent => remontez de 10 mV
```
### Étape 11 – Régler Speed Shift EPP et désactiver le BD PROCHOT

Dans la fenêtre principale, cochez **Speed Shift EPP** et réglez la valeur entre 0 et 255 (0 = performances maximales, 255 = économie maximale ; 80 est un bon compromis en jeu). Ouvrez ensuite **Options** et repérez la case **BD PROCHOT** : si elle est cochée, un composant comme le chargeur ou le GPU peut forcer le CPU à se brider même à basse température. La décocher règle bien des bridages inexpliqués – à condition de surveiller les températures ensuite.

### Étape 12 – Sauvegarder le profil et lancer ThrottleStop au démarrage

Nommez votre profil (par exemple « Gaming ») et cliquez sur **Save** pour l’écrire dans le fichier ThrottleStop.ini. Pour que l’undervolt s’applique à chaque démarrage, ne mettez pas ThrottleStop dans le dossier Démarrage classique : créez plutôt une tâche planifiée qui le lance avec les privilèges administrateur, sinon Windows demandera une confirmation à chaque ouverture. La commande suivante crée cette tâche en une ligne :

```
:: Lancer ThrottleStop au demarrage, sans invite UAC (cmd admin)
schtasks /Create /TN "ThrottleStop" ^
  /TR "C:\ThrottleStop\ThrottleStop.exe" ^
  /SC ONLOGON /RL HIGHEST /F
:: Verifier la tache
schtasks /Query /TN "ThrottleStop"
:: => l'undervolt s'applique des l'ouverture de session
```
## Régler les limites de puissance (TPL) et Speed Shift EPP

L’undervolt réduit la température ; les limites de puissance décident de la performance soutenue. Ouvrez la fenêtre **TPL** (Turbo Power Limits). Vous y trouverez PL1 (Long Power) et PL2 (Short Power), ainsi que le Turbo Time Limit. Sur un portable dont le refroidissement le permet, relever PL1 au niveau de PL2 peut débloquer une performance soutenue nettement supérieure – mais uniquement *après* avoir sécurisé l’undervolt, sinon vous ne ferez que chauffer davantage.

```
=== Fenetre TPL – exemple portable 45 W bien refroidi ===
[x] MMIO Lock desactive (si accessible)
Long Power PL1  (Turbo Boost Long)  : 55 W
Short Power PL2 (Turbo Boost Short) : 65 W
Turbo Time Limit (Tau)              : 56 s
[x] Speed Shift        active
Clamp                               : decoche
=> a valider avec un test thermique de 30 min
```
Attention : augmenter les limites de puissance ne sert à rien si votre système de refroidissement sature déjà. Sur un ultraportable fin, mieux vaut souvent *baisser* PL1 pour un fonctionnement silencieux et frais. La combinaison gagnante sur la majorité des portables gaming reste : undervolt agressif mais stable + PL1 relevé modérément + BD PROCHOT décoché. Testez chaque variable isolément pour comprendre son effet réel sur votre machine.

## BD PROCHOT, C-States et Speed Shift : aller plus loin

Trois réglages avancés méritent votre attention une fois l’undervolt maîtrisé. Le **BD PROCHOT** (Bi-Directional PROCHOT) est un signal qui permet à un autre composant – GPU, VRM, chargeur – d’ordonner au CPU de se brider. Sur certains portables, il se déclenche à des températures GPU pourtant raisonnables et plombe les performances CPU. Le décocher dans Options est souvent le gain le plus spectaculaire, mais surveillez alors la température des autres composants avec HWiNFO.

Les **C-States** gèrent les états de veille des cœurs au repos. Les laisser activés améliore l’autonomie et réduit la température au ralenti, sans nuire aux performances en charge. Enfin, **Speed Shift Technology** (SST) permet au CPU de changer de fréquence en quelques millisecondes plutôt qu’en dizaines de millisecondes : cochez la case Speed Shift et laissez ThrottleStop l’activer au démarrage. Pour la partie graphique et l’affichage des FPS, notre guide MSI Afterburner complète idéalement ce réglage CPU.

## Undervolter l’iGPU Intel et le System Agent (optionnel)

Une fois le CPU Core et le CPU Cache stabilisés, deux rails supplémentaires apparaissent dans la fenêtre FIVR : l’**Intel GPU** (iGPU) et le **System Agent** (SA). L’iGPU alimente le circuit graphique intégré ; l’undervolter apporte un léger gain thermique dans les tâches qui sollicitent le GPU embarqué (lecture vidéo, jeux légers, affichage sur écran externe). Un offset prudent de -30 à -50 mV sur ce rail est généralement sûr, à condition de le tester séparément avec une charge graphique dédiée. Sur un portable gaming équipé d’une carte dédiée, le gain reste marginal puisque l’iGPU est souvent au repos en jeu.

Le **System Agent** alimente le contrôleur mémoire et les bus internes. C’est le rail le plus délicat : un undervolt trop appuyé provoque des erreurs mémoire sournoises, parfois sans plantage immédiat, qui corrompent silencieusement des données. La recommandation des experts de l’undervolting est claire : laissez le System Agent à 0 mV, sauf besoin très précis et test mémoire approfondi (MemTest86). Le rapport bénéfice/risque penche nettement en défaveur de ce rail pour l’usage courant. Concentrez vos efforts sur le duo Core + Cache, qui apporte plus de 90 % du gain thermique total.

## Undervolt sur les portables gaming vendus en France : ROG, Legion, MSI

La compatibilité de l’undervolt dépend surtout de la marque et de la génération du portable. Chez **ASUS ROG**, très populaire en France, beaucoup de modèles Strix et Zephyrus laissent le FIVR accessible sur les générations plus anciennes, mais certains BIOS récents l’ont verrouillé ; la communauté du forum des portables ASUS francophone documente les cas au modèle près. Chez **Lenovo Legion**, l’undervolt fonctionne fréquemment, en complément de Legion Vantage pour les profils de ventilation. **MSI** (via MSI Center) et **HP Omen** présentent des situations variables selon la révision de BIOS.

Les **Razer Blade** et une partie des **Dell/Alienware** récents verrouillent souvent l’interface de tension par défaut, héritage direct de la correction Plundervolt. Avant d’acheter ou de vous lancer, la démarche est simple : identifiez la génération exacte de votre processeur Intel, mettez à jour ou notez votre version de BIOS, puis testez si les curseurs FIVR répondent. S’ils sont verrouillés, rabattez-vous sur les limites de puissance (TPL) et le BD PROCHOT, souvent encore accessibles, ainsi que sur la gestion de la ventilation propre au constructeur. Sur les machines déverrouillées, l’undervolt reste l’optimisation gratuite au meilleur rapport gain/effort pour un portable gaming.

## Résultats concrets : avant/après sur un portable gaming

Voici un exemple représentatif des gains obtenus sur un portable gaming équipé d’un Core i7 de 13e génération (TDP 45 W), avec un offset stable de -125 mV sur les rails Core et Cache. Ces valeurs sont fournies à titre d’illustration : vos résultats dépendront de votre puce (loterie du silicium), de votre refroidissement et de la charge testée.

| Mesure (charge CPU soutenue) | Avant undervolt | Après -125 mV | Gain | 
|---|---|---|---|
| Température package max | 100 °C | 85 °C | -15 °C | 
| Fréquence tous cœurs soutenue | 3,12 GHz | 3,60 GHz | +0,48 GHz | 
| Tension CPU sous charge | 1,248 V | 1,123 V | -0,125 V | 
| Drapeau THERMAL (bridage) | Actif | Inactif | Bridage supprimé | 
| Bruit ventilateurs | Maximum | Modéré | Plus silencieux | 
| FPS moyens (scène CPU-limitée) | Référence | +8 à +12 % | Gain net | 

Le schéma est presque toujours le même : la température chute de 10 à 15 °C, le CPU cesse d’atteindre son Tjmax, il maintient une fréquence plus élevée et plus régulière, et les FPS progressent surtout dans les jeux ou scènes limités par le processeur. Les ventilateurs, sollicités moins fort, deviennent plus discrets. C’est ce triple gain – température, performance, silence – qui explique la popularité durable de ThrottleStop sur les forums de portables gaming comme le fil de discussion officiel de TechPowerUp.

## 8 pièges courants de l’undervolting à éviter

La plupart des échecs d’undervolt ne viennent pas du logiciel mais de la méthode. Voici les huit erreurs qui reviennent le plus souvent, et comment les contourner.

1. **Partir trop fort d’emblée.** Commencer à -150 mV masque le vrai point de rupture. Débutez à -80 mV et descendez par -10 mV.
2. **Déséquilibrer Core et Cache.** Des offsets différents sur ces deux rails liés provoquent des plantages difficiles à diagnostiquer. Gardez-les identiques.
3. **Oublier « Save voltages immediately ».** Sans cette option, l’undervolt disparaît après une veille et le CPU repart à pleine tension.
4. **Tester uniquement au TS Bench.** Un offset stable au benchmark peut planter en jeu. Validez toujours par une vraie session de jeu prolongée.
5. **Ignorer les erreurs WHEA.** Un système qui ne plante pas mais accumule des corrections WHEA est déjà instable : remontez la tension.
6. **Ne pas lancer en administrateur.** Sans privilèges élevés, certains réglages ne s’appliquent pas et le profil ne se recharge pas au démarrage.
7. **Confondre undervolt verrouillé et undervolt inefficace.** Si la température ne bouge pas, vérifiez d’abord le verrouillage Plundervolt avant de creuser l’offset.
8. **Négliger le contexte thermique.** Un undervolt ne remplace pas une pâte thermique fatiguée ou des grilles d’aération obstruées. Nettoyez d’abord la machine.

## Dépannage : 8 problèmes fréquents avec ThrottleStop

Quand ThrottleStop ne se comporte pas comme prévu, la cause figure presque toujours dans ce tableau. Il couvre les huit situations les plus signalées par les utilisateurs francophones.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Curseurs FIVR grisés | Undervolt verrouillé (Plundervolt / BIOS) | Chercher un BIOS déverrouillé ; sinon miser sur TPL | 
| La température ne baisse pas | Offset non appliqué ou verrouillé | Vérifier « IA Offset » dans HWiNFO | 
| Écran bleu au démarrage | Offset trop agressif | Redémarrer (le profil non sauvé s’annule) et remonter de 20 mV | 
| Undervolt perdu après veille | « Save voltages immediately » non coché | Réactiver l’option dans FIVR | 
| Bridage à basse température | BD PROCHOT déclenché par un composant | Décocher BD PROCHOT dans Options | 
| Profil non appliqué au boot | Lancement sans droits administrateur | Créer une tâche planifiée /RL HIGHEST | 
| Erreurs WHEA dans l’Observateur | Instabilité silencieuse | Remonter la tension de 10 à 20 mV | 
| PL1/PL2 impossibles à modifier | Verrous MMIO / BIOS constructeur | Vérifier les options BIOS ; certains portables bloquent le TPL | 

Si votre portable reste chaud malgré un undervolt stable, le problème est peut-être ailleurs : pilotes graphiques encrassés, résidus de tension GPU, ou processus en arrière-plan. Un nettoyage propre des pilotes avec notre tutoriel DDU élimine une variable et facilite le diagnostic. Pensez aussi à vérifier que le mode d’alimentation de Windows est réglé sur « Performances optimales » pendant vos tests, puis sur un mode équilibré au quotidien.

## Astuces avancées : profils par jeu, autonomie et Game Mode

ThrottleStop gère quatre profils indépendants, et c’est là que réside sa vraie puissance. Créez un profil « Gaming » agressif (undervolt profond, PL1 relevé, Speed Shift EPP à 0) et un profil « Batterie » économe (undervolt conservateur, PL1 abaissé, EPP à 128 ou plus). ThrottleStop peut basculer automatiquement de l’un à l’autre selon que le portable est branché ou sur batterie, grâce à l’onglet Options et à la case « Battery » / « AC ». Vous obtenez ainsi un portable qui tient plus longtemps en déplacement et qui donne tout branché sur secteur.

Pour les joueurs, la combinaison la plus efficace consiste à associer un undervolt CPU via ThrottleStop à un undervolt GPU. Sur les consoles portables et PC compacts, cette approche double le gain thermique : c’est d’ailleurs l’un des atouts des puces récentes comme le Ryzen Z2 Extreme, pensées pour l’efficacité énergétique. Enfin, gardez un œil sur la tarification des consoles portables gaming : sur ces appareils au refroidissement contraint, un undervolt bien réglé fait souvent la différence entre 45 et 60 FPS.

## Conclusion : un portable plus frais, plus rapide et plus silencieux

L’undervolting avec ThrottleStop est l’une des rares optimisations qui ne demande aucun compromis : vous baissez la température, vous gagnez en performance soutenue et vous réduisez le bruit, le tout gratuitement et de façon totalement réversible. La méthode se résume à quatre réflexes : partir prudemment à -80 mV sur les rails Core et Cache, descendre par paliers de -10 mV, valider chaque palier par un test de stabilité réel, puis sauvegarder un profil qui se relance au démarrage. Si votre machine verrouille l’undervolt à cause de la correction Plundervolt, les limites de puissance (TPL) et le BD PROCHOT restent des leviers efficaces.

Comptez une trentaine de minutes pour un premier réglage, un peu plus si vous cherchez le dernier millivolt stable. Le jeu en vaut la chandelle : sur un portable gaming qui touchait son Tjmax à 100 °C, il n’est pas rare de repasser sous les 85 °C tout en gagnant plusieurs centaines de mégahertz soutenus. Associez ce réglage CPU à un undervolt GPU et à une surveillance HWiNFO, et vous obtiendrez la machine la plus efficace possible sans ouvrir le châssis. Vos ventilateurs, vos FPS et vos oreilles vous remercieront.

### Couverture associée

## FAQ : ThrottleStop et l’undervolting

### ThrottleStop peut-il endommager mon processeur ?

Non, tant que vous ne touchez qu’à l’Offset Voltage négatif. Undervolter réduit la tension : le pire scénario est un plantage ou un écran bleu, sans dommage matériel. Un simple redémarrage annule tout réglage non sauvegardé. Le seul vrai risque serait d’augmenter la tension au-delà des valeurs d’usine, ce que ce tutoriel ne fait jamais.

### ThrottleStop fonctionne-t-il sur un processeur AMD Ryzen ?

Non. ThrottleStop est conçu exclusivement pour les processeurs Intel. Pour undervolter un CPU AMD Ryzen, utilisez AMD Ryzen Master et son Curve Optimizer, qui offrent une logique d’offset comparable. La partie GPU, en revanche, se règle avec MSI Afterburner quelle que soit la marque du processeur.

### Pourquoi les curseurs de tension sont-ils grisés ?

Parce que l’undervolt est verrouillé au niveau du microcode Intel. Depuis la correction de la faille Plundervolt (CVE-2019-11157), de nombreux BIOS figent l’interface de tension pour empêcher l’attaque. Sur ces machines, aucun logiciel ne peut contourner le verrou ; seule une version de BIOS déverrouillée peut rouvrir l’accès à l’undervolting.

### Quel offset d’undervolt choisir pour commencer ?

Commencez à -80 mV sur les rails CPU Core et CPU Cache simultanément. C’est une valeur que la grande majorité des puces encaissent. Descendez ensuite par paliers de -10 mV en testant la stabilité à chaque étape, puis remontez de 10 à 20 mV par rapport à votre plus bas offset stable pour garder une marge de sécurité.

### Combien de degrés puis-je réellement gagner ?

Cela dépend de votre puce et de votre refroidissement, mais un undervolt de -100 à -125 mV fait généralement chuter la température de 10 à 15 °C sous charge, parfois jusqu’à 20 °C sur les portables qui chauffaient le plus. Le gain le plus visible est souvent l’arrêt du bridage thermique, qui redonne des fréquences soutenues plus élevées.

### Faut-il lancer ThrottleStop à chaque démarrage ?

Oui, l’undervolt n’est actif que lorsque ThrottleStop tourne. Le mieux est de créer une tâche planifiée avec privilèges administrateur (option /RL HIGHEST) qui le lance à l’ouverture de session, sans invite UAC. Le profil sauvegardé s’applique alors automatiquement dès l’arrivée sur le bureau.

### Comment savoir si mon undervolt est stable ?

Trois vérifications : aucun redémarrage ni écran bleu, zéro erreur de calcul dans le TS Bench, et aucun événement WHEA (Id 18/19) dans l’Observateur d’événements de Windows après une charge prolongée. Validez toujours par une vraie session de jeu de 1 à 2 heures, plus révélatrice qu’un simple benchmark.

### ThrottleStop est-il vraiment gratuit ?

Oui, ThrottleStop est entièrement gratuit et distribué sans publicité par TechPowerUp. Il pèse 1,7 Mo dans sa version 9.7.3 du 2 avril 2025, ne nécessite aucune installation et fonctionne sous Windows 7 à 11 ; le paquet TechPowerUp.ThrottleStop est même référencé sur winstall pour une installation via WinGet, avec une fiche mise à jour le 11 juin 2026. C’est l’un des utilitaires d’optimisation matérielle les plus téléchargés au monde – la version espagnole d’Uptodown recensait déjà 208 466 téléchargements pour la seule archive 9.7 (1,71 Mo, SHA256 d83d…800d) au compteur de septembre 2026 –, développé et maintenu bénévolement par Kevin Glynn (UncleWebb).

*Avertissement : l’undervolting reste une opération à vos risques. Procédez par petits paliers, sauvegardez vos données importantes et testez soigneusement la stabilité avant d’utiliser votre portable en production.*