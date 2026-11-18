Cursus

Les équipes d'ingénierie livrent aujourd'hui plus de code qu'elles ne peuvent en lire. Les assistants IA en écrivent désormais une grande partie, plus vite que n'importe quel relecteur ne peut suivre ligne par ligne. Ce basculement sert de toile de fond à la conférence DASH de Datadog à New York cette semaine, où le cofondateur et CTO Alexis Lê-Quôc anime une session intitulée « The New Shape of Engineering ».

Son constat est simple : la façon d'exploiter les logiciels n'a pas changé : on livre une modification, on la déploie, puis on observe. Ce qui change, c'est le volume et la cadence — et ça modifie ce qui garantit la sécurité de l'ensemble.

Dans cet article, je synthétise sa pensée en six leçons clés : évolution de la revue de code, production comme test ultime, et ce que vous devez en retenir.

Si vous découvrez l'observabilité des LLM, nous vous recommandons de lire nos guides pour bien démarrer avec le MLOps et l'évaluation des LLM comme point de départ.

## En bref

Le fil conducteur de Lê-Quôc : l'observabilité devient la couche de contrôle du logiciel écrit, testé et déployé par l'IA, au service des opérateurs comme des agents eux-mêmes.

Les six leçons, en résumé :

- **La revue se déplace hors du code lui-même.** Il y a trop de code généré par l'IA pour le lire ligne à ligne ; le véritable contrôle, ce sont les tests, spécifications et preuves que vous concevez en amont, en prévoyant aussi des garde-fous contre des agents qui chercheraient à biaiser ces tests.
- **La production est le seul test qui compte.** Un pipeline CI tout au vert ne prouve pas grand-chose face aux usages réels que vous n'aviez pas pu anticiper, et la sortie d'un modèle n'est jamais totalement certaine ; il faut donc la surveiller en direct et garder un bouton d'arrêt.
- **Laissez les agents prendre la pénibilité.** Confiez-leur la surveillance de tableaux de bord et la vérification d'hypothèses qui épuisent les humains, et gardez les personnes pour les décisions à fort jugement.
- **Séparez le travail en deux boucles :** une boucle de développement (écrire, livrer, vérifier, corriger) et une boucle opérations et sécurité (détecter, enquêter, résoudre).
- **Maîtrisez les dépenses d'IA.** Dimensionnez quel modèle fait quelle tâche à partir des trajectoires des agents, et laissez cette décision aux développeurs et SRE qui la prennent.
- **Apprenez à apprendre.** Les modèles sont des tuteurs infatigables, mais la compétence est de les questionner : comprendre les systèmes couche par couche et demander pourquoi le code qu'ils ont écrit a vraiment fonctionné.

## Améliorez les compétences de votre équipe en matière d'IA

Transformez votre entreprise en dotant votre équipe de compétences avancées en matière d'IA grâce à DataCamp for Business. Améliorez vos connaissances et votre efficacité.

## Leçon 1 : l'IA a fait voler en éclats l'ancienne revue de code

Commençons par la pression qui conditionne tout le reste : il y a plus de code qu'aucun humain ne peut en lire.

Lê-Quôc est direct : le modèle historique, un humain lisant une pull request ligne à ligne, ne résiste pas au développement assisté par IA. L'inquiétude qu'il entend dans tout le secteur porte sur l'impossibilité de la revue : il se passe trop de choses pour suivre en lisant des PR.

Sa réponse n'est pas de demander aux gens de lire plus vite, mais de déplacer la revue ailleurs.

La revue n'est plus à la ligne de code ; il y en a trop, vous ne pouvez pas suivre. Il s'agit des tests que nous conço ns en amont, et d'interdire à l'agent de les contourner.

Alexis Lê-Quôc, CTO at Datadog 

Ce dernier point est facile à manquer. Dès lors que vous orchestrez un agent pour planifier, un autre pour écrire et un troisième pour tester, vous devez aussi empêcher l'écrivain de jouer avec les tests automatisés au lieu de résoudre le problème.

Il va au-delà des tests. Datadog ajoute désormais des preuves semi-formelles et formelles que la spécification fait bien ce qu'elle doit faire — une démarche trop coûteuse à généraliser avant que les agents ne prennent le gros du travail. Cela fonctionne particulièrement bien sur les systèmes backend et de coordination, où le comportement est suffisamment mathématique pour raisonner avec précision.

## Leçon 2 : la production est le seul test qui compte

Réussir tous les tests en CI est nécessaire, et très loin d'être suffisant. Les défaillances qui comptent arrivent plus tard.

