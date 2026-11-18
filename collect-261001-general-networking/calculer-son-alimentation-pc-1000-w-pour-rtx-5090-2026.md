Un PC gamer qui redémarre seul en pleine partie, une carte graphique RTX 5090 qui plafonne ses performances, ou pire, un connecteur qui fond dans le boîtier : la majorité de ces incidents remontent à une alimentation mal dimensionnée. Avec l’arrivée des GPU RTX 50 et de la norme ATX 3.1, calculer précisément le wattage nécessaire n’est plus un détail réservé aux passionnés d’overclocking, c’est devenu une étape obligatoire pour tout le monde. Ce tutoriel vous montre comment calculer et choisir son alimentation PC (PSU) en 2026, du calcul de watts jusqu’à l’installation physique, avec des scripts pour automatiser le calcul et vérifier votre consommation réelle.

## Pourquoi le calcul de l’alimentation PC est devenu crucial en 2026

Il y a encore quelques années, choisir une alimentation PC se résumait à prendre un modèle 650 W ou 750 W “au cas où” et à ne plus y penser. Cette approche approximative ne fonctionne plus. Trois évolutions récentes ont rendu le calcul précis indispensable. D’abord, la consommation des cartes graphiques haut de gamme a fortement augmenté : une RTX 5090 consomme à elle seule plus qu’un PC gamer complet d’il y a cinq ans. Ensuite, le connecteur d’alimentation GPU a changé de nature avec le 12V-2×6, qui laisse beaucoup moins de marge d’erreur qu’un ancien connecteur PCIe 8 broches classique. Enfin, la flambée des prix des composants, RAM en tête avec une hausse de l’ordre de 171 % rapportée en 2026, pousse de nombreux acheteurs à vouloir optimiser chaque euro dépensé, y compris sur un composant aussi discret que le bloc d’alimentation.

Le résultat, c’est qu’une alimentation mal dimensionnée n’est plus seulement une question de performance : c’est devenu un enjeu de sécurité matérielle direct. Un sous-dimensionnement provoque des redémarrages en pleine partie ou des écrans bleus aléatoires, tandis qu’un mauvais choix de câblage ou d’adaptateur sur le connecteur GPU peut, dans les cas les plus graves, provoquer une surchauffe localisée du connecteur. À l’inverse, prendre systématiquement le modèle le plus puissant du marché “pour être tranquille” gaspille de l’argent sans bénéfice réel, puisqu’une alimentation surdimensionnée fonctionne loin de son point de rendement optimal la plupart du temps. La méthode présentée dans ce guide vise un équilibre précis entre sécurité et budget, construit à partir des données constructeur officielles plutôt que d’estimations approximatives glanées sur des forums.

## Prérequis : matériel, outils et informations nécessaires

Avant de vous lancer dans le calcul de votre alimentation PC, réunissez les informations et le matériel suivants. Ce tutoriel ne nécessite aucune compétence avancée en électronique, mais quelques prérequis techniques facilitent grandement la démarche.

- **Liste complète de vos composants** : référence exacte du CPU, du GPU, du nombre de SSD/HDD, de ventilateurs et de RGB installés ou prévus.
- **Fiche technique du GPU** : le TGP (Total Graphics Power) ou TDP annoncé par le fabricant, disponible sur la page produit NVIDIA ou AMD.
- **Python 3.10 ou supérieur** installé si vous voulez utiliser le script de calcul fourni plus bas (facultatif mais recommandé).
- **Pilotes GPU à jour** : GeForce Game Ready Driver ou AMD Adrenalin récents pour que`nvidia-smi` ou les outils AMD renvoient des mesures fiables.
- **HWiNFO64 (version 8.x)** pour lire la consommation réelle carte mère/CPU/GPU en temps réel.
- **Tournevis cruciforme PH2** et un bracelet antistatique pour l’installation physique de l’alimentation.
- **Multimètre** (optionnel) si vous voulez vérifier les tensions 12V/5V/3.3V après installation.
- **Budget indicatif** : comptez entre 90 € et 250 € selon la puissance et la certification visées.

Comptez environ 60 à 90 minutes pour l’ensemble du processus : calcul du wattage, choix du modèle, commande, puis installation et test. Si vous changez uniquement l’alimentation sur un PC déjà monté, l’installation seule prend 20 à 30 minutes.

