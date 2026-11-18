# Guide SQL complet — du SELECT à l'optimisation

**Public :** Zelef, chef de service Systèmes & Énergies, sysadmin.
**Objectif :** maîtriser SQL au quotidien (requêtes d'exploitation, reporting, dépannage),
sans installation lourde : on travaille sur les bases **déjà en place** (PostgreSQL, MySQL/MariaDB).
**Portabilité :** tout le SQL présenté est portable ; les différences MySQL/MariaDB ↔ PostgreSQL
sont signalées dans des encadrés `⚠️ Portabilité`.

**Tables de référence utilisées dans tout le guide** (entreprise fictive "SETIS", service maintenance) :

- `employes` — les techniciens et agents du service
- `equipements` — parc : onduleurs, copieurs, serveurs, commutateurs
- `interventions` — bons d'intervention (pannes, préventif)
- `onduleurs` — détail technique des UPS suivis

> Convention du guide : les mots-clés SQL en MAJUSCULES, les identifiants en minuscules_snake_case,
> les chaînes entre apostrophes simples `'texte'`, les dates au format ISO `YYYY-MM-DD`.

---

## 1. Pourquoi ce guide et comment l'utiliser

SQL est le langage commun de toutes les bases relationnelles : inventorié en 1974, normalisé
(ISO/IEC 9075), il n'a quasiment pas changé dans ses fondamentaux. Pour un sysadmin/chef de service,
SQL sert tous les jours :

- **Exploitation** : interroger GLPI, Zabbix, l'inventaire, les logs applicatifs stockés en base.
- **Reporting** : nombre d'interventions par mois, MTBF des onduleurs, SLA du service.
- **Dépannage** : trouver la requête qui rame, vérifier une incohérence de données.
- **Sauvegarde/audit** : contrôler ce qui est sauvegardé, extraire des jeux de données.

Méthode de lecture conseillée :

1. Lire les sections 1 à 13 dans l'ordre (socle).
2. Pratiquer chaque exemple sur une base de test (voir section 3).
3. Revenir aux sections 60+ (optimisation, pièges) dès qu'une requête réelle coince.
4. Garder la section 76 (pense-bête) imprimée ou en favori.

⚠️ **Portabilité** — 95 % du guide est du SQL standard. Quand une fonction ou syntaxe diffère,
le guide donne les deux variantes avec une mention explicite du SGBD.

Checklist de démarrage :

- [ ] Identifier les bases accessibles (PostgreSQL ? MariaDB ? SQLite locale ?)
- [ ] Obtenir un accès **lecture seule** pour s'entraîner sans risque
- [ ] Créer la base de test `sql_formation` et y charger les tables de la section 3
- [ ] Installer un client : `psql` (PostgreSQL), `mysql` (MariaDB), ou DBeaver (graphique, multi-SGBD)

---

## 2. Les 5 familles d'ordres SQL

| Famille | Nom | Rôle | Exemples |
|---|---|---|---|
| DQL | Data Query Language | Interroger | `SELECT` |
| DML | Data Manipulation Language | Modifier les données | `INSERT`, `UPDATE`, `DELETE` |
| DDL | Data Definition Language | Définir les structures | `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE` |
| DCL | Data Control Language | Gérer les droits | `GRANT`, `REVOKE` |
| TCL | Transaction Control Language | Gérer les transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

Ce guide couvre surtout DQL (sections 4–41), puis DDL (42–48), DML (49–53),
TCL (54–55), et les sujets transverses (index, sécurité, sauvegarde, optimisation).

Point crucial pour un sysadmin : **savoir lire** du SQL est aussi important que savoir l'écrire.
Les sections 59 (EXPLAIN) et 69 (dépannage) sont là pour ça.

---

## 3. Jeu de données de référence (à créer une fois)

Tout le guide utilise les mêmes tables. Les créer **une fois** dans une base de test,
et tous les exemples deviennent exécutables.

```sql
-- Base de test (PostgreSQL)
CREATE DATABASE sql_formation;

-- MySQL/MariaDB : CREATE DATABASE sql_formation CHARACTER SET utf8mb4;
-- puis : USE sql_formation;
```

```sql
CREATE TABLE employes (
    id          INTEGER PRIMARY KEY,
    nom         VARCHAR(50)  NOT NULL,
    prenom      VARCHAR(50)  NOT NULL,
    poste       VARCHAR(60)  NOT NULL,
    service     VARCHAR(40)  NOT NULL DEFAULT 'Maintenance',
    salaire     NUMERIC(10,2),
    date_embauche DATE NOT NULL,
    manager_id  INTEGER REFERENCES employes(id),
    actif       BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE equipements (
    id            INTEGER PRIMARY KEY,
    reference     VARCHAR(30)  NOT NULL UNIQUE,
    type_equip    VARCHAR(30)  NOT NULL,   -- 'onduleur','copieur','serveur','switch'
    marque        VARCHAR(30)  NOT NULL,
    modele        VARCHAR(60)  NOT NULL,
    date_achat    DATE,
    garantie_jusqu DATE,
    localisation  VARCHAR(60),
    statut        VARCHAR(20)  NOT NULL DEFAULT 'en_service'
                  CHECK (statut IN ('en_service','en_panne','en_maintenance','reforme'))
);

CREATE TABLE interventions (
    id              INTEGER PRIMARY KEY,
    equipement_id   INTEGER NOT NULL REFERENCES equipements(id),
    technicien_id   INTEGER REFERENCES employes(id),
    date_ouverture  TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    date_cloture    TIMESTAMP,
    type_interv     VARCHAR(20) NOT NULL DEFAULT 'curatif'
                    CHECK (type_interv IN ('curatif','preventif','installation')),
    priorite        INTEGER NOT NULL DEFAULT 3 CHECK (priorite BETWEEN 1 AND 5),
    description     TEXT NOT NULL,
    cout_eur        NUMERIC(10,2) DEFAULT 0,
    sla_respecte    BOOLEAN
);

CREATE TABLE onduleurs (
    equipement_id   INTEGER PRIMARY KEY REFERENCES equipements(id),
    puissance_kva   NUMERIC(6,2)  NOT NULL,
    topologie       VARCHAR(10)   NOT NULL DEFAULT 'VFI'
                    CHECK (topologie IN ('VFI','VI','VFD')),
    nb_batteries    INTEGER NOT NULL DEFAULT 0,
    date_dernier_test DATE,
    autonomie_min    INTEGER
);
```

Jeu d'exemple (extrait — à insérer pour suivre les exemples) :

```sql
INSERT INTO employes (id, nom, prenom, poste, salaire, date_embauche, manager_id) VALUES
 (1, 'Zelef',   'Karim',  'Chef de service',      5200.00, '2015-03-01', NULL),
 (2, 'Diallo',  'Awa',    'Technicien energie',   3100.00, '2018-06-15', 1),
 (3, 'Ndiaye',  'Moussa', 'Technicien reseau',    2950.00, '2019-01-20', 1),
 (4, 'Sow',     'Fatou',  'Technicienne copieurs',2800.00, '2020-09-01', 1),
 (5, 'Kane',    'Ibrahima','Apprenti',            1400.00, '2024-02-01', 2);

INSERT INTO equipements (id, reference, type_equip, marque, modele, date_achat, localisation, statut) VALUES
 (101, 'UPS-001', 'onduleur', 'APC',     'Easy UPS 3S 40 kVA', '2021-05-10', 'Local TGBT',   'en_service'),
 (102, 'UPS-002', 'onduleur', 'Eaton',   '9PX 11 kVA',         '2019-11-03', 'Salle serveurs','en_panne'),
 (103, 'CPY-001', 'copieur',  'Kyocera', 'TASKalfa 4054ci',    '2022-02-14', 'Bureau compta', 'en_service'),
 (104, 'SRV-001', 'serveur',  'Dell',    'PowerEdge R450',     '2023-08-01', 'Salle serveurs','en_service'),
 (105, 'UPS-003', 'onduleur', 'APC',     'Easy UPS 3M 60 kVA', '2020-04-22', 'Local TGBT',   'en_maintenance');

INSERT INTO onduleurs (equipement_id, puissance_kva, topologie, nb_batteries, date_dernier_test, autonomie_min) VALUES
 (101, 40.00, 'VFI', 32, '2026-08-15', 18),
 (102, 11.00, 'VI',  12, '2025-12-01', 25),
 (105, 60.00, 'VFI', 40, '2026-09-01', 15);

INSERT INTO interventions (id, equipement_id, technicien_id, date_ouverture, date_cloture,
                           type_interv, priorite, description, cout_eur, sla_respecte) VALUES
 (1001, 102, 2, '2026-09-20 08:15:00', '2026-09-20 14:30:00', 'curatif',   1, 'Onduleur en défaut batterie, remplacement bloc 7', 850.00, TRUE),
 (1002, 103, 4, '2026-09-21 09:00:00', '2026-09-22 11:00:00', 'curatif',   3, 'Bourrage papier récurrent tiroir 2',                 120.00, TRUE),
 (1003, 105, 2, '2026-09-25 07:45:00', NULL,                 'preventif', 2, 'Test batteries + thermographie armoire',             0.00,   NULL),
 (1004, 101, 5, '2026-09-26 10:00:00', NULL,                 'curatif',   2, 'Alarme température ambiante local TGBT',             0.00,   NULL),
 (1005, 104, 3, '2026-09-10 14:00:00', '2026-09-10 16:00:00', 'installation','4','Mise en baie + câblage',                         300.00, TRUE);
```

> Astuce sysadmin : gardez ce script dans `~/workspace/sql/00_init.sql`. Le rejouer
> (DROP + CREATE) remet la base de démo dans un état propre avant chaque session d'entraînement.

---

## 4. SELECT — lire des données

Le `SELECT` est l'ordre le plus utilisé. Forme canonique :

```sql
SELECT colonne1, colonne2
FROM   table
WHERE  condition
ORDER  BY colonne1;
```

Exemples progressifs :

```sql
-- 1. Tout lire (à éviter en production sur de grosses tables !)
SELECT * FROM employes;

-- 2. Choisir les colonnes (toujours préférer la liste explicite)
SELECT nom, prenom, poste FROM employes;

-- 3. Renommer à l'affichage avec AS
SELECT nom AS "Nom", prenom AS "Prénom", salaire * 12 AS salaire_annuel
FROM employes;

-- 4. Constantes et calculs dans le SELECT
SELECT reference, modele, 'PARC-SETIS' AS inventaire, puissance_kva * 0.8 AS puissance_kw_utile
FROM equipements e
JOIN onduleurs o ON o.equipement_id = e.id;
```

Règles d'or :

- En exploitation, **ne jamais** faire `SELECT *` sur une table métier : on ramène des colonnes
  inutiles (parfois lourdes : TEXT, BYTEA), on casse les applis si une colonne est ajoutée,
  et on empêche l'optimiseur d'utiliser un index couvrant (section 58).
- `AS` est optionnel (`SELECT nom n FROM employes` fonctionne) mais l'écrire rend la requête lisible.
- Les alias avec espaces ou accents exigent des guillemets doubles `"Prénom"` (standard).
  ⚠️ MySQL accepte aussi les backticks `` `Prénom` `` — non portable, à éviter.

---

## 5. WHERE — filtrer les lignes

`WHERE` ne garde que les lignes vérifiant la condition. Les opérateurs de base :

```sql
-- Égalité / inégalité
SELECT * FROM employes WHERE service = 'Maintenance';
SELECT * FROM equipements WHERE statut <> 'reforme';   -- <> est le standard ; != accepté partout

-- Comparaisons numériques
SELECT nom, salaire FROM employes WHERE salaire >= 3000;

-- Dates : toujours au format ISO entre apostrophes
SELECT * FROM interventions WHERE date_ouverture >= '2026-09-01';

-- Plusieurs conditions : AND a priorité sur OR → parenthèses obligatoires
SELECT * FROM interventions
WHERE type_interv = 'curatif'
  AND (priorite <= 2 OR sla_respecte = FALSE);
```

Ordre d'évaluation logique : `NOT` > `AND` > `OR`. En cas de doute, **parenthèses explicites**.
C'est l'erreur n°1 dans les filtres d'astreinte (« pannes critiques OU préventif du mois »
mal parenthésé = tout le préventif de l'année qui remonte).

```sql
-- ❌ FAUX (AND évalué d'abord) : ramène TOUT le préventif, même priorité 5
SELECT * FROM interventions
WHERE type_interv = 'curatif' AND priorite <= 2 OR type_interv = 'preventif';

-- ✅ CORRECT
SELECT * FROM interventions
WHERE (type_interv = 'curatif' AND priorite <= 2)
   OR type_interv = 'preventif';
```

---

## 6. Opérateurs de comparaison — le détail

| Opérateur | Sens | Exemple |
|---|---|---|
| `=` | égal | `statut = 'en_panne'` |
| `<>` | différent (standard) | `statut <> 'reforme'` |
| `!=` | différent (accepté partout, non standard) | — |
| `<`, `<=`, `>`, `>=` | comparaisons | `priorite <= 2` |
| `BETWEEN a AND b` | entre a et b **inclus** | `cout_eur BETWEEN 100 AND 1000` |
| `IN (...)` | dans une liste | `type_equip IN ('onduleur','copieur')` |
| `LIKE` | motif (section 10) | `modele LIKE 'Easy UPS%'` |
| `IS NULL` / `IS NOT NULL` | test de NULL | `date_cloture IS NULL` |
| `EXISTS` | existence (section 31) | — |

Points d'attention :

```sql
-- BETWEEN est INCLUSIF des deux bornes
SELECT * FROM interventions WHERE cout_eur BETWEEN 100 AND 1000;
-- équivaut à : cout_eur >= 100 AND cout_eur <= 1000

-- Comparer des dates : BETWEEN sur TIMESTAMP inclut minuit pile de la borne haute
-- Pour "tout le mois de septembre", préférer :
SELECT * FROM interventions
WHERE date_ouverture >= '2026-09-01' AND date_ouverture < '2026-10-01';

-- Comparaison de chaînes : sensible à la casse selon le collationnement
-- 'APC' <> 'apc' en général. Pour insensible à la casse :
-- PostgreSQL : WHERE marque ILIKE 'apc'
-- MySQL (collation *_ci par défaut) : WHERE marque = 'apc' suffit déjà
-- Portable : WHERE LOWER(marque) = 'apc'
```

⚠️ **Portabilité** — `ILIKE` n'existe que sous PostgreSQL. `LOWER(col) = 'apc'` marche partout.

---

## 7. Opérateurs logiques AND / OR / NOT

```sql
-- Interventions critiques non clôturées
SELECT id, description FROM interventions
WHERE priorite <= 2 AND date_cloture IS NULL;

-- Équipements à surveiller : en panne OU en maintenance
SELECT reference, statut FROM equipements
WHERE statut = 'en_panne' OR statut = 'en_maintenance';

-- NOT : tout sauf...
SELECT * FROM employes WHERE NOT (actif = FALSE);
-- équivaut à : WHERE actif <> FALSE  (mais attention à NULL, section 13 !)
```

Table de vérité avec NULL (à connaître par cœur, voir section 13) :

| A | B | A AND B | A OR B | NOT A |
|---|---|---|---|---|
| TRUE | TRUE | TRUE | TRUE | FALSE |
| TRUE | FALSE | FALSE | TRUE | FALSE |
| TRUE | NULL | **NULL** | TRUE | **NULL** |
| FALSE | NULL | FALSE | **NULL** | **NULL** |

Conséquence pratique : `WHERE NOT (sla_respecte = TRUE)` **exclut** les lignes où
`sla_respecte` est NULL (interventions en cours). Pour les inclure :

```sql
SELECT * FROM interventions
WHERE sla_respecte IS DISTINCT FROM TRUE;   -- standard SQL, supporté par PostgreSQL
-- MySQL/MariaDB : WHERE NOT (sla_respecte <=> TRUE)  -- opérateur <=> "NULL-safe equal"
-- Portable : WHERE sla_respecte = FALSE OR sla_respecte IS NULL
```

---

## 8. BETWEEN, IN, NOT IN

```sql
-- BETWEEN : intervalles inclusifs
SELECT nom, salaire FROM employes WHERE salaire BETWEEN 2500 AND 3500;

-- IN : liste de valeurs (plus lisible qu'une cascade de OR)
SELECT reference, type_equip FROM equipements
WHERE type_equip IN ('onduleur', 'copieur', 'serveur');

-- NOT IN : ⚠️ DANGER avec NULL (voir section 13)
SELECT * FROM employes WHERE id NOT IN (2, 3);

-- IN avec sous-requête (section 30)
SELECT * FROM equipements
WHERE id IN (SELECT equipement_id FROM interventions WHERE priorite = 1);
```

Performance : `IN` avec une petite liste de constantes est parfait. Avec une sous-requête
retournant des milliers de lignes, `EXISTS` (section 31) est souvent meilleur — l'optimiseur
moderne gère bien les deux, mais `EXISTS` s'arrête dès la première correspondance trouvée.

---

## 9. LIKE — recherche par motif

```sql
-- % : n'importe quelle séquence (y compris vide)
SELECT * FROM equipements WHERE modele LIKE 'Easy UPS%';   -- commence par
SELECT * FROM equipements WHERE modele LIKE '%kVA';        -- finit par
SELECT * FROM equipements WHERE modele LIKE '%UPS%';       -- contient

-- _ : exactement un caractère
SELECT * FROM equipements WHERE reference LIKE 'UPS-___'; -- UPS- + 3 caractères

-- Échapper un % ou _ littéral avec ESCAPE (ou le backslash sous MySQL)
SELECT * FROM equipements WHERE modele LIKE '100\%%' ESCAPE '\';  -- contient "100%"
```

| Motif | Sens |
|---|---|
| `%` | 0, 1 ou plusieurs caractères |
| `_` | exactement 1 caractère |
| `[abc]` | ⚠️ MySQL uniquement avec REGEXP, pas LIKE standard |

Recherche insensible à la casse :

```sql
-- PostgreSQL
SELECT * FROM equipements WHERE marque ILIKE 'apc%';
-- Portable (les deux SGBD)
SELECT * FROM equipements WHERE LOWER(marque) LIKE 'apc%';
```

⚠️ Performance : un `LIKE '%motif'` (motif en tête) **ne peut pas** utiliser un index B-tree
classique → scan complet. Sur des millions de lignes de logs, envisager un index trigramme
(PostgreSQL `pg_trgm`) ou full-text (voir section 60).

---

## 10. ORDER BY — trier

```sql
-- Tri simple
SELECT nom, salaire FROM employes ORDER BY salaire DESC;

-- Tri multiple : salaire décroissant, puis nom croissant
SELECT nom, salaire FROM employes ORDER BY salaire DESC, nom ASC;

-- Trier par position (1 = 1re colonne) — pratique mais fragile si on modifie le SELECT
SELECT nom, salaire * 12 FROM employes ORDER BY 2 DESC;

-- Trier par alias
SELECT nom, salaire * 12 AS annuel FROM employes ORDER BY annuel DESC;

-- NULLS FIRST / LAST (standard ; MySQL : NULLS considérés comme plus petits par défaut)
SELECT nom, date_dernier_test FROM employes e
LEFT JOIN ... -- exemple section 24
ORDER BY date_dernier_test DESC NULLS LAST;
```

⚠️ **Portabilité** — `NULLS FIRST` / `NULLS LAST` : standard SQL, supporté par PostgreSQL.
MySQL/MariaDB ne les acceptent pas ; émuler avec `ORDER BY date_dernier_test IS NULL, date_dernier_test DESC`
(`FALSE`=0 avant `TRUE`=1, donc les non-NULL d'abord).

Tri stable : `ORDER BY` sur une colonne non unique ne garantit **aucun** ordre pour les ex æquo.
Pour un ordre déterministe (pagination !), toujours ajouter la clé primaire en dernier :

```sql
SELECT * FROM interventions ORDER BY date_ouverture DESC, id DESC;
```

---

## 11. LIMIT / OFFSET — paginer

```sql
-- Les 5 interventions les plus récentes
SELECT * FROM interventions ORDER BY date_ouverture DESC LIMIT 5;

-- Page 2 de 10 lignes (lignes 11 à 20)
SELECT * FROM interventions ORDER BY id LIMIT 10 OFFSET 10;
```

⚠️ **Portabilité** — `LIMIT n OFFSET m` : PostgreSQL, MySQL, MariaDB, SQLite.
Syntaxe standard SQL:2008 (SQL Server, Oracle 12c+) : `OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY`.

Pièges de la pagination par OFFSET :

1. **Sans ORDER BY déterministe**, les pages peuvent contenir des doublons ou sauter des lignes
   si des insertions ont lieu entre deux pages.
2. **OFFSET grand = lent** : `OFFSET 100000` force le SGBD à lire et jeter 100 000 lignes.
   Pour de gros volumes, préférer la pagination par curseur (keyset) :

```sql
-- Pagination keyset : "la page suivante après l'id 1004"
SELECT * FROM interventions
WHERE id > 1004
ORDER BY id
LIMIT 10;
```

C'est la méthode à utiliser pour exporter des millions de lignes de logs ou d'historique.

---

## 12. SELECT DISTINCT — éliminer les doublons

```sql
-- Quelles marques sont présentes dans le parc ?
SELECT DISTINCT marque FROM equipements;

-- DISTINCT sur plusieurs colonnes = combinaison unique
SELECT DISTINCT type_equip, marque FROM equipements ORDER BY 1, 2;

-- Compter les valeurs distinctes
SELECT COUNT(DISTINCT technicien_id) AS nb_techniciens_actifs FROM interventions;
```

Attention : `DISTINCT` s'applique à **toute** la ligne projetée, pas à une colonne isolée.
`SELECT DISTINCT nom, salaire` déduplique les paires (nom, salaire).

⚠️ `COUNT(DISTINCT col)` ignore les NULL (comme toutes les fonctions d'agrégat, section 19).
MySQL accepte `COUNT(DISTINCT a, b)` (plusieurs colonnes) ; PostgreSQL non —
émuler avec `COUNT(DISTINCT (a, b))` (ligne composite) ou `COUNT(DISTINCT a || '|' || b)`.

---

## 13. NULL et la logique à trois valeurs — LE concept clé

En SQL, `NULL` signifie **« valeur inconnue / absente »**, pas zéro, pas chaîne vide.
Toute comparaison avec NULL donne NULL (ni vrai ni faux) : c'est la **logique à trois valeurs**
(TRUE / FALSE / UNKNOWN), et `WHERE` ne garde que les lignes **TRUE**.

```sql
-- ❌ NE MARCHE PAS : rien ne vaut NULL, pas même NULL
SELECT * FROM interventions WHERE date_cloture = NULL;   -- 0 ligne, toujours !
SELECT * FROM interventions WHERE date_cloture <> NULL;   -- 0 ligne aussi !

-- ✅ CORRECT
SELECT * FROM interventions WHERE date_cloture IS NULL;      -- en cours
SELECT * FROM interventions WHERE date_cloture IS NOT NULL;  -- clôturées
```

Le piège `NOT IN` avec NULL — **à connaître absolument** :

```sql
-- Si la sous-requête renvoie ne serait-ce qu'UN NULL, NOT IN renvoie... RIEN.
SELECT * FROM employes
WHERE id NOT IN (SELECT technicien_id FROM interventions);
-- technicien_id peut être NULL (intervention non assignée) → résultat vide, sans erreur !

-- ✅ Solutions sûres
SELECT * FROM employes e
WHERE NOT EXISTS (SELECT 1 FROM interventions i WHERE i.technicien_id = e.id);

-- ou filtrer les NULL :
SELECT * FROM employes
WHERE id NOT IN (SELECT technicien_id FROM interventions WHERE technicien_id IS NOT NULL);
```

Propagation de NULL dans les calculs :

```sql
SELECT
    cout_eur,                          -- 850.00
    cout_eur + 100,                    -- 950.00
    NULL + 100,                        -- NULL ! tout calcul avec NULL donne NULL
    COALESCE(cout_eur, 0) + 100        -- 100 si cout_eur est NULL (COALESCE = premier non-NULL)
FROM interventions;
```

Fonctions utiles :

| Fonction | Rôle | Exemple |
|---|---|---|
| `COALESCE(a, b, ...)` | premier argument non NULL | `COALESCE(sla_respecte, FALSE)` |
| `NULLIF(a, b)` | NULL si a = b | `NULLIF(division_par_zero, 0)` pattern |
| `GREATEST/LEAST` | max/min en ignorant... ⚠️ NULL les contamine sous PG | — |

⚠️ `GREATEST(10, NULL)` → NULL sous PostgreSQL (contamination). MySQL : idem.
Toujours `COALESCE` avant, ou filtrer.

---

## 14. Fonctions sur les chaînes de caractères

```sql
SELECT
    UPPER(nom)                 AS nom_maj,        -- 'ZELEF'
    LOWER(marque)              AS marque_min,     -- 'apc'
    LENGTH(modele)             AS longueur,       -- nb de caractères
    TRIM('  test  ')           AS nettoye,        -- 'test'
    SUBSTRING(modele FROM 1 FOR 4) AS prefixe,   -- 'Easy'  (standard)
    POSITION('UPS' IN modele)  AS pos,            -- position (1-based), 0 si absent
    REPLACE(modele, 'UPS', 'ASI') AS fr,          -- remplacement
    nom || ' ' || prenom      AS nom_complet      -- concaténation (standard ||)
FROM equipements;
```

⚠️ **Portabilité** — concaténation :

| SGBD | Syntaxe |
|---|---|
| Standard / PostgreSQL / SQLite | `'a' \|\| 'b'` |
| MySQL/MariaDB | `CONCAT('a','b')` (par défaut `\|\|` = OU logique !) |
| MySQL avec `PIPES_AS_CONCAT` | `\|\|` fonctionne aussi |

```sql
-- Extraction : tout ce qui est après le dernier espace (ex. "40 kVA" → "kVA")
-- PostgreSQL :
SELECT SUBSTRING(modele FROM '([0-9]+ kVA)$') FROM equipements;
-- MySQL 8+ :
SELECT REGEXP_SUBSTR(modele, '[0-9]+ kVA$') FROM equipements;

-- Découper une référence 'UPS-001' :
SELECT
    SPLIT_PART(reference, '-', 1) AS prefixe,   -- PostgreSQL : 'UPS'
    SPLIT_PART(reference, '-', 2) AS numero     -- '001'
FROM equipements;
-- MySQL : SUBSTRING_INDEX(reference, '-', 1) / SUBSTRING_INDEX(reference, '-', -1)
```

Règle sysadmin : normaliser les données **à l'écriture** (CHECK, trigger, application),
pas avec des fonctions dans chaque requête — sinon les index ne servent plus (section 60).

---

## 15. Fonctions numériques

```sql
SELECT
    ROUND(123.456, 2)      AS arrondi,      -- 123.46
    TRUNC(123.456, 1)      AS tronque_pg,   -- 123.4 (PostgreSQL ; MySQL: TRUNCATE(123.456,1))
    CEIL(123.1)            AS plafond,      -- 124
    FLOOR(123.9)           AS plancher,     -- 123
    ABS(-42)               AS absolu,       -- 42
    MOD(17, 5)             AS reste,        -- 2
    POWER(2, 10)           AS puissance,    -- 1024
    SQRT(144)              AS racine,       -- 12
    17 % 5                 AS reste_op,     -- 2 (opérateur %, portable)
    7 / 2                  AS division;     -- ⚠️ 3 (entier) sous PG si 2 entiers ; 3.5 sous MySQL !
```

⚠️ **Division entière** — le piège classique des pourcentages :

```sql
-- ❌ Sous PostgreSQL : 1/2 = 0 → pourcentage toujours 0 !
SELECT sla_respecte, COUNT(*) * 100 / COUNT(*) FROM interventions GROUP BY 1;

-- ✅ Forcer le décimal : multiplier par 100.0 ou caster
SELECT
    COUNT(*) FILTER (WHERE sla_respecte) * 100.0 / COUNT(*) AS pct_sla
FROM interventions WHERE sla_respecte IS NOT NULL;
-- MySQL : la division / donne déjà un décimal, mais 100.0 ne coûte rien et reste portable.
```

Autres utiles : `RANDOM()` (PG) / `RAND()` (MySQL) pour échantillonner,
`GREATEST/LEAST` avec COALESCE (section 13).

```sql
-- Échantillon aléatoire de 10 interventions pour audit qualité
-- PostgreSQL :
SELECT * FROM interventions ORDER BY RANDOM() LIMIT 10;
-- MySQL :
SELECT * FROM interventions ORDER BY RAND() LIMIT 10;
-- ⚠️ Sur de grosses tables c'est un tri complet : OK pour audit, pas en production continue.
```

---

## 16. Fonctions de date et heure — l'essentiel survie

```sql
-- "Maintenant"
SELECT CURRENT_DATE,              -- 2026-09-26
       CURRENT_TIMESTAMP,         -- 2026-09-26 23:41:00+...
       NOW();                     -- idem (PG et MySQL)

-- Extraire des parties
SELECT
    EXTRACT(YEAR  FROM date_ouverture) AS annee,
    EXTRACT(MONTH FROM date_ouverture) AS mois,
    EXTRACT(DOW   FROM date_ouverture) AS jour_semaine_pg,  -- 0=dimanche (PG)
    DATE_TRUNC('month', date_ouverture) AS mois_tronque     -- 2026-09-01 00:00
FROM interventions;
-- MySQL : YEAR(d), MONTH(d), DAYOFWEEK(d) (1=dimanche), DATE_FORMAT(d,'%Y-%m-01')
```

Arithmétique de dates (⚠️ grosse différence de syntaxe) :

```sql
-- PostgreSQL : intervalles
SELECT date_ouverture + INTERVAL '2 hours' FROM interventions;
SELECT * FROM interventions WHERE date_ouverture >= NOW() - INTERVAL '7 days';
SELECT AGE(date_cloture, date_ouverture) AS duree FROM interventions;  -- type interval

-- MySQL/MariaDB : fonctions
SELECT DATE_ADD(date_ouverture, INTERVAL 2 HOUR) FROM interventions;
SELECT * FROM interventions WHERE date_ouverture >= NOW() - INTERVAL 7 DAY;
SELECT TIMESTAMPDIFF(HOUR, date_ouverture, date_cloture) AS duree_h FROM interventions;
```

Durées d'intervention (portable, en heures décimales) :

```sql
-- PostgreSQL
SELECT id, EXTRACT(EPOCH FROM (date_cloture - date_ouverture))/3600.0 AS duree_h
FROM interventions WHERE date_cloture IS NOT NULL;

-- MySQL
SELECT id, TIMESTAMPDIFF(MINUTE, date_ouverture, date_cloture)/60.0 AS duree_h
FROM interventions WHERE date_cloture IS NOT NULL;
```

Recettes anti-erreur :

- Stocker en `TIMESTAMP`/`DATETIME`, **jamais** en VARCHAR.
- Écrire les littéraux en ISO `'2026-09-26'` / `'2026-09-26 14:30:00'`.
- Comparer un mois avec `>= premier_jour AND < premier_jour_suivant` (section 6), pas avec `BETWEEN`.
- ⚠️ Timezones : `TIMESTAMP WITH TIME ZONE` (PG) vs `TIMESTAMP` (MySQL, converti en UTC au stockage).
  Pour un parc multi-sites, stocker en UTC et convertir à l'affichage.

---

## 17. CASE — le couteau suisse conditionnel

`CASE` évalue des conditions ligne par ligne. Deux formes :

```sql
-- Forme "simple" : égalité sur une expression
SELECT nom,
       CASE poste
           WHEN 'Chef de service' THEN 'Encadrement'
           WHEN 'Apprenti'        THEN 'Formation'
           ELSE 'Technique'
       END AS categorie
FROM employes;

-- Forme "recherchée" : conditions libres (la plus utile)
SELECT reference, statut,
       CASE
           WHEN statut = 'en_panne' THEN '🔴 Intervention immédiate'
           WHEN statut = 'en_maintenance' THEN '🟡 Suivi en cours'
           WHEN garantie_jusqu < CURRENT_DATE THEN '🟠 Garantie expirée'
           ELSE '🟢 OK'
       END AS etat_affiche
FROM equipements;
```

`CASE` dans les agrégats = **pivot manuel** (hyper utile en reporting) :

```sql
-- Nombre d'interventions par type, en colonnes
SELECT
    COUNT(*) AS total,
    COUNT(*) FILTER (WHERE type_interv = 'curatif')   AS curatif,    -- PG
    COUNT(*) FILTER (WHERE type_interv = 'preventif') AS preventif,
    SUM(CASE WHEN sla_respecte THEN 1 ELSE 0 END)     AS sla_ok      -- portable
FROM interventions;
```

⚠️ **Portabilité** — `FILTER (WHERE ...)` : PostgreSQL et SQLite uniquement.
La version `SUM(CASE WHEN ... THEN 1 ELSE 0 END)` marche **partout** : à privilégier
dans le code partagé.

`CASE` retourne NULL si aucun WHEN ne matche et pas de ELSE → toujours mettre un `ELSE`
explicite sauf besoin précis.

---

## 18. CAST et conversions de types

```sql
-- Syntaxe standard
SELECT CAST(salaire AS INTEGER) FROM employes;          -- tronque : 5200
SELECT salaire::INTEGER FROM employes;                  -- opérateur PG uniquement

-- Conversions courantes
SELECT
    CAST('2026-09-26' AS DATE),
    CAST('42' AS INTEGER),
    CAST(123.456 AS NUMERIC(10,2)),
    CAST(id AS VARCHAR) || '-SETIS' AS code_inv
FROM employes;

-- Conversions sûres : TRY_CAST (SQL Server), PG 16+ :
SELECT PG_TRY_CAST('abc' AS INTEGER);   -- NULL au lieu d'erreur (PostgreSQL 16+)
-- MySQL : CAST('abc' AS UNSIGNED) → 0 avec warning (pas d'erreur !)
```

Règle : convertir **explicitement** plutôt que de compter sur la conversion implicite —
les comportements diffèrent (`'2026-13-45'` : erreur PG, warning+NULL MySQL selon sql_mode).

---

## 19. Agrégations — COUNT, SUM, AVG, MIN, MAX

Les fonctions d'agrégat réduisent un ensemble de lignes en **une** valeur.
Point fondamental : **elles ignorent les NULL** (sauf `COUNT(*)`).

```sql
SELECT
    COUNT(*)              AS nb_lignes,        -- compte les lignes, NULL inclus
    COUNT(date_cloture)   AS nb_cloturees,     -- ignore les NULL !
    COUNT(DISTINCT technicien_id) AS nb_tech,  -- valeurs distinctes non NULL
    SUM(cout_eur)         AS cout_total,
    AVG(cout_eur)         AS cout_moyen,       -- moyenne des non-NULL
    MIN(date_ouverture)   AS premiere,
    MAX(date_ouverture)   AS derniere
FROM interventions;
```

| Fonction | Ignore NULL ? | Ensemble vide → |
|---|---|---|
| `COUNT(*)` | non (compte lignes) | 0 |
| `COUNT(col)` | oui | 0 |
| `SUM(col)` | oui | NULL |
| `AVG(col)` | oui | NULL |
| `MIN/MAX(col)` | oui | NULL |

```sql
-- Coût moyen PAR TECHNICIEN, en traitant les interventions sans coût
SELECT technicien_id,
       COUNT(*) AS nb,
       COALESCE(SUM(cout_eur), 0) AS total,          -- SUM vide = NULL → 0
       COALESCE(AVG(cout_eur), 0) AS moyenne
FROM interventions
GROUP BY technicien_id;
```

⚠️ `AVG` sur des entiers : `AVG(priorite)` → PG renvoie `numeric` (décimal ✓),
MySQL renvoie un décimal aussi (4 décimales). Pas de piège ici, contrairement à `/`.

---

## 20. GROUP BY — regrouper avant d'agréger

```sql
-- Coût total par type d'intervention
SELECT type_interv, COUNT(*) AS nb, SUM(cout_eur) AS cout_total
FROM interventions
GROUP BY type_interv;

-- Par mois (tronquer la date d'abord)
SELECT DATE_TRUNC('month', date_ouverture) AS mois, COUNT(*) AS nb
FROM interventions
GROUP BY 1
ORDER BY 1;
-- MySQL : SELECT DATE_FORMAT(date_ouverture,'%Y-%m') AS mois, ... GROUP BY 1
```

Règle d'or : **toute colonne du SELECT non agrégée DOIT être dans le GROUP BY**
(ou être fonctionnellement dépendante de la clé groupée).

```sql
-- ❌ Invalide en standard (et refusé par PG) : poste n'est ni agrégé ni groupé
SELECT service, poste, COUNT(*) FROM employes GROUP BY service;

-- ✅ Correct
SELECT service, poste, COUNT(*) FROM employes GROUP BY service, poste;

-- ✅ Autorisé par PG : id est la PK, nom/prenom en dépendent fonctionnellement
SELECT e.id, e.nom, e.prenom, COUNT(i.id) AS nb_interv
FROM employes e LEFT JOIN interventions i ON i.technicien_id = e.id
GROUP BY e.id;
```

⚠️ MySQL avec `ONLY_FULL_GROUP_BY` désactivé (ancien défaut !) **accepte** la forme invalide
et renvoie une valeur **arbitraire** pour la colonne manquante → résultats faux silencieux.
Vérifier : `SELECT @@sql_mode;` doit contenir `ONLY_FULL_GROUP_BY`.

---

## 21. HAVING — filtrer APRÈS agrégation

`WHERE` filtre les lignes **avant** regroupement ; `HAVING` filtre les **groupes après** calcul.

```sql
-- Techniciens ayant coûté plus de 500 € au total
SELECT technicien_id, SUM(cout_eur) AS total
FROM interventions
GROUP BY technicien_id
HAVING SUM(cout_eur) > 500;

-- Ordre logique complet (mémoriser cet ordre !) :
-- FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
SELECT type_equip, COUNT(*) AS nb
FROM equipements
WHERE statut <> 'reforme'          -- 1. filtre lignes
GROUP BY type_equip                -- 2. regroupe
HAVING COUNT(*) >= 2               -- 3. filtre groupes
ORDER BY nb DESC                   -- 4. trie
LIMIT 5;                           -- 5. pagine
```

On peut réutiliser l'alias dans HAVING sous MySQL (`HAVING total > 500`) ;
PostgreSQL l'interdit (utiliser l'expression ou un sous-SELECT/CTE). Pour du portable :
répéter l'expression ou passer par une CTE (section 34).

---

## 22. Regroupements avancés : ROLLUP, CUBE, GROUPING SETS

Pour des sous-totaux en une seule requête (reporting) :

```sql
-- Sous-totaux par type + total général
SELECT type_interv, priorite, COUNT(*) AS nb, SUM(cout_eur) AS cout
FROM interventions
GROUP BY ROLLUP (type_interv, priorite)
ORDER BY type_interv NULLS LAST, priorite NULLS LAST;
-- Lignes où type_interv IS NULL = sous-total / total (utiliser GROUPING() pour distinguer)
```

| Construction | Produit |
|---|---|
| `ROLLUP(a, b)` | (a,b), (a), () — hiérarchique |
| `CUBE(a, b)` | (a,b), (a), (b), () — toutes combinaisons |
| `GROUPING SETS ((a,b),(a),())` | sur mesure |

```sql
-- Distinguer un vrai NULL d'un NULL de sous-total
SELECT type_interv,
       GROUPING(type_interv) AS est_sous_total,
       COUNT(*)
FROM interventions
GROUP BY ROLLUP (type_interv);
```

⚠️ **Portabilité** — supporté par PostgreSQL et MySQL 8+. MariaDB : `WITH ROLLUP`
(ancienne syntaxe MySQL, toujours valide : `GROUP BY type_interv WITH ROLLUP`).

---

## 23. INNER JOIN — l'intersection

`INNER JOIN` ne garde que les lignes qui **matchent des deux côtés**.

```sql
-- Interventions avec le nom du technicien et la référence de l'équipement
SELECT i.id, i.date_ouverture, e.reference, e.modele,
       emp.nom AS technicien, i.description
FROM interventions i
INNER JOIN equipements e ON e.id = i.equipement_id
INNER JOIN employes emp  ON emp.id = i.technicien_id
ORDER BY i.date_ouverture DESC;
```

Bonnes pratiques :

- **Toujours** aliaser (`i`, `e`, `emp`) et **préfixer** chaque colonne (`i.id`, pas `id`)
  dès qu'il y a 2+ tables — sinon `Column 'id' in field list is ambiguous`.
- `INNER` est optionnel : `JOIN` seul = `INNER JOIN`. L'écrire explicite aide la relecture.
- La condition dans `ON`, pas dans `WHERE` (pour INNER ça ne change rien au résultat,
  mais pour OUTER ça change tout — section 24).

---

## 24. LEFT JOIN — garder la gauche même sans match

`LEFT JOIN` garde **toutes** les lignes de la table de gauche ; les colonnes de droite
valent NULL quand rien ne matche. LE join le plus utile au quotidien.

```sql
-- Tous les employés + leur nombre d'interventions (même 0)
SELECT e.nom, e.prenom, COUNT(i.id) AS nb_interv
FROM employes e
LEFT JOIN interventions i ON i.technicien_id = e.id
GROUP BY e.id, e.nom, e.prenom
ORDER BY nb_interv DESC;
-- ⚠️ COUNT(i.id) et pas COUNT(*) : COUNT(*) compterait 1 pour les sans-intervention !

-- Équipements SANS aucune intervention (anti-join)
SELECT e.reference, e.modele
FROM equipements e
LEFT JOIN interventions i ON i.equipement_id = e.id
WHERE i.id IS NULL;
```

Le piège du WHERE sur la table de droite :

```sql
-- ❌ Le WHERE transforme le LEFT JOIN en INNER JOIN !
SELECT e.nom, i.description
FROM employes e
LEFT JOIN interventions i ON i.technicien_id = e.id
WHERE i.priorite = 1;   -- élimine les lignes où i.* est NULL → INNER déguisé

-- ✅ Condition dans le ON : on garde les employés sans intervention prio 1
SELECT e.nom, i.description
FROM employes e
LEFT JOIN interventions i ON i.technicien_id = e.id AND i.priorite = 1;
```

**Règle : condition sur la table optionnelle → dans le `ON`, jamais dans le `WHERE`.**

---

## 25. RIGHT JOIN et FULL OUTER JOIN

```sql
-- RIGHT JOIN : garde toute la table de DROITE
-- (équivalent à un LEFT JOIN en inversant les tables — rarement utilisé)
SELECT e.nom, i.description
FROM interventions i
RIGHT JOIN employes e ON e.id = i.technicien_id;
-- = tous les employés, même sans intervention

-- FULL OUTER JOIN : garde TOUT des deux côtés
SELECT e.reference AS equip, i.id AS interv
FROM equipements e
FULL OUTER JOIN interventions i ON i.equipement_id = e.id;
-- Équipements sans intervention (i.id NULL) + interventions orphelines (e.reference NULL)
```

⚠️ **Portabilité** — MySQL/MariaDB **ne supportent pas** `FULL OUTER JOIN`.
Émulation portable :

```sql
SELECT e.reference, i.id
FROM equipements e LEFT JOIN interventions i ON i.equipement_id = e.id
UNION
SELECT e.reference, i.id
FROM equipements e RIGHT JOIN interventions i ON i.equipement_id = e.id;
-- (UNION déduplique ; les lignes matchées apparaissent une fois)
```

En pratique : on écrit presque toujours `LEFT JOIN` (en choisissant l'ordre des tables),
`RIGHT JOIN` ne sert qu'en lecture de code existant.

---

## 26. CROSS JOIN — produit cartésien

```sql
-- Toutes les combinaisons technicien × type d'équipement (ex. matrice d'habilitation)
SELECT e.nom, t.type_equip
FROM (SELECT DISTINCT nom FROM employes) e
CROSS JOIN (SELECT DISTINCT type_equip FROM equipements) t;
```

Cas d'usage légitimes : matrices, calendriers (jours × équipes), jeux de test.
Danger : `n × m` lignes — 10 000 × 10 000 = 100 M de lignes. Un `JOIN` sans `ON`
(par oubli !) est un CROSS JOIN implicite : **toujours vérifier la condition de jointure**.

```sql
-- ❌ Oubli du ON : produit cartésien silencieux
SELECT * FROM interventions, equipements;
-- ✅ Explicite
SELECT * FROM interventions i JOIN equipements e ON e.id = i.equipement_id;
```

---

## 27. Self-join — une table jointe à elle-même

Indispensable pour les hiérarchies (manager/employé) et les comparaisons intra-table.

```sql
-- Employé + nom de son manager
SELECT e.nom AS employe, m.nom AS manager
FROM employes e
LEFT JOIN employes m ON m.id = e.manager_id;   -- LEFT : le chef n'a pas de manager

-- Équipements achetés la même année qu'un autre (paires)
SELECT a.reference AS eq1, b.reference AS eq2,
       EXTRACT(YEAR FROM a.date_achat) AS annee
FROM equipements a
JOIN equipements b ON EXTRACT(YEAR FROM a.date_achat) = EXTRACT(YEAR FROM b.date_achat)
                  AND a.id < b.id;   -- < évite les doublons (a,b)/(b,a) et l'auto-paire
```

Autre usage : détecter des anomalies (« deux interventions ouvertes le même jour
sur le même équipement ») — voir section 63 sur les doublons.

---

## 28. USING et NATURAL JOIN — à connaître, à éviter

```sql
-- USING : quand la colonne de jointure a le même nom des deux côtés
SELECT * FROM interventions JOIN equipements USING (id);  -- ⚠️ ici id ≠ id ! DANGER

-- Cas légitime :
SELECT * FROM equipements e JOIN onduleurs o USING (equipement_id);
-- Note : la colonne fusionnée n'apparaît qu'une fois dans SELECT *.
```

`NATURAL JOIN` joint sur **toutes** les colonnes de même nom, implicitement.
**À proscrire** : l'ajout futur d'une colonne homonyme change silencieusement la jointure.
Règle d'équipe : `ON` explicite, toujours.

---

## 29. Ordre et parenthésage des JOIN multiples

```sql
-- Chaîne de 3 tables : interventions → équipements (+ onduleurs) → techniciens
SELECT i.id, e.reference, o.puissance_kva, emp.nom AS tech
FROM interventions i
JOIN equipements e ON e.id = i.equipement_id
LEFT JOIN onduleurs o ON o.equipement_id = e.id   -- NULL si pas un onduleur
JOIN employes emp    ON emp.id = i.technicien_id;
```

L'ordre compte pour les OUTER JOIN enchaînés : `(A LEFT JOIN B) LEFT JOIN C`
≠ `A LEFT JOIN (B LEFT JOIN C)` dans certains cas. En cas de doute, parenthéser
explicitement — le SGBD respecte les parenthèses.

Astuce de lecture : partir de la table « sujet » (souvent la table de faits :
`interventions`), puis rattacher les dimensions (`equipements`, `employes`).

---

## 30. Sous-requêtes avec IN

Une sous-requête est un SELECT dans un SELECT. Avec `IN` :

```sql
-- Équipements ayant eu au moins une intervention curative prioritaire
SELECT reference, modele FROM equipements
WHERE id IN (
    SELECT equipement_id FROM interventions
    WHERE type_interv = 'curatif' AND priorite <= 2
);

-- IN avec liste ET sous-requête combinées : impossible directement → UNION dans la sous-requête
SELECT * FROM equipements
WHERE id IN (
    SELECT equipement_id FROM interventions WHERE priorite = 1
    UNION
    SELECT 105  -- ajout manuel : l'onduleur sous alarme
);
```

Rappel critique (section 13) : si la sous-requête peut renvoyer NULL,
`NOT IN` renvoie zéro ligne. `IN` (positif), lui, n'a pas ce problème :
`x IN (1, 2, NULL)` vaut TRUE si x=1, FALSE si x=3, UNKNOWN si x=NULL→ filtré.

---

## 31. EXISTS / NOT EXISTS — le test d'existence

`EXISTS` renvoie TRUE dès qu'**une** ligne correspond — sans NULL-piège, sans doublons.

```sql
-- Techniciens ayant réalisé au moins une intervention (semi-join)
SELECT nom, prenom FROM employes e
WHERE EXISTS (
    SELECT 1 FROM interventions i WHERE i.technicien_id = e.id
);

-- Équipements SANS intervention (anti-join) — la forme recommandée
SELECT reference FROM equipements e
WHERE NOT EXISTS (
    SELECT 1 FROM interventions i WHERE i.equipement_id = e.id
);
```

Comparatif des anti-joins (résultat identique, sémantiques différentes) :

| Forme | NULL-piège ? | Lisibilité |
|---|---|---|
| `NOT EXISTS (...)` | ✅ non | très bonne |
| `LEFT JOIN ... WHERE droite.id IS NULL` | ✅ non | bonne |
| `NOT IN (sous-requête)` | ❌ **OUI si NULL** | trompeuse |

**Règle d'équipe : pour « les X sans Y », utiliser `NOT EXISTS`.**
C'est aussi celle que l'optimiseur traite le mieux en général (semi/anti-join natifs
dans les plans d'exécution).

---

## 32. Sous-requêtes corrélées — la requête dans la boucle

Une sous-requête **corrélée** référence une colonne de la requête externe :
elle est réévaluée pour chaque ligne candidate (conceptuellement).

```sql
-- La dernière intervention de CHAQUE équipement
SELECT e.reference,
       (SELECT MAX(date_ouverture) FROM interventions i
         WHERE i.equipement_id = e.id) AS derniere_interv
FROM equipements e;

-- Employés payés plus que la moyenne de leur... (ici moyenne globale, non corrélée)
-- Version corrélée : techniciens dont le coût total dépasse la moyenne des techniciens
SELECT e.nom, tot.total
FROM employes e
JOIN (SELECT technicien_id, SUM(cout_eur) AS total
      FROM interventions GROUP BY technicien_id) tot ON tot.technicien_id = e.id
WHERE tot.total > (SELECT AVG(total) FROM
                   (SELECT SUM(cout_eur) AS total FROM interventions
                    GROUP BY technicien_id) t);
```

⚠️ Performance : une sous-requête corrélée dans le SELECT s'exécute N fois
(N = lignes externes). Souvent mieux : la réécrire en JOIN + GROUP BY,
ou utiliser une fonction fenêtre (section 36+).

---

## 33. Sous-requêtes dans FROM (tables dérivées) et LATERAL

```sql
-- Table dérivée : obligatoire d'aliaser (t)
SELECT t.type_interv, t.nb, t.cout_total
FROM (
    SELECT type_interv, COUNT(*) AS nb, SUM(cout_eur) AS cout_total
    FROM interventions
    GROUP BY type_interv
) AS t
WHERE t.nb > 1
ORDER BY t.cout_total DESC;
```

`LATERAL` (PG) / `CROSS APPLY` (SQL Server) : sous-requête du FROM qui référence
les tables précédentes — les « 3 dernières interventions de chaque équipement » :

```sql
-- PostgreSQL
SELECT e.reference, r.id, r.date_ouverture
FROM equipements e
CROSS JOIN LATERAL (
    SELECT i.id, i.date_ouverture FROM interventions i
    WHERE i.equipement_id = e.id
    ORDER BY i.date_ouverture DESC
    LIMIT 3
) AS r;

-- MySQL 8+ : pas de LATERAL → utiliser ROW_NUMBER() (section 37)
```

---

## 34. CTE — WITH, la requête nommée

Une CTE (Common Table Expression) nomme une sous-requête réutilisable.
Lisibilité +++, et souvent le même plan que la sous-requête.

```sql
WITH
interv_par_tech AS (
    SELECT technicien_id, COUNT(*) AS nb, SUM(cout_eur) AS cout_total
    FROM interventions
    GROUP BY technicien_id
),
techs AS (
    SELECT id, nom, prenom FROM employes WHERE actif
)
SELECT t.nom, t.prenom, i.nb, i.cout_total
FROM techs t
LEFT JOIN interv_par_tech i ON i.technicien_id = t.id
ORDER BY i.nb DESC NULLS LAST;
```

Avantages sur les sous-requêtes imbriquées :

- Chaque étape a un **nom** qui documente l'intention.
- On peut chaîner / réutiliser (la même CTE référencée 2 fois).
- Le débogage se fait étape par étape (exécuter chaque CTE seule).

⚠️ MySQL < 8.0 et MariaDB < 10.2 : pas de CTE → tables dérivées (section 33).
Aujourd'hui, MySQL 8+ / MariaDB 10.2+ / PG : OK partout.

Note PG : jusqu'à la v11, les CTE étaient des « barrières d'optimisation »
(toujours matérialisées). Depuis PG 12, l'optimiseur peut les « aplatir ».
`MATERIALIZED` / `NOT MATERIALIZED` permettent de forcer le comportement.

---

## 35. CTE récursives — hiérarchies et séries

```sql
-- Chaîne hiérarchique : qui est sous les ordres de qui (niveau par niveau)
WITH RECURSIVE hierarchie AS (
    -- Ancre : le sommet
    SELECT id, nom, manager_id, 0 AS niveau
    FROM employes WHERE manager_id IS NULL
    UNION ALL
    -- Récursion : les subordonnés du niveau précédent
    SELECT e.id, e.nom, e.manager_id, h.niveau + 1
    FROM employes e
    JOIN hierarchie h ON e.manager_id = h.id
)
SELECT nom, niveau FROM hierarchie ORDER BY niveau, nom;
```

Deuxième usage roi : **générer des séries** (calendrier, sans table calendrier) :

```sql
-- Tous les jours de septembre 2026, avec le nombre d'interventions (0 inclus)
WITH RECURSIVE jours AS (
    SELECT DATE '2026-09-01' AS j
    UNION ALL
    SELECT j + INTERVAL '1 day' FROM jours WHERE j < DATE '2026-09-30'
)
SELECT j.j AS jour, COUNT(i.id) AS nb_interv
FROM jours j
LEFT JOIN interventions i ON i.date_ouverture::date = j.j
GROUP BY j.j
ORDER BY j.j;
-- MySQL : j + INTERVAL 1 DAY ; i : DATE(i.date_ouverture) = j.j
```

⚠️ Toujours une condition d'arrêt (`WHERE j < ...`), sinon récursion infinie
(PG coupe à 100 itérations par défaut ? non — PG ne limite pas, MySQL/MariaDB :
`cte_max_recursion_depth` = 1000 par défaut).

---

## 36. Fonctions fenêtre — OVER, le game changer

Une fonction fenêtre calcule sur un **ensemble de lignes liées** à la ligne courante,
**sans réduire** le nombre de lignes (contrairement à GROUP BY).

```sql
SELECT
    nom, service, salaire,
    AVG(salaire) OVER () AS moyenne_globale,              -- toute la table
    AVG(salaire) OVER (PARTITION BY service) AS moy_service, -- par groupe
    salaire - AVG(salaire) OVER (PARTITION BY service) AS ecart_service
FROM employes;
```

Anatomie : `FONCTION(...) OVER (PARTITION BY ... ORDER BY ... ROWS/RANGE ...)`.

- `PARTITION BY` = le « GROUP BY » de la fenêtre (optionnel).
- `ORDER BY` = ordre **dans** la fenêtre (obligatoire pour les classements).
- Sans PARTITION : une seule fenêtre = toute la table.

Cas sysadmin typique : comparer chaque intervention au coût moyen **de son type**,
sur la même ligne, sans perdre le détail :

```sql
SELECT id, type_interv, cout_eur,
       AVG(cout_eur) OVER (PARTITION BY type_interv) AS cout_moyen_type,
       cout_eur - AVG(cout_eur) OVER (PARTITION BY type_interv) AS ecart
FROM interventions;
```

---

## 37. ROW_NUMBER, RANK, DENSE_RANK

```sql
SELECT nom, salaire,
       ROW_NUMBER() OVER (ORDER BY salaire DESC) AS rang_strict,
       RANK()       OVER (ORDER BY salaire DESC) AS rang,
       DENSE_RANK() OVER (ORDER BY salaire DESC) AS rang_dense
FROM employes;
```

| nom | salaire | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|---|
| Zelef | 5200 | 1 | 1 | 1 |
| Diallo | 3100 | 2 | 2 | 2 |
| Ndiaye | 2950 | 3 | 3 | 3 |
| Sow | 2800 | 4 | 4 | 4 |
| Kane | 1400 | 5 | 5 | 5 |

Avec ex æquo (deux salaires à 3100) : ROW_NUMBER donne 2 et 3 (arbitraire mais unique),
RANK donne 2, 2 puis 4 (trou), DENSE_RANK donne 2, 2 puis 3 (sans trou).

Recette « top-N par groupe » (les 2 dernières interventions de chaque équipement) :

```sql
WITH classees AS (
    SELECT i.*,
           ROW_NUMBER() OVER (PARTITION BY equipement_id
                              ORDER BY date_ouverture DESC) AS rn
    FROM interventions i
)
SELECT * FROM classees WHERE rn <= 2;
```

⚠️ On **ne peut pas** mettre une fonction fenêtre dans WHERE (évaluée après) →
toujours via CTE ou sous-requête comme ci-dessus.

---

## 38. LAG / LEAD — regarder la ligne voisine

```sql
-- Évolution du coût : comparer chaque intervention à la précédente (par équipement)
SELECT equipement_id, date_ouverture, cout_eur,
       LAG(cout_eur) OVER (PARTITION BY equipement_id
                           ORDER BY date_ouverture) AS cout_precedent,
       cout_eur - LAG(cout_eur) OVER (PARTITION BY equipement_id
                                     ORDER BY date_ouverture) AS delta,
       LEAD(date_ouverture) OVER (PARTITION BY equipement_id
                                  ORDER BY date_ouverture) AS prochaine_date
FROM interventions;
```

Arguments : `LAG(colonne [, décalage [, défaut]])`.
Exemple métier : détecter les équipements dont le coût d'intervention **augmente**
trois fois de suite (signe de fin de vie → arbitrage remplacement) :

```sql
WITH c AS (
    SELECT equipement_id, cout_eur,
           LAG(cout_eur, 1) OVER w AS c1,
           LAG(cout_eur, 2) OVER w AS c2
    FROM interventions
    WINDOW w AS (PARTITION BY equipement_id ORDER BY date_ouverture)
)
SELECT DISTINCT equipement_id FROM c
WHERE cout_eur > c1 AND c1 > c2;
```

(`WINDOW w AS (...)` : clause standard pour factoriser la définition de fenêtre —
supportée par PG ; MySQL 8+ aussi depuis 8.0.)

---

## 39. Agrégats et cadres de fenêtre (ROWS / RANGE)

Par défaut, avec ORDER BY dans OVER, le cadre est
`RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` : cumul progressif.

```sql
-- Coût cumulé des interventions dans le temps
SELECT date_ouverture::date AS jour, cout_eur,
       SUM(cout_eur) OVER (ORDER BY date_ouverture
                           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS cumul
FROM interventions
ORDER BY date_ouverture;

-- Moyenne mobile sur 3 interventions (lignes)
SELECT id, cout_eur,
       AVG(cout_eur) OVER (ORDER BY date_ouverture
                           ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moy_mobile_3
FROM interventions;
```

Cadres utiles :

| Cadre | Sens |
|---|---|
| `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` | cumul depuis le début |
| `ROWS BETWEEN 6 PRECEDING AND CURRENT ROW` | 7 dernières lignes (moyenne mobile) |
| `RANGE BETWEEN INTERVAL '30 days' PRECEDING AND CURRENT ROW` | 30 derniers jours (valeurs, pas lignes) |

⚠️ `RANGE` avec offset n'accepte qu'**une** expression d'ordre, et les intervalles
de dates en RANGE sont PG-spécifiques. Pour du portable : `ROWS`.

---

## 40. UNION, INTERSECT, EXCEPT — combiner des requêtes

```sql
-- Tous les identifiants "personnes ou équipements" suivis (ex. inventaire global)
SELECT id, nom AS libelle, 'employe' AS nature FROM employes
UNION
SELECT id, reference, 'equipement' FROM equipements
ORDER BY 1;
```

| Opérateur | Résultat | Doublons |
|---|---|---|
| `UNION` | lignes des deux | dédupliqués |
| `UNION ALL` | lignes des deux | **conservés** (plus rapide) |
| `INTERSECT` | lignes communes | dédupliqués |
| `EXCEPT` | lignes de la 1re absentes de la 2e | dédupliqués |

Règles :

- Même **nombre** de colonnes, types **compatibles** (les noms viennent de la 1re requête).
- `ORDER BY` / `LIMIT` : une seule fois, à la fin, sur l'ensemble.
- `UNION ALL` est plus rapide (pas de tri de déduplication) : l'utiliser dès qu'on sait
  qu'il n'y a pas de doublons ou qu'on les veut.

```sql
-- Techniciens n'ayant fait QUE du curatif (jamais de préventif)
SELECT DISTINCT technicien_id FROM interventions WHERE type_interv = 'curatif'
EXCEPT
SELECT DISTINCT technicien_id FROM interventions WHERE type_interv = 'preventif';
-- MySQL/MariaDB : pas de EXCEPT/INTERSECT → réécrire avec NOT EXISTS (section 31)
```

⚠️ **Portabilité** — `INTERSECT` / `EXCEPT` : PostgreSQL, SQL Server, Oracle, SQLite.
MySQL/MariaDB : **absents** → passer par `IN`/`EXISTS`/`NOT EXISTS`.

---

## 41. Ordre d'exécution logique — à graver

```
FROM / JOIN        → quelles tables, comment jointes
WHERE              → filtre les lignes
GROUP BY           → regroupe
HAVING             → filtre les groupes
SELECT             → projette / calcule (DISTINCT ici)
   └── fenêtres    → OVER() évalué après SELECT de base
ORDER BY           → trie
LIMIT / OFFSET     → pagine
```

Conséquences directes :

1. On ne peut pas utiliser un alias du SELECT dans WHERE (évalué avant).
2. On ne peut pas mettre une fonction fenêtre dans WHERE ou GROUP BY.
3. `ORDER BY` peut utiliser les alias (évalué après SELECT).
4. `WHERE` ne voit pas les agrégats → `HAVING`.

```sql
-- ❌ WHERE sur un alias
SELECT salaire * 12 AS annuel FROM employes WHERE annuel > 40000;
-- ✅
SELECT salaire * 12 AS annuel FROM employes WHERE salaire * 12 > 40000;
-- ✅ (plus propre) : CTE
WITH s AS (SELECT *, salaire * 12 AS annuel FROM employes)
SELECT * FROM s WHERE annuel > 40000;
```

---

## 42. DDL — CREATE TABLE proprement

```sql
CREATE TABLE IF NOT EXISTS maintenances_planifiees (
    id              SERIAL PRIMARY KEY,              -- PG : auto-incrément
    equipement_id   INTEGER NOT NULL REFERENCES equipements(id),
    date_prevue     DATE NOT NULL,
    type_maintenance VARCHAR(20) NOT NULL DEFAULT 'preventif',
    commentaire     TEXT,
    realisee        BOOLEAN NOT NULL DEFAULT FALSE,
    CONSTRAINT ck_date_future CHECK (date_prevue >= DATE '2020-01-01'),
    CONSTRAINT uq_equip_date UNIQUE (equipement_id, date_prevue)
);
```

⚠️ **Portabilité auto-incrément** :

| SGBD | Syntaxe |
|---|---|
| PostgreSQL | `SERIAL` / `BIGSERIAL` (ou `GENERATED ALWAYS AS IDENTITY`, standard) |
| MySQL/MariaDB | `INT AUTO_INCREMENT PRIMARY KEY` |
| Standard SQL:2008 | `GENERATED ALWAYS AS IDENTITY` (PG ≥ 10, Oracle, DB2) |

Conseils de création :

- `IF NOT EXISTS` : rend les scripts rejouables (déploiements, Ansible).
- Nommer les contraintes (`CONSTRAINT ck_...`) : les messages d'erreur deviennent lisibles.
- Choisir le bon type **dès le départ** (section 43) : changer après coûte cher.
- Toujours une PK (même technique) : sans PK, pas de réplication logique fiable,
  pas d'UPDATE ciblé propre, ORM en difficulté.

---

## 43. Types de données — choisir juste

| Besoin | PostgreSQL | MySQL/MariaDB | Notes |
|---|---|---|---|
| Entier 32 bits | `INTEGER` | `INT` | PK courantes |
| Entier 64 bits | `BIGINT` | `BIGINT` | gros volumes |
| Auto-incrément | `SERIAL`/`IDENTITY` | `INT AUTO_INCREMENT` | §42 |
| Décimal exact (monnaie) | `NUMERIC(p,s)` | `DECIMAL(p,s)` | **toujours** pour l'argent |
| Flottant approximatif | `DOUBLE PRECISION` | `DOUBLE` | mesures, jamais monnaie |
| Chaîne courte | `VARCHAR(n)` | `VARCHAR(n)` | n = garde-fou, pas perf |
| Texte long | `TEXT` | `TEXT`/`MEDIUMTEXT` | descriptions, logs |
| Booléen | `BOOLEAN` | `BOOLEAN` (= TINYINT(1)) | ⚠️ MySQL : 0/1 |
| Date / heure | `DATE`, `TIMESTAMP`, `TIMESTAMPTZ` | `DATE`, `DATETIME`, `TIMESTAMP` | §16 |
| Binaire | `BYTEA` | `BLOB` | fichiers, empreintes |
| JSON | `JSONB` | `JSON` | semi-structuré (§79) |
| Réseau (PG) | `INET`, `CIDR`, `MACADDR` | — (VARCHAR) | adresses IP : typé > texte |

Règles :

- **Monnaie → NUMERIC/DECIMAL**, jamais FLOAT (0.1 + 0.2 ≠ 0.3 en binaire).
- IP, emails : VARCHAR + CHECK de format, ou types natifs (PG `INET` permet
  les opérateurs réseau `>>`, `<<`).
- `VARCHAR(255)` par habitude : OK, mais mettre une vraie limite métier quand elle existe
  (`statut VARCHAR(20)` + CHECK).
- ⚠️ MySQL `VARCHAR` sans longueur de charset utf8mb4 : 4 octets/caractère pour l'indexation
  (limite 191 caractères pour index unique en InnoDB ancien).

---

## 44. ALTER TABLE — faire évoluer sans casser

```sql
-- Ajouter une colonne (avec défaut pour les lignes existantes)
ALTER TABLE equipements ADD COLUMN IF NOT EXISTS
    puissance_kw NUMERIC(8,2);

-- Ajouter une contrainte
ALTER TABLE equipements ADD CONSTRAINT ck_puissance_positive
    CHECK (puissance_kw IS NULL OR puissance_kw > 0);

-- Renommer (sans toucher aux données)
ALTER TABLE equipements RENAME COLUMN localisation TO site;

-- Changer le type (⚠️ peut réécrire toute la table)
ALTER TABLE equipements ALTER COLUMN site TYPE VARCHAR(80);
-- MySQL : ALTER TABLE equipements MODIFY site VARCHAR(80);

-- Poser / retirer NOT NULL
ALTER TABLE equipements ALTER COLUMN marque SET NOT NULL;    -- PG
ALTER TABLE equipements ALTER COLUMN marque DROP NOT NULL;   -- PG

-- Supprimer une colonne
ALTER TABLE equipements DROP COLUMN IF EXISTS puissance_kw;
```

En production, sur de grosses tables :

1. `ADD COLUMN` avec DEFAULT **non volatile** : instantané sous PG 11+ (métadonnée),
   mais réécrit la table sous MySQL < 8 (ALGORITHM=INPLACE l'évite souvent).
2. Tester sur une copie (`CREATE TABLE test AS SELECT * FROM prod LIMIT 0` + jeu d'essai).
3. Faire les ALTER en heure creuse, avec un backup avant (section 68).
4. ⚠️ `ALTER ... TYPE` avec conversion : `USING` (PG) pour exprimer la conversion.

---

## 45. DROP TABLE et TRUNCATE

```sql
DROP TABLE IF EXISTS maintenances_planifiees;            -- supprime structure + données
DROP TABLE IF EXISTS a, b, c CASCADE;                    -- PG : CASCADE supprime les dépendances (vues, FK)

TRUNCATE TABLE interventions;                            -- vide la table, rapide
TRUNCATE TABLE interventions RESTART IDENTITY;           -- + remet les compteurs à zéro (PG)
-- MySQL : TRUNCATE TABLE interventions;  (remet AUTO_INCREMENT à 1, pas de RESTART IDENTITY)
```

| | `DELETE` sans WHERE | `TRUNCATE` | `DROP` |
|---|---|---|---|
| Vitesse | lent (ligne à ligne) | très rapide | instantané |
| WHERE possible | oui | non | — |
| Déclenche triggers DELETE | oui | non (PG) | — |
| Rollback possible | oui (transaction) | oui sous PG, **non sous MySQL** (DDL implicite) | non |
| Remet les séquences à zéro | non | option | — |

⚠️ Sous MySQL, `TRUNCATE` = DDL → **commit implicite**, impossible à annuler.
En cas de doute sur une table métier : `DELETE` en transaction, vérification, puis `COMMIT`.

---

## 46. Clés primaires (PRIMARY KEY)

```sql
-- En ligne
CREATE TABLE sites (
    id   SERIAL PRIMARY KEY,
    code VARCHAR(10) NOT NULL UNIQUE,
    nom  VARCHAR(80) NOT NULL
);

-- Clé composite
CREATE TABLE interventions_techniciens (
    intervention_id INTEGER NOT NULL REFERENCES interventions(id),
    technicien_id   INTEGER NOT NULL REFERENCES employes(id),
    role            VARCHAR(20) NOT NULL DEFAULT 'principal',
    PRIMARY KEY (intervention_id, technicien_id)
);
```

Propriétés : UNIQUE + NOT NULL, **une seule** par table, crée un index automatiquement.
Choix PK :

- **Surrogate** (id technique auto) : stable, étroite, jamais métier → choix par défaut.
- **Naturelle** (ex. `reference` unique) : parlante mais risque de changement
  (renumérotation d'inventaire → cascade de mises à jour).
- Éviter les PK composites larges : chaque FK et chaque index secondaire les embarque.

---

## 47. Clés étrangères (FOREIGN KEY) — l'intégrité référentielle

```sql
CREATE TABLE interventions (
    ...
    equipement_id INTEGER NOT NULL
        REFERENCES equipements(id)
        ON DELETE RESTRICT      -- refuse la suppression d'un équipement suivi
        ON UPDATE CASCADE,      -- propage un changement d'id (rare avec surrogate)
    ...
);
```

Comportements `ON DELETE` :

| Option | Effet |
|---|---|
| `RESTRICT` / `NO ACTION` | refuse si des enfants existent (défaut, le plus sûr) |
| `CASCADE` | supprime les enfants en cascade ⚠️ |
| `SET NULL` | met la FK à NULL (colonne doit accepter NULL) |
| `SET DEFAULT` | met la valeur par défaut |

```sql
-- Ajouter après coup + nommer la contrainte
ALTER TABLE interventions
    ADD CONSTRAINT fk_interv_equip
    FOREIGN KEY (equipement_id) REFERENCES equipements(id)
    ON DELETE RESTRICT;

-- Trouver les orphelins AVANT de poser la FK (sinon l'ALTER échoue)
SELECT i.* FROM interventions i
LEFT JOIN equipements e ON e.id = i.equipement_id
WHERE e.id IS NULL;
```

⚠️ MySQL : les FK n'existent qu'en **InnoDB** (pas MyISAM !). Vérifier le moteur :
`SHOW TABLE STATUS`. Une FK silencieusement ignorée = corruption logique future.

---

## 48. UNIQUE, CHECK, NOT NULL, DEFAULT

```sql
CREATE TABLE contrats_maintenance (
    id          SERIAL PRIMARY KEY,
    equipement_id INTEGER NOT NULL UNIQUE REFERENCES equipements(id),
                       -- UNIQUE + FK : relation 1-1 (un contrat par équipement)
    prestataire VARCHAR(80) NOT NULL,
    date_debut  DATE NOT NULL,
    date_fin    DATE NOT NULL,
    montant_annuel NUMERIC(10,2) NOT NULL DEFAULT 0 CHECK (montant_annuel >= 0),
    CONSTRAINT ck_dates CHECK (date_fin > date_debut),
    CONSTRAINT uq_presta_periode UNIQUE (prestataire, date_debut)
);
```

| Contrainte | Garantit |
|---|---|
| `NOT NULL` | pas de valeur absente |
| `UNIQUE` | pas de doublon (plusieurs NULL autorisés en standard !) |
| `CHECK` | règle métier au plus près des données |
| `DEFAULT` | valeur si non fournie |

⚠️ `UNIQUE` et NULL : en standard, deux NULL ne sont **pas** considérés égaux →
plusieurs NULL autorisés dans une colonne UNIQUE. (SQL Server fait exception.)
PG 15+ : `UNIQUE NULLS NOT DISTINCT` pour interdire les doublons de NULL.

Philosophie : **les contraintes sont la dernière ligne de défense**.
L'application peut avoir un bug, le script d'import aussi ; la base, elle, refuse
la donnée incohérente. Coût : quasi nul en écriture unitaire, énorme en fiabilité.

---

## 49. INSERT — insérer des données

```sql
-- Toujours lister les colonnes (résiste aux ALTER TABLE)
INSERT INTO equipements (id, reference, type_equip, marque, modele, date_achat, localisation)
VALUES (106, 'SW-001', 'switch', 'Cisco', 'Catalyst 9300', '2024-06-01', 'Salle serveurs');

-- Plusieurs lignes en une fois (1 seul aller-retour réseau)
INSERT INTO equipements (id, reference, type_equip, marque, modele)
VALUES
 (107, 'UPS-004', 'onduleur', 'Vertiv', 'Liebert EXS 30 kVA'),
 (108, 'CPY-002', 'copieur',  'Kyocera','TASKalfa 3554ci');

-- Récupérer l'id généré (PG)
INSERT INTO interventions (equipement_id, technicien_id, type_interv, priorite, description)
VALUES (103, 4, 'preventif', 4, 'Nettoyage + remplacement kit maintenance')
RETURNING id, date_ouverture;
-- MySQL : utiliser LAST_INSERT_ID() après l'INSERT
```

Règles :

- Liste de colonnes **explicite** : protège contre l'ajout d'une colonne.
- `DEFAULT` pour forcer la valeur par défaut : `VALUES (109, 'SRV-002', ..., DEFAULT)`.
- Gros imports : `COPY` (PG) / `LOAD DATA INFILE` (MySQL), pas des millions d'INSERT
  unitaires (facteur ×100 en vitesse).

---

## 50. INSERT ... SELECT — copier/transformer en masse

```sql
-- Archiver les interventions clôturées de plus de 3 ans
INSERT INTO interventions_archive
    (id, equipement_id, technicien_id, date_ouverture, date_cloture, description, cout_eur)
SELECT id, equipement_id, technicien_id, date_ouverture, date_cloture, description, cout_eur
FROM interventions
WHERE date_cloture < CURRENT_DATE - INTERVAL '3 years';
-- MySQL : WHERE date_cloture < CURDATE() - INTERVAL 3 YEAR

-- Peupler une table de reporting
INSERT INTO rapport_mensuel (mois, nb_interv, cout_total)
SELECT DATE_TRUNC('month', date_ouverture), COUNT(*), SUM(cout_eur)
FROM interventions
GROUP BY 1;
```

C'est transactionnel : tout ou rien (section 54). Pour des volumes énormes,
découper par lots (ex. par mois) pour ne pas gonfler le journal de transactions.

---

## 51. UPDATE — modifier (avec prudence)

```sql
-- ✅ Ciblé par PK
UPDATE equipements
SET statut = 'en_maintenance', localisation = 'Atelier'
WHERE id = 102;

-- Mise à jour calculée
UPDATE onduleurs
SET autonomie_min = autonomie_min - 2
WHERE date_dernier_test < CURRENT_DATE - INTERVAL '1 year';
-- MySQL : WHERE date_dernier_test < CURDATE() - INTERVAL 1 YEAR

-- UPDATE avec jointure : répercuter une info
-- PostgreSQL :
UPDATE interventions i
SET sla_respecte = TRUE
FROM equipements e
WHERE e.id = i.equipement_id AND i.date_cloture IS NOT NULL AND e.statut <> 'reforme';

-- MySQL :
UPDATE interventions i
JOIN equipements e ON e.id = i.equipement_id
SET i.sla_respecte = TRUE
WHERE i.date_cloture IS NOT NULL AND e.statut <> 'reforme';
```

**Discipline UPDATE/DELETE** (à afficher au-dessus du clavier) :

1. Écrire d'abord le `SELECT` avec le même WHERE → vérifier le périmètre.
2. Encadrer dans une transaction : `BEGIN; UPDATE...; SELECT...;` puis `COMMIT` ou `ROLLBACK`.
3. Ne **jamais** exécuter sans WHERE sur une table métier (ou alors `WHERE TRUE` écrit
   volontairement pour marquer l'intention... et relu deux fois).

---

## 52. DELETE — supprimer (avec encore plus de prudence)

```sql
-- Supprimer les interventions d'un équipement réformé (après archivage !)
BEGIN;
SELECT COUNT(*) FROM interventions WHERE equipement_id = 109;  -- contrôle
DELETE FROM interventions WHERE equipement_id = 109;
SELECT COUNT(*) FROM interventions WHERE equipement_id = 109;  -- 0 attendu
COMMIT;   -- ou ROLLBACK si le contrôle échoue
```

```sql
-- DELETE avec sous-requête : purger les brouillons sans équipement
DELETE FROM interventions
WHERE equipement_id NOT IN (SELECT id FROM equipements WHERE id IS NOT NULL);
-- (mieux : NOT EXISTS, section 31)

-- PostgreSQL : RETURNING pour auditer ce qu'on supprime
DELETE FROM interventions WHERE date_cloture < DATE '2020-01-01'
RETURNING id, date_ouverture, cout_eur;
```

Alternative sûre au DELETE physique : **suppression logique** (`actif = FALSE`,
`date_suppression`) — conserve l'historique, annulable, mais penser à filtrer
`WHERE actif` dans toutes les requêtes (ou utiliser une vue, section 56).

---

## 53. UPSERT — insérer ou mettre à jour

Synchroniser un inventaire (ex. import GLPI quotidien) sans doublons :

```sql
-- PostgreSQL : ON CONFLICT
INSERT INTO equipements (id, reference, type_equip, marque, modele, statut)
VALUES (101, 'UPS-001', 'onduleur', 'APC', 'Easy UPS 3S 40 kVA', 'en_service')
ON CONFLICT (id) DO UPDATE
SET statut = EXCLUDED.statut,
    modele  = EXCLUDED.modele;
-- Variantes : ON CONFLICT (reference) DO NOTHING  (ignorer les déjà-là)

-- MySQL/MariaDB : ON DUPLICATE KEY UPDATE
INSERT INTO equipements (id, reference, type_equip, marque, modele, statut)
VALUES (101, 'UPS-001', 'onduleur', 'APC', 'Easy UPS 3S 40 kVA', 'en_service')
ON DUPLICATE KEY UPDATE
    statut = VALUES(statut),
    modele = VALUES(modele);
-- MySQL 8.0.20+ : VALUES() déprécié → alias : ... AS n ON DUPLICATE KEY UPDATE statut = n.statut
```

⚠️ `ON CONFLICT` exige une contrainte d'unicité réelle (PK/UNIQUE) sur la cible.
Sans index unique, pas d'arbitre → erreur. C'est voulu : l'upsert n'a de sens
que si « déjà-là » est défini sans ambiguïté.

---

## 54. Transactions et ACID

Une transaction = un lot d'ordres **atomique** : tout est validé ou rien.

```sql
-- Transfert d'un équipement d'un site à l'autre + traçabilité : indissociable
BEGIN;
UPDATE equipements SET localisation = 'Annexe B' WHERE id = 103;
INSERT INTO interventions (equipement_id, technicien_id, type_interv, priorite, description)
VALUES (103, 3, 'installation', 3, 'Déménagement vers annexe B');
COMMIT;      -- tout est validé
-- En cas de problème : ROLLBACK;  (annule tout depuis BEGIN)
```

ACID :

| Lettre | Sens | Concrètement |
|---|---|---|
| **A**tomicité | tout ou rien | panne en milieu de transaction → rien n'est validé |
| **C**ohérence | contraintes respectées | une transaction ne laisse jamais la base incohérente |
| **I**solation | transactions concurrentes isolées | niveaux réglables (section 55) |
| **D**urabilité | validé = persisté | survit au crash (journal WAL / redo log) |

Points de vigilance :

- `SAVEPOINT` : rollback partiel dans une grosse transaction.
- Sous MySQL, certaines instructions (DDL : CREATE/ALTER/DROP/TRUNCATE) provoquent
  un **commit implicite** → découper les migrations.
- Ne jamais laisser une transaction ouverte « en attendant » (verrous conservés →
  blocages en cascade, voir section 69).

```sql
BEGIN;
UPDATE onduleurs SET nb_batteries = 32 WHERE equipement_id = 101;
SAVEPOINT sp1;
UPDATE onduleurs SET puissance_kva = -5 WHERE equipement_id = 101;  -- CHECK va échouer
ROLLBACK TO sp1;    -- annule juste la 2e mise à jour
COMMIT;            -- la 1re est conservée
```

---

## 55. Niveaux d'isolation — comprendre les anomalies

| Niveau | Dirty read | Non-repeatable read | Phantom read | Usage |
|---|---|---|---|---|
| READ UNCOMMITTED | possible | possible | possible | quasi jamais (PG le traite comme READ COMMITTED) |
| READ COMMITTED | non | possible | possible | **défaut PG/MySQL** |
| REPEATABLE READ | non | non | possible (PG : non) | rapports cohérents |
| SERIALIZABLE | non | non | non | criticité max, coût max |

```sql
-- Rapport mensuel cohérent : figer la vision des données
BEGIN ISOLATION LEVEL REPEATABLE READ;
SELECT SUM(cout_eur) FROM interventions WHERE date_ouverture >= '2026-09-01';
-- ... autres requêtes du rapport : mêmes données, même si ça bouge en face ...
COMMIT;

-- Forcer au niveau session (avec parcimonie)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

En pratique sysadmin : le défaut (READ COMMITTED) convient à 95 % des cas.
Passer en REPEATABLE READ pour les exports/reportings multi-requêtes qui doivent
« voir » la même photo. SERIALIZABLE : réservé aux invariants critiques
(ex. ne jamais dépasser un stock) — avec gestion des retries (erreur 40001).

---

## 56. Vues — requêtes stockées

```sql
-- Vue : le parc "vu" par les techniciens (sans les salaires, sans les réformés)
CREATE OR REPLACE VIEW v_parc_actif AS
SELECT e.id, e.reference, e.type_equip, e.marque, e.modele, e.localisation, e.statut,
       o.puissance_kva, o.autonomie_min
FROM equipements e
LEFT JOIN onduleurs o ON o.equipement_id = e.id
WHERE e.statut <> 'reforme';

-- Usage : comme une table
SELECT * FROM v_parc_actif WHERE type_equip = 'onduleur' ORDER BY puissance_kva DESC;
```

Intérêts : masquer la complexité, **sécuriser** (exposer sans GRANT sur les tables de base),
garantir une définition unique (« le parc actif, c'est ÇA »).

Limites :

- Une vue simple est **réévaluée à chaque usage** (pas de stockage).
- Mise à jour via une vue : possible si elle mappe 1-1 à une table (sinon triggers
  `INSTEAD OF` sous PG).
- `CREATE OR REPLACE` ne peut pas changer les colonnes existantes de type incompatible →
  `DROP VIEW` + `CREATE` (attention aux dépendances).

---

## 57. Vues matérialisées — le cache requêtable

```sql
-- PostgreSQL : stocke le résultat, à rafraîchir explicitement
CREATE MATERIALIZED VIEW mv_couts_mensuels AS
SELECT DATE_TRUNC('month', date_ouverture) AS mois,
       type_interv, COUNT(*) AS nb, SUM(cout_eur) AS cout_total
FROM interventions
GROUP BY 1, 2;

CREATE UNIQUE INDEX ON mv_couts_mensuels (mois, type_interv);

-- Rafraîchir (chaque nuit via cron/pg_cron par ex.)
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_couts_mensuels;
```

| | Vue simple | Vue matérialisée |
|---|---|---|
| Stockage | non | oui (table physique) |
| Fraîcheur | temps réel | au dernier REFRESH |
| Vitesse lecture | = requête sous-jacente | très rapide |
| Écriture | — | coût au REFRESH |

⚠️ **Portabilité** — MySQL/MariaDB : **pas** de vues matérialisées natives.
Émulation : table + `INSERT/REPLACE` périodique via EVENT, ou outil externe.
`CONCURRENTLY` (PG) : rafraîchit sans bloquer les lectures (exige un index unique).

Cas d'usage : tableaux de bord (Grafana/Metabase) sur des agrégats lourds,
requêtes répétées à l'identique.

---

## 58. Index — quand et pourquoi

Un index = une structure (souvent B-tree) qui évite de **scanner toute la table**.

```sql
-- Index sur les colonnes filtrées/jointes
CREATE INDEX idx_interv_equipement ON interventions (equipement_id);
CREATE INDEX idx_interv_date       ON interventions (date_ouverture DESC);
CREATE INDEX idx_equip_statut_type ON interventions (statut, type_equip);  -- composite

-- Index unique = contrainte + performance
CREATE UNIQUE INDEX uq_equip_reference ON equipements (reference);

-- Index partiel (PG) : n'indexer que ce qui sert
CREATE INDEX idx_interv_en_cours ON interventions (date_ouverture)
    WHERE date_cloture IS NULL;

-- Index d'expression (PG) : requêtes avec LOWER()
CREATE INDEX idx_equip_marque_lower ON equipements (LOWER(marque));
```

Quand indexer :

- ✅ Colonnes de `JOIN` (FK), de `WHERE` sélectif, de `ORDER BY` / `GROUP BY`.
- ✅ Contraintes UNIQUE/PK (automatique).
- ❌ Colonnes à faible cardinalité seules (`statut` avec 4 valeurs sur 10 M de lignes :
  l'index ne sert presque jamais — sauf partiel).
- ❌ Tables minuscules (< quelques milliers de lignes) : le scan séquentiel est plus rapide.
- ❌ Colonnes écrites intensivement et jamais lues par filtre : chaque INSERT/UPDATE
  paie le coût de maintenance de l'index.

Sur-indexer ralentit les écritures ; sous-indexer tue les lectures.
**Mesurer** avec EXPLAIN (section 59), pas au doigt mouillé.

⚠️ MySQL : les index sur expressions exigent MySQL 8.0.13+ (colonnes générées avant).
Les index partiels n'existent pas sous MySQL (index complet seulement).

---

## 59. Lire un EXPLAIN — diagnostiquer une requête lente

```sql
-- PostgreSQL
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT e.reference, COUNT(i.id)
FROM equipements e
LEFT JOIN interventions i ON i.equipement_id = e.id
GROUP BY e.reference;
```

Ce qu'il faut y voir :

| Nœud | Signification | Bon / mauvais signe |
|---|---|---|
| `Seq Scan` | lit toute la table | OK si petite table ou gros % lu ; sinon suspect |
| `Index Scan` / `Index Only Scan` | utilise un index | bon signe |
| `Bitmap Heap Scan` | index + accès table | normal pour sélectivité moyenne |
| `Nested Loop` | boucle imbriquée | bien pour petits volumes ; catastrophique si l'intérieur est gros |
| `Hash Join` / `Merge Join` | jointures par hachage/tri | normal sur gros volumes |
| `Sort` | tri explicite | vérifier qu'un index ne l'éviterait pas |

```sql
-- MySQL
EXPLAIN FORMAT=TREE
SELECT e.reference, COUNT(i.id)
FROM equipements e
LEFT JOIN interventions i ON i.equipement_id = e.id
GROUP BY e.reference;
-- Colonnes clés : type (ALL = scan complet !), key (index utilisé), rows (estimation), Extra
```

Méthode express (5 minutes) :

1. `EXPLAIN` la requête → repérer `Seq Scan`/`ALL` sur de grosses tables.
2. Vérifier que les colonnes de JOIN/WHERE ont un index (section 58).
3. Regarder les **estimations vs réel** (`ANALYZE`) : un écart ×100 = statistiques
   obsolètes → `ANALYZE table;` (PG) / `ANALYZE TABLE` (MySQL).
4. Chercher les fonctions sur colonnes indexées (`WHERE LOWER(x) = ...` tue l'index
   simple → index d'expression ou normalisation à l'écriture).

---

## 60. Optimisation — les 10 réflexes avant de toucher au hardware

1. **Indexer les jointures** : toute FK filtrée/jointe mérite un index.
2. **Éviter `SELECT *`** : moins de colonnes = moins d'I/O, index couvrants possibles.
3. **Filtrer tôt** : WHERE sélectif avant JOIN quand c'est équivalent.
4. **Pas de fonction sur colonne indexée** dans WHERE : `WHERE YEAR(d) = 2026` →
   `WHERE d >= '2026-01-01' AND d < '2027-01-01'`.
5. **LIMIT dès que possible** sur les requêtes d'exploration.
6. **EXISTS > IN** pour les sous-requêtes corrélées/volumineuses (souvent équivalent
   pour l'optimiseur moderne, mais EXISTS documente l'intention).
7. **Éviter les `OR` sur plusieurs colonnes** : `WHERE a = 1 OR b = 2` casse l'indexage →
   réécrire en `UNION` de deux requêtes indexées.
8. **Pagination keyset** au lieu d'`OFFSET` profond (section 11).
9. **Agréger en base**, pas dans l'application : `GROUP BY` côté SQL, pas 10 000 lignes
   ramenées en Python pour compter.
10. **Statistiques à jour** : `ANALYZE` régulier (autovacuum le fait sous PG ;
    vérifier qu'il n'est pas désactivé).

```sql
-- ❌ Lent : fonction sur colonne indexée + OR
SELECT * FROM interventions
WHERE YEAR(date_ouverture) = 2026 OR priorite = 1;

-- ✅ Rapide : plage sargable + UNION
SELECT * FROM interventions
WHERE date_ouverture >= '2026-01-01' AND date_ouverture < '2027-01-01'
UNION
SELECT * FROM interventions
WHERE priorite = 1 AND NOT (date_ouverture >= '2026-01-01' AND date_ouverture < '2027-01-01');
```

---

## 61. Dépannage — « la requête rame » pas à pas

Checklist d'intervention (dans l'ordre) :

- [ ] **1. Reproduire** : la lenteur est-elle constante ? (`EXPLAIN ANALYZE` 2-3 fois)
- [ ] **2. Isoler** : quelle partie ? Découper la requête (CTE par CTE, enlever les JOIN un par un).
- [ ] **3. Plan** : `Seq Scan` sur grosse table ? Mauvaises estimations (rows estimés vs réels) ?
- [ ] **4. Index** : colonnes de JOIN/WHERE indexées ? Index **utilisé** (pas seulement existant) ?
- [ ] **5. Verrous** : une autre session bloque ? (PG : `pg_locks` + `pg_stat_activity` ; MySQL : `SHOW ENGINE INNODB STATUS`, `performance_schema`)
- [ ] **6. Ressources** : I/O disque saturé ? `work_mem` trop petit → tri sur disque (`Sort Method: external merge` dans EXPLAIN) ?
- [ ] **7. Statistiques** : `ANALYZE` récent ? (table venant de subir un gros import ?)

Requêtes de diagnostic :

```sql
-- PostgreSQL : requêtes en cours et leur durée
SELECT pid, now() - query_start AS duree, state, LEFT(query, 120)
FROM pg_stat_activity
WHERE state <> 'idle'
ORDER BY duree DESC;

-- PostgreSQL : qui bloque qui
SELECT blocked.pid AS pid_bloque, blocking.pid AS pid_bloquant,
       blocked.query AS requete_bloquee
FROM pg_locks blocked
JOIN pg_locks blocking ON blocking.locktype = blocked.locktype
    AND blocking.pid <> blocked.pid;

-- MySQL : processus en cours
SHOW FULL PROCESSLIST;
-- MySQL : transactions InnoDB qui attendent
SELECT * FROM information_schema.INNODB_TRX ORDER BY trx_started;
```

Tuer une requête runaway (avec discernement) :

```sql
-- PostgreSQL
SELECT pg_cancel_query(12345);      -- poli : annule la requête
SELECT pg_terminate_backend(12345); -- brutal : tue la connexion
-- MySQL
KILL 12345;            -- idem
KILL QUERY 12345;      -- tue juste la requête
```

---

## 62. Sécurité — comprendre l'injection SQL

L'injection = concaténer une entrée utilisateur **dans** la requête. Exemple d'horreur :

```python
# ❌ JAMAIS ÇA (exemple Python)
saisie = input("Référence : ")          # l'attaquant tape : ' OR '1'='1
query = f"SELECT * FROM equipements WHERE reference = '{saisie}'"
# Requête réelle : SELECT * FROM equipements WHERE reference = '' OR '1'='1'
# → renvoie TOUTE la table. Variante : '; DROP TABLE employes; --
```

Ce qui est en jeu : lecture de toutes les données, modification, suppression,
voire exécution système selon la configuration. **Toute** entrée (formulaire, API,
fichier d'import, nom de fichier !) est suspecte.

Défenses en profondeur :

1. **Requêtes paramétrées** (section 63) — la défense n°1, non négociable.
2. Principe du moindre privilège : le compte applicatif n'a que les droits nécessaires
   (SELECT sur certaines tables, jamais DROP/ALTER ; voir section 64).
3. Ne jamais exposer les messages d'erreur SQL bruts à l'utilisateur final.
4. Validation des entrées côté application (type, longueur, format) — complément, pas substitut.
5. Pas de construction dynamique de SQL avec des identifiants (noms de tables/colonnes)
   venus de l'utilisateur ; si inévitable : liste blanche stricte.

---

## 63. Requêtes paramétrées — la bonne pratique (exemples)

Le principe : la requête est envoyée **une fois** avec des placeholders,
les valeurs **séparément** — le SGBD ne les confond jamais avec du code.

```python
# ✅ Python + psycopg (PostgreSQL) — placeholder %s
import psycopg
with psycopg.connect("dbname=sql_formation") as conn:
    with conn.cursor() as cur:
        cur.execute(
            "SELECT * FROM equipements WHERE reference = %s AND statut = %s",
            ("UPS-001", "en_service"),   # valeurs : jamais concaténées
        )
        rows = cur.fetchall()

# ✅ Python + mysql-connector — placeholder %s aussi (ne pas confondre avec le % Python !)
# cur.execute("SELECT * FROM equipements WHERE id = %s", (101,))
```

```php
<?php
// ✅ PHP + PDO — placeholders nommés
$pdo = new PDO($dsn, $user, $pass, [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]);
$stmt = $pdo->prepare("SELECT * FROM interventions WHERE technicien_id = :tech AND priorite <= :prio");
$stmt->execute(['tech' => 2, 'prio' => 2]);
$rows = $stmt->fetchAll();
?>
```

```bash
# ✅ Shell : psql avec variables (pas d'interpolation bash dans le SQL !)
psql -d sql_formation -v ref="UPS-001" -c "SELECT * FROM equipements WHERE reference = :'ref';"
# ⚠️ :'ref' (avec apostrophes) = valeur quotée ; :ref sans apostrophes = identifiant
```

Règle : **si vous voyez une concaténation de variable dans du SQL, c'est un bug de sécurité.**
Chercher `+ "` / `f"` / `. "` dans le code = audit express.

---

## 64. Droits et rôles — moindre privilège

```sql
-- Rôle applicatif lecture seule (dashboard Grafana, exports)
CREATE ROLE lecture_seule LOGIN PASSWORD 'mot_de_passe_solide';
GRANT CONNECT ON DATABASE sql_formation TO lecture_seule;
GRANT USAGE ON SCHEMA public TO lecture_seule;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lecture_seule;
-- Pour les futures tables :
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO lecture_seule;

-- Rôle technicien : lire + insérer des interventions, pas toucher au reste
CREATE ROLE technicien LOGIN PASSWORD '...';
GRANT SELECT ON equipements, employes TO technicien;
GRANT SELECT, INSERT, UPDATE ON interventions TO technicien;
-- MySQL :
-- CREATE USER 'technicien'@'%' IDENTIFIED BY '...';
-- GRANT SELECT ON sql_formation.equipements TO 'technicien'@'%';

-- Révoquer
REVOKE DELETE ON interventions FROM technicien;
DROP ROLE IF EXISTS ancien_prestataire;
```

Bonnes pratiques :

- Un compte **par usage** (appli, dashboard, humain, backup) — jamais le compte
  `postgres`/`root` dans une appli.
- Le compte de **sauvegarde** : SELECT + droits de verrouillage, rien d'autre.
- Auditer périodiquement : PG `\du` / `SELECT * FROM information_schema.role_table_grants;`,
  MySQL `SHOW GRANTS FOR 'user'@'host';`.

---

## 65. Sauvegarde — rappels opérationnels

Le DBA/sysadmin retient : **on ne sauvegarde pas des fichiers, on sauvegarde la capacité
à restaurer**. Tester la restauration, sinon la sauvegarde n'existe pas.

```bash
# PostgreSQL : dump logique (SQL rejouable)
pg_dump -d sql_formation -F p -f sauvegarde_$(date +%F).sql        # format texte
pg_dump -d sql_formation -F c -f sauvegarde_$(date +%F).dump       # format custom (compressé)

# Restauration
psql -d sql_formation_restauree -f sauvegarde_2026-09-26.sql
pg_restore -d sql_formation_restauree sauvegarde_2026-09-26.dump

# MySQL / MariaDB
mysqldump --single-transaction --routines --triggers sql_formation > sauvegarde_$(date +%F).sql
mysql sql_formation_restauree < sauvegarde_2026-09-26.sql
```

| Option | Pourquoi |
|---|---|
| `--single-transaction` (mysqldump) | dump cohérent sans verrouiller (InnoDB) |
| `--routines --triggers` | sinon procédures et triggers **perdus** |
| `-F c` (pg_dump custom) | compressé + restauration sélective (`-t table`) |

Stratégie minimale viable :

- [ ] Dump logique **quotidien** (rétention 7 j) + **hebdo** (rétention 4 sem.) + **mensuel** (1 an).
- [ ] Dump **avant** chaque migration/ALTER sur table métier.
- [ ] Test de restauration **mensuel** sur une base à part, chronométré (connaître son RTO).
- [ ] Sauvegarde **hors site** (règle 3-2-1 : 3 copies, 2 supports, 1 hors site).
- [ ] Chiffrer les dumps contenant des données personnelles/salaires.
- [ ] Bonus PG : archivage WAL + `pg_basebackup` pour du point-in-time-recovery (PITR).

---

## 66. Dix erreurs classiques (et comment les éviter)

**Erreur 1 — `= NULL` au lieu de `IS NULL`.**
Résultat : zéro ligne, sans erreur. Réflexe : NULL ne se compare pas, il se teste.

**Erreur 2 — `NOT IN` avec un NULL dans la liste.**
Résultat : zéro ligne. Réflexe : `NOT EXISTS` (section 31).

**Erreur 3 — Oublier le WHERE sur UPDATE/DELETE.**
Résultat : table entière modifiée. Réflexe : transaction + SELECT de contrôle d'abord (sections 51-52).

**Erreur 4 — Division entière.**
`COUNT(*) * 100 / COUNT(*)` = 0 sous PG. Réflexe : `* 100.0` ou CAST (section 15).

**Erreur 5 — `GROUP BY` incomplet accepté par MySQL laxiste.**
Résultat : valeurs arbitraires silencieuses. Réflexe : `ONLY_FULL_GROUP_BY` activé, GROUP BY complet (section 20).

**Erreur 6 — LEFT JOIN + WHERE sur la table de droite.**
Résultat : INNER JOIN déguisé. Réflexe : condition dans le `ON` (section 24).

**Erreur 7 — `COUNT(*)` après LEFT JOIN pour compter les enfants.**
Résultat : 1 au lieu de 0 pour les parents sans enfant. Réflexe : `COUNT(table_droite.id)` (section 24).

**Erreur 8 — BETWEEN sur TIMESTAMP pour un mois.**
`BETWEEN '2026-09-01' AND '2026-09-30'` rate le 30 septembre après minuit.
Réflexe : `>= '2026-09-01' AND < '2026-10-01'` (section 6).

**Erreur 9 — `SELECT *` en production / dans une vue.**
Résultat : colonnes surprises après ALTER, perfs dégradées. Réflexe : liste explicite (section 4).

**Erreur 10 — Concaténer des variables dans le SQL.**
Résultat : injection SQL (section 62). Réflexe : requêtes paramétrées, toujours (section 63).

**Erreur 11 — Comparer des dates en texte non ISO.**
`'26/09/2026' > '01/10/2026'` est TRUE en tri alphabétique ! Réflexe : type DATE + format ISO (section 16).

**Erreur 12 — Oublier que les agrégats ignorent NULL.**
`AVG(cout_eur)` sur (100, NULL) = 100, pas 50. Réflexe : COALESCE si la sémantique l'exige (section 19).

---

## 67. Piège : les doublons silencieux

Les doublons viennent de 3 sources : données dupliquées, JOIN en éventail (fan-out),
UNION ALL vs UNION.

```sql
-- 1. Détecter les doublons métier (même référence 2 fois)
SELECT reference, COUNT(*) AS nb
FROM equipements
GROUP BY reference
HAVING COUNT(*) > 1;

-- 2. Le fan-out : joindre 2 tables "plusieurs" fait exploser les lignes
-- Un équipement avec 3 interventions ET 2 contrats → 3 × 2 = 6 lignes !
-- → agréger AVANT de joindre :
WITH nb_i AS (
    SELECT equipement_id, COUNT(*) AS nb_interv FROM interventions GROUP BY 1
),
nb_c AS (
    SELECT equipement_id, COUNT(*) AS nb_contrats FROM contrats_maintenance GROUP BY 1
)
SELECT e.reference, COALESCE(nb_i.nb_interv, 0), COALESCE(nb_c.nb_contrats, 0)
FROM equipements e
LEFT JOIN nb_i ON nb_i.equipement_id = e.id
LEFT JOIN nb_c ON nb_c.equipement_id = e.id;

-- 3. Dédupliquer en gardant une ligne (la plus récente) — PostgreSQL
DELETE FROM interventions a
USING interventions b
WHERE a.id > b.id
  AND a.equipement_id = b.equipement_id
  AND a.date_ouverture = b.date_ouverture;
-- MySQL : même logique en auto-jointure DELETE a FROM interventions a JOIN interventions b ON ...
```

Réflexe : après tout JOIN multiple, `COUNT(*)` de contrôle vs `COUNT(DISTINCT id)`
de la table de faits. Écart = fan-out.

---

## 68. Piège : GROUP BY abusif et agrégats trompeurs

```sql
-- ❌ Moyenner des moyennes : FAUX (les groupes n'ont pas le même poids)
SELECT AVG(moy_par_type) FROM (
    SELECT type_interv, AVG(cout_eur) AS moy_par_type FROM interventions GROUP BY 1
) t;

-- ✅ La vraie moyenne globale
SELECT SUM(cout_eur) * 1.0 / COUNT(cout_eur) FROM interventions;
-- = AVG(cout_eur) tout simplement. La moyenne des moyennes n'est juste que si
-- les groupes ont tous le même effectif — rarement le cas.

-- ❌ Sommer après un JOIN qui duplique (fan-out, section 67)
SELECT e.reference, SUM(i.cout_eur)
FROM equipements e JOIN interventions i ON i.equipement_id = e.id
GROUP BY e.reference;   -- OK ici (1 seul "plusieurs")...

-- ...mais avec 2 tables plusieurs, les sommes sont multipliées. Toujours agréger avant de joindre.
```

Autre classique : `HAVING` sans `GROUP BY` (groupe unique implicite — valide mais obscur),
et `GROUP BY` sur une colonne calculée différemment entre SELECT et GROUP BY
(PG exige la même expression ; utiliser l'alias via CTE ou la position `GROUP BY 1`).

---

## 69. Piège : le problème N+1

Le N+1 : 1 requête pour la liste + N requêtes pour le détail de chaque ligne.
Côté application (boucle), pas côté SQL pur — mais c'est le sysadmin qui le voit
dans les logs : 1 001 requêtes pour afficher 1 000 équipements.

```python
# ❌ N+1 : 1 + N requêtes
equips = db.execute("SELECT id, reference FROM equipements").fetchall()
for e in equips:
    nb = db.execute("SELECT COUNT(*) FROM interventions WHERE equipement_id = %s", (e.id,))
```

```sql
-- ✅ 1 seule requête : le JOIN + GROUP BY fait le travail
SELECT e.id, e.reference, COUNT(i.id) AS nb_interv
FROM equipements e
LEFT JOIN interventions i ON i.equipement_id = e.id
GROUP BY e.id, e.reference;
```

Signes dans les logs : des centaines de requêtes identiques à un paramètre près,
en rafale. Correction : jointure, `IN (...)` avec la liste des ids, ou chargement
eager de l'ORM (`selectinload`/`prefetch_related`).

---

## 70. Bonnes pratiques — nommage et style

```sql
-- ✅ Style recommandé dans ce guide
SELECT e.reference,
       e.modele,
       COUNT(i.id) AS nb_interventions
FROM   equipements AS e
LEFT   JOIN interventions AS i
       ON i.equipement_id = e.id
WHERE  e.statut = 'en_service'
GROUP  BY e.reference, e.modele
HAVING COUNT(i.id) > 0
ORDER  BY nb_interventions DESC;
```

Conventions :

- **Tables** : pluriel, minuscules, snake_case (`interventions`, `contrats_maintenance`).
- **Colonnes** : snake_case, sans accent, sans espace (`date_ouverture`, pas `Date d'ouverture`).
- **PK** : `id` (simple). **FK** : `<table_singulier>_id` (`equipement_id`).
- **Booléens** : adjectif/participe (`actif`, `sla_respecte`, `realisee`) — pas `flag1`.
- **Mots-clés** en MAJUSCULES, un par ligne pour les clauses principales.
- **Alias** courts mais lisibles (`e`, `i`, `emp` — pas `a`, `b`, `c`).
- **Virgules en début de ligne** ou fin : choisir UNE convention d'équipe et s'y tenir
  (début de ligne = diffs git plus propres).
- Commenter l'**intention** (`-- SLA : interventions critiques non clôturées`),
  pas l'évidence (`-- sélectionne les employés`).

---

## 71. Bonnes pratiques — requêtes et exploitation

- [ ] `SELECT` explicite (pas de `*`) sauf exploration ad hoc.
- [ ] `WHERE` + `ORDER BY` déterministe (+ PK) avant tout `LIMIT`/`OFFSET`.
- [ ] Requêtes paramétrées dès qu'une valeur vient de l'extérieur (section 63).
- [ ] Transactions pour tout lot multi-instructions (sections 51-54).
- [ ] `EXPLAIN` avant de mettre en production une requête sur table volumineuse.
- [ ] Tester sur un **échantillon** (`TABLESAMPLE` PG / `LIMIT`) avant le full scan.
- [ ] Nommer les contraintes pour des erreurs lisibles (section 42).
- [ ] Dater/ signer les scripts : en-tête `-- auteur, date, objet, ticket`.
- [ ] Versionner les scripts SQL (git) comme le reste du code.
- [ ] Ne jamais lancer un script DDL/DML trouvé sur internet sans le lire :
  chercher `DROP`, `DELETE`, `TRUNCATE`, `;--` dedans.

```sql
-- En-tête type d'un script d'exploitation
-- =====================================================================
-- Objet   : Purge des interventions clôturées depuis plus de 3 ans
-- Auteur  : Service Systèmes & Énergies
-- Date    : 2026-09-26
-- Ticket  : GLPI #4521
-- Cible   : sql_formation (PROD après validation sur PREPROD)
-- Rollback: restaurer depuis sauvegarde_2026-09-26.dump
-- =====================================================================
BEGIN;
-- 1. Contrôle du périmètre
SELECT COUNT(*) FROM interventions
WHERE date_cloture < CURRENT_DATE - INTERVAL '3 years';
-- 2. (après validation du count) DELETE ...
-- COMMIT;
```

---

## 72. Cas pratique 1 — Tableau de bord du service (commenté)

Besoin : pour la réunion hebdo, un tableau par technicien : interventions ouvertes,
clôturées, coût total, respect SLA, ancienneté de la plus vieille ouverte.

```sql
WITH stats AS (
    SELECT
        technicien_id,
        -- pivot manuel portable (section 17)
        SUM(CASE WHEN date_cloture IS NULL THEN 1 ELSE 0 END) AS ouvertes,
        SUM(CASE WHEN date_cloture IS NOT NULL THEN 1 ELSE 0 END) AS cloturees,
        COALESCE(SUM(cout_eur), 0) AS cout_total,
        -- % SLA sur les clôturées évaluées (évite la division entière, section 15)
        CASE WHEN COUNT(*) FILTER (WHERE sla_respecte IS NOT NULL) = 0 THEN NULL
             ELSE SUM(CASE WHEN sla_respecte THEN 1 ELSE 0 END) * 100.0
                / COUNT(*) FILTER (WHERE sla_respecte IS NOT NULL)
        END AS pct_sla,
        MIN(CASE WHEN date_cloture IS NULL THEN date_ouverture END) AS plus_vieille_ouverte
    FROM interventions
    WHERE technicien_id IS NOT NULL
    GROUP BY technicien_id
)
SELECT
    e.nom || ' ' || e.prenom AS technicien,   -- PG ; MySQL : CONCAT(e.nom,' ',e.prenom)
    COALESCE(s.ouvertes, 0)  AS ouvertes,
    COALESCE(s.cloturees, 0) AS cloturees,
    COALESCE(s.cout_total, 0) AS cout_total_eur,
    ROUND(s.pct_sla, 1) AS sla_pct,
    s.plus_vieille_ouverte,
    -- alerte : une ouverte depuis plus de 7 jours
    CASE WHEN s.plus_vieille_ouverte < NOW() - INTERVAL '7 days'
         THEN '⚠️ À relancer' ELSE 'OK' END AS vigilance
FROM employes e
LEFT JOIN stats s ON s.technicien_id = e.id
WHERE e.actif
ORDER BY ouvertes DESC, e.nom;
```

Points à noter : `COALESCE` après LEFT JOIN (les techniciens sans stats ont des NULL),
`FILTER` PG remplacé par `CASE` si besoin de portabilité stricte, `ROUND(pct,1)`
pour un affichage propre.

---

## 73. Cas pratique 2 — Plan de maintenance des onduleurs

Besoin : lister les onduleurs dont le **test batterie** date de plus de 6 mois
ou n'a jamais eu lieu, avec criticité selon la puissance.

```sql
SELECT
    e.reference,
    e.localisation,
    o.puissance_kva,
    o.date_dernier_test,
    -- jours depuis le dernier test (portable via les deux variantes)
    CURRENT_DATE - o.date_dernier_test AS jours_depuis_test,   -- PG : entier
    CASE
        WHEN o.date_dernier_test IS NULL THEN '🔴 Jamais testé'
        WHEN o.date_dernier_test < CURRENT_DATE - INTERVAL '6 months' AND o.puissance_kva >= 30
            THEN '🔴 Critique : gros onduleur non testé depuis 6 mois'
        WHEN o.date_dernier_test < CURRENT_DATE - INTERVAL '6 months'
            THEN '🟠 À planifier'
        ELSE '🟢 À jour'
    END AS priorite_maintenance,
    -- prochaine échéance : test annuel glissant
    o.date_dernier_test + INTERVAL '1 year' AS prochain_test  -- PG ; MySQL : DATE_ADD(..., INTERVAL 1 YEAR)
FROM equipements e
JOIN onduleurs o ON o.equipement_id = e.id
WHERE e.statut <> 'reforme'
  AND (o.date_dernier_test IS NULL
       OR o.date_dernier_test < CURRENT_DATE - INTERVAL '6 months')
ORDER BY
    CASE WHEN o.date_dernier_test IS NULL THEN 0 ELSE 1 END,  -- jamais testés d'abord
    o.puissance_kva DESC;
```

Variante MySQL de `CURRENT_DATE - date` : `DATEDIFF(CURRENT_DATE, o.date_dernier_test)`.

---

## 74. Cas pratique 3 — Suivi du parc copieurs (coûts par page)

Besoin : coût au millier de pages par copieur pour arbitrer les renouvellements.
(On suppose une table `compteurs_copieurs(copieur_id, date_releve, pages_total)`.)

```sql
-- Consommation entre deux relevés consécutifs (LAG, section 38)
WITH releves AS (
    SELECT
        copieur_id,
        date_releve,
        pages_total,
        pages_total - LAG(pages_total) OVER (
            PARTITION BY copieur_id ORDER BY date_releve
        ) AS pages_periode
    FROM compteurs_copieurs
),
couts AS (
    SELECT equipement_id, SUM(cout_eur) AS cout_total
    FROM interventions
    WHERE type_interv = 'curatif'
    GROUP BY equipement_id
)
SELECT
    e.reference,
    e.modele,
    SUM(r.pages_periode) AS pages_imprimees,
    COALESCE(c.cout_total, 0) AS cout_curatif_eur,
    -- coût pour 1000 pages (protégé contre la division par zéro)
    CASE WHEN SUM(r.pages_periode) > 0
         THEN ROUND(COALESCE(c.cout_total, 0) * 1000.0 / SUM(r.pages_periode), 2)
    END AS cout_pour_1000_pages
FROM equipements e
LEFT JOIN releves r ON r.copieur_id = e.id
LEFT JOIN couts c   ON c.equipement_id = e.id
WHERE e.type_equip = 'copieur'
GROUP BY e.reference, e.modele, c.cout_total
HAVING SUM(r.pages_periode) > 0
ORDER BY cout_pour_1000_pages DESC NULLS LAST;
```

Lecture : en tête du classement = les copieurs les plus chers à la page →
candidats au remplacement ou à la renégociation du contrat de maintenance.

---

## 75. Cas pratique 4 — Reporting mensuel automatisable

Besoin : un rapport mensuel unique, rejouable, qui sort : volume, coûts,
délais moyens de clôture, top 3 des équipements les plus coûteux.

```sql
-- Paramètre : mois à traiter (à changer chaque mois, ou via variable psql)
-- psql : psql -v mois="2026-09-01" -f rapport.sql  → :'mois'
WITH perimetre AS (
    SELECT * FROM interventions
    WHERE date_ouverture >= DATE '2026-09-01'
      AND date_ouverture <  DATE '2026-10-01'
),
delais AS (
    SELECT *,
           -- durée en heures (PG) ; MySQL : TIMESTAMPDIFF(MINUTE,...)/60.0
           EXTRACT(EPOCH FROM (date_cloture - date_ouverture)) / 3600.0 AS delai_h
    FROM perimetre
    WHERE date_cloture IS NOT NULL
)
SELECT 'volume' AS indicateur, COUNT(*)::TEXT AS valeur FROM perimetre
UNION ALL
SELECT 'cout_total_eur', COALESCE(SUM(cout_eur), 0)::TEXT FROM perimetre
UNION ALL
SELECT 'delai_moyen_cloture_h', COALESCE(AVG(delai_h), 0)::TEXT FROM delais
UNION ALL
SELECT 'pct_sla_respecte',
       (SUM(CASE WHEN sla_respecte THEN 1 ELSE 0 END) * 100.0
        / NULLIF(COUNT(*) FILTER (WHERE sla_respecte IS NOT NULL), 0))::TEXT
FROM perimetre;
-- Top 3 équipements (requête séparée, même périmètre)
SELECT e.reference, e.modele, COUNT(p.id) AS nb, SUM(p.cout_eur) AS cout
FROM perimetre p
JOIN equipements e ON e.id = p.equipement_id
GROUP BY e.reference, e.modele
ORDER BY cout DESC NULLS LAST
LIMIT 3;
```

`NULLIF(x, 0)` : retourne NULL si x = 0 → évite la division par zéro **et**
signale « pas de données » (NULL) plutôt qu'un 0 trompeur.

---

## 76. Pense-bête de poche — une page

```
SELECT      colonnes | * (éviter en prod)
FROM        table [AS alias]
JOIN        table ON condition      (INNER / LEFT / RIGHT / FULL / CROSS)
WHERE       filtres lignes          (AND > OR : parenthèses !)
GROUP BY    colonnes de regroupement
HAVING      filtres sur groupes      (après agrégation)
ORDER BY    col [ASC|DESC] [, id]    (toujours PK en dernier pour paginer)
LIMIT n [OFFSET m]                   (OFFSET profond → keyset)

NULL :  IS NULL / IS NOT NULL  (jamais = NULL)
        COALESCE(a, 0)  |  NOT IN ⚠️ NULL → NOT EXISTS
Texte : || (PG) / CONCAT (MySQL) | UPPER LOWER TRIM SUBSTRING LIKE '%motif%'
Dates : CURRENT_DATE | + INTERVAL '7 days' (PG) / DATE_ADD (MySQL)
        mois : >= '2026-09-01' AND < '2026-10-01'
CASE  : CASE WHEN cond THEN x ELSE y END
Agrég : COUNT(*) COUNT(col) SUM AVG MIN MAX (ignorent NULL sauf COUNT(*))
Fenet : ROW_NUMBER() RANK() LAG() LEAD() SUM() OVER (PARTITION BY x ORDER BY y)
CTE   : WITH etape AS (SELECT ...) SELECT * FROM etape
DDL   : CREATE TABLE / ALTER TABLE ADD|DROP COLUMN / DROP TABLE / TRUNCATE
DML   : INSERT (colonnes !) / UPDATE ... WHERE (SELECT d'abord !) / DELETE ... WHERE
TXN   : BEGIN; ... COMMIT;  / ROLLBACK;   (MySQL : DDL = commit implicite !)
Sécur : requêtes PARAMÉTRÉES, jamais de concaténation
Sauve : pg_dump -Fc / mysqldump --single-transaction --routines --triggers
Diag  : EXPLAIN (ANALYZE, BUFFERS) → Seq Scan ? index manquant ? ANALYZE ?
```

À garder sous les yeux : **NULL ≠ 0**, **WHERE avant GROUP BY avant HAVING**,
**condition OUTER JOIN dans le ON**, **paramétrer les entrées**, **backup avant ALTER**.

---

## 77. Glossaire — les 60 termes à connaître

| Terme | Définition courte |
|---|---|
| ACID | Atomicité, Cohérence, Isolation, Durabilité : garanties d'une transaction |
| Agrégat | Fonction réduisant un ensemble de lignes (COUNT, SUM, AVG, MIN, MAX) |
| Alias | Nom temporaire donné à une table/colonne dans une requête (AS) |
| Anti-join | Requête « les X sans Y » (NOT EXISTS / LEFT JOIN ... IS NULL) |
| Autovacuum | Processus PG qui nettoie et met à jour les stats automatiquement |
| B-tree | Structure d'index par défaut, efficace pour =,<,>,BETWEEN, tri |
| Cardinalité | Nombre de valeurs distinctes d'une colonne |
| CASCADE | Propagation d'une suppression/mise à jour via les FK |
| CHECK | Contrainte validant une règle métier sur les données |
| CTE | Common Table Expression : requête nommée avec WITH |
| Curseur (keyset) | Pagination par « après l'id X » au lieu d'OFFSET |
| DDL / DML / DQL / DCL / TCL | Familles d'ordres SQL (section 2) |
| Deadlock | Deux transactions qui s'attendent mutuellement → l'une est tuée |
| DISTINCT | Déduplication des lignes du résultat |
| Dump | Export logique d'une base (SQL ou custom) |
| EXPLAIN | Plan d'exécution estimé (ANALYZE = réel) |
| Fan-out | Multiplication des lignes par jointure de 2 tables « plusieurs » |
| Fenêtre (fonction) | Calcul sur un ensemble lié à la ligne courante, sans réduire les lignes |
| FK (clé étrangère) | Colonne référençant la PK d'une autre table |
| FULL OUTER JOIN | Garde toutes les lignes des deux tables (émulé sous MySQL) |
| GROUP BY | Regroupement avant agrégation |
| HAVING | Filtre sur les groupes (après agrégation) |
| IDENTITÉ | Colonne auto-incrémentée standard (GENERATED ... AS IDENTITY) |
| Index | Structure accélérant les recherches (B-tree, partiel, d'expression...) |
| Index couvrant | Index contenant toutes les colonnes requises (Index Only Scan) |
| Injection SQL | Exécution de code via une entrée concaténée dans la requête |
| Isolation (niveau) | Degré de visibilité entre transactions concurrentes |
| JOIN | Combinaison de tables sur une condition |
| LATERAL | Sous-requête du FROM référençant les tables précédentes (PG) |
| Logique à 3 valeurs | TRUE / FALSE / UNKNOWN (NULL) |
| Matérialisée (vue) | Vue dont le résultat est stocké et rafraîchi explicitement |
| MVCC | Multi-Version Concurrency Control : lecteurs jamais bloqués par écrivains (PG) |
| N+1 | Antipattern : 1 requête + N requêtes unitaires en boucle |
| Normalisation | Découpage en tables pour éliminer redondances (1NF → 3NF) |
| NOT NULL | Contrainte interdisant l'absence de valeur |
| NULL | Absence/inconnu de valeur — ni 0 ni '' |
| OFFSET | Saut de N lignes (pagination, à éviter en profondeur) |
| Optimiseur | Composant choisissant le plan d'exécution |
| ORm | Object-Relational Mapping (couche applicative) |
| Partitionnement | Découpage physique d'une grosse table (par date, etc.) |
| PIVOT | Transformation lignes → colonnes (via CASE en SQL standard) |
| PK (clé primaire) | Identifiant unique et non nul d'une table |
| Plan d'exécution | Stratégie choisie par l'optimiseur (lisible via EXPLAIN) |
| Redo/WAL | Journal des modifications garantissant la durabilité |
| Requête corrélée | Sous-requête référençant la requête externe |
| REPEATABLE READ | Niveau d'isolation : vision figée pendant la transaction |
| ROLLBACK | Annulation d'une transaction |
| ROLLUP/CUBE | Sous-totaux multi-niveaux dans un GROUP BY |
| Sargable | Prédicat pouvant utiliser un index (Search ARGument ABLE) |
| SAVEPOINT | Point de rollback partiel dans une transaction |
| Schéma | Espace de noms regroupant des objets (public, ...) |
| Sélectivité | Proportion de lignes filtrées (basse = bon candidat index) |
| Semi-join | « les X ayant au moins un Y » (EXISTS / IN) |
| Séquence | Compteur servant aux auto-incréments (PG) |
| SLA | Service Level Agreement : engagement de délai/qualité |
| Sous-requête | SELECT imbriqué (WHERE, FROM, SELECT) |
| Statistiques | Infos sur les données guidant l'optimiseur (ANALYZE) |
| Surrogate key | Clé primaire technique (id auto), vs clé naturelle métier |
| THREE-valued logic | Voir « Logique à 3 valeurs » |
| Transaction | Lot atomique d'ordres (BEGIN/COMMIT/ROLLBACK) |
| Trigger | Code exécuté automatiquement sur INSERT/UPDATE/DELETE |
| UNIQUE | Contrainte d'unicité (plusieurs NULL permis en standard) |
| UPSERT | INSERT ou UPDATE si conflit (ON CONFLICT / ON DUPLICATE KEY) |
| VACUUM | Nettoyage PG des lignes mortes (libère de la place, fige les stats) |
| Verrou (lock) | Mécanisme d'exclusion mutuelle entre transactions |
| Vue | Requête stockée, réévaluée à chaque usage |

---

## 78. Quiz — 10 questions (réponses en section 79)

**Q1.** Que renvoie `SELECT * FROM interventions WHERE date_cloture = NULL;` ?
a) Les interventions en cours — b) Rien, toujours — c) Erreur de syntaxe

**Q2.** Dans `SELECT COUNT(*) FROM interventions`, que compte-t-on si `cout_eur`
contient des NULL ?
a) Toutes les lignes — b) Seulement les lignes où cout_eur est non NULL — c) Erreur

**Q3.** Quelle est la différence entre `RANK()` et `DENSE_RANK()` sur des ex æquo ?
a) Aucune — b) RANK laisse un trou après les ex æquo, DENSE_RANK non — c) L'inverse

**Q4.** Pourquoi ce LEFT JOIN se comporte comme un INNER JOIN ?
```sql
SELECT e.nom FROM employes e
LEFT JOIN interventions i ON i.technicien_id = e.id
WHERE i.priorite = 1;
```

**Q5.** Écrire la requête « les équipements sans aucune intervention » de **deux**
manières différentes.

**Q6.** Corriger : `SELECT type_interv, COUNT(*) FROM interventions HAVING COUNT(*) > 2;`
(il manque quelque chose).

**Q7.** Un rapport calcule `AVG(prix_moyen_par_mois)` sur 12 mois. Pourquoi le résultat
peut-il être faux, et que faire à la place ?

**Q8.** Citer **trois** défenses contre l'injection SQL, dans l'ordre de priorité.

**Q9.** `SELECT * FROM interventions ORDER BY date_ouverture LIMIT 10 OFFSET 100000;`
rame. Proposer une méthode plus rapide pour la page suivante.

**Q10.** Quelle commande MySQL/MariaDB garantit un dump **cohérent sans verrouiller**
les tables InnoDB, et quelles options évitent de perdre routines et triggers ?

---

## 79. Quiz — réponses détaillées

**R1. b) Rien, toujours.** `= NULL` donne UNKNOWN pour chaque ligne, et WHERE ne garde
que TRUE. Il faut `IS NULL`. C'est l'erreur n°1 (section 66).

