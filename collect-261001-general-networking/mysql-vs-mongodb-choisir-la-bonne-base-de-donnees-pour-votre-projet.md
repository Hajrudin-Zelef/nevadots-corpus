Cours

Le choix de la bonne technologie de base de données est une décision cruciale pour tout projet de développement. MySQL et MongoDB sont les deux principales options. MySQL offre un modèle relationnel structuré avec des garanties ACID, tandis que MongoDB propose une architecture flexible et orientée vers les documents.

Dans ce guide, nous allons comparer et opposer ces méthodologies et fournir des exemples concrets de modélisation de données pour chaque plateforme. En évaluant leurs avantages et leurs inconvénients, vous serez en mesure de choisir la base de données qui correspond le mieux aux exigences de votre application en matière de données, aux objectifs de performance et aux objectifs d'évolutivité.

Si vous souhaitez vous initier à l'un ou l'autre de ces outils, n'hésitez pas à consulter nos cours Introduction à NoSQL ou Introduction à MongoDB en Python.

## MySQL vs MongoDB : Modèles de données et conception de schémas

Les données se présentent sous différentes formes. Les données structurées sont classées dans des catégories prédéfinies avec des relations claires, tandis que les données non structurées sont plus libres. Les bases de données relationnelles stockent des données structurées dans des tableaux avec des schémas fixes, qui appliquent des règles sur la façon dont les données peuvent être ajoutées ou modifiées. Les bases de données orientées documents enregistrent chaque enregistrement comme son propre "document", dont les champs ne doivent pas nécessairement correspondre exactement à ceux d'autres documents.

### Tableaux structurés contre documents flexibles

Décortiquons la manière dont chaque base de données organise les informations, en opposant les schémas rigides de MySQL, basés sur des tableaux, aux documents sans schéma de MongoDB, et en expliquant pourquoi cette distinction importe pour les applications du monde réel.

#### Données structurées avec MySQL

Le langage standard pour interroger et gérer les bases de données est le langage de requête structuré (SQL). MySQL est un système de gestion de bases de données relationnelles (SGBDR) qui prend en charge l'intégralité des bases de données : authentification de l'utilisateur et contrôles d'accès, procédures stockées et intégrité des données grâce à des contraintes. Comme beaucoup d'autres SGBDR, il utilise SQL pour interroger ses données. Pour comparer MySQL avec d'autres systèmes de gestion de bases de données relationnelles, consultez ces liens :

- PostgreSQL vs. MySQL : Choisir la bonne base de données pour votre projet
- SQL Server, PostgreSQL, MySQL : Quelle est la différence ?

MySQL conserve les données dans des tableaux, à l'instar des feuilles de calcul. Chaque tableau définit des colonnes nommées pour des champs avec des types de données spécifiques (nombres, mots ou dates) et utilise des lignes pour représenter des enregistrements individuels. Chaque ligne possède un identifiant unique, appelé clé primaire, qui permet une consultation rapide. Une colonne peut être une clé étrangère qui pointe vers une clé primaire d'un autre tableau afin de relier des données connexes, par exemple l'enregistrement d'un étudiant à ses notes.

Avant d'utiliser un tableau, vous devez d'abord définir sa structure et ses règles à l'aide de commandes telles que CREATE TABLE. Par exemple, vous pouvez spécifier que "cette colonne ne peut pas être vide" ou que "les valeurs doivent être uniques". Pour récupérer ou modifier des données, vous utilisez des instructions SQL telles que SELECT et INSERT. Le moteur de requête de MySQL détermine le plan d'exécution le plus rapide.

La conception d'une base de données est une science et un art importants en soi.

#### Données non structurées avec MongoDB

MongoDB est un exemple de base de données NoSQL qui ne repose pas sur le modèle relationnel traditionnel basé sur des tableaux. MongoDB stocke les informations dans des "documents" au lieu de tableaux. Un document est un objet de type JSON qui peut contenir n'importe quelle combinaison de champs. Les documents similaires sont regroupés dans une "collection".

Les documents sont flexibles et n'ont pas de schéma. Cela signifie que les champs des différents documents d'une collection ne doivent pas nécessairement correspondre exactement. Par exemple, un document peut avoir un champ `maidenName` alors qu'un autre n'en a pas.

Cette approche sans schéma permet aux développeurs d'ajouter ou de supprimer des champs à la volée sans avoir à redéfinir la base de données. Sous le capot, MongoDB continue d'indexer et de rechercher efficacement ces documents. Il permet également aux développeurs de modifier leur modèle de données au fur et à mesure que leur application se développe.

