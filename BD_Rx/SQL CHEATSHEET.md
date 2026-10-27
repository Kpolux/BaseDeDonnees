#sql #cheat-sheet #database

# SQL CHEATSHEET
*Your quick reference for SQL Queries*

---

## 1. SELECT BASICS
```sql
SELECT * FROM table_name;
-- All columns

SELECT column1, column2 FROM table_name;
-- Specific columns

SELECT DISTINCT column_name FROM table_name;
-- Unique values

SELECT column1 AS alias_name FROM table_name;
-- Alias
````

## 2. WHERE CLAUSE


```SQL
SELECT * FROM table_name WHERE condition;

SELECT * FROM employees WHERE salary > 50000;

SELECT * FROM employees WHERE department = 'IT';

SELECT * FROM employees WHERE salary BETWEEN 40000 AND 80000;

SELECT * FROM employees WHERE name LIKE 'A%';
-- Starts with A

SELECT * FROM employees WHERE name LIKE '%son';
-- Ends with son

SELECT * FROM employees WHERE age IN (25, 30, 35);

SELECT * FROM employees WHERE city IS NULL;
```

## 3. ORDER BY, LIMIT & OFFSET

```SQL
SELECT * FROM table_name ORDER BY column_name ASC;
-- Ascending

SELECT * FROM table_name ORDER BY column_name DESC;
-- Descending

SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC;

SELECT * FROM table_name LIMIT 10;
-- Top 10 rows

SELECT * FROM table_name LIMIT 10 OFFSET 5;
-- Skip 5, next 10
```

## 4. AGGREGATE FUNCTIONS


```SQL
SELECT COUNT(*) FROM table_name;
-- Count rows

SELECT COUNT(column_name) FROM table_name;
-- Count non-null

SELECT SUM(column_name) FROM table_name;
-- Sum

SELECT AVG(column_name) FROM table_name;
-- Average

SELECT MIN(column_name) FROM table_name;
-- Minimum

SELECT MAX(column_name) FROM table_name;
-- Maximum
```

## 5. GROUP BY & HAVING

```SQL
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;

SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
-- Filter groups
```

## 6. JOINS

### 6.1 INNER JOIN

```SQL
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;
```

### 6.2 LEFT JOIN


```SQL
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;
```

### 6.3 RIGHT JOIN

```SQL
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;
```

## 7. SUBQUERIES

### 7.1 Subquery in WHERE

```SQL
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### 7.2 Subquery in FROM

```SQL
SELECT department, avg_salary
FROM (SELECT department, AVG(salary) AS avg_salary
      FROM employees
      GROUP BY department) AS dept_avg;
```

## 8. SET OPERATORS

### 8.1 UNION

```SQL
SELECT column_name FROM table1
UNION
SELECT column_name FROM table2;
-- Unique values from both
```

### 8.2 UNION ALL

SQL

```
SELECT column_name FROM table1
UNION ALL
SELECT column_name FROM table2;
-- All values (including duplicates)
```

### 8.3NTERSECT

```SQL
SELECT column_name FROM table1
INTERSECT
SELECT column_name FROM table2;
-- Common values
```

### EXCEPT

SQL

```
SELECT column_name FROM table1
EXCEPT
SELECT column_name FROM table2;
-- Values in table1 not in table2
```

## 9. INSERT, UPDATE, DELETE

> [!WARNING] Danger
> N'oubliez jamais la clause `WHERE` dans un `UPDATE` ou un `DELETE`, sous peine de modifier ou supprimer l'intégralité de la table !
### 9.1 INSERT

```SQL
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);
```

### 9.2 UPDATE

```SQL
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

### 9.3 DELETE

```SQL
DELETE FROM table_name
WHERE condition;
```


## 10. COMMON DATA TYPES

|**Data Type**|**Description**|
|---|---|
|**INT / INTEGER**|Whole numbers|
|**BIGINT**|Large whole numbers|
|**VARCHAR(n)**|Variable-length string|
|**CHAR(n)**|Fixed-length string|
|**DATE**|Date (YYYY-MM-DD)|
|**DATETIME**|Date and time|
|**BOOLEAN**|True / False|
|**DECIMAL(p,s)**|Decimal numbers (precision, scale)|

## 11. WILDCARDS

```SQL
- `%` : Matches any sequence
    
- `_` : Matches any single char
    
```
### Examples:
```SQL
- `'A%'` : Starts with A
    
- `'%son'` : Ends with son
    
- `'%an%'` : Contains an
    
- `'_a_'` : Any 3-letter word with 'a' in middle 
```
## 12. USEFUL TIPS

### 12.1 NULL Handling

```SQL
SELECT column_name
FROM table_name
WHERE column_name IS NULL;
```

### 12.2 Case Expressions

```SQL
SELECT name, salary,
CASE
    WHEN salary > 50000 THEN 'High'
    WHEN salary > 30000 THEN 'Medium'
    ELSE 'Low'