## Comprendre le TGP et la consommation réelle des GPU RTX 50 et RX 9000

Le point de départ de tout calcul d’alimentation PC gamer, c’est la consommation de la carte graphique. C’est de loin le composant le plus gourmand d’une configuration moderne, et les GPU de génération RTX 50 ont fait grimper les besoins de façon significative par rapport aux générations précédentes. Le TGP (Total Graphics Power) est le chiffre annoncé par le fabricant pour la consommation maximale soutenue de la carte, ventilateurs compris. C’est ce chiffre, et non un TDP théorique, qu’il faut utiliser pour votre calcul.

Le tableau ci-dessous résume le TGP officiel des GPU RTX 50 et de l’équivalent AMD RX 9070 XT, ainsi que la puissance d’alimentation recommandée pour chaque carte, telle que documentée par les fabricants et les guides d’assemblage 2026.

| Carte graphique | TGP / TDP officiel | Connecteur | PSU minimum | PSU recommandé | 
|---|---|---|---|---|
| RTX 5090 | 575 W | 12V-2×6 natif | 1000 W | 1200 W | 
| RTX 5080 | 360 W | 12V-2×6 natif | 750-800 W | 850 W | 
| RTX 5070 Ti | 300 W | 12V-2×6 ou 8-pin | 650 W | 750 W | 
| RTX 5070 | 250 W | 8-pin ou 12V-2×6 | 550 W | 650 W | 
| RX 9070 XT | 304 W | 8-pin x2 | 650 W | 750 W | 

Sur une RTX 5090, le TGP de 575 W représente environ 96 % de la capacité nominale du connecteur 12V-2×6, qui est évalué à 600 W. La marge est donc très faible, ce qui explique pourquoi un mauvais contact ou un câble mal inséré peut provoquer une surchauffe localisée. Sur les cartes moins puissantes comme la RTX 5070 ou la RX 9070 XT, la marge de sécurité électrique reste nettement plus confortable.

## Le connecteur 12V-2×6 et la norme ATX 3.1 : ce qui change

Depuis l’arrivée des RTX 50, la norme ATX 3.1 (associée à PCIe 5.1) s’est imposée comme le standard pour toute alimentation PC gamer haut de gamme. Elle intègre nativement un connecteur 16 broches appelé 12V-2×6, successeur direct du 12VHPWR utilisé sur les RTX 40. Ce connecteur, formalisé par un ECN du PCI-SIG, conserve la même géométrie mais améliore les tolérances de contact pour limiter les défauts d’insertion.

Le sujet est sensible car des cas de connecteurs fondus ont continué à être rapportés en 2025 et 2026 sur des RTX 5090, avec des points chauds pouvant atteindre environ 150 °C en cas de mauvais contact. Dans la grande majorité des cas documentés, le problème provient de trois causes précises : l’utilisation d’anciens câbles 12VHPWR avec un adaptateur vers 12V-2×6, l’usage d’adaptateurs coudés tiers non certifiés, ou une insertion incomplète du connecteur qui n’a pas été enfoncé jusqu’au clic. En réponse, plusieurs fabricants de cartes mères et de PSU (MSI, ASRock, entre autres) ont ajouté des embouts de couleur pour vérifier visuellement l’insertion complète, ainsi que des capteurs de température ou de courant sur certains câbles haut de gamme.

- Utilisez exclusivement un câble natif 12V-2×6 fourni avec votre alimentation ATX 3.1, jamais un adaptateur multiple.
- Évitez les coudes trop serrés à moins de 3,5 cm du connecteur, cela stresse mécaniquement les broches.
- Vérifiez que le connecteur est enfoncé jusqu’au clic audible des deux côtés.
- Si votre PSU n’a pas de câble 12V-2×6 natif et que vous devez utiliser un adaptateur, choisissez uniquement celui fourni par NVIDIA ou le fabricant de votre carte graphique.

## Lire une fiche technique d’alimentation : rails, connecteurs et courbe d’efficacité

Avant d’entrer dans le calcul étape par étape, il est utile de savoir décoder une fiche technique de PSU, car les fabricants n’affichent pas toujours l’information la plus importante en premier. Trois éléments méritent une attention particulière.