**R2. a) Toutes les lignes.** `COUNT(*)` compte les lignes, pas les valeurs —
les NULL ne le gênent pas. C'est `COUNT(cout_eur)` qui ignorerait les NULL (section 19).

**R3. b)** Exemple avec deux 2èmes ex æquo : RANK → 1, 2, 2, 4 (trou) ;
DENSE_RANK → 1, 2, 2, 3 (continu). ROW_NUMBER → 1, 2, 3, 4 (unique arbitraire) (section 37).

**R4.** Le `WHERE i.priorite = 1` élimine les lignes où `i.*` vaut NULL (les employés
sans intervention prioritaire), car `NULL = 1` est UNKNOWN. Le LEFT JOIN est
« re-transformé » en INNER. Correction : mettre la condition dans le `ON`
(section 24).

**R5.**
```sql
-- Méthode 1 : NOT EXISTS (recommandée)
SELECT reference FROM equipements e
WHERE NOT EXISTS (SELECT 1 FROM interventions i WHERE i.equipement_id = e.id);
-- Méthode 2 : LEFT JOIN + IS NULL
SELECT e.reference FROM equipements e
LEFT JOIN interventions i ON i.equipement_id = e.id
WHERE i.id IS NULL;
```
(Section 31. `NOT IN` est la 3e méthode mais piégée par NULL.)

