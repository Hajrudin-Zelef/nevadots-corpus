Cours

Vous avez sans doute remarqué que MySQL apparaît dans quasiment toutes les fiches de poste liées aux bases de données. Et pour cause : MySQL fait tourner une multitude de services, des réseaux sociaux que vous consultez aux applications que vous utilisez au quotidien.

J'ai réuni dans ce guide les questions d'entretien MySQL les plus fréquentes. Nous passerons en revue l'essentiel — des notions que tout junior doit maîtriser — jusqu'aux sujets plus avancés attendus sur des postes seniors. Je partagerai également des conseils pour vous présenter en candidat(e) sûr(e) de soi lors de vos prochains entretiens autour de la donnée.

## Qu'est-ce que MySQL ?

MySQL est un SGBDR open source (système de gestion de bases de données relationnelles) fondé sur SQL, qui organise les données en tables structurées. Il est développé par Oracle Corporation.

Il s'est classé comme le SGBD open source le plus populaire en 2024. Cependant, l'enquête Stack Overflow Developer de 2025 a montré que PostgreSQL est devenu la base la plus utilisée par les développeurs professionnels, dépassant MySQL pour la première fois.

Cela ne veut pas dire que MySQL a perdu de sa superbe : il reste extrêmement populaire — 40,5 % d'utilisation chez les développeurs en 2025 — et alimente toujours d'innombrables applications web, CMS et outils d'entreprise. Et si vous travaillez sur des applications web ou la pile LAMP, MySQL demeure une compétence de premier plan.

En 2024, MySQL était le SGBD open source le plus populaire au monde, avec un score de 1061. Source : Statista.

## Questions MySQL de base

Lors des premiers échanges, l'intervieweur peut poser des questions fondamentales pour évaluer votre compréhension des concepts de base des bases de données et de MySQL.

### 1. Qu'est-ce qu'une base de données et en quoi diffère-t-elle d'un SGBD ?

Une base de données est un conteneur de stockage qui conserve des données auxquelles on peut accéder, que l'on peut modifier et analyser. Par exemple, les plateformes sociales stockent dans des bases de données qui a aimé nos publications.

Un SGBD (Système de gestion de base de données) est le logiciel qui permet d'interagir avec ces données et de les administrer, par exemple en créant des utilisateurs et en gérant leurs droits. MySQL est l'un des SGBD les plus populaires. D'autres exemples incluent PostgreSQL, MongoDB, et Microsoft SQL Server.

### 2. En quoi MySQL diffère-t-il des autres systèmes de gestion de bases de données relationnelles ?

MySQL est un SGBDR open source qui utilise SQL pour gérer les données. Il est réputé pour sa simplicité, sa rapidité et sa compatibilité avec les applications web.

Voici en quoi MySQL se distingue :

- Simplicité et performances : MySQL est souvent salué pour sa facilité d'utilisation et ses performances optimisées, ce qui en fait un choix privilégié des développeurs web et des startups.
- Fonctionnalités avancées : Bien que MySQL excelle en simplicité, il peut manquer de certaines fonctionnalités avancées présentes dans d'autres SGBDR comme PostgreSQL, par exemple un support plus complet des transactions ACID, des indexations avancées et un éventail plus large de types de données.
- Moteurs de stockage : MySQL permet de choisir différents moteurs de stockage (ex. : InnoDB, MyISAM) pour les tables, offrant de la flexibilité selon les cas d'usage.

MySQL est idéal pour les scénarios nécessitant vitesse et montée en charge. Pour des besoins plus complexes et des fonctionnalités "entreprise", PostgreSQL peut être un meilleur choix.

### 3. Quels sont les principaux types de données disponibles dans MySQL ?

MySQL prend en charge une variété de types de données, regroupés en :

- 
Numériques : `INT` ,`DECIMAL` ,`FLOAT` ,`DOUBLE` , etc.
- 
**Chaînes :**`CHAR` ,`VARCHAR` ,`TEXT` ,`BLOB` .
- 
**Date/heure :**`DATE` ,`DATETIME` ,`TIMESTAMP` ,`TIME` .
- 
**JSON :** pour stocker des objets JSON.

### 4. Quelle est la différence entre les types de données INT et DECIMAL ?

