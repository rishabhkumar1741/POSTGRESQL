# 🐘 PostgreSQL Learning Notes

A structured collection of notes and examples while learning **PostgreSQL**.  
This repository is maintained as a personal guide and quick reference for database concepts, queries, and best practices.

---

## 📂 Table of Contents
1. 📖 [Introduction](#-introduction)
   - [ What is a Database?](#-what-is-a-database)
   - [What is a DBMS (Database Management System)?](#-what-is-a-dbms-database-management-system)
   - [What is an RDBMS (Relational Database Management System)?](#-what-is-an-rdbms-relational-database-management-system)
   - [Database vs Schema vs Table](#-database-vs-schema-vs-table)
2. 🗂 [Data Types](#-data-types)
3. 🔒 [Constraints](#-constraints)
4. 📝 [SQL Basics](#-sql-basics)
5. 🔗 [Joins](#-joins)
6. ⚡ [Indexes](#-indexes)
7. 🔧 [Functions & Operators](#-functions--operators)
8. 🔄 [Transactions](#-transactions)
9. 👀 [Views & Materialized Views](#-views--materialized-views)
10. 🚀 [Performance Tuning](#-performance-tuning)
11. 🎯 [Advanced Topics](#-advanced-topics)
12. 🏋️ [Practice Queries](#-practice-queries)
13. 📌 [References](#-references)

---

## 📖 Introduction
### 📌 What is a Database?

- A database is a collection of data stored in an organized way.
- Example: A school database may store student names, roll numbers, marks, etc.

👉 Think of it like a digital cupboard where data is kept safely.
### 📌 What is a DBMS (Database Management System)?
- A DBMS is software that helps you create, store, and manage data in a database.
- It allows adding, updating, deleting, and retrieving data.
- Example: MySQL, PostgreSQL, MongoDB, SQLite.

👉 Think of it like the librarian who helps you manage all books (data).
### 📌 What is an RDBMS (Relational Database Management System)?
- RDBMS is a type of DBMS where data is stored in tables (rows & columns).
- Uses relationships between tables (foreign keys).
- Follows SQL for queries.
- Example: PostgreSQL, MySQL, Oracle, SQL Server.

👉 Think of it like an Excel sheet system where multiple sheets (tables) can be linked together.

## 📊 Difference Between Database, DBMS, and RDBMS

| Concept      | Meaning                                            | Example           | Analogy                               |
|--------------|----------------------------------------------------|-------------------|---------------------------------------|
| **Database** | Organized collection of data                       | School records    | 📂 Cupboard with files                |
| **DBMS**     | Software to manage data                            | MongoDB, SQLite   | 👨‍🏫 Librarian managing the cupboard |
| **RDBMS**    | DBMS that stores data in **tables with relations** | PostgreSQL, MySQL | 📊 Excel with multiple linked sheets  |

---

## 🖥️ Common `psql` Terminal Commands
### 🔹 1. Connect with a specific user and database
```bash
psql -U nayra -d postgres

- \list,                    \l                Shows all available databases.
- \connect databasename     \c databasename   👉 Connects to school_db without leaving psql.
```
- -U nayra → login as user nayra
- -d postgres → connect to the postgres database

- If you don’t provide -d, PostgreSQL tries to connect to a database with the same name as the user.

## 📂 Database vs Schema vs Table
### 🗄️ Database

- A database is the top-level container that holds everything.
- It contains schemas, tables, views, functions, users, etc.
- Example: school_db, company_db.

👉 Analogy: A library building.
### 🗂️ Schema
- A schema is a logical container inside a database.
- It groups related objects (tables, views, functions).
- Helps organize data and avoid naming conflicts.
- Example: public (default schema), sales, hr.

👉 Analogy: Different sections inside the library (Science, History, Literature).

### 📊 Table
- A table stores actual data in rows and columns.
- Belongs to a schema inside a database.
- Example: students, teachers, courses.

👉 Analogy: A book inside a section of the library.


| Concept      | Meaning                                                                | Example                           | Analogy                        |
|--------------|------------------------------------------------------------------------|-----------------------------------|--------------------------------|
| **Database** | Top-level container that holds schemas, tables, users, functions, etc. | `school_db`, `company_db`         | 🏢 Library building            |
| **Schema**   | Logical container inside a database that groups related objects        | `public`, `sales`, `hr`           | 📚 Sections inside the library |
| **Table**    | Stores actual data in rows & columns inside a schema                   | `students`, `teachers`, `courses` | 📖 Books inside a section      |


## 🗂 Data Types
- 🔢 **Numeric**: `INT`, `BIGINT`, `DECIMAL`, `NUMERIC`
- 🔤 **Character**: `CHAR(n)`, `VARCHAR(n)`, `TEXT`
- 📅 **Date/Time**: `DATE`, `TIME`, `TIMESTAMP`, `INTERVAL`
- ✅ **Boolean**: `TRUE`, `FALSE`
- 🆔 **UUID**
- 📦 **JSON/JSONB**, **ARRAY**

---

## 🔒 Constraints
- ❌ `NOT NULL`
- 🔑 `UNIQUE`
- 🗝️ `PRIMARY KEY`
- 🌍 `FOREIGN KEY`
- ✅ `CHECK`
- 📝 `DEFAULT`
#####  🔒 Constraints in PostgreSQL

Constraints are rules applied on table columns to maintain **data integrity**.

| Constraint      | Description                                                                         | Example                                           |
|-----------------|-------------------------------------------------------------------------------------|---------------------------------------------------|
| **NOT NULL**    | Ensures a column cannot store `NULL` (empty) values                                 | `name VARCHAR(50) NOT NULL`                       |
| **UNIQUE**      | Ensures all values in a column are unique (no duplicates)                           | `email VARCHAR(100) UNIQUE`                       |
| **PRIMARY KEY** | Uniquely identifies each row in a table. Combines **NOT NULL + UNIQUE**             | `id SERIAL PRIMARY KEY`                           |
| **FOREIGN KEY** | Creates a relationship between two tables (referencing another table’s primary key) | `FOREIGN KEY (dept_id) REFERENCES department(id)` |
| **CHECK**       | Ensures values meet a condition                                                     | `age INT CHECK (age >= 18)`                       |
| **DEFAULT**     | Assigns a default value if no value is given                                        | `created_at TIMESTAMP DEFAULT NOW()`              |

---

### 📌 Example: Using Constraints
```sql
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    dept_id INT,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY (dept_id) REFERENCES department(id)
);
```



## 📝 SQL Basics

### 🏗️ Create & Delete Database in PostgreSQL

| Command                                        | Description                                         | Example                                   |
|------------------------------------------------|-----------------------------------------------------|-------------------------------------------|
| `CREATE DATABASE databasename;`                | Creates a new database                              | `CREATE DATABASE school_db;`              |
| `CREATE DATABASE databasename OWNER username;` | Creates a new database with a specific owner        | `CREATE DATABASE company_db OWNER nayra;` |
| `DROP DATABASE databasename;`                  | Deletes an existing database permanently            | `DROP DATABASE school_db;`                |
| `DROP DATABASE IF EXISTS databasename;`        | Deletes a database only if it exists (avoids error) | `DROP DATABASE IF EXISTS old_db;`         |

## 📋 CRED 
- ➕➖ Read,Inserting, Updating, Deleting Records
##### 📊 What is a Table?

- A **table** is a collection of related data held in a **row and column format** within a database.
- Each **row** = a single record (data entry).
- Each **column** = a specific attribute (field).
- Tables belong to a **schema** inside a database.

👉 Think of a table like an **Excel sheet** where rows = records and columns = fields.
```groovy
CREATE TABLE person (
    id INT,
    full_name VARCHAR(100),
    city VARCHAR(100)
);
```
#### 🗑️ Delete (Drop) Table in PostgreSQL

| Command                           | Description                                                                                      | Example                           |
|-----------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------|
| `DROP TABLE tablename;`           | Deletes an existing table permanently                                                            | `DROP TABLE employees;`           |
| `DROP TABLE IF EXISTS tablename;` | Deletes a table only if it exists (avoids error if table is missing)                             | `DROP TABLE IF EXISTS old_data;`  |
| `DROP TABLE tablename CASCADE;`   | Deletes a table **and** automatically removes objects depending on it (like foreign keys, views) | `DROP TABLE department CASCADE;`  |
| `DROP TABLE tablename RESTRICT;`  | Default option – prevents table deletion if other objects depend on it                           | `DROP TABLE department RESTRICT;` |

---

### ⚠️ Notes
- Once dropped, the table and its data are **gone permanently** (unless you have a backup).
- You cannot recover dropped tables directly.
- Use `\dt` inside `psql` to list available tables before dropping.

#### 🔄 CRED Operations in PostgreSQL

| Operation  | Command                                                | Example                                                           | Description                      |
|------------|--------------------------------------------------------|-------------------------------------------------------------------|----------------------------------|
| **Create** | `INSERT INTO tablename (columns) VALUES (values);`     | `INSERT INTO students (name, age, grade) VALUES ('Riya', 14, 9);` | Adds new data (row) into a table |
| **Read**   | `SELECT columns FROM tablename;`                       | `SELECT * FROM students;`                                         | Retrieves data from a table      |
| **Update** | `UPDATE tablename SET column = value WHERE condition;` | `UPDATE students SET grade = 10 WHERE name = 'Riya';`             | Modifies existing data           |
| **Delete** | `DELETE FROM tablename WHERE condition;`               | `DELETE FROM students WHERE name = 'Riya';`                       | Removes data from a table        |

---

## 🔗 Joins
- 🤝 `INNER JOIN`
- 👈 `LEFT JOIN`
- 👉 `RIGHT JOIN`
- 🔄 `FULL OUTER JOIN`
- 🪞 `SELF JOIN`

---

## ⚡ Indexes
- 🌳 B-Tree Index (default)
- #️⃣ Hash Index
- 📖 GIN & GiST Indexes
- 🎯 Partial Indexes
- 📘 Covering Indexes

---

## 🔧 Functions & Operators
- 🔤 String: `CONCAT`, `SUBSTRING`, `LENGTH`
- 🔢 Numeric: `ROUND`, `ABS`, `RANDOM`
- 📅 Date/Time: `NOW`, `AGE`, `EXTRACT`
- Σ Aggregate: `SUM`, `AVG`, `COUNT`

---

## 🔄 Transactions
- ▶️ `BEGIN`
- 💾 `COMMIT`
- ⏪ `ROLLBACK`
- 🏷️ Savepoints
- ⚖️ ACID Properties

---

## 👀 Views & Materialized Views
- 👓 Creating a view
- ✏️ Updating through views
- 🔄 Refreshing materialized views

---

## 🚀 Performance Tuning
- 🕵️ `EXPLAIN` & `EXPLAIN ANALYZE`
- 🧹 Vacuum & Analyze
- ⚡ Query optimization
- 📊 Indexing strategy

---

## 🎯 Advanced Topics
- 🔔 Triggers & Stored Procedures
- 🧩 Partitioning
- 🪟 Window Functions
- 📝 CTEs (`WITH` queries)

---

## 🏋️ Practice Queries
- 📊 Real-world examples and exercises
- 🎓 SQL interview-style questions

---

## 📌 References
- 📖 [PostgreSQL Official Docs](https://www.postgresql.org/docs/)
- 📘 [TutorialsPoint PostgreSQL](https://www.tutorialspoint.com/postgresql/index.htm)
- 📑 [SQL Style Guide](https://www.sqlstyle.guide/)

---

## 👩‍💻 Author
Maintained by **[Your Name]** – Learning PostgreSQL step by step 🚀  

