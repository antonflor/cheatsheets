# SQL Cheat Sheet

## Introduction to SQL

SQL (Structured Query Language) is a standard language for storing, manipulating, and retrieving data in databases. It works with relational databases like MySQL, PostgreSQL, SQL Server, and Oracle.

## Basic SQL Commands

- **SELECT**: Retrieves data from a database.
- **INSERT INTO**: Inserts new data into a database.
- **UPDATE**: Updates existing data within a database.
- **DELETE**: Deletes data from a database.
- **CREATE DATABASE**: Creates a new database.
- **ALTER DATABASE**: Modifies a database.
- **CREATE TABLE**: Creates a new table.
- **ALTER TABLE**: Modifies a table.
- **DROP TABLE**: Deletes a table.
- **CREATE INDEX**: Creates an index (search key).
- **DROP INDEX**: Deletes an index.

## SELECT Statement

**Basic SELECT**
```sql
SELECT column1, column2 FROM table_name;
```

**Select All Columns**
```sql
SELECT * FROM table_name;
```

**Select With Condition**
```sql
SELECT * FROM table_name WHERE condition;
```

**Distinct Values**
```sql
SELECT DISTINCT column1 FROM table_name;
```

## INSERT INTO Statement

```sql
INSERT INTO table_name (column1, column2) VALUES (value1, value2);
```

## UPDATE Statement

```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```

## DELETE Statement

```sql
DELETE FROM table_name WHERE condition;
```

## JOIN Operations

**INNER JOIN** — Returns rows with matching values in both tables
```sql
SELECT columns FROM table1 INNER JOIN table2 ON table1.column = table2.column;
```

**LEFT JOIN** — Returns all rows from the left table, matched rows from the right
```sql
SELECT columns FROM table1 LEFT JOIN table2 ON table1.column = table2.column;
```

**RIGHT JOIN** — Returns all rows from the right table, matched rows from the left
```sql
SELECT columns FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;
```

**FULL OUTER JOIN** — Returns all rows when there is a match in either table
```sql
SELECT columns FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column;
```

**SELF JOIN** — Joins a table to itself
```sql
SELECT a.column1, b.column2 FROM table_name a, table_name b WHERE condition;
```

**CROSS JOIN** — Returns the Cartesian product of both tables
```sql
SELECT columns FROM table1 CROSS JOIN table2;
```

## Aggregate Functions

**COUNT**
```sql
SELECT COUNT(column_name) FROM table_name;
```

**SUM**
```sql
SELECT SUM(column_name) FROM table_name;
```

**MAX / MIN**
```sql
SELECT MAX(column_name) FROM table_name;
SELECT MIN(column_name) FROM table_name;
```

**AVG**
```sql
SELECT AVG(column_name) FROM table_name;
```

**GROUP BY**
```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name;
```

**HAVING** — Filters results after GROUP BY (like WHERE but for aggregates)
```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 5;
```

## Sorting and Limiting

**ORDER BY**
```sql
SELECT * FROM table_name ORDER BY column_name ASC;
SELECT * FROM table_name ORDER BY column_name DESC;
```

**LIMIT / TOP** — Restrict the number of rows returned
```sql
-- MySQL / PostgreSQL
SELECT * FROM table_name LIMIT 10;

-- SQL Server
SELECT TOP 10 * FROM table_name;

-- Oracle
SELECT * FROM table_name WHERE ROWNUM <= 10;
```

**OFFSET** — Skip rows (useful for pagination)
```sql
SELECT * FROM table_name LIMIT 10 OFFSET 20;
```

## SQL Data Types

- **Numeric**: INT, SMALLINT, BIGINT, DECIMAL, NUMERIC, FLOAT, REAL
- **Character**: CHAR, VARCHAR, TEXT
- **Date and Time**: DATE, TIME, DATETIME, TIMESTAMP
- **Boolean**: BOOLEAN (or TINYINT in MySQL)
- **Binary**: BINARY, VARBINARY, BLOB

## SQL Constraints