**R6.** Il manque le `GROUP BY` : `HAVING` filtre des groupes, il faut les former.
```sql
SELECT type_interv, COUNT(*) AS nb FROM interventions GROUP BY type_interv HAVING COUNT(*) > 2;
```
Ou avec alias CTE pour la portabilité (section 21).

**R7.** C'est la **moyenne des moyennes** : fausse sauf effectifs mensuels identiques
(section 68). À la place : recalculer sur les données brutes —
`SUM(total) / SUM(effectif)` ou `AVG()` directement sur la colonne source.

**R8.** 1) **Requêtes paramétrées** (non négociable) ; 2) moindre privilège du compte
applicatif ; 3) ne pas exposer les erreurs SQL brutes (+ validation des entrées
en complément) (sections 62-64).

**R9.** Pagination **keyset** : mémoriser le dernier id/date de la page et faire
`WHERE id > :dernier_id ORDER BY id LIMIT 10` — pas de scan des 100 000 lignes
sautées (section 11).

**R10.** `mysqldump --single-transaction` (dump cohérent InnoDB sans verrou global)
avec `--routines --triggers` pour ne pas perdre procédures stockées et déclencheurs
(section 65).

**Score indicatif :** 8+/10 = autonome au quotidien ; 6-7 = relire les sections
13, 19, 21, 24, 31 ; moins = reprendre depuis la section 4 avec la base de test.