END AS salary_level
FROM employees;
```

### 12.3 Alias Example


```SQL
SELECT e.name AS employee_name,
       d.department_name AS dept
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

### 12.4 Comment Syntax

```SQL
-- This is a single-line comment

/* This is a
multi-line comment */
```

## 13. WINDOW FUNCTIONS (Fonctions de fenêtrage)

_Essentielles pour les analyses de données avancées sans regrouper les lignes._


```SQL
-- Calculer un classement (Rank)
SELECT name, department, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank_in_dept
FROM employees;

-- Calculer un cumulatif (Running Total)
SELECT date, revenue,
       SUM(revenue) OVER (ORDER BY date) as running_total
FROM sales;

-- Récupérer la valeur de la ligne précédente (LAG) ou suivante (LEAD)
SELECT date, revenue,
       LAG(revenue, 1) OVER (ORDER BY date) as previous_day_revenue
FROM sales;
```

## 14. CTE (Common Table Expressions)

_Idéal pour remplacer les sous-requêtes complexes (point 7) par un code beaucoup plus lisible._


```SQL
WITH dept_average AS (
    SELECT department, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT e.name, e.salary, d.avg_salary
FROM employees e
JOIN dept_average d ON e.department = d.department
WHERE e.salary > d.avg_salary;
```

## 15. FONCTIONS DE MANIPULATION (Spécifiques MySQL)

### Chaînes de caractères (Strings)
*Ces fonctions fonctionnent à l'identique sous MySQL.*

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name, -- Combiner plusieurs chaînes
       UPPER(last_name) AS upper_name,                  -- Passer en majuscules
       LOWER(first_name) AS lower_name,                 -- Passer en minuscules
       SUBSTRING(email, 1, 5) AS short_email,           -- Extraire (Position de départ, Longueur)
       LENGTH(password) AS pass_length,                 -- Longueur de la chaîne en octets
       REPLACE(phone, ' ', '-') AS formatted_phone      -- Remplacer un caractère par un autre
FROM users;
````

### Dates (Le standard MySQL)

_MySQL n'utilise pas tout à fait les mêmes fonctions que PostgreSQL ou SQL Server pour les dates. Voici les plus importantes :_



```SQL
SELECT NOW(),                                      -- Date ET heure actuelles (YYYY-MM-DD HH:MM:SS)
       CURDATE(),                                  -- Date actuelle uniquement (YYYY-MM-DD)
       YEAR(hire_date) AS hire_year,               -- Extrait l'année (ex: 2026)
       MONTH(hire_date) AS hire_month,             -- Extrait le numéro du mois (1-12)
       DAY(hire_date) AS hire_day,                 -- Extrait le jour du mois (1-31)
       
       -- Ajouter ou soustraire du temps (Très utilisé)
       DATE_ADD(order_date, INTERVAL 7 DAY) AS delivery_date,
       DATE_SUB(NOW(), INTERVAL 1 MONTH) AS last_month,
       
       -- Différence entre deux dates (Renvoie le nombre de JOURS)
       DATEDIFF(NOW(), hire_date) AS days_in_company,
       
       -- Formater une date selon vos propres critères
       DATE_FORMAT(hire_date, '%d/%m/%Y') AS french_date -- Donne ex: 12/06/2026
FROM orders;
```


### 💡 Petit tips pour Obsidian :
En MySQL, la fonction `LENGTH()` calcule la taille en *octets*. Si vous manipulez des caractères spéciaux ou des émojis (souvent codés sur plusieurs octets en UTF-8), préférez utiliser `CHAR_LENGTH()` qui calcule le nombre réel de *caractères*.

## 15. FONCTIONS DE MANIPULATION (String & Dates)

_Les formats varient légèrement selon les SGBD (PostgreSQL, MySQL, SQL Server), mais voici les plus standards :_

### Chaînes de caractères (Strings)

```SQL
SELECT CONCAT(first_name, ' ', last_name) AS full_name, -- Combiner
       UPPER(last_name) AS upper_name,                  -- Majuscules
       SUBSTRING(email FROM 1 FOR 5) AS short_email,    -- Extraire une partie
       LENGTH(password) AS pass_length                  -- Longueur
FROM users;
```

### Dates

``` SQL
SELECT NOW(),                                      -- Date et heure actuelles
       EXTRACT(YEAR FROM hire_date) AS hire_year,  -- Extraire l'année (PostgreSQL)
       DATE_ADD(order_date, INTERVAL 7 DAY)        -- Ajouter du temps (MySQL)
FROM orders;
```

## 16. INDEXATION (Optimisation des performances)

_Pour aller au-delà de la syntaxe et optimiser les requêtes._

- **Créer un Index :** Accélère grandement les requêtes de recherche (`WHERE`).

