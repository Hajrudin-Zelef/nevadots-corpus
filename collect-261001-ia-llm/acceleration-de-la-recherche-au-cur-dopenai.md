Pour que l’AGI bénéficie à toute l’humanité, sa gouvernance doit, selon nous, être démocratique. Cela exige un débat public éclairé sur les capacités, les risques et les garde-fous des systèmes d’IA très performants. Chacun doit comprendre l’évolution probable de l’IA de pointe pour pouvoir réellement influer sur son développement.

La transparence sur les risques, incidents et garde-fous précis est nécessaire, mais insuffisante. Le public doit aussi comprendre comment les systèmes les plus performants évoluent et font avancer la recherche dans les laboratoires de pointe.

Nous visons à créer de façon sûre un chercheur en IA automatisé qui, sous supervision humaine, fera progresser l’apprentissage profond et l’alignement par itérations. Selon nos mesures, nous avons atteint l’objectif annoncé(ouverture dans une nouvelle fenêtre) l’automne dernier : disposer d’un stagiaire de recherche automatisé d’ici septembre de cette année. Par « stagiaire de recherche », nous entendons un système capable d’effectuer, sous direction humaine, des tâches bien définies, même de plusieurs jours pour un chercheur qualifié. Nous progressons nettement vers la création d’un chercheur en IA automatisé d’ici mars 2028.

Cette année, le travail quotidien des chercheurs d’OpenAI a profondément changé. Ils utilisent des agents de programmation toute la journée, souvent en parallèle. L’utilisation totale croît rapidement, plus vite que dans les autres équipes d’OpenAI. Les chercheurs produisent du code plus vite et mènent davantage d’expériences. Leurs usages évoluent aussi : les agents traitent des tâches toujours plus complexes et les réussissent plus souvent. La recherche en IA comporte de nombreux freins potentiels : son rythme global ne suivra sans doute pas celui de ces indicateurs précis. Ces résultats concordent toutefois avec notre impression interne : les outils agentiques accélèrent sensiblement la recherche. Les humains fixent toujours les priorités, choisissent les idées et résultats à approfondir, et décident d’étendre, de suspendre ou de déployer les systèmes.

Menée de façon responsable, la recherche automatisée en IA produira, selon nous, des modèles améliorant le bien-être humain et servant la mission d’OpenAI. Elle peut réduire le coût de l’intelligence avancée pour en faire bénéficier le monde entier. Nous la poursuivons notamment pour résoudre l’alignement et bâtir des défenses face à une IA toujours plus performante. Un chercheur en IA automatisé peut aussi travailler sur la sécurité ou l’alignement. Des systèmes plus performants et alignés pourraient sécuriser les infrastructures critiques, contrer les agents d’IA dangereux et créer de nouvelles protections.

Ces raisons justifient des capacités utiles de recherche automatisée, mais pas nécessairement la poursuite d’une RSI rapide. Poursuivre, et comment, doit dépendre de notre capacité à préserver le contrôle humain et de choix démocratiques éclairés sur les bénéfices et les risques.

Nous ne savons pas encore atteindre en toute sécurité une RSI complète et alignée. Nous renforçons l’alignement et la sécurité à mesure que les capacités progressent. Mais rien ne garantit qu’ils suivront le rythme, et les systèmes plus performants peuvent être plus difficiles à surveiller. Au cœur de cet effort, un travail rigoureux d’alignement et de sécurité commence par mesurer et atténuer les problèmes actuels des systèmes de programmation agentique. Face à un risque de sécurité inacceptable, nous agirons, quitte à ralentir ou arrêter le développement ou le déploiement de systèmes que nous ne pouvons suffisamment sécuriser.

Après l’incident Hugging Face, nous avons concrétisé cet engagement : suspension de l’apprentissage par renforcement (RL) sur nos derniers modèles à déployer, renforcement et tests adversariaux des environnements de recherche, et extension de la surveillance. Toute la recherche n’a pas cessé : certaines charges de travail ont repris sous des contrôles renforcés, d’autres sont restées suspendues. Nous avons relevé nos normes et mieux intégré la sécurité au cycle de vie des modèles, exigeant davantage de preuves d’alignement tout au long de l’entraînement.

Nous présentons aujourd’hui un bilan détaillé de la contribution des systèmes agentiques à nos progrès vers la RSI ces derniers mois. Ces systèmes sont nouveaux et évoluent vite ; nos mesures restent préliminaires. Partager ces premiers résultats et méthodes vise à informer le public, à encourager la divulgation et à favoriser des normes de mesure communes.