---

## 80. Pour aller plus loin — feuille de route

**Niveau 2 — administration légère (sysadmin) :**

- `pg_stat_statements` (PG) : quelles requêtes consomment le plus (temps total, appels).
  `CREATE EXTENSION pg_stat_statements;` puis top 10 par `total_exec_time`.
- `VACUUM` / `ANALYZE` : comprendre le bloat et l'autovacuum (ne jamais le désactiver
  sans raison mesurée).
- Réplication : primaire/standby (streaming replication PG, replica MySQL) pour la HA
  et pour déporter les gros reportings.
- Partitionnement : `PARTITION BY RANGE (date)` pour les tables d'historique/logs
  (purge par `DROP PARTITION` instantanée).
- `pg_cron` / MySQL EVENT scheduler : automatiser REFRESH des vues matérialisées,
  purges, rapports.

**Niveau 3 — SQL avancé :**

- JSON : `JSONB` (PG) — opérateurs `->`, `->>`, `@>`, index GIN. Stocker du
  semi-structuré (config d'équipements, réponses d'API) tout en restant requêtable.
- Full-text search : `tsvector`/`tsquery` (PG), `MATCH ... AGAINST` (MySQL) —
  mieux que `LIKE '%...%'` sur de gros textes.
- Triggers et procédures stockées (`PL/pgSQL`) : audit automatique
  (`qui a modifié quoi quand`), règles métier complexes.