*Les index permettent d'accélérer les requêtes de lecture (SELECT) au détriment d'une légère baisse de performance lors des écritures (INSERT, UPDATE, DELETE). MySQL utilise principalement des index B-Tree.*



```sql
-- Créer un index simple sur une colonne fréquemment filtrée (WHERE) ou triée (ORDER BY)

-- Créer un index simple (sur une colonne fréquemment filtrée ou triée)
CREATE INDEX idx_employee_lastname ON employees(last_name);

-- Créer un index composite (sur plusieurs colonnes)
CREATE INDEX idx_emp_dept_salary ON employees(department, salary);

-- Créer un index unique (empêche les doublons)
CREATE UNIQUE INDEX idx_user_email ON users(email);

-- Créer un index FULLTEXT (pour les recherches de texte complexes avec MATCH() AGAINST())
CREATE FULLTEXT INDEX idx_prod_desc ON products(description);

-- Afficher les index existants sur une table
SHOW INDEX FROM employees;

-- Supprimer un index (Syntaxe spécifique MySQL)
DROP INDEX idx_employee_lastname ON employees;

```

## 17. TRIGGERS (Déclencheurs )

_Un Trigger est un bloc de code SQL qui s'exécute automatiquement lorsqu'un événement (INSERT, UPDATE ou DELETE) survient sur une table._

> [!INFO] Syntaxe
> 
> La syntaxe des triggers varie fortement selon le SGBD (PostgreSQL, MySQL, Oracle). 

> [!ATTENTION] Syntaxe MySQL En MySQL, si le trigger contient plusieurs instructions, il faut modifier temporairement le délimiteur (`DELIMITER //`) pour éviter que le point-virgule `;` interne ne coupe l'exécution de la requête prématurément.



```SQL
DELIMITER //

CREATE TRIGGER trigger_audit_salary
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    -- Vérifie si le salaire a été modifié
    IF OLD.salary <> NEW.salary THEN
        INSERT INTO salary_history(employee_id, old_salary, new_salary, changed_at)
        VALUES (OLD.id, OLD.salary, NEW.salary, NOW());
    END IF;
END //

DELIMITER ;
```

### Événements et mots-clés à retenir :

- **Moments :** `BEFORE` (pour valider ou modifier la donnée _avant_ qu'elle ne soit écrite) ou `AFTER` (pour de l'audit ou synchroniser d'autres tables).
    
- **Événements :** `INSERT`, `UPDATE`, `DELETE`.
    
- **Variables :** `NEW.nom_colonne` (nouvelle valeur) et `OLD.nom_colonne` (ancienne valeur).
    
- **Suppression :** `DROP TRIGGER IF EXISTS trigger_audit_salary;`


## 18. IMPORT & EXPORT DE DONNÉES EN CSV (MySQL)

_Permet de transférer des données en masse entre vos tables MySQL et des fichiers CSV._


> [!WARNING] Droits d'accès
> 
> Les commandes directes comme `COPY` s'exécutent généralement côté serveur et nécessitent des privilèges d'administrateur (Superuser).


> [!WARNING] Erreur fréquente : `secure-file-priv` MySQL restreint par défaut les dossiers où il peut lire et écrire des fichiers pour des raisons de sécurité. Pour connaître le seul dossier autorisé sur votre machine, exécutez :`SHOW VARIABLES LIKE 'secure_file_priv';`

### Exporter une table vers un fichier CSV

```SQL
SELECT id, name, department, salary 
FROM employees
INTO OUTFILE '/var/lib/mysql-files/employees.csv' -- Utilisez le chemin de votre dossier sécurisé
FIELDS TERMINATED BY ',' 
OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n';
```

### Importer un fichier CSV vers une table

```SQL
LOAD DATA INFILE '/var/lib/mysql-files/nouvel_import.csv'
INTO TABLE employees
FIELDS TERMINATED BY ',' 
OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES; -- Ignore la première ligne si elle contient les en-têtes (headers)
```

### Alternative en ligne de commande (Sans contrainte de dossier sécurisé)

Si vous utilisez le terminal et que vous n'avez pas accès au dossier `secure_file_priv` du serveur, vous pouvez utiliser l'outil client `mysql` ou `mysqldump` :

```Bash
# Export direct depuis le terminal vers un fichier local
mysql -u mon_utilisateur -p -e "SELECT * FROM ma_base.employees" > export.txt

# Import d'un fichier CSV via l'option --local (nécessite l'activation du paramètre local_infile)
mysqlimport --local --fields-terminated-by=',' --lines-terminated-by='\n' -u utilisateur -p ma_base /chemin/vers/employees.csv
```


## 💡 Astuces bonus 
    
- **Analyser une requête :** Ajouter `EXPLAIN` ou `EXPLAIN ANALYZE` devant votre `SELECT` pour comprendre comment le moteur de base de données exécute la requête et identifier les lenteurs.