`INT` stocke des nombres entiers sans décimales. On l'utilise lorsqu'il n'y a pas besoin de fractions. À l'inverse, `DECIMAL` permet de stocker des valeurs financières et convient aux calculs précis avec décimales. 

### 5. En quoi DATE diffère-t-il de DATETIME dans MySQL ?

Le type `DATE` dans MySQL stocke une date au format année, mois, jour : 

`YYYY-MM-DD`

Le type `DATETIME` stocke la date avec l'heure, au format : 

`YYYY-MM-DD HH:MM:SS` 

### 6. Qu'est-ce qu'une clé étrangère et comment l'utiliser en base ?

Une clé étrangère est un champ d'une table qui fait référence à la clé primaire d'une autre table.

Par exemple, dans une table `customers` qui stocke les informations clients, chaque client a un `customer_id` unique. Dans une autre table appelée `transactions` (qui enregistre les achats), on utilise `customer_id` comme clé étrangère. Le `customer_id` de la table transactions lie chaque achat à un client précis de la table `customers` .

Voici à quoi cela ressemble en SQL :

```
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY,
    customer_id INT,
    amount DECIMAL(10,2),
    date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```
### 7. Quelles sont les différences entre INNER JOIN, LEFT JOIN, RIGHT JOIN et FULL JOIN ?

Les jointures combinent des lignes de deux tables ou plus à partir de colonnes liées. Leurs différences :

- 
INNER JOIN : retourne uniquement les lignes ayant une correspondance dans les deux tables.
- 
LEFT JOIN : retourne toutes les lignes de la table de gauche et les lignes correspondantes de la table de droite. S'il n'y a pas de correspondance, les colonnes de la table de droite valent `NULL` .
- 
RIGHT JOIN : similaire au `LEFT JOIN` , mais retourne toutes les lignes de la table de droite et les correspondances de la gauche.
- 
**FULL JOIN** : combine les résultats de`LEFT JOIN` et`RIGHT JOIN` , en incluant les lignes sans correspondance des deux côtés.**Remarque :** MySQL ne prend pas en charge nativement la syntaxe`FULL JOIN` . Pour obtenir le même résultat, utilisez un`UNION` d'un`LEFT JOIN` et d'un`RIGHT JOIN`

### 8. Quelle est la différence entre DELETE, TRUNCATE et DROP dans MySQL ?

Les commandes `DELETE`, `TRUNCATE` et `DROP` peuvent paraître similaires, mais leur comportement diffère :

DELETE : supprime des lignes d'une table selon une condition. Peut être annulé si exécuté dans une transaction. Exemple :

`DELETE FROM employees WHERE department_id = 5;`
**TRUNCATE : supprime toutes les lignes d'une table, mais la structure reste intacte. Plus rapide que `DELETE` et non annulable. Exemple :**

`TRUNCATE TABLE employees;`
DROP : supprime complètement la table (structure et données), ainsi que ses dépendances (index, etc.). Exemple :

`DROP TABLE employees;`
### 9. Comment créer et modifier une table dans MySQL ? Donnez des exemples.

Pour créer des tables, utilisez l'instruction `CREATE TABLE`, et pour les modifier, le plus souvent `ALTER TABLE`. Exemples :

Création de table :

`CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50), department VARCHAR(50));`
Ajout d'une colonne :

`ALTER TABLE employees ADD COLUMN salary DECIMAL(10, 2);`
### 10. Qu'est-ce qu'une table temporaire en SQL ?

Une table temporaire n'existe que pendant la session en cours. Une fois la session fermée, la table est supprimée. Elle sert à stocker des résultats intermédiaires, par exemple pour tester, filtrer ou préparer des données avant de les insérer dans une table permanente.

Exemple :

```
CREATE TEMPORARY TABLE temp_employees (
    id INT,
    name VARCHAR(50)
);
INSERT INTO temp_employees VALUES (1, 'John Doe');
SELECT * FROM temp_employees;
```
### 11. Qu'est-ce qu'une sous-requête en MySQL ? Expliquez avec un exemple.

Une sous-requête (ou requête imbriquée) est une requête incluse dans une autre. Elle permet de décomposer des opérations complexes en étapes plus simples. Par exemple, pour trouver les employés gagnant au-dessus de la moyenne :

