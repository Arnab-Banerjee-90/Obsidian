## 1. `RDBMS` and its features
---
To mitigate the several drawbacks of a File Based management System, The Relational Database Management System was introduced in the 1970's. It is a type of database management system that is based on the relational model of data. Below are its main features

### 1.1. `RDBMS` Schema
---
A `schema` refers to the logical structure or blueprint that defines the organization, relationships, and constraints of a database. A Database Schema Consists of the following

#### 1.1.1 Data Organization
---
In `RDBMS` a database is organized data into tables, which consist of rows and columns.
Each table represents an entity or a relationship between entities in the real-world domain.

We can think of a database like a folder and a table like a spreadsheet. We can have many spreadsheets in a folder.

#### 1.2.1 Data Integrity (Constraints)
---
`RDBMS` enforces data integrity by supporting various constraints. These constraints include primary keys, foreign keys, unique keys, and check constraints. They ensure that data remains consistent, accurate, and valid throughout the database.

#### 1.3.1 Data Relationships
---
`RDBMS` enables the establishment of relationships between tables using primary and foreign keys. This allows for the creation of complex data structures and facilitates data integrity and consistency. Relationships, such as one-to-one, one-to-many, and many-to-many, can be defined and enforced.

### 1.2. SQL Language
---
`RDBMS` uses Structured Query Language (SQL) as the standard language for managing and manipulating data. SQL provides a powerful and standardized set of commands for creating, modifying, and querying databases. It allows users to interact with the database using simple and declarative  (non-procedural) statements.

`RDBMS` includes query optimizer that analyzes SQL queries and determine the most efficient way to retrieve data.

### 1.3. Concurrent Access
---
`RDBMS` implements concurrency control mechanisms to handle multiple users concurrently accessing and modifying the database.

### 1.4. Scalability and Performance
---
`RDBMS` is designed to handle large-scale data and high-performance requirements. It provides mechanisms like `indexing`, `query optimization`, and `caching` to improve data retrieval speed and system performance.

### 1.5. ACID Compliance
---
`RDBMS` ensures ACID (Atomicity, Consistency, Isolation, Durability)
- Atomicity ensures that a transaction is treated as a single unit of work and either succeeds completely or fails entirely.
- Consistency guarantees that the database remains in a valid state before and after a transaction.
- Isolation ensures that concurrent transactions do not interfere with each other.
- Durability ensures that once a transaction is committed, its changes are permanently saved, even in the event of a system failure.

### 1.6. Pros and Cons of `RDBMS`
---
#### 1.6.1. The Pros
---
- Scenarios where we have to use transactions
- Where our data is structured and relational
- In cases where we are not sure which DB to go with

#### 1.6.2 The Cons
---
- Cannot be used in cases where Dynamic Schema is necessary 
- Horizontal Scalability --> When data can't fit in single machine, then to perform joins across multiple machines, there will be n/w call involved and performance deteriorates.

## 2. Introduction to SQL
---
The history of SQL begins in an IBM laboratory in San Jose, California, where SQL was developed in the late 1970's. The initials stand for Structured Query Language,
and the language itself is often referred to as "sequel." It was originally developed
for IBM's DB2 product (a relational database management system, or `RDBMS`, that
can still be bought today for various platforms and environments). In fact, `SQL`
makes an `RDBMS` possible. 

SQL is a non-procedural language, in contrast to the procedural languages such as COBOL and C that had been created up to that time. Non-procedural means what rather than how. For example, SQL describes  what data to retrieve, delete, or insert, rather than how to perform the operation

### 2.1 An overview
---
SQL is the standard language used to manipulate and retrieve data from these  relational databases. SQL enables a programmer or database administrator to do the following
- Create a Database
- Modify the structure of a Database
- Query a Database for information
- Update the contents of a Database
- Change System security settings
- Add user permissions on Databases or tables

## 3. SQL statement categorization
---
Structured query language is a collection of various types of statements shown below.

### 3.1. Data Query Language
---
This category of statements is used to fetch information from any table or tables of a database. It contains only the `SELECT` statement

### 3.2. Data Manipulation Language
---
This category of statements is used to manipulate one or multiple rows of data of a certain table of a database. It contains the below statements
`INSERT`,  `UPDATE`, `DELETE`, `MERGE`

### 3.3. Data Definition Language
---
This category of statements is used to define/redefine the structure of an entire table of a database. It contains the below statements
`CREATE`, `ALTER`, `TRUNCATE`, `DROP`, `RENAME`

### 3.4. Data Control Language
---
Used to grant or revoke permissions of a certain user to do particular changes to a database. It contains the statements `GRANT`, `REVOKE`

### 3.5 Transaction Control
---
Transaction control statements are used to manage and control the transactional behavior of a group of SQL statements. They allow us to define the boundaries of a transaction, specify when to commit or roll back the changes made within a transaction, and control the level of isolation and durability. It contains the below statements `COMMIT`, `ROLLBACK`, `SAVEPOINT`