Le premier est la **répartition de puissance par rail**, indiquée dans un petit tableau appelé “étiquette de puissance” (power label), généralement imprimé sur le côté du boîtier du PSU ou disponible dans le manuel PDF téléchargeable. Sur un modèle single rail, vous verrez une seule ligne “+12V” avec un ampérage maximal élevé (par exemple 83A sur un modèle 1000 W). Sur un modèle multi-rail, vous verrez plusieurs lignes +12V1, +12V2, etc., chacune avec son propre ampérage limité : c’est cette information qui vous permettra, le cas échéant, de savoir sur quel rail brancher votre GPU sans risquer un déclenchement prématuré de la protection.

Le deuxième élément est le **nombre et le type de connecteurs PCIe disponibles**. Un PSU ATX 3.1 destiné aux RTX 50 doit proposer au moins un connecteur 12V-2×6 natif (parfois listé “PCIe 5.0” ou “600W 12V-2×6” dans la fiche produit). Méfiez-vous des modèles qui annoncent la compatibilité RTX 50 uniquement via un adaptateur fourni dans la boîte : ce n’est pas équivalent à un câble natif en matière de fiabilité thermique, comme expliqué plus haut.

Le troisième élément est la **courbe de rendement en fonction de la charge**, parfois publiée par les fabricants sérieux (Seasonic, Corsair, be quiet!) sous forme de graphique. Un bon PSU atteint son rendement maximal entre 40 % et 60 % de sa charge nominale. C’est une raison supplémentaire de ne pas systématiquement acheter le modèle le plus puissant disponible : une alimentation 1600 W utilisée en permanence à 300 W (moins de 20 % de charge) fonctionne dans une zone de rendement dégradée, alors qu’une alimentation 850 W bien dimensionnée pour le même usage tournerait autour de 35-40 % de charge, une zone beaucoup plus efficace.

## Alimentation SFX vs ATX : quel format pour votre boîtier

La méthode de calcul du wattage reste identique quel que soit le format physique de l’alimentation, mais le choix du format a une influence directe sur les modèles disponibles et sur le budget. Le format **ATX** (150 x 86 x 140 mm environ) reste la référence pour les boîtiers standards et moyens-tours : c’est le format qui offre le plus grand choix de puissances, de la 450 W à la 1600 W, avec le meilleur rapport prix/watt.

Le format **SFX** et sa variante allongée **SFX-L** (125 x 63,5 x 100 mm environ) ciblent les boîtiers compacts type mini-ITX. Leur taille réduite impose une densité de composants plus élevée, ce qui se traduit généralement par un ventilateur plus petit tournant plus vite, donc un niveau sonore plus élevé à puissance égale, et un prix supérieur de 20 à 40 % par rapport à un modèle ATX équivalent. Pour une configuration compacte avec une RTX 5070 ou 5070 Ti, un modèle SFX-L 750-850 W Gold reste tout à fait viable ; au-delà, pour une RTX 5090 en boîtier mini-ITX, vérifiez soigneusement l’espace disponible car peu de modèles SFX dépassent 1000 W sur le marché européen en 2026.

Un dernier point pratique souvent négligé : dans un boîtier compact, la longueur des câbles fournis avec un PSU SFX peut être un facteur limitant. Vérifiez la longueur du câble 12V-2×6 avant achat si votre GPU se trouve loin de l’emplacement de l’alimentation, en particulier sur les boîtiers avec compartiment PSU séparé.

## Étape 1 à 3 : lister et additionner la consommation de vos composants

Voici la première partie concrète de la méthode de calcul, en trois étapes.

**Étape 1 : notez le TGP exact de votre GPU.** Reportez-vous au tableau ci-dessus ou à la fiche technique officielle du fabricant. N’utilisez jamais un chiffre approximatif trouvé sur un forum : le TGP officiel est la seule donnée fiable pour un calcul de sécurité.

**Étape 2 : additionnez la consommation du CPU.** Un processeur haut de gamme comme un Ryzen 9 9950X3D ou un Core Ultra 9 consomme généralement entre 150 W et 250 W en pleine charge avec le turbo actif, selon le réglage PBO ou le profil de refroidissement. Un CPU milieu de gamme (Ryzen 7, Core i5/i7) se situe plutôt entre 65 W et 125 W.

**Étape 3 : ajoutez la plateforme.** Comptez environ 80 à 150 W pour l’ensemble carte mère, RAM, SSD NVMe, ventilateurs de boîtier et éclairage RGB. Une configuration avec plusieurs SSD, un système de watercooling AIO et un éclairage RGB complet se situe en haut de cette fourchette.