Pour plus d'informations sur MongoDB, jetez un coup d'œil à ces ressources DataCamp.

- Un tutoriel NoSQL complet utilisant MongoDB
- Les 25 meilleures questions et réponses d'entretien sur MongoDB pour 2025

#### Bases de données relationnelles et bases de données documentaires

Les bases de données relationnelles sont excellentes lorsque vos données sont regroupées dans des tableaux aux colonnes fixes et aux relations claires. Par exemple, une école pourrait gérer les profils et les notes des élèves dans des tableaux liés, et les banques pourraient relier les comptes, les transactions et les détails des clients. Les tableaux étant définis à l'avance, des règles sont appliquées pour éviter les erreurs et garantir la fiabilité des données.

Les bases de données orientées documents sont appropriées lorsque les données sont variées ou évolutives. Par exemple, une plateforme de blogs peut avoir des caractéristiques différentes pour chaque article. Chaque document peut comporter une combinaison d'auteurs, de balises, de commentaires, voire de vidéos ou de sondages. Il n'est pas nécessaire de revoir le schéma lorsque vous ajoutez des fonctionnalités.

### Complexité de la migration des schémas

Dans MySQL, les modifications de schéma nécessitent l'écriture et l'exécution de scripts de migration SQL (ALTER TABLE, CREATE TABLE). Une planification et des tests minutieux sont nécessaires pour éviter les blocages de tableaux ou les temps d'arrêt et pour gérer les retours en arrière en cas de problème.

Le modèle sans schéma de MongoDB signifie qu'il n'y a pas de structure fixe à modifier. Au lieu de cela, les migrations impliquent l'écriture de scripts qui analysent chaque document et appliquent les mises à jour (à l'aide d'opérateurs tels que $set, $rename ou $unset). Les documents pouvant être différents, les migrations doivent être testées de manière approfondie afin d'éviter toute perte ou corruption de données.

## Langages de requête et capacités opérationnelles dans MongoDB vs MySQL

Dans cette section, nous comparons les langages d'interrogation de MySQL et de MongoDB. Nous illustrons chacun d'entre eux par des exemples pratiques et abordons les considérations de performance pour les lectures et les écritures.

### MySQL vs MQL: Approches divergentes de la recherche de données

MySQL fonctionne avec des tableaux et des relations fixes. Il fournit un riche ensemble de commandes d'interrogation pour combiner, filtrer et résumer à l'aide de ces tableaux et relations.

Les tableaux sont liés avec `JOIN`, filtrés avec `WHERE`, et triés avec `ORDER BY`. Les données peuvent être agrégées à travers des groupes avec `GROUP BY` pour définir le groupe, et des commandes telles que `AVG()` ou `COUNT()` agrègent à l'intérieur de ces groupes. 

Les fonctions de fenêtre sont utilisées pour attribuer des rangs par ligne sans réduire l'ensemble des résultats. Par exemple, attribuez des rangs par ligne à l'adresse `RANK() OVER (ORDER BY score) DESC))`.

Le langage de requête de MongoDB(MQL) travaille sur des documents de type JSON dans des collections plutôt que dans des tableaux rigides. Vous récupérez les enregistrements avec `find()`, vous les filtrez avec des opérateurs de comparaison (`$gt`, `$and`, `$or`), vous ne projetez que les champs dont vous avez besoin, et vous les ordonnez et les paginez avec `.sort()`, `.limit()`, et `.skip()`. 

Joignez des collections à l'aide de `$lookup`, et utilisez les étapes du pipeline d'agrégation (`$match`, `$group`, `$sort`, `$lookup`) pour transformer et combiner des documents dans des flux de travail à plusieurs étapes. 

Illustrons ces différences par des exemples. Supposons tout d'abord que nous disposions d'un tableau contenant des étudiants et leurs notes. Dressons la liste des meilleures notes, de la plus élevée à la plus basse.

Dans MySQL, cette opération peut se présenter comme suit :

```
SELECT name, grade
FROM students
WHERE grade > 90
ORDER BY grade DESC;
```
En MQL, vous écririez une requête similaire à celle-ci.

```
db.students.find({ grade: { $gt: 90 } },
                                        { name: 1, grade: 1 })  # only project name and grade
                   .sort({ grade: -1 })
```
Chaque approche tire parti de son modèle sous-jacent - les tableaux pour SQL et les documents pour MQL - pour obtenir une récupération puissante des données.