```
SELECT first_name, last_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```
Décomposition :

1. 
La requête interne `SELECT AVG(salary) FROM employees` calcule d'abord le salaire moyen.
2. 
La requête externe utilise ensuite cette moyenne pour trouver les employés qui la dépassent.

### 12. Comment utiliser l'instruction INSERT pour ajouter des données à une table ? Des bonnes pratiques ?

On utilise l'instruction `INSERT` pour ajouter des données. La syntaxe de base :

```
INSERT INTO table_name (column1, column2, ...) 
VALUES (value1, value2, ...); 
```
Quelques bonnes pratiques à suivre avec `INSERT` :

1. 
Lister explicitement les colonnes. Le code est plus clair et reste robuste si la structure évolue.
2. 
Pour les colonnes `AUTO_INCREMENT` (comme les IDs), ne pas les inclure dans l'`INSERT` . MySQL les gère pour éviter les doublons.
3. 
Être cohérent dans l'usage des guillemets pour les chaînes. Les simples quotes sont souvent préférées, mais les deux fonctionnent.
4. 
Pour insérer plusieurs lignes, privilégier une seule instruction pour de meilleures performances.

### 13. À quoi sert l'attribut AUTO_INCREMENT dans MySQL ?

L'attribut `AUTO_INCREMENT` génère des nombres uniques et séquentiels pour une colonne, en général la clé primaire.

Exemple de création d'une table avec une colonne `AUTO_INCREMENT` :

```
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50)
);
```
Et insertion de lignes :

```
INSERT INTO employees (name, department) VALUES ('John Doe', 'Sales');
INSERT INTO employees (name, department) VALUES ('Jane Smith', 'Marketing');
```
### 14. Qu'est-ce qu'une vue dans MySQL ?

Une vue est une requête enregistrée qui se comporte comme une table virtuelle. Elle permet d'encapsuler une requête complexe sous un nom et de la réutiliser comme une table. Ainsi, vous n'avez pas à retaper la requête entière à chaque fois.

Par exemple, pour simplifier l'interrogation des employés avec le nom de leur service, vous pouvez créer une vue :

```
CREATE VIEW employee_details AS
SELECT 
    e.id,
    e.name,
    d.department_name,
    e.salary
FROM 
    employees e
JOIN 
    departments d ON e.department_id = d.department_id;
```
Vous pouvez ensuite interroger la vue `employee_details` comme une table :

`SELECT * FROM employee_details;`
En revanche, l'insertion et la mise à jour via des vues sont limitées. La plupart sont en lecture seule et masquent l'accès direct aux tables, renforçant ainsi la sécurité. Les vues peuvent aussi ralentir certaines requêtes, car la requête sous-jacente est exécutée à chaque accès.

## Amélioration de SQL pour les débutants

## Questions MySQL intermédiaires

Dans cette section, nous abordons des sujets de niveau intermédiaire. Ces questions servent principalement à évaluer votre connaissance des types de données et de la structure MySQL.

### 15. Que sont les tables versionnées par le système et comment fonctionnent-elles ?

Les tables versionnées par le système conservent l'historique complet des modifications d'une table. Comme elles gardent les versions précédentes de chaque ligne, on peut les utiliser pour l'audit et la récupération de données.

Elles fonctionnent en ajoutant deux colonnes supplémentaires — `StartTime` et `EndTime` — qui enregistrent la période de validité de chaque ligne. Lors des insertions, mises à jour ou suppressions, ces horodatages sont ajustés :

- 
Insertion : une nouvelle ligne est ajoutée avec un `StartTime` défini à l'horodatage courant et un`EndTime` à`9999-12-31 23:59:59` — la valeur`DATETIME` maximale de MySQL, utilisée comme sentinelle pour indiquer "ligne actuellement active".
- 
Mise à jour : le `EndTime` de la ligne d'origine est mis à l'horodatage courant pour la marquer comme non valide, puis une nouvelle ligne mise à jour est créée avec`StartTime` à l'instant courant et`EndTime` "infini".
- 
Suppression : le `EndTime` de la ligne existante est mis à l'horodatage courant, indiquant qu'elle n'est plus valide.

