# 💾 AWS Databases and SQL Fundamentals

A comprehensive guide to core database concepts and managed database services on AWS, including Amazon RDS, Amazon Aurora, and Amazon DynamoDB.

---

## 1. Introduction to Databases

A **Database** is an organized collection of structured information, or data, typically stored electronically in a computer system. It is managed by a **Database Management System (DBMS)**.

* **Relational Databases (SQL/RDBMS):** Store data in tables with predefined schemas. They enforce **ACID** properties (Atomicity, Consistency, Isolation, Durability) and use **Structured Query Language (SQL)**. (e.g., MySQL, PostgreSQL, Amazon RDS/Aurora).
* **Non-Relational Databases (NoSQL):** Store data in flexible schemas (key-value, document, graph, etc.). They prioritize scalability and performance over strict ACID compliance. (e.g., Amazon DynamoDB, MongoDB, Cassandra).

### Data Interaction and Database Transaction

* **Data Interaction:** The process of performing **CRUD** operations (**C**reate, **R**ead, **U**pdate, **D**elete) on data in the database.
* **Database Transaction:** A single logical unit of work, which may consist of one or more SQL statements. Transactions must adhere to **ACID** properties to ensure data integrity.

| Code | Summary/Use |
| :--- | :--- |
| `BEGIN TRANSACTION;` | Starts a new transaction. |
| `COMMIT;` | Permanently saves all changes made during the transaction. |
| `ROLLBACK;` | Undoes all changes since the last `COMMIT` or `BEGIN TRANSACTION`. |

---

## 2. SQL Fundamentals (DML/DDL)

### Creating Tables and Learning Different Data Types

The **Data Definition Language (DDL)** is used to define the database structure (schema).

| Code | Summary/Use |
| :--- | :--- |
| `CREATE TABLE table_name ( column1 datatype, column2 datatype, PRIMARY KEY (column1) );` | Creates a new table with specified columns and defines a **Primary Key**. |
| `DROP TABLE table_name;` | Deletes an entire table and all its data. |
| `INT`, `VARCHAR(size)`, `DECIMAL`, `BOOLEAN`, `DATE` | Common SQL **Data Types** for storing integers, variable-length strings, precise numbers, true/false values, and dates. |

### Inserting Data into a Database

The **Data Manipulation Language (DML)** is used to modify the data within the tables.

| Code | Summary/Use |
| :--- | :--- |
| `INSERT INTO table_name (col1, col2) VALUES (val1, 'val2');` | Inserts a new row of data into the specified columns of a table. |

### Selecting Data

The **Data Query Language (DQL)** is used to retrieve data from the database.

| Code | Summary/Use |
| :--- | :--- |
| `SELECT col1, col2 FROM table_name;` | **Retrieves** the specified columns from the table. |
| `SELECT * FROM table_name;` | Retrieves **all** columns and rows from the table. |

### Performing a Conditional Search

The `WHERE` clause is used to filter records based on a specified condition.

| Code | Summary/Use |
| :--- | :--- |
| `SELECT * FROM table_name WHERE col1 = 'value';` | Selects rows where `col1` matches 'value'. |
| `... WHERE col1 > 10 AND col2 IS NOT NULL;` | Uses **Logical Operators** (`AND`, `OR`, `NOT`) and **Null Conditions** (`IS NULL`, `IS NOT NULL`). |
| `... WHERE col1 LIKE 'A%';` | Uses the **LIKE** operator for pattern matching (`%` for any string, `_` for any single character). |
| `... WHERE col1 BETWEEN 10 AND 20;` | Selects rows where `col1` is within a specified range (inclusive). |

### Working with Functions

SQL supports various built-in functions, including **Aggregate Functions** that perform calculations on a set of rows.

| Code | Summary/Use |
| :--- | :--- |
| `SELECT COUNT(*) FROM table_name;` | Returns the total number of rows. |
| `SELECT AVG(col) FROM table_name;` | Calculates the **average** value of a numeric column. |
| `SELECT SUM(col) FROM table_name;` | Calculates the **sum** of a numeric column. |
| `SELECT MAX(col), MIN(col) FROM table_name;` | Returns the **maximum** and **minimum** values in a column. |

### Organizing Data

| Code | Summary/Use |
| :--- | :--- |
| `SELECT col, COUNT(*) FROM table_name GROUP BY col;` | Groups rows that have the same values in `col` into summary rows. Essential with aggregate functions. |
| `... GROUP BY col HAVING COUNT(*) > 5;` | Filters the **groups** created by `GROUP BY`. `WHERE` filters rows, `HAVING` filters groups. |
| `SELECT * FROM table_name ORDER BY col DESC;` | **Sorts** the result set by `col` (Descending: `DESC`, Ascending: `ASC`). |
| `SELECT * FROM table_name LIMIT 10 OFFSET 5;` | **Limits** the result set to 10 rows, starting after skipping the first 5 rows. |

### Retrieving Data from Multiple Tables (Joins)

**Joins** are used to combine rows from two or more tables based on a related column between them (often a **Foreign Key**). 