Comme indiqué dans notre plan de politique pour l’IA de pointe, nous pensons que les entreprises, dont OpenAI, devraient être tenues de rendre publics leurs progrès vers la RSI. Même sans obligation, nous comptons rester transparents sur nos progrès vers la RSI. Notre transparence évoluera avec nos méthodes et notre compréhension, tout en préservant la sécurité et les informations propriétaires.

1. Les agents de programmation transforment le quotidien des chercheurs d’OpenAI

En début d’année, le chercheur médian d’OpenAI, selon l’utilisation des agents, ne les utilisait que modestement. À la mi-août, il les intégrait chaque jour à son travail, consommant plus de 600 $ d’inférence par jour aux tarifs de l’API. Au 90e percentile, un utilisateur de notre organisation de recherche consomme désormais plus de 7 000 $ de tokens par jour.

Avant juin 2026, la durée totale d’exécution des agents en recherche restait inférieure au temps de travail humain total. Ce n’est plus le cas. Sur la base d’une journée de 8 heures, à la mi-août, la recherche utilise au total 3,1 journées-agent pour chaque journée de travail humain.

On peut aussi compter les chercheurs utilisant des flux fortement parallèles, par exemple 4 agents ou plus à la fois. Comme le montre le graphique ci-dessous, ce nombre augmente. Ces chiffres incluent les pics quotidiens des agents lancés directement par l’utilisateur et des sous-agents créés par ces derniers.

2. Les chercheurs écrivent plus de code et mènent plus d’expériences

Une grande part de la recherche en IA consiste à intégrer, au prix d’un travail intensif, des gains d’intelligence ou de performance à nos modèles principaux. Toutes les étapes doivent réussir ensemble : concevoir des améliorations, écrire des évaluations et l’infrastructure pour les tester à grande échelle, détecter les bugs et comportements dangereux ou non alignés, puis intégrer les idées gagnantes à un entraînement principal. Un échec à une seule étape peut freiner toute la boucle.

Écrire du code et mener des expériences sont deux activités majeures des chercheurs ; nos observations montrent qu’elles s’accélèrent.

Ces indicateurs sont faciles à mesurer, mais parfois difficiles à interpréter. Avec l’automatisation, les tâches les moins automatisables mobiliseront davantage les chercheurs et deviendront les principaux freins au progrès. Le calcul est un autre facteur limitant, qui pourrait peser davantage à mesure que les autres freins s’atténuent.

En 2026, le nombre d’expériences par expérimentateur actif a augmenté, atteignant en août un record depuis janvier 2025. Cette hausse est corrélée à l’adoption de Codex, mais le calcul disponible a aussi fortement augmenté depuis 2025.

3. Les tâches confiées aux agents évoluent

Observations qualitatives et données internes montrent que les chercheurs délèguent de plus en plus aux agents des tâches de haut niveau et de plus longue durée.

Pour éclairer cette tendance, nous avons analysé les usages récents en recherche avec une taxonomie récemment publiée(ouverture dans une nouvelle fenêtre) par Epoch AI des tâches du cycle de R&D en IA. Inspirée du système de classification des métiers O*NET, cette taxonomie adaptée à la R&D en IA de pointe distingue six phases :

Décider : sujets, poursuite des travaux, allocation des ressources

Concevoir : idées de recherche et spécifications techniques

Construire : code et jeux de données

Exécuter : entraînement, évaluations, matériel, service d’inférence

Analyser : expériences, modèles, déploiement, travaux externes

Ci-dessous, nous classons les tokens des agents de programmation selon cette taxonomie.

Toutes les catégories ont progressé de janvier à août 2026. En janvier, le code de recherche et d’infrastructure dominait. Cette catégorie a grandi, mais d’autres aussi, surtout l’assistance technique et la surveillance des exécutions. La planification de haut niveau reste une part minime des tokens de sortie des agents.

Selon nos collègues, les agents excellent dans le dépannage de l’infrastructure de recherche interne, levant un frein important. Plusieurs équipes proposant des permanences de dépannage ont vu leur fréquentation baisser en 2026. L’une les a supprimées pour se consacrer à d’autres améliorations.

Ce graphique montre les messages initiaux quotidiens d’un des principaux canaux internes où les chercheurs sollicitent l’assistance technique d’autres équipes. À notre connaissance, cette baisse n’a pas été compensée par un transfert vers un autre canal d’assistance assuré par des humains. La baisse du trafic concorde avec cette évolution générale.