- Fenêtres avancées : `ntile()`, `percent_rank()`, `cume_dist()`, cadres `RANGE`
  temporels, `IGNORE NULLS` (selon SGBD).
- Modélisation : normalisation jusqu'en 3NF, puis **dénormalisation raisonnée**
  pour la lecture (vues matérialisées plutôt que redondance manuelle).

**Ressources :**

- Documentation officielle PostgreSQL (la référence la plus pédagogique) :
  chapitres « Tutorial » puis « Performance Tips ».
- Use The Index, Luke (use-the-index-luke.com) — le site de référence sur l'indexation.
- « SQL Antipatterns » (Bill Karwin) — les pièges à éviter, avec solutions.
- Exercices : SQLZoo, PGExercises (PostgreSQL), LeetCode Database — 15 min/jour.

**Projet fil rouge conseillé :** modéliser VOTRE parc réel (onduleurs, copieurs,
interventions) dans la base de test, y importer un export CSV, et réécrire les
tableaux de bord du service en SQL. C'est là que le SQL devient un réflexe.

---

## 81. Checklist quotidienne du sysadmin SQL

**Le matin (5 minutes) :**

- [ ] Espace disque des volumes de données et de dumps (`df -h`, taille des derniers dumps).
- [ ] Dernier dump : présent ? taille cohérente ? heure correcte ?
- [ ] Requêtes longues en cours (`pg_stat_activity` / `SHOW FULL PROCESSLIST`).
- [ ] Alertes applicatives liées à la base (connexions refusées, erreurs).