Avec la clause `FOR SYSTEM_TIME` de SQL, vous pouvez interroger l'état de la table à un instant donné ou sur une période. Par exemple :

- 
`FOR SYSTEM_TIME AS OF '2024-01-01'` : récupère l'état de la table tel qu'il était le 1er janvier 2024.
- 
`FOR SYSTEM_TIME BETWEEN '2024-01-01' AND '2024-12-31'` : affiche toutes les lignes valides sur cet intervalle.

### 16. Qu'est-ce qu'une transaction MySQL et comment l'utiliser ?

Une transaction est un ensemble d'opérations exécutées comme une unité. Elle garantit l'intégrité des données en imposant que toutes les opérations réussissent ou échouent ensemble.

Exemple d'utilisation :

```
START TRANSACTION;
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
COMMIT; -- Enregistre définitivement
-- ou
ROLLBACK; -- Annule les changements
```
### 17. Qu'est-ce qu'une contrainte par défaut dans MySQL ? Comment définir une valeur par défaut pour une colonne ?

Une contrainte par défaut attribue une valeur à une colonne quand aucune valeur explicite n'est fournie lors d'un `INSERT`. Elle garantit la validité de la colonne si l'utilisateur l'omet à la saisie.

Exemple de création avec valeur par défaut :

```
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    status VARCHAR(10) DEFAULT 'active'
);
```
Vous pouvez ensuite insérer une ligne sans préciser `status` :

`INSERT INTO employees (name) VALUES ('John Doe');`
Cette approche réduit le risque de `NULL` ou de données invalides sur des colonnes critiques et simplifie les requêtes en évitant de gérer les cas par défaut dans le code.

| **Champ** | **Type** | **Null** | **Clé** | **Défaut** | **Extra** | 
| **id** | INT | NO | PRI | NULL | AUTO_INCREMENT | 
| **name** | VARCHAR(50) | YES |  | NULL |  | 
| **status** | VARCHAR(10) | YES |  | active |  | 

Cette commande est utile car :

- Elle aide les développeurs à comprendre le schéma avant d'écrire des requêtes.
- Elle sert au débogage, notamment sur des bases méconnues.
- Elle permet d'identifier rapidement les contraintes, comme les clés primaires ou les valeurs par défaut.

### 18. Quelle est la différence entre CHAR et VARCHAR dans MySQL ?

Les deux stockent des chaînes, mais diffèrent dans la gestion de l'espace :

- 
`CHAR(n)` stocke toujours exactement`n` caractères, en complétant avec des espaces si besoin. Longueur fixe, ce qui peut être légèrement plus rapide pour des colonnes de longueur uniforme (codes pays, statuts, etc.).
- 
`VARCHAR(n)` ne stocke que les caractères saisis, jusqu'à`n` . Il économise de l'espace pour les données à longueur variable, avec un léger surcoût de gestion de longueur.

```
CREATE TABLE example (
    country_code CHAR(2),      -- Toujours 2 caractères, ex. 'US', 'UK'
    email VARCHAR(255)         -- Longueur variable jusqu'à 255
);
```
Règle pratique : utilisez `CHAR` pour les valeurs à longueur fixe, `VARCHAR` pour le reste.

### 19. Comment utiliser les fonctions de chaîne en SQL pour traiter du texte ?

Différentes fonctions de chaîne aident à manipuler des noms et autres textes. Par exemple :

- 
La fonction `LENGTH()` affiche le nombre de caractères d'un nom.
- 
`UPPER()` et`LOWER()` convertissent le texte en majuscules ou minuscules.
- 
`CONCAT()` assemble le prénom et le nom dans une même colonne.
- 
`SUBSTRING()` extrait des parties spécifiques d'un texte. Par exemple, isoler le mois à partir d'une date de naissance.

Exemple de requête :

```
SELECT 
    UPPER(first_name) AS upper_name,
    CONCAT(first_name, ' ', last_name) AS full_name,
    SUBSTRING(birthdate, 6, 2) AS birth_month,
    TRIM(last_name) AS trimmed_last_name,
    REPLACE(first_name, 'a', '@') AS replaced_name
FROM employees;
```
Cette requête :