Là où ça compte vraiment, c'est en production.

Alexis Lê-Quôc, CTO at Datadog 

Chaque livraison repose sur des hypothèses impossibles à vérifier pleinement en amont : la forme des données et le comportement des utilisateurs. Exposez ces hypothèses à suffisamment de trafic réel, et les cas rares cessent de l'être : ils deviennent les lenteurs et erreurs quotidiennes de la dérive des données et des modèles.

Les LLM compliquent encore la donne : avec du code classique, on peut au moins raisonner sur chaque branche. Personne ne peut expliquer mécaniquement pourquoi un modèle renvoie telle réponse ; le même input ne garantit jamais le même output. Les résultats étranges occasionnels ne peuvent pas être éradiqués par l'ingénierie.

Vous cessez donc d'essayer de prouver qu'un système est correct avant la mise en production. À la place, vous :

- Rédigez des évaluations du comportement attendu
- Le surveillez en production
- Gardez un dispositif d'arrêt pour un déploiement qui tourne mal.

La question n'est plus de savoir s'il a réussi, mais si un problème est isolé ou le début d'une tendance.

Ce signal en direct n'est pas qu'un tableau de bord pour humains. Branché au système de déploiement, il permet à un agent de dérouler un changement comme le ferait un ingénieur prudent : 1 % des utilisateurs, puis 5 %, en jugeant sur données réelles si la modification produit l'effet attendu.

## Leçon 3 : laissez les agents prendre la pénibilité

Pour Lê-Quôc, les agents ne remplacent pas les ingénieurs : ils prennent les tâches qui usent.

Diagnostiquer un incident, c'est tester des hypothèses face à un symptôme et, lors des incidents longs, c'est souvent l'hypothèse improbable qui s'avère juste. L'agent Bits AI de Datadog les vérifie toutes en parallèle, en amont de l'ingénieur, tandis que la personne l'oriente vers l'intuition qu'aucun dashboard ne ferait ressortir.

Le fond de l'affaire, c'est la fatigue. Un déploiement en astreinte, c'est des pics d'alerte suivis d'heures de vide, répétés jusqu'à éroder le jugement.

Vous êtes en mode alerte maximale, puis vous regardez la peinture sécher.

Alexis Lê-Quôc, CTO at Datadog 

Un agent ne s'en formalise pas et ne décline pas après quatre heures à fixer des chiffres. Le stress et la fatigue dégradent les performances humaines, raison pour laquelle on fait tourner les personnes en astreinte.

Confiez la veille inlassable à une machine, et les équipes reviennent reposées pour les décisions qui comptent. Même logique en sécurité, où les analystes s'épuisent à trier faux positifs et vraies menaces.

## Leçon 4 : séparez le travail en deux boucles

Lê-Quôc organise le travail des agents chez Datadog autour de deux boucles.

### La boucle de développement

La première boucle sera familière à la plupart des ingénieurs :

1. Écrire du code
2. Le livrer
3. Vérifier si ça fonctionne
4. Corriger
5. Répéter

L'angle de Datadog : un problème qui naît dans le code se répare généralement dans le code. La plateforme tente donc de vous proposer cette correction, à partir de ce qu'elle sait de l'application : propriétaires, changements récents, erreurs remontées.

Il cite l'optimisation de requêtes base de données comme exemple. N'importe quel modèle peut réécrire une requête lente ; le plus difficile est de prouver que la version réécrite est plus rapide et sûre avant d'atteindre la production. Datadog la teste donc sur une copie réaliste des données de production, puis fournit une pull request assortie des preuves.

### La boucle opérations et sécurité

L'autre boucle tourne en parallèle, chez les mêmes personnes ou une équipe différente :

1. Détecter
2. Enquêter
3. Corriger
4. Répéter

C'est là que l'AI Guard de Datadog priorise les événements de sécurité et bloque les attaques plus vite qu'un analyste ne le ferait à la main. Les agents peuvent également gérer des tâches opérationnelles routinières que les ingénieurs effectuent chaque jour sans enthousiasme, comme redimensionner ce fameux pod Kubernetes.

Dans les deux boucles, Lê-Quôc reste clair sur l'ordre des priorités. Datadog ne part pas de « voici l'IA, quel problème peut-elle résoudre ? » : on part d'une douleur client avérée, généralement une variante de « je ne veux plus faire cette tâche répétitive », puis on évalue si un agent est digne de confiance pour s'en charger.