| Code | Summary/Use |
| :--- | :--- |
| `SELECT * FROM tableA A JOIN tableB B ON A.id = B.fk_id;` | **INNER JOIN:** Returns only rows with **matching** values in both tables. |
| `SELECT * FROM tableA A LEFT JOIN tableB B ON A.id = B.fk_id;` | **LEFT JOIN:** Returns all rows from the left table (`A`), and the matched rows from the right table (`B`). `NULL` for no match. |
| `SELECT * FROM tableA A RIGHT JOIN tableB B ON A.id = B.fk_id;` | **RIGHT JOIN:** Returns all rows from the right table (`B`), and the matched rows from the left table (`A`). `NULL` for no match. |

### Build Your Database Server and Interact with Your DB Using an App

In a typical cloud application, a database server (like an **Amazon RDS Instance**) is provisioned. An application (e.g., an EC2 instance, Lambda function) interacts with the database using a **database driver** (e.g., MySQL Connector) to send and receive SQL commands over a network connection.

---

## 3. AWS Managed Relational Databases

### Amazon RDS (Relational Database Service)

**Amazon RDS** simplifies the setup, operation, and scaling of relational databases in the cloud. It manages administrative tasks like patching, backups, and scaling.

* **Supported Engines:** MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, and Amazon Aurora.
* **Features:**
    * **Automated Backups:** Point-in-time recovery.
    * **Multi-AZ Deployments:** For high availability and automatic failover.
    * **Read Replicas:** For read scaling and increasing availability.

#### Amazon RDS Demonstration (Summary)
A typical RDS setup involves:
1.  Launching a DB instance in the AWS Console, specifying engine, instance size, and security group.
2.  Ensuring the **VPC Security Group** allows inbound traffic on the database port (e.g., 3306 for MySQL) from the application server.
3.  Connecting to the DB instance using the provided **Endpoint** and credentials.

### Introduction to Amazon Aurora

**Amazon Aurora** is a proprietary relational database built by AWS that is compatible with MySQL and PostgreSQL. It is designed for high performance and availability.

* **Key Features:**
    * Up to 5x better performance than standard MySQL and 3x better than standard PostgreSQL.
    * **Distributed, fault-tolerant, self-healing storage** system that auto-scales up to 128TB.
    * Replicates data across 3 **Availability Zones (AZs)** and 6 storage nodes.
    * Supports **Aurora Global Database** for disaster recovery across regions (low RPO/RTO).

---

## 4. AWS Managed NoSQL Database

### Amazon DynamoDB

**Amazon DynamoDB** is a fast, flexible **NoSQL** database service for all applications that need consistent, single-digit millisecond latency at any scale. It supports **key-value** and **document** data models.

* **Key Features:**
    * **Fully Managed:** No servers to manage, no OS patching.
    * **Scalability:** Scales horizontally to handle massive request volumes.
    * **Data Model:** Schema-less (flexible), centered on **Tables**, **Items** (rows), and **Attributes** (columns).
    * **Primary Key:** Requires a **Partition Key** (for hashing) and optionally a **Sort Key** (for data organization).
    * **Consistency:** Offers both **Eventual Consistency** (default, lower latency) and **Strong Consistency** (higher latency).

#### Amazon DynamoDB Demonstration (Summary)
Interacting with DynamoDB is typically done via the AWS CLI, SDKs, or the AWS Management Console, using APIs instead of SQL.

| Code/Operation | Summary/Use |
| :--- | :--- |
| `CreateTable` | Creates a table, defining the **Primary Key** (Partition Key + optional Sort Key) and throughput capacity (Read/Write Capacity Units). |
| `PutItem` | Adds a new item (row) to the table or replaces an existing item with the same primary key. |
| `GetItem` | Retrieves a single item by its **Primary Key**. Highly efficient. |
| `UpdateItem` | Modifies attributes of an existing item. |
| `DeleteItem` | Removes a single item from the table. |
| `Query` | Efficiently retrieves items based on the **Partition Key** and an optional condition on the **Sort Key**. |
| `Scan` | Examines **every** item in the entire table. Should be avoided for large tables due to inefficiency and cost. |

---

## 5. Databases Advanced Topics

* **Normalization:** Structuring a relational database to reduce data redundancy and improve data integrity (1NF, 2NF, 3NF, etc.).
* **Indexing:** Creating special lookup tables to speed up data retrieval operations (`SELECT` queries) by minimizing the disk I/O required.
* **Stored Procedures/Triggers:** **Stored Procedures** are prepared SQL code that you can save and reuse. **Triggers** are stored procedures that automatically run when a specific event (e.g., `INSERT`, `UPDATE`) occurs on a table.
* **ACID vs. BASE:** Relational DBs follow **ACID**. Many NoSQL DBs follow the **BASE** model (**B**asically **A**vailable, **S**oft state, **E**ventual consistency), prioritizing availability and partition tolerance.
* **Database Migration Service (DMS):** An AWS service for migrating databases to AWS quickly and securely, supporting both homogeneous (same engine) and heterogeneous (different engine) migrations.
