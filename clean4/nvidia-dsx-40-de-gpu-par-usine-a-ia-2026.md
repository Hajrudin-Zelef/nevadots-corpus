Le 31 mai 2026, lors de la conférence GTC Taipei, NVIDIA a dévoilé **DSX**, une plateforme présentée comme le « mode d’emploi » complet pour construire des **usines à IA** (AI factories). L’annonce intervient quelques jours seulement après des résultats trimestriels records – 81,62 milliards de dollars de chiffre d’affaires au premier trimestre de l’exercice 2027 – et confirme un basculement industriel : le centre de données n’est plus un simple hébergeur de serveurs, mais une chaîne de production de « tokens » d’intelligence artificielle dont l’unité de mesure est désormais le mégawatt, voire le gigawatt.

Pour la France et l’Europe, le sujet est loin d’être anecdotique. Deux des partenaires industriels phares de DSX – le français **Schneider Electric** et l’éditeur **Dassault Systèmes** – figurent au cœur de l’écosystème. Au même moment, des projets d’infrastructure géants se dessinent sur le continent, du hub de Dunkerque aux ambitions de calcul souverain. Cette analyse décortique ce qu’est réellement NVIDIA DSX, ce que la plateforme change pour le marché, et pourquoi la bataille des usines à IA va structurer la décennie technologique européenne.

## NVIDIA DSX : qu’est-ce qu’une « usine à IA » exactement ?

NVIDIA DSX est une plateforme de conception, de simulation, d’exploitation et d’optimisation d’usines à IA couvrant l’intégralité de la pile technologique : des puces et des systèmes jusqu’aux logiciels d’infrastructure, en passant par les installations physiques et les technologies des partenaires. Selon le constructeur, l’objectif est simple à énoncer mais redoutablement difficile à atteindre : produire de l’intelligence artificielle au **coût du token le plus bas possible**.

La notion d’« usine à IA » mérite une définition précise. Contrairement à un centre de données classique, qui loue de la capacité de stockage et de calcul à des usages variés, une usine à IA est une installation conçue d’un seul tenant pour entraîner et faire tourner des modèles d’intelligence artificielle. Sa production se mesure en tokens générés par seconde, sa contrainte première est l’énergie disponible, et son rendement dépend autant de la climatisation et de la topologie réseau que de la puissance brute des processeurs graphiques. C’est précisément cette complexité que DSX entend industrialiser.

« NVIDIA est la seule entreprise qui construit l’usine à IA complète », a déclaré Jensen Huang, fondateur et directeur général de NVIDIA, en présentant la plateforme. La formule résume l’ambition stratégique : ne plus seulement vendre des cartes graphiques, mais fournir une référence d’ingénierie de bout en bout que les opérateurs – hyperscalers, États, industriels – peuvent dupliquer pour bâtir des installations de plusieurs centaines de mégawatts en limitant les erreurs de conception coûteuses.

## Les six briques de la plateforme DSX

DSX n’est pas un produit unique mais un ensemble modulaire de composants logiciels et de références matérielles. L’annonce du 31 mai 2026 détaille six briques principales, chacune répondant à une étape du cycle de vie d’une usine à IA, de la planche à dessin à l’exploitation quotidienne.

| Composant DSX | Rôle | Bénéfice clé revendiqué | 
|---|---|---|
| DSX Reference Design | Plan de référence de l’usine à IA complète | Réduit les erreurs de conception et le temps de déploiement | 
| DSX Sim | Jumeau numérique pour simuler énergie, refroidissement et réseau | Test virtuel avant construction physique | 
| DSX MaxLPS | Optimisation du débit de tokens par mégawatt | Jusqu’à 40 % de GPU en plus à budget énergétique constant | 
| DSX Flex | Gestion flexible de la charge et de l’alimentation | Adaptation aux contraintes du réseau électrique local | 
| DSX Exchange | Place de marché de composants et modules certifiés | Approvisionnement standardisé auprès des partenaires | 
| DSX OS | Système d’exploitation open source de l’usine à IA | Ordonnancement, résilience et multi-tenant unifiés | 