## Leçon 5 : maîtrisez les dépenses d'IA

Le coût est la contrainte jumelle de la sécurité, et contenir le prix de la mise en production des grands modèles de langage devient une discipline à part entière. La réponse présentée par Lê-Quôc à DASH : l'Agent Console de Datadog.

Demandez à un développeur quel modèle il lui faut : souvent, il citera le plus puissant (et le plus cher). Parfois, c'est le bon choix, mais une grande part du travail est du générique qu'un modèle plus économique et rapide gère tout aussi bien. Les distinguer suppose d'analyser les trajectoires des agents d'une organisation : quels outils ils appellent, à quelle fréquence ils réussissent, jusqu'à faire apparaître des motifs.

Ces motifs deviennent des heuristiques plutôt que des règles : un modèle de pointe comme le dernier Claude Opus ou les modèles GPT pour la planification, un modèle économique comme Claude Haiku pour générer des tests.

| Tâche | Niveau de modèle | Pourquoi | 
|---|---|---|
| Planification et raisonnement complexe | Modèle de pointe (ex. : Claude Opus, GPT) | La meilleure capacité de raisonnement se rentabilise ici | 
| Code routinier, générique | Niveau intermédiaire (ex. : Claude Sonnet, GPT-mini) | Assez performant, et bien moins coûteux à exécuter fréquemment | 
| Génération de tests et transformations simples | Rapide et peu cher (ex. : Claude Haiku, GPT-nano) | La vitesse et le prix l'emportent tant que la qualité tient | 

Le principe sous-jacent concerne la propriété de la décision. Si vous remontez le coût à un seul chiffre, vous obtenez ce que Lê-Quôc appelle une « très faible actionnabilité » : soit tout le monde coupe les dépenses, et on tue des travaux utiles, soit tout le monde continue, et l'entreprise ne peut pas suivre. Il préfère mettre les données sous les yeux des développeurs et SRE qui choisissent les modèles.

## Leçon 6 : apprenez à apprendre

Interrogé sur ce que les nouveaux ingénieurs devraient étudier, Lê-Quôc donne une réponse qui paraît ancienne, mais ne l'est pas.

Vous devez apprendre à apprendre.

Alexis Lê-Quôc, CTO at Datadog 

Les modèles sont les tuteurs les plus patients jamais inventés, capables d'expliquer n'importe quoi à n'importe quel rythme — un niveau d'accès autrefois réservé à quelques privilégiés. Mais un tuteur n'est utile que si vous l'interrogez. La compétence, c'est savoir quoi demander et comment vérifier la réponse.

Il recommande de comprendre l'informatique couche par couche plutôt que de la traiter comme de la magie. Prenez un ordonnanceur, un équilibreur de charge, un bac à sable, et demandez à un modèle d'expliquer son fonctionnement, puis creusez :

- Que signifie ce terme ?
- Comment le mesurer ?
- Quelles sont les bases mathématiques ?
- Comment savoir si cela fonctionne bien ?

Étudier les classiques de cette manière est volontairement lent. Il compare cela à l'apprentissage d'un instrument : vous pouvez écouter de la musique toute la journée, mais pour jouer du piano, il faut poser les mains sur le clavier.

Même chose pour le code écrit par l'IA. Le vibe coding est très bien, dit-il, à condition d'y revenir et de demander pourquoi ça a marché : pourquoi tel choix d'architecture, existe-t-il de meilleures approches, sur quoi cela s'est-il fondé. Le but n'est pas d'écrire moins de code avec l'IA, mais de mieux comprendre le code que vous produisez désormais en bien plus grande quantité.

## En conclusion

Le message central de Lê-Quôc : la boucle n'a pas changé, mais la vitesse, si. Désormais, aucun humain ne peut observer d'assez près à la cadence de l'IA : la surveillance, et une part croissante de la construction, passent à des agents qui ne se fatiguent ni ne paniquent.

Il plaide pour traiter l'observabilité comme un plan de contrôle, pas comme une collection de graphiques. Si des agents écrivent, testent, livrent et exploitent du logiciel, ils ont besoin du même ancrage dans les données de production réelles que les bons ingénieurs, avec en plus une personne qui garde le jugement et le bouton d'arrêt. Datadog positionne l'observabilité comme la couche qui rend cet équilibre sûr.

La compétence attendue des ingénieurs est claire : lire les systèmes à travers leur comportement en production, pas seulement via leur source. Pour ancrer cette habitude, notre parcours de compétences Machine Learning in Production est un bon point de départ.

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**