Exemple concret pour une configuration RTX 5080 : GPU 360 W + CPU 200 W + plateforme 120 W = **680 W** de consommation théorique cumulée sous charge maximale simultanée (un scénario rare mais qu’il faut couvrir).

## Étape 4 à 6 : appliquer la marge de sécurité et choisir la certification

**Étape 4 : ajoutez une marge de 30 à 40 % pour les pics transitoires.** Les GPU modernes, en particulier les RTX 50, produisent des pics de consommation instantanés (transients) qui dépassent largement le TGP moyen pendant quelques millisecondes. Des tests indépendants ont mesuré des pics système autour de 1044 W sur une configuration RTX 5090 dont le TGP carte seule est de 575 W. Sans cette marge, l’alimentation peut déclencher sa protection de surintensité (OCP) et couper brutalement le PC.

**Étape 5 : reprenez notre exemple RTX 5080.** 680 W de base x 1,35 (marge de 35 %) = 918 W. On arrondit à la puissance commerciale supérieure disponible, soit **850 W ou 1000 W** selon la marge de confort souhaitée. C’est cohérent avec la recommandation constructeur de 850 W pour une RTX 5080.

**Étape 6 : choisissez la certification 80 PLUS.** Pour un PC gamer 2026, visez au minimum le niveau Gold. Une alimentation mieux certifiée chauffe moins, dure plus longtemps et consomme moins d’électricité sur la facture annuelle, un argument qui compte davantage avec la hausse du prix de l’énergie en Europe.

## Comprendre les certifications 80 PLUS : tableau des rendements

La certification 80 PLUS garantit un rendement énergétique minimum mesuré à 20 %, 50 % et 100 % de charge. Plus le rendement est élevé, moins d’énergie est perdue sous forme de chaleur entre la prise secteur et vos composants. Voici les seuils officiels par palier.

| Certification | Rendement à 20 % | Rendement à 50 % | Rendement à 100 % | 
|---|---|---|---|
| 80 PLUS (standard) | 80 % | 80 % | 80 % | 
| 80 PLUS Bronze | 82 % | 85 % | 82 % | 
| 80 PLUS Silver | 85 % | 88 % | 85 % | 
| 80 PLUS Gold | 88 % | 90 % | 87 % | 
| 80 PLUS Platinum | 90 % | 92 % | 89 % | 
| 80 PLUS Titanium | 92 % (+90 % à 10 %) | 94 % | 90 % | 

Pour une configuration au-delà de 1000 W (typiquement une RTX 5090), privilégiez Platinum ou Titanium : les pertes de rendement à cette puissance représentent plusieurs dizaines de watts dissipés en chaleur, ce qui sollicite davantage la ventilation du boîtier.

L’écart de rendement entre Bronze et Platinum se traduit aussi directement sur la facture d’électricité. Pour un usage intensif (jeu 4 heures par jour à pleine charge GPU), la différence de pertes entre un modèle Bronze et un modèle Platinum sur une configuration RTX 5090 représente environ 15 à 25 kWh consommés en plus par an avec le modèle Bronze, soit quelques euros par an au tarif réglementé en France. L’écart reste modeste sur une seule année, mais un PSU Platinum ou Titanium dure généralement plus longtemps et chauffe moins l’intérieur du boîtier, ce qui réduit indirectement la charge de travail des autres ventilateurs.

## Étape 7 à 9 : single rail vs multi rail et choix du modèle

**Étape 7 : comprenez la différence entre single rail et multi rail.** Le rail 12V est le circuit qui alimente le GPU, le CPU et le stockage. Une alimentation single rail regroupe toute la puissance 12V sur un seul circuit protégé par une seule limite de courant (OCP), ce qui simplifie le câblage et convient bien aux GPU très gourmands comme la RTX 5090. Une alimentation multi rail répartit le 12V sur plusieurs circuits indépendants, chacun avec sa propre protection, ce qui coupe plus vite en cas de court-circuit localisé mais impose de bien répartir les connecteurs entre les rails.

**Étape 8 : pour un PC gamer avec une seule carte graphique** (le cas de la grande majorité des configurations en 2026), une alimentation single rail 12V est généralement recommandée. Elle évite les erreurs de répartition et garantit que le connecteur 12V-2×6 peut recevoir toute la puissance nécessaire sans être limité artificiellement.

