### SQL Cheat Sheet

#### Introduction to SQL

```
SQL (Structured Query Language) is a standard language for storing, manipulating, and retrieving data in databases. It works with relational databases like MySQL, PostgreSQL, SQL Server, and Oracle.
```

#### Basic SQL Commands

```
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
```

#### SELECT Statement

- **Basic SELECT**

```
SELECT column1, column2 FROM table_name;
```

- **Select All Columns**

```
SELECT * FROM table_name;
```

- **Select With Condition**

```
SELECT * FROM table_name WHERE condition;
```

- **Distinct Values**

```
SELECT DISTINCT column1 FROM table_name;
```

#### INSERT INTO Statement

- **Inserting Data**
```
INSERT INTO table_name (column1, column2) VALUES (value1, value2);
```
#### UPDATE Statement

- **Updating Data**
```
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
#### DELETE Statement

- **Deleting Data**
```
DELETE FROM table_name WHERE condition;
```
#### JOIN Operations

- **INNER JOIN**
```
SELECT columns FROM table1 INNER JOIN table2 ON table1.column = table2.column;
```

- **LEFT JOIN**

```
SELECT columns FROM table1 LEFT JOIN table2 ON table1.column = table2.column;
```

- **RIGHT JOIN**

```
SELECT columns FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;
```

- **FULL OUTER JOIN**

```
SELECT columns FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column;
```

#### Aggregate Functions
- **COUNT**
```
SELECT COUNT(column_name) FROM table_name;
```

- **SUM**

```
SELECT SUM(column_name) FROM table_name;
```

- **MAX/MIN**

```
SELECT MAX(column_name) FROM table_name;
SELECT MIN(column_name) FROM table_name;
```

- **AVG**

```
SELECT AVG(column_name) FROM table_name;
```

- **GROUP BY**

```
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name;
```

#### SQL Data Types

```
- **Numeric**: INT, SMALLINT, DECIMAL, NUMERIC, FLOAT, REAL
- **Character**: CHAR, VARCHAR, TEXT
- **Date and Time**: DATE, TIME, TIMESTAMP
- **Binary**: BINARY, VARBINARY, BLOB
```

#### SQL Constraints

```
- **NOT NULL**: Ensures that a column cannot have NULL value.
- **UNIQUE**: Ensures that all values in a column are different.
- **PRIMARY KEY**: A combination of NOT NULL and UNIQUE.
- **FOREIGN KEY**: Uniquely identifies a row/record in another table.
- **CHECK**: Ensures that all values in a column satisfy a specific condition.
- **DEFAULT**: Sets a default value for a column when no value is specified.
```

#### SQL Indexes

- **Creating Index**
```
CREATE INDEX index_name ON table_name (column1, column2);
```

- **Dropping Index**

```
DROP INDEX index_name ON table_name;
```