**En intervention :**

- [ ] Toujours sur la **bonne base** (`SELECT current_database();` / `SELECT DATABASE();`)
      — le nombre d'accidents « je croyais être sur la préprod » est légendaire.
- [ ] `BEGIN;` avant tout DML manuel ; `SELECT` de contrôle avant `COMMIT`.
- [ ] Dump ciblé (`pg_dump -t table`) avant ALTER sur table métier.
- [ ] Noter la requête exécutée dans le ticket (traçabilité).

**Le vendredi (15 minutes) :**

- [ ] Test de restauration du dump hebdo sur base jetable (chronométré).
- [ ] `ANALYZE` / vérification autovacuum sur les tables à fort churn.
- [ ] Revue des droits : comptes inactifs à révoquer, mots de passe à rotation.
- [ ] Taille des tables qui grossissent vite (logs, historique) — anticiper le partitionnement.

---

## 82. Modèles de requêtes prêts à copier — exploitation

```sql
-- Top 10 des tables les plus volumineuses (PostgreSQL)
SELECT schemaname || '.' || tablename AS table,
       pg_size_pretty(pg_total_relation_size(schemaname || '.' || tablename)) AS taille
FROM pg_tables
WHERE schemaname NOT IN ('pg_catalog', 'information_schema')
ORDER BY pg_total_relation_size(schemaname || '.' || tablename) DESC
LIMIT 10;

-- MySQL : tailles des tables
SELECT table_name AS tbl,
       ROUND((data_length + index_length) / 1024 / 1024, 1) AS mo
FROM information_schema.tables
WHERE table_schema = DATABASE()
ORDER BY (data_length + index_length) DESC
LIMIT 10;

-- Dernière analyse / vacuum par table (PostgreSQL) — repérer les tables oubliées
SELECT relname AS table, last_vacuum, last_autovacuum, last_analyze, last_autoanalyze,
       n_dead_tup AS lignes_mortes
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC NULLS LAST
LIMIT 15;

-- Interventions ouvertes triées par ancienneté, avec âge en jours (portable)
SELECT id, equipement_id, description, date_ouverture,
       EXTRACT(DAY FROM (NOW() - date_ouverture)) AS age_jours_pg
       -- MySQL : DATEDIFF(NOW(), date_ouverture) AS age_jours
FROM interventions
WHERE date_cloture IS NULL
ORDER BY date_ouverture;

-- Export CSV propre depuis psql (évite les copier-coller hasardeux)
-- \copy (SELECT * FROM v_parc_actif) TO '/tmp/parc.csv' WITH (FORMAT csv, HEADER)
-- MySQL : SELECT ... INTO OUTFILE '/tmp/parc.csv' FIELDS TERMINATED BY ';' ENCLOSED BY '"';
```