**Étape 9 : sélectionnez un modèle full-modulaire, ATX 3.1, avec au moins 10 ans de garantie.** Le câblage full-modulaire permet de ne brancher que les câbles réellement utilisés, ce qui améliore l’aération du boîtier. Une garantie longue (Corsair, Seasonic et be quiet! proposent généralement 10 ans sur leurs gammes Gold et supérieures) est un bon indicateur de la qualité des composants internes.

## Étape 10 à 12 : installer, câbler et tester votre alimentation

**Étape 10 : installez physiquement le bloc.** Éteignez et débranchez le PC, patientez 60 secondes pour laisser les condensateurs se décharger. Glissez l’alimentation dans son emplacement (généralement en bas du boîtier), ventilateur orienté vers le bas si votre boîtier a une aération dédiée, ou vers le haut sinon. Fixez avec les 4 vis fournies.

**Étape 11 : câblez dans cet ordre** pour éviter les oublis : connecteur ATX 24 broches sur la carte mère, connecteur(s) EPS 4+4 ou 8 broches pour le CPU, connecteur 12V-2×6 pour le GPU (ou PCIe 8 broches selon votre carte), puis les câbles SATA/périphériques pour le stockage et les ventilateurs. Vérifiez chaque connecteur à la main : vous devez sentir un clic net.

**Étape 12 : premier démarrage et test de stabilité.** Rebranchez le secteur, allumez l’interrupteur à l’arrière du PSU, puis démarrez. Entrez dans le BIOS pour vérifier que les tensions 12V, 5V et 3,3V affichées sont dans une marge de ±5 %. Lancez ensuite un stress test GPU/CPU de 15 minutes pour confirmer qu’aucun redémarrage ni coupure ne se produit sous charge maximale simultanée.

## Automatiser le calcul avec un script Python

Plutôt que de refaire le calcul à la main à chaque changement de composant, voici un petit script Python qui automatise la méthode décrite plus haut, marge de sécurité comprise.

```
calculateur_watts.py
def calculer_psu(gpu_tgp, cpu_watts, plateforme_watts=120, marge=0.35):
    total_base = gpu_tgp + cpu_watts + plateforme_watts
    total_avec_marge = total_base * (1 + marge)
    return round(total_base), round(total_avec_marge)
# Exemple : RTX 5090 + Ryzen 9 9950X3D
base, recommande = calculer_psu(gpu_tgp=575, cpu_watts=230, plateforme_watts=140)
print(f"Consommation de base : {base} W")
print(f"PSU recommandé (marge 35%) : {recommande} W")
# Arrondi à la puissance commerciale la plus proche
puissances_commerciales = [550, 650, 750, 850, 1000, 1200, 1600]
choix = next(p for p in puissances_commerciales if p >= recommande)
print(f"Modèle à choisir : {choix} W minimum")
```
Voici le résultat obtenu en exécutant ce script pour une configuration RTX 5090 :

```
$ python3 calculateur_watts.py
Consommation de base : 945 W
PSU recommandé (marge 35%) : 1276 W
Modèle à choisir : 1600 W minimum
```
Ce résultat peut surprendre, mais il illustre bien pourquoi les guides constructeurs recommandent 1000 à 1200 W pour une RTX 5090 : la marge de 35 % appliquée à une base déjà élevée pousse rapidement vers le palier supérieur. Vous pouvez ajuster la variable `marge` à 0,25 si vous ne comptez pas overclocker, ou la laisser à 0,35-0,40 si vous voulez une marge confortable pour l’avenir.

Ce script reste volontairement simple, mais vous pouvez l’étendre facilement. Par exemple, ajoutez un paramètre `nb_ssd` qui multiplie le nombre de SSD NVMe par 6-8 W chacun, ou un paramètre `overclock` qui augmente la marge de 10 points supplémentaires si vous prévoyez de pousser le GPU au-delà de ses réglages d’usine. Si vous gérez plusieurs configurations (un PC personnel et un PC pour un proche, par exemple), transformez la fonction en une petite bibliothèque réutilisable avec un dictionnaire de profils GPU prédéfinis, en reprenant les valeurs du tableau TGP présenté plus haut dans cet article.

## Vérifier la consommation réelle avec nvidia-smi et HWiNFO