- **NOT NULL**: Ensures that a column cannot have a NULL value.
- **UNIQUE**: Ensures that all values in a column are different.
- **PRIMARY KEY**: A combination of NOT NULL and UNIQUE. Uniquely identifies each row.
- **FOREIGN KEY**: Links to a primary key in another table, enforcing referential integrity.
- **CHECK**: Ensures that all values in a column satisfy a specific condition.
- **DEFAULT**: Sets a default value for a column when no value is specified.

## SQL Indexes

**Creating an Index**
```sql
CREATE INDEX index_name ON table_name (column1, column2);
```

**Creating a Unique Index**
```sql
CREATE UNIQUE INDEX index_name ON table_name (column_name);
```

**Dropping an Index**
```sql
DROP INDEX index_name ON table_name;
```

---

## Common SQL Command Examples

**Creating a Database**
```sql
CREATE DATABASE example_db;
```

**Deleting a Database**
```sql
DROP DATABASE example_db;
```

**Creating a Table**
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Deleting a Table**
```sql
DROP TABLE users;
```

**Adding a Column to a Table**
```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
```

**Deleting a Column from a Table**
```sql
ALTER TABLE users DROP COLUMN phone;
```

**Renaming a Column**
```sql
ALTER TABLE users RENAME COLUMN email TO user_email;
```

**Inserting Data into a Table**
```sql
INSERT INTO users (name, email, age) VALUES ('John Doe', 'john@example.com', 28);
```

**Inserting Multiple Rows**
```sql
INSERT INTO users (name, email, age) VALUES
  ('Jane Smith', 'jane@example.com', 32),
  ('Bob Jones', 'bob@example.com', 25);
```

**Updating Data in a Table**
```sql
UPDATE users SET name = 'Jane Doe' WHERE id = 1;
```

**Deleting Data from a Table**
```sql
DELETE FROM users WHERE id = 1;
```

**Selecting All Data from a Table**
```sql
SELECT * FROM users;
```

**Selecting Specific Columns**
```sql
SELECT name, email FROM users;
```

**Filtering Data with WHERE**
```sql
SELECT * FROM users WHERE age > 25;
```

**Filtering with Multiple Conditions**
```sql
SELECT * FROM users WHERE age > 25 AND name LIKE 'J%';
```

**Sorting Data with ORDER BY**
```sql
SELECT * FROM users ORDER BY age DESC;
```

**Counting Rows**
```sql
SELECT COUNT(*) FROM users;
```

**Grouping Data with GROUP BY**
```sql
SELECT age, COUNT(*) FROM users GROUP BY age;
```

**Using Aggregate Functions**
```sql
SELECT AVG(age) FROM users;
SELECT MAX(age), MIN(age) FROM users;
```

**Joining Tables (INNER JOIN)**
```sql
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

**Left Join**
```sql
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

**Using Subqueries**
```sql
SELECT name FROM users WHERE id IN (SELECT user_id FROM orders WHERE amount > 100);
```

**Correlated Subquery**
```sql
SELECT name FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id AND o.amount > 500);
```

**Using CASE (conditional logic)**
```sql
SELECT name,
  CASE
    WHEN age < 30 THEN 'Young'
    WHEN age BETWEEN 30 AND 50 THEN 'Mid-range'
    ELSE 'Senior'
  END AS age_group
FROM users;
```

**String Functions**
```sql
SELECT UPPER(name), LOWER(email), LENGTH(name) FROM users;
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;
SELECT TRIM(name) FROM users;
```

**Date Functions**
```sql
SELECT NOW();                            -- Current date and time
SELECT CURDATE();                        -- Current date
SELECT DATEDIFF(NOW(), created_at) FROM users;  -- Days since record created
SELECT DATE_FORMAT(created_at, '%Y-%m') FROM users;  -- Format a date
```

**Transactions**
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- To undo:
ROLLBACK;
```

**Views**
```sql
-- Create a view
CREATE VIEW active_users AS
SELECT * FROM users WHERE status = 'active';

-- Query a view like a table
SELECT * FROM active_users;

-- Drop a view
DROP VIEW active_users;
```
