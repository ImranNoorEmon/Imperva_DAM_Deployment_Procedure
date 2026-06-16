## 1. What is a Database?

### Definition

A **database** is an organised, electronic system for storing, retrieving, managing, and updating structured information at scale. Unlike a flat file or spreadsheet, a database is engineered to handle massive data volumes while guaranteeing consistency, accuracy, and simultaneous multi-user access without conflict.

The **Database Management System (DBMS)** is the software layer (e.g., Oracle, SQL Server) that sits between users/applications and the raw data. It serves as the interface for executing queries, enforcing permissions, managing transactions, and applying security rules. The database is the *container*; the DBMS is the *engine*.

### Core Characteristics

| Characteristic | Description |
|---|---|
| **Data Integrity** | Ensures information is entered correctly and cannot be easily corrupted — enforced through constraints, transactions, and validation rules |
| **Concurrency** | Allows thousands of users to read and write simultaneously without errors — critical when DPE teachers across Bangladesh all update records at the same time |
| **Persistence** | Ensures data survives power outages, system failures, or crashes — data written to disk is durable |
| **ACID Compliance** | Atomicity, Consistency, Isolation, Durability — the gold standard for transaction reliability in relational databases |

### Types of Databases

#### Relational Databases (RDBMS)
The most common enterprise type. Data is stored in tables (rows and columns) linked by unique keys. Uses SQL for querying. Strong ACID compliance makes it the standard for mission-critical systems.

| Database | Key Traits |
|---|---|
| **Oracle Database** | High-end enterprise features, massive scalability, robust built-in security (TDE, VPD, Audit Vault) |
| **Microsoft SQL Server** | Deep Windows integration, user-friendly management tools (SSMS), widely used in government |
| **EDB Postgres (EPAS)** | Enterprise PostgreSQL with Oracle compatibility layer, open-source flexibility + enterprise security |
| **MySQL** | Fast, reliable, popular for web-based applications; MySQL Enterprise adds audit and firewall |

#### Non-Relational (NoSQL)
Schema-less storage for unstructured or semi-structured data — documents, key-value pairs, graphs, wide-column. Designed for horizontal scalability and flexible data models. Examples: MongoDB, Redis, Cassandra.

#### In-Memory Databases
Stores data entirely in RAM for microsecond response times. Used for caching, real-time analytics, session management. Examples: Redis, Memcached, SAP HANA.

#### Distributed Databases
Data is spread across multiple physical locations or servers. If one site fails, data remains available from another. Examples: AWS RDS, Azure SQL, Google Cloud Spanner.

### Who Uses Databases at DPE?

- **End-Users** — DPE officials, teachers, school staff interacting with IPEMIS to input student data or check school records
- **Database Administrators (DBAs)** — Manage health, performance, backups, and user access
- **Data Analysts** — Generate reports: student enrollment trends, resource allocation across primary schools
- **Decision Makers** — Use data for long-term educational planning and policy

### Why Databases Matter at DPE

- **Centralisation** — Millions of student and teacher records stored in one secure digital system instead of paper files
- **Automation** — Payroll calculations, textbook distribution tracking, attendance management
- **Historical Archiving** — Permanent student progress records spanning decades
- **National Necessity** — The databases contain PII of millions of citizens; their security is a legal and ethical obligation

---

## 2. Database Security Fundamentals

### The Four Pillars of Native Database Security

Modern RDBMS platforms provide built-in security mechanisms built around four core pillars:

#### 1. Authentication (Identity Verification)
Ensuring every user or application is who they claim to be before granting any access. Implemented via:
- Username/password credentials
- LDAP/Active Directory integration
- Certificate-based authentication
- Kerberos Network Authentication (common in Windows/MS SQL environments)

#### 2. Authorisation (Access Control)
Granting specific, least-privilege permissions based on the verified identity:
- A teacher might have `READ` access to a student's grade records
- An administrator might have `UPDATE` or `DELETE` access
- An application service account might only have `EXECUTE` on specific stored procedures

#### 3. Encryption
Scrambling data so it is unreadable without the correct key:
- **At Rest (TDE)** — Transparent Data Encryption protects data on disk; even if physical storage media is stolen, the data is unreadable
- **In Transit (SSL/TLS)** — Encrypts the network stream between the application and the database server

#### 4. Native Auditing
Writing log files that record who logged in, at what time, from where, and what SQL statements they executed. Stored locally within the database engine's own logging framework.

### Vendor-Specific Native Security Controls

#### Oracle Database
- **TDE (Transparent Data Encryption)** — Encrypts data at rest at tablespace or column level
- **VPD (Virtual Private Database)** — Dynamically filters rows based on who is logged in; a user can only see rows relevant to their context
- **Fine-Grained Auditing (FGA)** — Triggers audit records on specific column access conditions
- **Database Vault** — Restricts privileged DBA accounts from accessing application data
- **Label Security** — Data classification labels control access based on sensitivity level
- **Audit Vault** — Centralised audit data repository separate from the production DB

#### EDB Postgres (EPAS)
- **Row-Level Security (RLS)** — Policies restrict which rows a user can see or modify
- **pg_audit extension** — Provides detailed session-level and object-level audit logging for compliance
- **pgcrypto** — Robust cryptographic functions for data encryption at the application layer
- **LDAP/Kerberos Auth** — Enterprise-grade external authentication integration
- **EDB Audit plugin** — Enterprise audit logging with configurable granularity

#### MySQL
- **Enterprise Firewall** — Blocks suspicious or unrecognised SQL queries at the DB level
- **Role-Based Access Control (RBAC)** — Granular privilege management via roles (since MySQL 8.0)
- **InnoDB Encryption** — Tablespace-level encryption at rest
- **Enterprise Audit plugin** — Session and query-level audit logging for compliance

#### Microsoft SQL Server
- **Always Encrypted** — Data remains encrypted even during query processing; the DB engine never sees plaintext values
- **TDE** — Database-level encryption at rest
- **Dynamic Data Masking** — Masks sensitive columns for non-privileged users (e.g., phone numbers shown as `XXXXX-XX1234`)
- **Row-Level Security** — Filter predicates control which rows each user can access
- **SQL Server Audit** — Built-in audit feature writing to Windows Event Log, Security Log, or flat file
- **Extended Events** — Lightweight, high-performance telemetry framework for detailed session monitoring


---

[← Back to README](../README.md)