Le calcul théorique est une base de départ, mais rien ne remplace une mesure réelle une fois le PC monté. Sur une carte NVIDIA, la commande `nvidia-smi` permet d’interroger la consommation instantanée du GPU directement depuis un terminal.

`nvidia-smi --query-gpu=power.draw,power.limit,temperature.gpu --format=csv -l 1`
Cette commande affiche, chaque seconde, la puissance instantanée consommée par le GPU, sa limite de puissance configurée et sa température. Voici un exemple de sortie pendant une session de jeu sur une RTX 5090 :

```
power.draw [W], power.limit [W], temperature.gpu
542.18 W, 600.00 W, 68
571.90 W, 600.00 W, 70
563.44 W, 600.00 W, 71
575.02 W, 600.00 W, 71
```
Pour une vue globale du système (CPU, carte mère, disques), installez HWiNFO64 en mode capteurs seuls, activez la journalisation CSV via le menu *Settings > Logging*, puis lancez un benchmark de 10 minutes. Ouvrez ensuite le fichier CSV généré dans un tableur pour repérer le pic maximal sur la colonne “CPU Package Power” et “GPU Power” : additionnez ces deux valeurs de pic (pas les moyennes) pour valider que votre alimentation dispose d’une marge suffisante par rapport au pic mesuré réel, et non uniquement par rapport au TGP théorique.

## Projet complet : budget d’alimentation pour un PC RTX 5090

Pour rendre cette méthode concrète, voici un exemple de configuration complète haut de gamme avec le détail du calcul, du choix de l’alimentation jusqu’au budget final.

- **GPU** : RTX 5090, TGP 575 W
- **CPU** : Ryzen 9 9950X3D, environ 230 W en charge avec PBO
- **Plateforme** : carte mère X870E, 64 Go de DDR5, 2 SSD NVMe PCIe 5.0, watercooling AIO 360 mm, 6 ventilateurs, RGB : environ 140 W
- **Consommation de base cumulée** : 575 + 230 + 140 = 945 W
- **Marge de sécurité appliquée (35 %)** : 945 x 1,35 = 1276 W
- **Choix final** : alimentation 1200 W, 80 PLUS Platinum, ATX 3.1, connecteur 12V-2×6 natif, full-modulaire
- **Budget alimentation** : environ 220 à 280 € pour ce palier de puissance et de certification en France en 2026

Ce budget alimentation représente environ 6 à 8 % du coût total d’un PC gamer haut de gamme équipé d’une RTX 5090, une proportion qu’il ne faut surtout pas rogner : une alimentation sous-dimensionnée ou d’entrée de gamme sur ce type de configuration est la cause la plus fréquente d’instabilité et, dans les cas les plus graves, de dommages matériels.

Pour comparer avec un budget plus mesuré, voici le même calcul appliqué à une configuration milieu-haut de gamme, très représentative des PC gamer vendus en France en 2026.

- **GPU** : RTX 5070 Ti, TGP 300 W
- **CPU** : Ryzen 7 9800X3D, environ 130 W en charge
- **Plateforme** : carte mère B650, 32 Go de DDR5, 1 SSD NVMe PCIe 4.0, ventirad air, 4 ventilateurs : environ 90 W
- **Consommation de base cumulée** : 300 + 130 + 90 = 520 W
- **Marge de sécurité appliquée (30 %)** : 520 x 1,30 = 676 W
- **Choix final** : alimentation 750 W, 80 PLUS Gold, ATX 3.0 ou 3.1, semi-modulaire ou full-modulaire
- **Budget alimentation** : environ 90 à 120 € pour ce palier de puissance en France en 2026

Cet exemple illustre un point important : pour une configuration mid-range, la marge de sécurité de 30 % suffit généralement, contre 35-40 % recommandé sur les configurations RTX 5090 où les pics transitoires sont proportionnellement plus élevés. Adaptez toujours la marge à la classe de GPU utilisée plutôt que d’appliquer un pourcentage fixe à toutes les configurations.

## Tableau comparatif des prix d’alimentations en France 2026

Voici une fourchette de prix réaliste pour des modèles populaires 850 W, certifiés Gold, compatibles ATX 3.x, disponibles chez les revendeurs français en 2026.