---

## 83. Anti-sèches des différences MySQL ↔ PostgreSQL

| Sujet | PostgreSQL | MySQL / MariaDB |
|---|---|---|
| Pagination | `LIMIT n OFFSET m` | `LIMIT m, n` ou `LIMIT n OFFSET m` |
| Concaténation | `'a' \|\| 'b'` | `CONCAT('a','b')` |
| Insensible à la casse | `ILIKE` | collation `*_ci` par défaut |
| Auto-incrément | `SERIAL` / `IDENTITY` | `AUTO_INCREMENT` |
| UPSERT | `ON CONFLICT ... DO UPDATE` | `ON DUPLICATE KEY UPDATE` |
| Retour d'INSERT | `RETURNING` | `LAST_INSERT_ID()` |
| CTE récursive | `WITH RECURSIVE` | `WITH RECURSIVE` (8.0+/10.2+) |
| Fenêtres | complet | complet (8.0+/10.2+) |
| `FILTER (WHERE)` | ✅ | ❌ → `SUM(CASE...)` |
| FULL OUTER JOIN | ✅ | ❌ → UNION de LEFT+RIGHT |
| INTERSECT/EXCEPT | ✅ | ❌ → EXISTS/NOT EXISTS |
| Vues matérialisées | ✅ | ❌ → table + EVENT |
| Index partiels | ✅ (`WHERE ...`) | ❌ |
| `NULLS FIRST/LAST` | ✅ | ❌ → `ORDER BY col IS NULL, col` |
| Intervalle | `INTERVAL '7 days'` | `INTERVAL 7 DAY` |
| Différence de dates | `d2 - d1` (interval) | `DATEDIFF`/`TIMESTAMPDIFF` |
| Aléatoire | `RANDOM()` | `RAND()` |
| Tronquer nombre | `TRUNC(x, n)` | `TRUNCATE(x, n)` |
| Division `/` | entière si 2 entiers | toujours décimale |
| `BOOLEAN` | vrai type | alias de `TINYINT(1)` |
| TRUNCATE | transactionnel | commit implicite |
| DDL | transactionnel | commit implicite |
| FK | toujours vérifiées | seulement en InnoDB |