La brique la plus commentée est **DSX OS**, un système d’exploitation modulaire et open source qui prend en charge la gestion du cycle de vie, l’ordonnancement, la cohérence d’exécution, l’automatisation de la supervision, la résilience et l’exploitation multi-locataires. En ouvrant ce socle logiciel, NVIDIA cherche à reproduire la stratégie qui a fait le succès de CUDA : verrouiller l’écosystème non pas par la fermeture, mais par l’ubiquité d’une couche logicielle que tout le monde finit par adopter.

## De 100 mégawatts à plusieurs gigawatts : la nouvelle échelle

Le changement d’échelle est vertigineux. Selon NVIDIA, DSX est conçu pour soutenir des usines à IA allant de **100 mégawatts à plusieurs gigawatts**. À titre de comparaison, un centre de données traditionnel de grande taille consomme rarement plus de quelques dizaines de mégawatts. Une usine à IA d’un gigawatt équivaut donc à la consommation électrique d’une ville moyenne, et nécessite une planification énergétique digne d’un projet industriel lourd.

Cette inflation des besoins explique pourquoi NVIDIA a intégré dès la conception des partenaires de l’énergie comme GE Vernova, Hitachi, Siemens Energy et la jeune pousse Emerald AI. L’enjeu n’est plus seulement d’acheter des GPU, mais de sécuriser des dizaines de mégawatts d’électricité, de concevoir des systèmes de refroidissement liquide à très haute densité, et de raccorder l’ensemble au réseau sans le déstabiliser. Le jumeau numérique DSX Sim permet précisément de modéliser ces contraintes avant le premier coup de pioche.

Pour l’Europe, où le foncier industriel est rare et où l’acceptabilité sociale des centres de données énergivores fait débat, cette approche par la simulation pourrait s’avérer décisive. Un opérateur capable de prouver, chiffres à l’appui, la performance énergétique de son installation avant construction dispose d’un argument de poids face aux autorités locales et aux régulateurs.

## DSX MaxLPS : 40 % de GPU en plus à budget énergétique constant

Le chiffre qui a retenu l’attention des analystes est celui de **DSX MaxLPS**. NVIDIA affirme que cette technologie permet aux opérateurs de faire fonctionner jusqu’à **40 % de GPU supplémentaires** à leur point de fonctionnement le plus efficace sur le plan énergétique, avec un impact minimal sur la performance des charges de travail, à budget de puissance fixe.

Cette promesse touche au nerf de la guerre. Dans un monde où l’électricité est devenue la ressource limitante de l’industrie de l’IA, gagner 40 % de capacité de calcul sans consommer un watt de plus revient à reculer d’autant la contrainte physique qui plafonne la croissance. En d’autres termes, MaxLPS transforme une limite énergétique en levier économique : pour un même raccordement, l’opérateur produit davantage de tokens, donc davantage de revenus.

### Pourquoi le « token par mégawatt » devient l’indicateur roi

L’industrie a longtemps raisonné en FLOPS (opérations en virgule flottante par seconde) ou en téraoctets de mémoire. DSX impose un nouveau référentiel : le débit de tokens par mégawatt. Cet indicateur réconcilie la performance de calcul et la réalité physique de l’alimentation électrique. Il explique pourquoi les contrats d’approvisionnement en énergie, l’efficacité du refroidissement et la densité par baie comptent désormais autant que la génération de la puce. C’est un changement de paradigme comptable autant que technique.

## Vera Rubin : le successeur de Blackwell attendu au second semestre 2026

DSX s’articule autour de la prochaine génération de matériel NVIDIA, baptisée **Vera Rubin**. Dès le 16 mars 2026, lors de la GTC de San José, le constructeur avait publié le *Vera Rubin DSX AI Factory reference design*, un plan de référence couvrant l’ensemble de la pile d’infrastructure : calcul, réseau Ethernet Spectrum-X et stockage. À cette occasion, NVIDIA a également annoncé la disponibilité générale d’Omniverse DSX, compatible avec cette nouvelle architecture.