### Critères de performance

MongoDB offre généralement des performances d'écriture plus rapides que MySQL, car il écrit des documents entiers sans vérifier un schéma rigide. Même lorsque MongoDB utilise la validation de schéma ou des index uniques, il est généralement plus rapide pour les insertions simples, car il écrit l'intégralité du document en une seule fois.

Les performances de lecture varient selon les cas d'utilisation. En raison de son modèle centré sur les documents, MongoDB excelle dans la recherche de documents individuels ou de petits lots par identifiant. My SQL excelle pour les jointures complexes entre plusieurs tableaux ou les grandes agrégations grâce à son optimiseur basé sur les coûts et à ses stratégies d'indexation matures.

## Stratégies de montée en charge de MySQL vs MongoDB

Il existe deux stratégies opposées pour la mise à l'échelle : la stratégie verticale et la stratégie horizontale. La mise à l'échelle verticale consiste à améliorer une machine unique en ajoutant des unités centrales, de la mémoire vive ou des disques plus rapides pour gérer une charge accrue.

Cette approche est simple en ce sens qu'elle ne nécessite pas de grappes ou d'équilibreurs de charge. Cependant, elle est limitée par le matériel et peut être coûteuse. Il y a un seul point de défaillance : Si le serveur tombe en panne, tout s'arrête.

En revanche, la mise à l'échelle horizontale consiste à ajouter des machines pour partager la charge de travail. Il vous permet de vous développer à moindre coût (pas besoin d'un énorme serveur onéreux) et garantit la poursuite des opérations en cas de défaillance d'une machine. Cependant, comme elle nécessite davantage de coordination, comme l'équilibrage de la charge et la synchronisation des données, elle peut entraîner une surcharge du réseau.

### Limites de la mise à l'échelle verticale de MySQL

La mise à l'échelle verticale de MySQL se fait en deux étapes : d'abord, vous mettez à niveau le matériel, puis vous réglez les paramètres de configuration pour utiliser efficacement ces nouvelles ressources. En ce qui concerne le matériel, vous pouvez ajouter des cœurs de processeur, augmenter la mémoire vive et utiliser des composants de stockage et de réseau plus rapides. Une fois que la machine est suffisamment puissante, utilisez ces paramètres de configuration :

- `innodb_buffer_pool_size` . Augmentez la mémoire pour qu'elle corresponde à votre RAM.
- `innodb_log_file_size` ,`innodb_log_buffer_size` : augmentez-les pour mettre en lot davantage d'opérations d'écriture.
- `max_connections` ,`table_open_cache, thread_cache_size` . Augmentez-les pour garder plus de clients et de tableaux au chaud dans la mémoire.

Cependant, l'échelonnement vertical a des limites pratiques. À un moment donné, une machine unique atteindra ses limites en termes de cœurs de CPU ou de mémoire. Il existe un point de rendement décroissant pour l'investissement en matériel ; doubler le prix que vous payez pour les cœurs de l'unité centrale, par exemple, ne double pas la puissance de l'unité centrale.

Les charges de travail à forte intensité d'écriture peuvent rencontrer des problèmes avec des lignes ou des index verrouillés. Une défaillance du serveur peut mettre l'ensemble de la base de données hors ligne.

MySQL peut utiliser et utilise des techniques horizontales, mais cela nécessite une configuration supplémentaire par rapport au modèle de partage intégré de MongoDB.

### La mise à l'échelle horizontale de MongoDB via le sharding