- 
Convertit les prénoms en majuscules.
- 
Combine prénom et nom en un nom complet.
- 
Extrait le mois de la colonne `birthdate` .
- 
Supprime les espaces superflus des noms.
- 
Remplace toutes les occurrences de "a" par "@" dans les prénoms.

### 20. Comment mettre à jour une ligne précise en SQL ?

Utilisez l'instruction `UPDATE` avec une clause `WHERE` pour identifier l'enregistrement à modifier. 

Par exemple, pour mettre à jour le genre du film « Inception » (2010) en « Sci-Fi » :

```
UPDATE movies
SET genre = 'Sci-Fi'
WHERE movie_title = 'Inception' AND year = 2010;
```
Ici, `UPDATE movies` précise la table visée, et la clause `WHERE` cible la ligne où le titre est « Inception » et l'année « 2010 ». 

## Questions MySQL avancées

Les questions avancées évaluent votre capacité à gérer des scénarios MySQL complexes et donnent un aperçu de votre façon de décider.

### 21. Qu'est-ce qu'un trigger dans MySQL ? Comment l'implémenter ?

Un trigger (déclencheur) est un ensemble d'actions exécutées lorsqu'un événement survient en base. Les triggers peuvent s'exécuter avant ou après des événements comme `INSERT`, `UPDATE` ou `DELETE`. 

Par exemple, supposons une table `orders` où sont ajoutées de nouvelles commandes. Nous pouvons créer un trigger qui journalise chaque commande dans une table `order_history` :

```
CREATE TRIGGER after_order_insert
AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    INSERT INTO order_history (order_id, action, timestamp)
    VALUES (NEW.order_id, 'inserted', NOW());
END;
```
Après l'exécution du trigger, la table `order_history` est automatiquement mise à jour :

| **history_id** | **order_id** | **action** | **timestamp** | 
| **1** | 1 | inserted | 2024-12-24 10:00:00 | 
| **2** | 2 | inserted | 2024-12-24 11:00:00 | 

### 22. Pourquoi l'ajout d'un index accélère-t-il les requêtes SQL ?

Sans index, la base doit scanner chaque ligne pour trouver une entrée. Un index s'apparente à une table des matières, permettant d'accéder directement aux lignes pertinentes. L'ajout d'un index réduit donc le temps de recherche et accélère les requêtes.

Les index s'appuient généralement sur des structures de données comme les B-arbres (B-trees) ou des tables de hachage, qui facilitent recherches, lookups et parcours par plage.

Exemple de création d'index :

```
-- Sans index :
SELECT * FROM employees WHERE last_name = 'Smith';
-- Ajout d'un index sur la colonne last_name :
CREATE INDEX idx_last_name ON employees(last_name);
-- Avec l'index, la base localise rapidement les lignes portant 'Smith' en last_name.
```
Les index ont aussi des inconvénients :

- 
Écritures ralenties : les opérations `INSERT` ,`UPDATE` et`DELETE` sont plus lentes car l'index doit être mis à jour à chaque changement.
- 
Coût de stockage : les index consomment de l'espace disque supplémentaire.

### 23. Quels types de données utiliser pour le poids et le prix d'un produit, et pourquoi ?

Pour le poids, `DECIMAL` est en général le choix le plus sûr. `FLOAT` et `REAL` stockent des décimales mais utilisent l'arithmétique en virgule flottante, source de petits écarts d'arrondi.

Pour des poids nécessitant de la précision (expédition, inventaire), `DECIMAL(8, 3)` offre un contrôle précis avec 3 décimales sans mauvaises surprises d'arrondi. `FLOAT` n'est acceptable que si une petite marge d'erreur est tolérée.

### 24. Comment trouver des doublons en SQL avec une fonction fenêtre ?

Voici comment repérer des doublons avec la fonction `ROW_NUMBER()` :

```
WITH DuplicateCheck AS (
    SELECT product_name, 
           category,
           ROW_NUMBER() OVER(
               PARTITION BY product_name, category 
               ORDER BY id
           ) AS row_num
    FROM sales
)
SELECT *
FROM DuplicateCheck
WHERE row_num > 1;
```
Explication :

1. `ROW_NUMBER()` numérote chaque ligne du résultat. 