Vera Rubin succède à la génération Blackwell, qui a porté la croissance de NVIDIA au cours des derniers trimestres. D’après les prévisions de marché relayées par S&P Global, la production de Vera Rubin doit démarrer au troisième trimestre 2026, pour une disponibilité au second semestre, et le consensus financier table sur une contribution de l’ordre de 38 milliards de dollars de revenus liés à Rubin sur l’exercice 2027. La cadence – une nouvelle architecture majeure par an – illustre l’accélération imposée par NVIDIA à l’ensemble de la filière.

« Avec le plan de référence Vera Rubin DSX AI Factory et le blueprint Omniverse DSX, nous fournissons les fondations pour bâtir les usines à IA les plus productives au monde, accélérer le délai jusqu’au premier revenu et maximiser l’échelle et l’efficacité énergétique », a résumé Jensen Huang lors de l’annonce de mars 2026. La notion de « délai jusqu’au premier revenu » (*time to first revenue*) trahit l’objectif : raccourcir le temps qui sépare la décision d’investir de la première facturation de capacité de calcul.

## Des résultats financiers records : 81,6 milliards de dollars au premier trimestre

L’annonce de DSX s’inscrit dans une dynamique financière exceptionnelle. Le 20 mai 2026, NVIDIA a publié les résultats de son premier trimestre de l’exercice 2027 : un chiffre d’affaires total de **81,62 milliards de dollars**, en hausse de 85 % sur un an, dont **75,2 milliards** pour le seul segment data center, en progression de 92 %. Le centre de données représente ainsi plus de 90 % de l’activité du groupe – une dépendance qui fait la force et la fragilité du modèle.

| Trimestre | CA total | CA Data Center | Croissance annuelle (data center) | 
|---|---|---|---|
| T1 exercice 2026 (mai 2025) | 44,1 Mds$ | 39,1 Mds$ | +73 % | 
| T1 exercice 2027 (mai 2026) | 81,62 Mds$ | 75,2 Mds$ | +92 % | 
| Variation sur un an | +85 % | +92 % | – | 

En un an, le chiffre d’affaires data center de NVIDIA a quasiment doublé, passant de 39,1 à 75,2 milliards de dollars. Cette trajectoire confirme que la demande d’infrastructure d’IA n’a pas atteint son plateau, contrairement aux craintes exprimées par certains observateurs début 2025. DSX apparaît dès lors comme l’outil destiné à transformer cette demande en commandes répétables, en standardisant la construction des installations qui consommeront les GPU des prochaines générations.

## Un écosystème de partenaires où la France joue sa carte

La force de DSX réside dans son réseau de partenaires industriels. NVIDIA a cité parmi les contributeurs de la conception de référence : Cadence, Dassault Systèmes, Eaton, Jacobs, NScale, Phaidra, Procore Technologies, PTC, Schneider Electric, Siemens, Switch, Trane Technologies et Vertiv. Côté énergie, figurent Emerald AI, GE Vernova, Hitachi et Siemens Energy. Pour la livraison de modules préfabriqués, NVIDIA s’appuie notamment sur Bechtel et Vertiv.

Deux noms français se distinguent. **Schneider Electric**, leader mondial de la gestion de l’énergie et de l’automatisation, est en première ligne sur les volets alimentation, distribution électrique et refroidissement des usines à IA – un positionnement qui fait du groupe l’un des grands bénéficiaires industriels de la vague des centres de données. **Dassault Systèmes**, avec sa plateforme de jumeaux numériques, s’inscrit naturellement dans la logique de simulation portée par DSX Sim et Omniverse.

Cette présence française dans l’écosystème NVIDIA illustre une réalité souvent occultée du débat sur la souveraineté technologique : si les puces et les modèles dominants restent américains, l’infrastructure physique – énergie, automatisation, ingénierie, jumeaux numériques – mobilise des champions européens de premier plan. La valeur ne se loge pas uniquement dans le silicium.