En cas de doute sur une fonction : la tester sur les deux avec la base de démo
vaut mieux qu'un pari en production.

---

## 84. Conclusion — les 10 commandements du SQL d'exploitation

1. **Tu testeras sur une copie** avant de toucher à la production.
2. **Tu listeras tes colonnes** : `SELECT *` est réservé à l'exploration.
3. **Tu respecteras NULL** : ni égal, ni différent — il s'apprivoise avec IS, COALESCE, EXISTS.
4. **Tu parenthèseras tes OR** : AND d'abord, le reste ensuite.
5. **Tu mettras tes conditions d'OUTER JOIN dans le ON**, pas dans le WHERE.
6. **Tu paramètreras tes requêtes** : aucune concaténation d'entrée utilisateur.
7. **Tu encadreras tes DML** : BEGIN, contrôle, puis COMMIT ou ROLLBACK.
8. **Tu sauvegarderas avant de migrer**, et tu testeras tes restaurations.
9. **Tu EXPLAINeras avant d'indexer**, et tu ANALYzeras après un gros import.
10. **Tu documenteras tes scripts** : objet, auteur, date, ticket, rollback.

> « Les données sont le patrimoine de l'entreprise ; SQL est la langue pour le protéger,
> l'interroger et le faire parler. Le reste n'est que de la pratique — 15 minutes par jour
> sur la base de démo, et dans trois mois ces 84 sections seront des réflexes. »

*Fin du guide — bon SQL, et que vos sauvegardes soient toujours restaurables.* ⚡