2. `PARTITION BY` regroupe par `product_name` et `category`. 

3. Dans chaque groupe, la numérotation commence à 1.

4. Toute ligne avec `row_num` > 1 est un doublon. 

Par exemple, pour ces enregistrements :

- Product A, Category X, row_num = 1
- Product A, Category X, row_num = 2 (doublon)
- Product B, Category Y, row_num = 1

La requête retournera la deuxième ligne, car son `row_num` est supérieur à 1.

### 25. Comment créer et utiliser une procédure stockée avec paramètres dans MySQL ? Donnez un exemple.

Les procédures stockées permettent d'enregistrer et réutiliser des requêtes complexes pour rendre les opérations plus efficaces et maintenables. Voici un exemple avec paramètres.

Supposons une base d'étudiants et la nécessité de filtrer par âge. Créons une procédure prenant un paramètre d'âge :

D'abord, une procédure simple avec paramètre d'entrée :

```
CREATE PROCEDURE get_student_info(IN age INT)
BEGIN
    SELECT * FROM student WHERE student.age = age;
END;
```
Pour l'exécuter, on la `CALL` avec l'âge souhaité :

`CALL get_student_info(21);`
On peut aller plus loin avec des paramètres de sortie. Par exemple, compter les étudiants d'un âge donné :

```
CREATE PROCEDURE count_students_by_age(IN age INT, OUT student_count INT)
BEGIN
    SELECT COUNT(*) INTO student_count FROM students WHERE students.age = age;
END;
```
Pour récupérer le résultat :

```
SET @count = 0;
CALL count_students_by_age(21, @count);
SELECT @count AS total_students;
```
### 26. Pourquoi l'intégrité référentielle est-elle importante en base ?

L'intégrité référentielle assure la cohérence des relations entre tables. La clé étrangère garantit que les valeurs d'une table correspondent à une valeur unique de la table référencée.

Exemple concret : vous gérez une base e-commerce avec une table `Customers` et une table `Orders`. Chaque commande doit appartenir à un client réel. L'intégrité référentielle, via les clés étrangères, s'assure que :

- On ne peut pas créer une commande pour un client inexistant.
- On ne peut pas supprimer un client ayant des commandes (sauf règle explicite sur le devenir de ces commandes).
- On ne peut pas modifier l'ID d'un client ayant des commandes existantes.

Ainsi, en créant une contrainte de clé étrangère :

```
ALTER TABLE Orders
ADD FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID);
```
La base applique automatiquement ces règles :