| Modèle | Puissance | Certification | Norme | Prix indicatif | 
|---|---|---|---|---|
| Corsair RM850e | 850 W | 80 PLUS Gold | ATX 3.x | 120 – 150 € | 
| be quiet! Pure Power 12 M 850 W | 850 W | 80 PLUS Gold | ATX 3.0 | 130 – 160 € | 
| Seasonic Focus GX-850 | 850 W | 80 PLUS Gold | ATX 3.x | 120 – 180 € | 
| MSI MPG A850G | 850 W | 80 PLUS Gold | ATX 3.x / PCIe 5 | 130 – 190 € | 

Ces prix sont des ordres de grandeur observés sur les comparateurs français début 2026 : vérifiez toujours le tarif du jour chez votre revendeur habituel avant achat, les promotions ponctuelles peuvent faire varier ces montants de 20 à 30 €.

## 5 erreurs courantes à éviter lors du choix de son alimentation

Voici les pièges les plus fréquents constatés chez les monteurs de PC, débutants comme confirmés.

1. **Se baser sur le TDP du CPU seul et oublier le GPU.** Le GPU représente souvent 50 à 60 % de la consommation totale sous charge, c’est le poste à calculer en priorité.
2. **Ignorer les pics transitoires.** Un calcul sans marge de sécurité (30-40 %) sous-estime systématiquement le besoin réel, en particulier avec les GPU RTX 50.
3. **Utiliser un adaptateur multi-connecteurs 4×8 broches vers 12V-2×6.** Cette pratique concentre plusieurs câbles anciens sur un connecteur récent non conçu pour ce type d’adaptation, l’une des causes principales de connecteurs fondus documentées en 2025-2026.
4. **Choisir une alimentation non modulaire pour économiser 20 €.** Les câbles fixes en surnombre bloquent la circulation d’air, augmentent les températures internes de 3 à 5 °C et compliquent le câblage.
5. **Réutiliser une vieille alimentation de plus de 6-7 ans sur un nouveau GPU haut de gamme.** Les condensateurs vieillissent et la puissance réellement délivrable chute sous charge, même si l’étiquette affiche toujours le wattage d’origine.

## Dépannage : 8 problèmes fréquents et solutions

Si votre PC présente un comportement instable après un changement de composant ou d’alimentation, voici les causes les plus courantes et comment les résoudre. Dans la majorité des cas, un diagnostic méthodique en moins de 15 minutes permet d’identifier la cause exacte sans avoir à démonter l’intégralité de la configuration.

- **Le PC redémarre seul en jeu, jamais au bureau** : quasi toujours un sous-dimensionnement de l’alimentation par rapport aux pics du GPU. Recalculez avec une marge de 40 % et vérifiez les watts réels via`nvidia-smi` .
- **Odeur de plastique brûlé près du GPU** : éteignez immédiatement le PC, débranchez, et inspectez le connecteur 12V-2×6 pour des traces de fonte ou de noircissement. Ne rallumez pas avant d’avoir remplacé le câble.
- **Le PC ne démarre pas du tout, aucun ventilateur ne tourne** : vérifiez l’interrupteur à l’arrière du PSU, testez une autre prise secteur, puis testez le connecteur ATX 24 broches avec la méthode du trombone (PSU débranché du reste du PC).
- **Écran noir avec bips au démarrage** : vérifiez que le connecteur EPS 4+4 broches du CPU est bien enfoncé jusqu’au clic des deux côtés.
- **Le GPU plafonne ses performances en dessous des scores attendus** : ouvrez le BIOS, vérifiez la tension du rail 12V (doit rester au-dessus de 11,4 V sous charge). Une tension qui chute sous 11,2 V indique une alimentation insuffisante.
- **Coupure brutale uniquement pendant un benchmark ou un rendu 3D lourd** : c’est le signe classique d’un déclenchement de la protection OCP. Passez au palier de puissance supérieur.
- **Bruit de “coil whine” (sifflement aigu) sous forte charge** : généralement inoffensif, lié à la qualité des bobines internes. Un modèle Platinum/Titanium de meilleure qualité réduit ce phénomène par rapport à une entrée de gamme Bronze.
- **Ventilateur du PSU qui tourne à plein régime en permanence** : vérifiez que l’alimentation n’est pas positionnée trop près d’une source de chaleur (GPU, watercooling) et que les grilles d’aération du boîtier ne sont pas obstruées par la poussière.

## Astuces avancées pour les configurations haut de gamme