MongoDB utilise une stratégie de mise à l'échelle horizontale, appelée "sharding", qui divise une grande collection en morceaux plus petits sur la base d'une "clé de shard" (par exemple, des plages d'identifiants de documents). Chaque morceau est stocké sur un serveur différent.

Un groupe de serveurs de configuration tient à jour une carte indiquant les zones contenant les différents morceaux. Les routeurs de requêtes utilisent ce répertoire pour envoyer les requêtes des clients aux unités de stockage appropriées. Un équilibreur automatique redistribue les morceaux au fur et à mesure que les données et les schémas de trafic changent, ce qui garantit qu'aucune unité n'est surchargée. Augmentez la capacité en ajoutant des serveurs de stockage.

Cette approche est idéale pour les applications soumises à des charges imprévisibles ou massives, telles que les médias sociaux, les flux vidéo et le commerce électronique pendant les grandes ventes. Les charges de stockage et d'interrogation étant réparties sur de nombreuses machines, le système reste réactif et résilient en cas de pics de trafic.

## Considérations relatives à la sécurité et à la conformité

Pour tout déploiement de base de données, il est essentiel de veiller à ce que les données restent sécurisées et conformes aux réglementations sectorielles. Comparons MySQL et MongoDB en termes d'authentification, de cryptage et de prise en charge de normes telles que HIPAA et GDPR.

### Authentification et cryptage

#### MySQL

MySQL sécurise les données en combinant l'authentification de l'utilisateur, les connexions cryptées et le cryptage au niveau du disque. Chaque utilisateur doit se connecter avec un compte et un mot de passe uniques et ne se voit accorder que les privilèges précis dont il a besoin, soit individuellement, soit en fonction de son rôle. Toutes les données en transit peuvent être protégées par TLS/SSL, imposées comme SSL uniquement, et validées par des certificats de serveur. Les données au repos sont cryptées par le moteur InnoDB. Les clés de chiffrement peuvent être changées sans interruption de service.

#### MongoDB

MongoDB sécurise les données par l'authentification, le cryptage et l'audit. Chaque utilisateur se connecte avec un nom d'utilisateur et un mot de passe uniques (via SCRAM, certificats x.509, Kerberos ou LDAP) et se voit attribuer des autorisations par le biais de rôles, intégrés ou personnalisés. Le trafic en transit est protégé par TLS/SSL, et les données au repos sont protégées par le moteur de stockage de WiredTiger. Des journaux d'audit détaillés enregistrent les tentatives de connexion et les modifications de données afin de faciliter les contrôles de sécurité et la conformité.

### Conformité

HIPAA, la loi américaine qui protège les dossiers médicaux, exige des contrôles d'accès stricts, le cryptage en transit et au repos, ainsi qu'un audit complet. MySQL répond à ces exigences avec des comptes utilisateurs basés sur des rôles, TLS/SSL pour les données en vol, InnoDB Transparent Data Encryption pour les données au repos, et des plugins d'audit qui enregistrent chaque accès.

MongoDB répond aux mêmes normes en utilisant l'authentification SCRAM ou x.509, TLS/SSL, le chiffrement du stockage WiredTiger, la journalisation d'audit d'entreprise et le chiffrement optionnel au niveau du champ côté client.

Le GDPR accorde aux résidents de l'UE le droit d'accéder à leurs données personnelles, de les corriger et de les effacer, en imposant des contrôles d'accès stricts, le cryptage et l'auditabilité.

MySQL répond à ces exigences grâce à des privilèges basés sur les rôles, au cursus TLS/SSL pour les connexions sécurisées, au chiffrement transparent des données InnoDB, aux plugins d'audit pour le suivi des modifications et aux cascades de clés étrangères pour l'exécution des suppressions.

MongoDB répond aux normes GDPR avec l'authentification SCRAM/x.509, TLS/SSL, le chiffrement au repos WiredTiger, la journalisation d'audit d'entreprise, les mises à jour et suppressions flexibles au niveau des documents et les options de déploiement spécifiques aux régions pour la résidence des données.

## Tableau comparatif des avantages et inconvénients de MySQL et MongoDB

| Aspect | MySQL (relationnel) | MongoDB (orienté documents) | 
| Paradigme des données | Données structurées avec un schéma fixe. | Données non structurées ou semi-structurées avec un schéma flexible. | 
| Modèle de stockage | Tableaux de lignes et de colonnes. | Documents de type JSON stockés dans des collections. | 
| Définition du schéma | Explicite. Définies en amont via `CREATE TABLE` ; les colonnes ont des types de données et des contraintes (`NOT NULL` ,`UNIQUE` , clés étrangères). | Implicite. Les documents peuvent contenir n'importe quel champ. Aucune définition préalable n'est requise. | 
| Les relations | Renforcé par des contraintes de clés étrangères dans les tableaux. | Il s'agit d'intégrer des données connexes dans un seul document ou de faire référence à d'autres documents. | 
| Flexibilité | Rigide. L'ajout ou la modification de colonnes nécessite des migrations sur le site `ALTER TABLE` et une planification minutieuse. | Flexible. Les champs peuvent être ajoutés ou supprimés à la volée sans temps d'arrêt. | 
| Migration des schémas | Scripts (`ALTER TABLE` ,`CREATE TABLE` ), verrous de tableaux potentiels, tests et planification du retour en arrière. | Scripts de mise à jour au niveau du document utilisant `$set` ,`$rename` ,`$unset` , généralement idempotents et exécutables en arrière-plan. | 
| Indexation et requêtes | Optimisé pour les tableaux multiples `JOIN` s et les agrégations. | Index sur les champs et sous-champs des documents. Les pipelines d'agrégation prennent en charge le regroupement, le filtrage et les jointures `$lookup` . | 

## Analyse des cas d'utilisation : Choisir le bon outil

Le choix de la bonne base de données dépend de la charge de travail, du modèle de données et des modèles de croissance de votre application. Comparons les scénarios dans lesquels MySQL et MongoDB apportent la plus grande valeur ajoutée.

### Scénarios idéaux pour MySQL

MySQL excelle dans les applications qui nécessitent un traitement fiable et volumineux des transactions et des données bien structurées.

Les institutions financières utilisent MySQL pour enregistrer les transferts de compte et les mises à jour de solde, les plateformes de commerce électronique l'utilisent pour gérer les commandes, l'inventaire et les informations sur les clients. Les systèmes de santé stockent les dossiers des patients et les données relatives aux rendez-vous, et les grandes entreprises utilisent des systèmes ERP ou CRM qui relient les ventes, la facturation et l'assistance.

La richesse des fonctionnalités de MySQL (GROUP BY, JOIN, fonctions de fenêtre et recherches indexées) facilite la création de rapports structurés, tels que les totaux mensuels des ventes ou les temps de réponse moyens, et permet de se connecter facilement aux outils de BI.

### Les atouts de MongoDB dans les applications modernes

La conception sans schéma de MongoDB et sa mise à l'échelle horizontale en font la solution idéale pour les applications qui doivent évoluer rapidement ou traiter des ensembles de données massifs. De nouvelles fonctionnalités peuvent être ajoutées sans migration coûteuse des schémas.

Les services globaux répartissent les données entre les régions pour assurer la rapidité et la résilience des lectures et des écritures. Sa capacité à gérer des flux d'événements en temps réel à haut volume le rend approprié pour les cas d'utilisation qui traitent des millions d'impressions par seconde, tels que les plates-formes de technologie publicitaire.

Le modèle de document flexible de MongodDB fonctionne bien avec la gestion de contenu, permettant aux articles, aux commentaires des utilisateurs, avec leurs combinaisons uniques de métadonnées, de cohabiter sans tableaux rigides. Les systèmes marketing peuvent utiliser cette flexibilité pour stocker les paramètres des tests A/B, les balises de suivi et les règles de personnalisation.

L'architecture horizontale de MongoDB garantit que les charges de travail massives et imprévisibles, telles que celles que l'on trouve dans les backends IoT ou de jeux, sont réparties sur de nombreux serveurs pour que le système reste réactif et tolérant aux pannes.

## Stratégies et outils de migration

MySQL utilise un modèle relationnel, tandis que MongoDB utilise un modèle basé sur des documents. Par conséquent, le passage de MySQL à MongoDB implique une planification minutieuse pour repenser à la fois les structures de données et la logique de l'application.

### Passer de MySQL à MongoDB

Pour migrer de MySQL à MongoDB, suivez les étapes suivantes.

1. Analyser le schéma MySQL. Inventoriez les tableaux MySQL, y compris les colonnes, les types de données et les relations à clé étrangère pour identifier les entités logiques et les connexions que vous devrez modéliser dans MongoDB.
2. Concevoir le modèle de document. Sélectionnez les collections de premier niveau et décidez des données connexes à intégrer ou à référencer. Déterminez les index et, si vous voulez faire un partage, choisissez une clé de partage appropriée.
3. Extraire les données. Exportez les tableaux MySQL au format JSON ou CSV (à l'aide de `mysqldump` ou de scripts ETL), en divisant les tableaux volumineux en lots gérables si nécessaire.
4. Transformer et nettoyer les données. Remodelage de chaque ligne pour l'adapter à la structure du document cible. Fusionnez les lignes parent et enfant pour les documents incorporés, en conservant les ID pour les références. Gérer les NULL et les conversions de type.
5. Chargez les données dans MongoDB. Utilisez `mongoimport` ou des scripts personnalisés pour insérer des documents. Créez ensuite les index nécessaires.
6. Refondre le code de l'application. Remplacez les requêtes SQL par des appels MongoDB (tels que find(), insertOne(), et les pipelines d'agrégation). Mettez à jour la logique de transaction si nécessaire.
7. Testez et validez. Vérifiez que toutes les opérations de création, de lecture, de mise à jour et de suppression se comportent de manière identique. Exécutez des tests d'intégration et de charge pour confirmer les performances et l'intégrité des données.
8. Déployez. Déployez votre version de Mongo-DB aux côtés de MySQL dans un environnement de test, surveillez les performances, puis basculez le trafic de production vers MongoDB et mettez MySQL hors service une fois que vous êtes sûr de la réussite de la migration.

## Conclusion

Le choix entre MySQL et MongoDB dépend de la manière dont votre application traite les données. Le schéma fixe de MySQL, basé sur des tableaux, la conformité ACID, l'interrogation SQL mature et la mise à l'échelle verticale en font la solution idéale pour les scénarios de rapports structurés et à forte intensité de transactions, tels que la banque ou le commerce électronique.

Le modèle de document sans schéma de MongoDB, les pipelines d'agrégation flexibles et le sharding horizontal intégré excellent dans les cas d'utilisation à évolution rapide, à volume élevé ou à distribution géographique, tels que l'analyse en temps réel, la télémétrie IoT et la gestion de contenu. En tenant compte de facteurs tels que la fiabilité des transactions, la flexibilité des schémas, la complexité des requêtes et la stratégie d'évolution, les équipes peuvent sélectionner la base de données qui correspond le mieux à leurs besoins en termes de performances et de croissance.

Initiez-vous dès aujourd'hui à ces deux technologies grâce à nos cours d'introduction à NoSQL ou d'introduction à MongoDB en Python.

## FAQ MySQL vs MongoDB

### Quelle est la principale différence entre MySQL et MongoDB ?

**MySQL est une base de données relationnelle qui stocke les données dans des tableaux fixes avec des schémas prédéfinis, tandis que MongoDB est une base de données orientée documents qui stocke des documents flexibles, de type JSON, sans nécessiter de schéma rigide.**

### Quand devrais-je choisir MySQL plutôt que MongoDB ?

Utilisez MySQL pour les applications à fort volume de transactions avec des données bien structurées, des jointures complexes et des exigences strictes en matière d'intégrité des données, telles que les systèmes bancaires et le traitement des commandes de commerce électronique.

Utilisez MongoDB dans les projets qui nécessitent une évolution rapide des schémas, qui traitent de grands volumes de données semi-structurées ou qui requièrent une évolutivité horizontale. Les exemples incluent l'analyse en temps réel, la gestion de contenu et la télémétrie IoT.

### Que signifie la conformité ACID et quelles sont les bases de données qui la prennent en charge ?

**ACID signifie Atomicité, Cohérence, Isolation et Durabilité. MySQL est entièrement compatible ACID par défaut. MongoDB prend en charge les transactions ACID au niveau des documents et les transactions multi-documents dans les versions récentes, mais sa force principale réside dans la flexibilité et la cohérence de la mise à l'échelle.**

### Comment MySQL et MongoDB gèrent-ils la sécurité et la conformité ?

**Tous deux prennent en charge le contrôle d'accès basé sur les rôles, le cryptage TLS/SSL en transit et le cryptage au repos (InnoDB TDE dans MySQL ; cryptage WiredTiger dans MongoDB). Chacun d'entre eux propose des enregistrements d'audit et des intégrations pour répondre à des normes telles que HIPAA et GDPR.**

### MySQL et MongoDB peuvent-ils être utilisés ensemble ?

**Oui, de nombreuses architectures les combinent : MySQL traite les données transactionnelles et structurées, tandis que MongoDB gère les charges de travail flexibles, volumineuses ou géo-distribuées, chacune jouant sur ses points forts.**

Mark Pedigo, PhD, est un éminent scientifique des données, spécialisé dans la science des données de santé, la programmation et l'éducation. Titulaire d'un doctorat en mathématiques, d'une licence en informatique et d'un certificat professionnel en intelligence artificielle, Mark allie connaissances techniques et résolution de problèmes pratiques. Au cours de sa carrière, il a joué un rôle dans la détection des fraudes, la prédiction de la mortalité infantile et les prévisions financières, et a contribué au logiciel d'estimation des coûts de la NASA. En tant qu'éducateur, il a enseigné à DataCamp et à l'université Washington de St. Louis et a encadré des programmeurs juniors. Pendant son temps libre, Mark profite de la nature du Minnesota avec sa femme Mandy et son chien Harley, et joue du piano jazz.