## Impact sur le marché : la course mondiale aux usines à IA

En industrialisant la construction des usines à IA, NVIDIA accélère un mouvement déjà engagé. Les hyperscalers américains – Amazon, Microsoft et Google – investissent des sommes colossales dans leurs propres infrastructures, et contrôlaient en 2025 environ 70 % du marché du cloud européen, selon Synergy Research Group. À l’inverse, les fournisseurs européens stagnent autour de 15 % de parts de marché sur leur propre continent, un chiffre stable depuis plusieurs années alors même que le marché explose.

DSX risque d’accentuer cette dynamique à double tranchant. D’un côté, la plateforme abaisse la barrière à l’entrée : un acteur de taille moyenne, un État ou un consortium industriel peut désormais s’appuyer sur un plan de référence éprouvé pour bâtir son usine à IA, sans réinventer chaque détail d’ingénierie. De l’autre, cette standardisation renforce la dépendance à l’écosystème NVIDIA – matériel, logiciel et désormais méthode de construction – au moment précis où l’Europe cherche à réduire sa dépendance technologique.

Pour les opérateurs de cloud souverain comme les français OVHcloud et Scaleway, le pari est subtil : adopter DSX pour gagner en rapidité de déploiement et en efficacité énergétique, tout en préservant une marge d’autonomie sur la couche d’orchestration et les données. L’ouverture de DSX OS en open source offre ici une porte de sortie partielle, en évitant un verrouillage logiciel total.

## L’Europe et les usines à IA : Dunkerque, Mistral et la souveraineté

Le continent n’est pas spectateur. Le 1er juin 2026, le groupe japonais SoftBank a annoncé un engagement pouvant atteindre **75 milliards d’euros** pour un projet d’infrastructure d’IA de **5 gigawatts** en France, avec un hub majeur à Dunkerque et une première phase de 3,1 gigawatts. Ce projet, parmi les plus ambitieux jamais annoncés sur le sol européen, illustre l’échelle exacte que DSX prétend industrialiser : non plus le centre de données, mais l’usine à IA de plusieurs gigawatts.

En parallèle, la France pousse ses propres champions de l’IA, à commencer par Mistral AI, qui multiplie les partenariats industriels avec Airbus et BMW, et développe ses propres capacités de calcul. La question n’est plus seulement de disposer de modèles européens performants, mais de maîtriser l’infrastructure physique qui les fait tourner – précisément le terrain sur lequel DSX redéfinit les règles.

Ce mouvement s’inscrit dans le cadre plus large de la stratégie européenne de souveraineté numérique face à la domination du cloud américain. Bruxelles a présenté le 3 juin 2026 un paquet législatif comprenant un Cloud and AI Development Act destiné à fluidifier le déploiement de centres de données et à instaurer un cadre d’évaluation de la souveraineté du cloud et de l’IA. Les usines à IA sont au cœur de cette équation politique.

## Comparaison concurrentielle : NVIDIA face à AMD et aux puces maison

Si NVIDIA domine outrageusement le marché, la concurrence s’organise. AMD pousse sa gamme Instinct pour grignoter des parts sur l’inférence, tandis que les hyperscalers développent leurs propres accélérateurs maison – Trainium et Inferentia chez Amazon, TPU chez Google, Maia chez Microsoft – afin de réduire leur dépendance et leurs coûts. La stratégie de NVIDIA avec DSX consiste précisément à élargir son avantage au-delà de la puce : en fournissant la méthode de construction de l’usine entière, le constructeur rend son écosystème plus difficile à contourner.

| Plateforme / acteur | Positionnement | Statut 2026 | 
|---|---|---|
| NVIDIA Blackwell | Génération actuelle, entraînement et inférence | En production, moteur des ventes data center | 
| NVIDIA Vera Rubin | Prochaine génération, réseau Spectrum-X | Production prévue au T3 2026 | 
| AMD Instinct | Alternative axée inférence et rapport prix/performance | Montée en puissance | 
| Accélérateurs maison (Trainium, TPU, Maia) | Réduction de la dépendance des hyperscalers | Déploiement interne croissant | 
| NVIDIA DSX | Plateforme de construction d’usines à IA complètes | Annoncée le 31 mai 2026 | 