Si vous montez une configuration extrême (RTX 5090, overclocking CPU/GPU, multi-stockage), quelques ajustements supplémentaires permettent de sécuriser davantage votre installation et d’en tirer le meilleur parti sur la durée, au-delà du simple calcul initial du wattage.

- **Limitez le power limit du GPU à 90-95 %** via l’application constructeur si vous êtes proche du palier de puissance supérieur : la perte de performance est généralement inférieure à 3 %, pour une baisse de consommation de pointe non négligeable.
- **Privilégiez les câbles natifs plutôt que les câbles sleevés tiers** pour le connecteur 12V-2×6, sauf si le fabricant du câble alternatif garantit explicitement la compatibilité 600 W et la certification ATX 3.1.
- **Sur une configuration multi-GPU ou avec carte d’acquisition/eGPU** , envisagez une alimentation multi-rail pour isoler les circuits et limiter le risque en cas de défaut sur un composant.
- **Surveillez le vieillissement** : au-delà de 5 ans d’utilisation intensive, la capacité réelle d’une alimentation baisse de 5 à 10 %. Recalculez votre marge de sécurité en conséquence avant d’ajouter un GPU plus gourmand.
- **Pensez à l’onduleur (UPS)** pour les configurations dépassant 1000 W : une micro-coupure secteur sur ce type de système peut endommager des composants coûteux, un onduleur avec régulation de tension ligne interactive protège l’ensemble.

## FAQ : questions fréquentes sur le calcul de l’alimentation PC

**Quelle alimentation pour une RTX 5090 ?**

Un minimum de 1000 W est recommandé par NVIDIA, avec 1200 W conseillé pour une configuration confortable incluant un CPU haut de gamme et de la marge pour l’overclocking.

**Faut-il toujours prendre une alimentation plus puissante que le calcul théorique ?**

Oui, dans une limite raisonnable. La marge de 30-40 % couvre les pics transitoires réels, mais inutile de viser 1600 W pour une configuration RTX 5070 : cela augmente le coût sans bénéfice, une alimentation trop peu chargée n’est pas plus efficace.

**Le connecteur 12V-2×6 est-il compatible avec les anciens câbles 12VHPWR ?**

Mécaniquement oui via un adaptateur, mais ce n’est pas recommandé pour les GPU les plus gourmands (RTX 5090). Privilégiez un câble natif 12V-2×6 fourni avec une alimentation ATX 3.1.

**80 PLUS Gold suffit-il pour un PC gamer en 2026 ?**

Pour la majorité des configurations jusqu’à 850 W, oui. Au-delà de 1000 W, Platinum ou Titanium limitent mieux la chaleur dissipée et la facture d’électricité sur le long terme.

**Single rail ou multi rail pour un seul GPU ?**

Single rail est généralement recommandé pour une configuration mono-GPU, car cela évite les erreurs de répartition de charge entre plusieurs circuits.

**Combien de temps dure une alimentation PC ?**

Entre 7 et 10 ans en usage normal pour un modèle Gold ou supérieur avec une bonne garantie constructeur, moins si elle est utilisée en permanence proche de sa charge maximale.

**Peut-on réutiliser son alimentation actuelle en changeant de GPU ?**

Cela dépend du wattage disponible et de l’âge du bloc. Recalculez systématiquement avec le TGP du nouveau GPU avant de décider, en particulier si votre alimentation a plus de 5 ans.

**Pourquoi mon alimentation 750 W ne suffit-elle pas pour une RTX 5080 avec un gros CPU ?**

Le TGP de 360 W plus un CPU haut de gamme à 230 W et la plateforme dépassent souvent 700 W de base, et la marge de sécurité de 30-40 % pousse le besoin réel au-delà de 750 W. Le passage à 850-1000 W est alors nécessaire.

Pour aller plus loin sur les cartes graphiques RTX 50 et leurs spécifications complètes, consultez la page officielle NVIDIA RTX 5090. Les seuils de rendement 80 PLUS sont détaillés sur le site de l’organisme de certification 80 PLUS. Pour l’historique de la norme ATX, la fiche ATX sur Wikipédia reste une référence utile. Tom’s Hardware publie régulièrement des tests d’alimentations PC, tandis qu’AMD détaille les spécifications de ses cartes RX 9000 sur son site officiel. Le comparateur français Les Numériques publie aussi des comparatifs de composants PC actualisés.