- 
Chaque `CustomerID` dans`Orders` doit exister dans`Customers` .
- 
Toute tentative de violation (insertion d'un `CustomerID` invalide) est rejetée.

Cela évite des incohérences critiques, telles que des commandes impossibles à rattacher à des clients, ou des rapports lacunaires.

## Questions MySQL pour administrateurs de bases de données

Si vous postulez spécifiquement à un poste d'administrateur de bases de données, voici des questions possibles.

### 27. Pourquoi une application à grande échelle recourt-elle au sharding ? Quels sont les défis associés ?

Le sharding permet de répartir un gros volume de données sur plusieurs serveurs. Chaque shard contient une partie des données. En répartissant la charge, on évite du matériel haut de gamme. La vitesse et l'évolutivité s'améliorent, mais il existe des défis :

- Certaines requêtes comme les jointures deviennent difficiles, ce qui complique la gestion des données.
- Avec la croissance, des shards peuvent devenir saturés, créant des points chauds qui dégradent les performances.

### 28. Expliquez le rôle des redo logs dans la récupération après crash MySQL.

À chaque modification, MySQL doit écrire sur le disque. Écrire directement dans les fichiers de données est lent et risqué. C'est pourquoi, avant de modifier ces fichiers, MySQL écrit d'abord ce qu'il va faire dans le redo log. C'est plus sûr que de mettre à jour les fichiers de données à la volée.

Par exemple, vous mettez à jour l'adresse d'un client :

1. MySQL écrit d'abord ce changement dans le redo log.
2. Il confirme ensuite votre transaction comme validée.
3. Enfin, il applique la modification aux fichiers de données.

La récupération après crash intervient si MySQL tombe après l'étape 1 ou 2 mais avant la 3. Au redémarrage, MySQL lit ses redo logs puis termine le travail en rejouant les changements. Cela garantit qu'une transaction validée n'est pas perdue, même en cas de panne au mauvais moment.

### 29. Quels sont les moteurs de stockage disponibles dans MySQL et en quoi diffèrent-ils ?

MySQL prend en charge plusieurs moteurs de stockage, chacun optimisé pour des besoins différents. Voici une comparaison des plus courants :

| Moteur de stockage | Fonctionnalités clés | Idéal pour | 
|---|---|---|
| **InnoDB** | Moteur par défaut. Conforme ACID, verrous au niveau ligne, transactions et clés étrangères. | E-commerce, systèmes financiers, tout ce qui requiert l'intégrité des données. | 
| **MyISAM** | Lectures rapides, verrous au niveau table. Pas de transactions ni de clés étrangères. | Applications à forte lecture où la vitesse prime sur l'intégrité. | 
| **Memory** | Données en RAM. Extrêmement rapide mais perdues au redémarrage. | Cache, gestion de sessions, données temporaires. | 
| **CSV** | Stocke les données en fichiers CSV. Pas d'indexation. | Échanges de données entre applis ou stockage plat simple. | 
| **Archive** | Forte compression. INSERT et SELECT uniquement. Pas d'index. | Journaux ou historiques rarement interrogés. | 
| **NDB (Clustered)** | Stockage distribué, haute disponibilité, tolérance aux pannes, transactions prises en charge. | Applications distribuées à grande échelle nécessitant du temps réel. | 

### 30. Comment définir un moteur de stockage par défaut dans MySQL ?

Commencez par vérifier le moteur par défaut actuel :

`SHOW ENGINES;`
InnoDB est recommandé comme moteur par défaut, car il prend en charge des fonctionnalités clés :

- Transactions conformes ACID
- Contraintes de clés étrangères
- Récupération après crash
- Verrouillage au niveau ligne

Pour changer temporairement le moteur par défaut sur votre session :

`SET default_storage_engine = 'InnoDB';`
Pour un changement permanent, modifiez le fichier de configuration MySQL en ajoutant la ligne suivante sous la section `[mysqld]` :

`default-storage-engine = InnoDB`
### 31. Comment réparer des tables corrompues dans MySQL ?

Commencez par vérifier toutes les bases de données avec la commande suivante :

`mysqlcheck --check --all-databases -u root -p`
Elle scanne les tables et signale toute corruption. Vous pouvez ensuite lancer la réparation :

`mysqlcheck --repair database_name table_name -u root -p`
Attention : en cas de corruption sévère, des pertes de données sont possibles. Pensez à sauvegarder avant.

## Questions MySQL scénarisées et de résolution de problèmes

Ces questions évaluent votre expérience sur des scénarios complexes réels et votre sens de la résolution de problèmes.

### 32. Décrivez un scénario où vous avez utilisé des sous-requêtes en MySQL.

Voici un exemple de réponse :

Dans mon précédent poste, je gérais la base d'une boutique e-commerce et devais préparer un rapport produits. L'objectif était d'identifier les produits générant des ventes supérieures à la moyenne, ce qui impliquait une analyse en plusieurs étapes via des sous-requêtes.

Voici la requête SQL que j'ai conçue :

```
SELECT 
    p.product_id,
    p.product_name,
    s.sales_amount
FROM products p
JOIN sales s ON p.product_id = s.product_id
WHERE s.sales_amount > (
    SELECT AVG(sales_amount)
    FROM sales
)
ORDER BY s.sales_amount DESC;
```
J'ai d'abord établi un seuil en calculant la vente moyenne sur l'ensemble des produits via une sous-requête dans la clause `WHERE`. Ce seuil dynamique servait de référence pour évaluer chaque produit.

La requête principale joignait ensuite les tables produits et ventes afin d'obtenir les informations utiles, tandis que la clause `WHERE` excluait les produits sous la moyenne. 

Structurée ainsi, la requête permettait d'identifier les meilleurs produits en une seule opération.

### 33. Expliquez une situation où vous avez combiné des données de plusieurs tables avec des jointures SQL.

Exemple de réponse :

Récemment, j'ai travaillé sur un projet avec deux tables principales — l'une contenant les ventes produits et l'autre les détails produits. Ma mission : créer un rapport avec `sales`, `product name`, `category` et `price`.

Pour combiner les données, j'ai utilisé un INNER JOIN sur la colonne commune `product_id` afin de relier transactions et détails produits :

```
SELECT 
    s.sales_date,
    p.product_name,
    p.category,
    s.quantity_sold,
    p.price
FROM 
    sales s
INNER JOIN 
    products p
ON 
    s.product_id = p.product_id;
```
Le rapport offrait une vision claire des tendances de vente, aidant les parties prenantes à identifier les catégories performantes et celles à améliorer.

### 34. Avez-vous de l'expérience avec les triggers ? Décrivez votre usage.

Voici un exemple de réponse :

Oui, j'ai une solide expérience des triggers. Dans mon dernier poste, j'ai mis en place un trigger `AFTER UPDATE` pour l'audit des changements de prix. 

Concrètement, j'ai créé un trigger qui enregistre automatiquement l'historique dès qu'un prix produit change. Voici le script SQL :

```
CREATE TRIGGER tr_AuditPriceChanges
AFTER UPDATE ON Products
FOR EACH ROW
BEGIN
    -- Journaliser uniquement si le prix a réellement changé
    IF OLD.UnitPrice <> NEW.UnitPrice THEN
        INSERT INTO PriceAudit (
            ProductID,
            OldPrice,
            NewPrice,
            ChangedBy,
            ChangeDate,
            PercentageChange
        )
        VALUES (
            NEW.ProductID,
            OLD.UnitPrice,
            NEW.UnitPrice,
            CURRENT_USER(),
            NOW(),
            ROUND(((NEW.UnitPrice - OLD.UnitPrice) / OLD.UnitPrice * 100), 2)
        );
    END IF;
END;
```
Les points forts de cette solution :

1. Le trigger ne se déclenche que si le prix a réellement changé.
2. Il capture l'utilisateur à l'origine du changement via `SYSTEM_USER` .
3. Il calcule le pourcentage d'évolution pour le reporting.
4. Il inclut une condition pour ignorer les mises à jour sans changement de prix.

J'ai également ajouté de la gestion d'erreurs et du logging après avoir identifié des cas limites avec des prix `NULL`.

## Conseils pour préparer un entretien MySQL

Si vous débutez, voici quelques conseils pour réussir vos prochains entretiens :

Maîtrisez les fondamentaux MySQL : Apprenez les fondamentaux des bases de données comme l'indexation, les transactions et l'optimiseur de requêtes. Comprenez comment MySQL traite les requêtes et gère le stockage. Vous écrirez des requêtes plus efficaces et saurez justifier vos choix en entretien.

Pratiquez concrètement : Installez MySQL sur votre ordinateur et entraînez-vous régulièrement. Créez des bases de test, écrivez différents types de requêtes et essayez de les optimiser. La pratique réelle est le meilleur moyen de comprendre et de gagner en confiance.

Pour aller plus loin, explorez les ressources DataCamp :

- Pour une intro à SQL : Introduction to SQL Course
- Pour s'entraîner : Applying SQL to real-world problems

Familiarisez-vous avec les outils et intégrations MySQL : découvrez MySQL Workbench et d'autres outils d'administration et de supervision. Vous pouvez aussi explorer comment MySQL s'intègre avec Python et des frameworks pertinents, afin de montrer votre aisance en environnement de développement réel.

## Conclusion

Voilà ! Nous avons parcouru 34 questions d'entretien MySQL pour vous aider à décrocher votre prochain poste. Que vous visiez un rôle junior ou un poste avancé d'administrateur de données, vous devez maîtriser les fondamentaux MySQL, l'optimisation des requêtes et l'administration pour vous démarquer.

**Pour approfondir vos connaissances d'autres SGBD, découvrez** les cours SQL de DataCamp.

## Devenez certifié SQL

Je suis un stratège du contenu qui aime simplifier les sujets complexes. J'ai aidé des entreprises comme Splunk, Hackernoon et Tiiny Host à créer un contenu attrayant et informatif pour leur public.