L’avantage concurrentiel de DSX ne se situe donc pas au niveau d’une puce comparée à une autre, mais à l’échelon supérieur : celui du système. Tant que les opérateurs raisonneront en termes d’usine complète plutôt que de composant isolé, NVIDIA conservera une longueur d’avance structurelle, car aucun concurrent ne propose aujourd’hui une chaîne aussi intégrée, du processeur graphique au jumeau numérique du bâtiment.

## Contexte historique : de la carte graphique à l’usine à IA

La trajectoire de NVIDIA résume trois décennies de mutation. Fondée en 1993 pour accélérer les jeux vidéo, l’entreprise a d’abord vendu des cartes graphiques grand public. L’invention de CUDA en 2006 a transformé ces puces en accélérateurs de calcul scientifique, ouvrant la voie à l’apprentissage profond à partir de 2012. La déferlante des grands modèles de langage, depuis 2022, a propulsé NVIDIA au rang de fournisseur incontournable de l’IA générative.

DSX marque l’étape suivante de cette ascension : le passage du composant à l’infrastructure complète. Là où NVIDIA vendait hier des GPU, puis des serveurs DGX, puis des systèmes en racks, l’entreprise propose désormais le plan d’ensemble d’un bâtiment de plusieurs centaines de mégawatts. Chaque montée en abstraction a élargi la valeur captée par le groupe – et chaque fois, les concurrents ont peiné à suivre sur le même périmètre.

Cette logique d’intégration verticale rappelle d’autres moments charnières de l’histoire industrielle, de la chaîne de montage automobile aux écosystèmes logiciels propriétaires. À chaque fois, l’acteur qui a su standardiser la production d’un bien complexe a redéfini les règles de toute une filière. NVIDIA tente aujourd’hui le même coup avec l’intelligence artificielle.

## Ce qu’en disent les experts

Pour Jensen Huang, le message est clair et répété à chaque prise de parole : « NVIDIA est la seule entreprise qui construit l’usine à IA complète. » Le dirigeant insiste sur la notion de « délai jusqu’au premier revenu », signe que l’argument de vente n’est plus la performance brute mais la rapidité de mise en production et la rentabilité de l’installation.

Du côté des analystes financiers, S&P Global souligne dans sa note de prévision que la transition de Blackwell vers Vera Rubin constitue le principal moteur de croissance attendu pour l’exercice 2027, avec une contribution estimée autour de 38 milliards de dollars pour la seule nouvelle génération. La maison de recherche pointe toutefois la concentration du chiffre d’affaires sur le segment data center comme un facteur de volatilité.

Les spécialistes du marché du cloud, comme Synergy Research Group, rappellent enfin le contexte structurel : avec environ 70 % du marché européen aux mains des trois hyperscalers américains et des fournisseurs locaux figés autour de 15 %, l’industrialisation de la construction d’usines à IA pourrait soit creuser l’écart, soit, si l’Europe s’en empare, offrir un levier de rattrapage. Tout dépendra de la capacité des acteurs du continent à transformer leurs atouts industriels – énergie, ingénierie, foncier – en infrastructures de calcul compétitives.

## Cinq prédictions pour les usines à IA en 2026-2027