Nous pouvons aussi étudier si les agents réussissent les tâches confiées par les chercheurs. Un classificateur agentique montre que, de janvier à juillet, la réussite a généralement progressé dans plusieurs niveaux de difficulté, estimés par la durée humaine, pour les tâches au résultat vérifiable. Les agents ont toutefois encore besoin d’un guidage humain important, surtout pour les tâches complexes. Ces 6 derniers mois, plus de la moitié des tâches de 4 à 8 heures réussies ont nécessité au moins une intervention.

4. Réguler le rythme de développement des modèles

Les progrès vers des systèmes plus performants au service d’une AGI sûre et bénéfique dépendront aussi des garde-fous nécessaires à ces travaux. Notre évaluation des garde-fous nécessaires peut évoluer à mesure que nous comprenons mieux les risques.

Comme nous l’avons expliqué, nous avons récemment actualisé nos normes de surveillance, d’alignement et de sécurité. Nous montrons ici comment les restrictions récentes ont affecté un aspect de l’activité de recherche.

Le 20 juillet, après avoir découvert que des agents avaient compromis notre infrastructure de recherche, nous avons temporairement arrêté le service de conteneurs utilisé pour l’entraînement, puis l’avons rétabli avec d’importantes restrictions supplémentaires.

Cela a entraîné une forte baisse du calcul consacré à l’entraînement en RL, le temps que les équipes reconfigurent leurs flux de travail pour opérer dans cet environnement de recherche renforcé. Le graphique ci-dessus inclut la suspension de deux semaines de l’apprentissage par renforcement sur nos derniers modèles destinés au déploiement. Entre le 20 juillet et le 6 août, la majorité des exécutions des expériences de RL de classe Astra, en termes d’allocation de GPU, visait à tester la mise en œuvre d’améliorations de sûreté et de sécurité.

Le 7 août, des indices préliminaires suggérant qu’Astra pourrait posséder des cybercapacités critiques au sens de notre cadre de préparation(ouverture dans une nouvelle fenêtre) ont conduit à des restrictions de sécurité supplémentaires propres à ce modèle, imposant son exécution dans des environnements de recherche plus sécurisés. La semaine suivante, l’allocation de GPU à la classe Astra a encore baissé de 59,2 %, tandis que celle des autres classes de modèles a augmenté de 17,2 %. Cette hausse a compensé environ 85 % de la baisse de la classe Astra, laissant l’allocation totale des charges de travail de RL analysées largement inchangée. Cette tendance concorde avec le transfert d’une partie de l’entraînement et de l’expérimentation vers des modèles non-Astra pendant les restrictions touchant Astra. Elle rejoint les témoignages de chercheurs ayant trouvé d’autres usages aux ressources de calcul qui ne pouvaient plus servir aux charges de travail soumises aux nouvelles contraintes.

Ces données éclairent utilement les débats actuels sur l’entraînement et la sécurité : lorsque de nouveaux contrôles sont instaurés, les ressources de calcul restent précieuses et flexibles, et sont naturellement réorientées vers d’autres usages au sein de la recherche. À plus long terme, les discussions sur le rythme des progrès de l’IA devraient aussi aborder la meilleure façon d’utiliser les ressources de calcul soumises à des contrôles nouveaux ou envisagés.

5. La voie à suivre

Progresser vers une RSI alignée et comprendre ces progrès est essentiel à notre mission. Nous continuerons d’affiner nos méthodes, de rendre compte de l’évolution de notre compréhension et d’œuvrer pour un débat public éclairé et une véritable gouvernance démocratique des systèmes de pointe.

Annexe : méthodes utilisées pour cet article

La recherche en IA appuyée par des agents est encore nouvelle, et nous apprenons toujours à la mesurer. Certains indicateurs, comme la quantité de code générée par nos équipes de recherche, sont relativement faciles à recueillir, mais difficiles à interpréter, car leur lien avec les progrès de la recherche est incertain. Des indicateurs plus directement centrés sur les progrès de la recherche, comme la fréquence à laquelle les agents réussissent les tâches confiées par les chercheurs, pourraient être plus utiles, mais sont complexes à élaborer et à valider. L’évolution rapide des outils et systèmes utilisés par les chercheurs ajoute à cette difficulté. Approfondir notre compréhension de l’accélération de la recherche constitue un axe majeur dans l’ensemble d’OpenAI.

Dans toutes ces analyses, sauf indication contraire :

Le terme « chercheur » désigne au sens large tout membre de notre organisation de recherche, y compris ceux qui développent l’infrastructure, gèrent les projets ou soutiennent l’activité d’une autre manière.

Les indicateurs d’utilisation des agents de programmation couvrent la plupart des usages, mais pas tous, compte tenu de l’évolution rapide des outils et systèmes utilisés par les chercheurs.