- **L’énergie deviendra le premier critère de localisation.** Les futures usines à IA s’implanteront prioritairement là où l’électricité est abondante, décarbonée et bon marché – un atout majeur pour la France et son parc nucléaire.
- **Le « token par mégawatt » s’imposera comme métrique de référence.** Les opérateurs communiqueront de plus en plus sur leur efficacité énergétique plutôt que sur la seule puissance de calcul installée.
- **Vera Rubin alimentera une nouvelle vague de commandes.** Avec une production attendue au troisième trimestre 2026, la nouvelle architecture devrait soutenir la croissance du chiffre d’affaires data center de NVIDIA au-delà de l’exercice en cours.
- **Les champions européens de l’infrastructure capteront une part croissante de la valeur.** Schneider Electric, Siemens, Dassault Systèmes et les énergéticiens du continent profiteront de la course aux usines à IA, même si les puces restent américaines.
- **La standardisation accentuera le débat sur la souveraineté.** Plus DSX deviendra le standard de fait, plus la question de la dépendance européenne à l’écosystème NVIDIA pèsera dans les arbitrages politiques et industriels.

## Foire aux questions sur NVIDIA DSX et les usines à IA

### Qu’est-ce que NVIDIA DSX exactement ?

NVIDIA DSX est une plateforme annoncée le 31 mai 2026 qui fournit un « mode d’emploi » complet pour concevoir, simuler, construire et exploiter des usines à IA. Elle couvre toute la pile, des puces aux installations physiques, et vise à produire de l’intelligence artificielle au coût du token le plus bas possible.

### Quelle est la différence entre une usine à IA et un centre de données ?

Un centre de données classique loue de la capacité de calcul et de stockage pour des usages variés. Une usine à IA est une installation conçue spécifiquement pour entraîner et exécuter des modèles d’intelligence artificielle. Sa production se mesure en tokens par seconde, et sa contrainte première est l’énergie disponible, avec des besoins allant de 100 mégawatts à plusieurs gigawatts.

### Que promet DSX MaxLPS ?

Selon NVIDIA, DSX MaxLPS permet de faire fonctionner jusqu’à 40 % de GPU supplémentaires à leur point de fonctionnement le plus efficace, avec un impact minimal sur la performance des charges de travail, le tout à budget de puissance fixe. Cela revient à augmenter la capacité de calcul sans accroître la consommation électrique.

### Qu’est-ce que Vera Rubin et quand sera-t-elle disponible ?

Vera Rubin est la prochaine génération d’architecture NVIDIA, successeur de Blackwell. D’après les prévisions de marché relayées par S&P Global, sa production doit démarrer au troisième trimestre 2026 pour une disponibilité au second semestre, avec une contribution attendue d’environ 38 milliards de dollars de revenus sur l’exercice 2027.

### Quel est l’impact de DSX pour l’Europe ?

Deux partenaires français majeurs, Schneider Electric et Dassault Systèmes, figurent dans l’écosystème DSX. De plus, des projets géants comme le hub de Dunkerque financé par SoftBank (jusqu’à 75 milliards d’euros pour 5 gigawatts) montrent que l’Europe construit ses propres usines à IA. L’enjeu de souveraineté reste central, entre adoption d’un standard américain et préservation de l’autonomie sur les données et l’orchestration.

### NVIDIA est-il vraiment incontournable face à AMD et aux puces maison ?

AMD avec sa gamme Instinct et les hyperscalers avec leurs accélérateurs maison (Trainium, TPU, Maia) cherchent à réduire la dépendance à NVIDIA. Mais l’avantage de DSX se situe à l’échelon du système complet : tant que les opérateurs raisonnent en termes d’usine entière, NVIDIA conserve une longueur d’avance qu’aucun concurrent n’égale aujourd’hui sur ce périmètre intégré.

### Pourquoi NVIDIA ouvre-t-il DSX OS en open source ?

En ouvrant le système d’exploitation de l’usine à IA, NVIDIA cherche à favoriser l’adoption massive de sa couche logicielle, comme il l’a fait avec CUDA. L’ubiquité d’un standard open source renforce l’écosystème sans le verrouiller frontalement, tout en laissant aux opérateurs une marge d’autonomie sur l’orchestration.

### Related Coverage

*Sources et références : NVIDIA Newsroom – DSX, NVIDIA DSX Platform, Vera Rubin DSX reference design, Résultats financiers NVIDIA, Synergy Research Group – marché du cloud européen.*