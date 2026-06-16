# Imperva SecureSphere DAM — Deployment & Operations Documentation

**Project:** Database Activity Monitoring at Directorate of Primary Education (DPE), Bangladesh  
**Vendor:** Imperva SecureSphere  
**Deployed by:** Infosonik Systems Limited  
**Environment:** Test Database (Pre-Production)  
**Year:** 2025

---

## Table of Contents

1. [What is a Database?](#1-what-is-a-database)
2. [Database Security Fundamentals](#2-database-security-fundamentals)
3. [Risks & Vulnerabilities](#3-risks--vulnerabilities)
4. [How Imperva DAM Fills the Gap](#4-how-imperva-dam-fills-the-gap)
5. [Architecture Overview](#5-architecture-overview)
6. [Deployment at DPE — Installation Procedure](#6-deployment-at-dpe--installation-procedure)
7. [Features & Functionalities](#7-features--functionalities)
   - [7.1 UI Customisation & Workspace](#71-ui-customisation--workspace)
   - [7.2 Log Filtering & Search](#72-log-filtering--search)
   - [7.3 System Administration & Security Definitions](#73-system-administration--security-definitions)
   - [7.4 Identity & Access Management](#74-identity--access-management)
   - [7.5 Infrastructure Setup — Global Objects & Sites Tree](#75-infrastructure-setup--global-objects--sites-tree)
   - [7.6 Asset Discovery & Operation Modes](#76-asset-discovery--operation-modes)
   - [7.7 Agent Configuration & Tuning](#77-agent-configuration--tuning)
   - [7.8 Deep Database Interactivity](#78-deep-database-interactivity)
   - [7.9 Traffic Decryption & Session Diagnostics](#79-traffic-decryption--session-diagnostics)
   - [7.10 Behavioural Profiling (SQL Profile Engine)](#710-behavioural-profiling-sql-profile-engine)
   - [7.11 Data Masking & Personal Information Masking](#711-data-masking--personal-information-masking)
   - [7.12 Data Classification](#712-data-classification)
   - [7.13 Action Interfaces & Action Sets](#713-action-interfaces--action-sets)
   - [7.14 Archiving & Retention](#714-archiving--retention)
   - [7.15 Audit Framework](#715-audit-framework)
   - [7.16 SIEM Integration & Analytics](#716-siem-integration--analytics)
   - [7.17 Security Policies & Blocking](#717-security-policies--blocking)
   - [7.18 Monitoring & Incident Response](#718-monitoring--incident-response)
   - [7.19 Risk Management & Vulnerability Assessment](#719-risk-management--vulnerability-assessment)
   - [7.20 User Rights Management (URM)](#720-user-rights-management-urm)
   - [7.21 Reporting Engine](#721-reporting-engine)
8. [Risk Elimination Summary](#8-risk-elimination-summary)
9. [Conclusion & Production Readiness](#9-conclusion--production-readiness)

---

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

## 3. Risks & Vulnerabilities

### Active Threats Against Databases

#### SQL Injection (SQLi)
Attackers insert malicious SQL code into web forms or API parameters. The application passes this unsanitised input to the database, which executes it blindly. This can result in full data extraction, authentication bypass, data deletion, or OS command execution. Native databases generally cannot distinguish a legitimate application query from a SQLi attack — they execute whatever SQL they receive.

#### Privilege Abuse
DBAs require high-level access to maintain systems. If a DBA's credentials are stolen via phishing, or if a DBA themselves goes rogue, native security cannot stop them — they *are* the gatekeeper. They have access to both the data and the audit logs that would record their theft.

#### Unpatched Software Vulnerabilities
Every DBMS version contains CVEs (Common Vulnerabilities and Exposures). A Zero-Day vulnerability in Oracle or SQL Server can be exploited by attackers before the vendor even releases a fix. Organisations running outdated DB versions are permanently exposed.

#### Misconfigurations
Databases are complex. Common misconfigurations include:
- Default vendor passwords left active
- `PUBLIC` access granted to sensitive tables
- Unnecessary network ports left open
- Over-privileged application service accounts
- DEBUG or diagnostic features left enabled in production

### Limitations of Native Database Security

| Limitation | Explanation |
|---|---|
| **Performance Penalty** | Granular native auditing (logging every single SELECT query) consumes massive CPU and disk I/O. In production environments, this can degrade application performance to unacceptable levels, so organisations either disable it or use it minimally |
| **No Segregation of Duties** | DBAs manage the database AND the audit logs. A malicious DBA can steal data and then delete the audit log that recorded the theft. This is a critical compliance failure — there is no independent, tamper-proof witness |
| **Reactive, Not Proactive** | Native logs tell you what happened *after the fact*. They do not block a user who is extracting 50,000 student records right now. Detection comes hours, days, or never |
| **Siloed Management** | Oracle, SQL Server, and Postgres each use completely different audit systems, log formats, and management tools. There is no single unified security view across a mixed database environment |
| **Encrypted Traffic Blind Spot** | When database logins use SSL/Kerberos encryption, native audit logs cannot resolve the actual username — it appears as unknown or hashed |
| **Local Session Blind Spot** | Network firewalls only see remote connections. A DBA logging directly into the physical server via a terminal is invisible to any network-level monitoring |

---

## 4. How Imperva DAM Fills the Gap

**Database Activity Monitoring (DAM)** continuously monitors and analyses all database activity in real time, completely independent of the database engine itself. Audit logs are stored on a separate hardened server where DBAs have no access — making them genuinely tamper-proof.

### Problem → Solution Mapping

| Native Security Problem | Imperva DAM Solution |
|---|---|
| **Performance Penalty** | OS-level agent captures traffic at the kernel — zero CPU/memory use from the database engine. DB runs at 100% efficiency |
| **No Segregation of Duties** | Audit logs stored on the MX Management Server. DBAs have zero login rights to the MX — they cannot alter or delete evidence of their own actions |
| **Reactive Logging** | Real-time blocking. If a SQL injection or bulk data extraction is detected, the Gateway issues a TCP reset before the database executes the malicious command |
| **Siloed Management** | Single policy written once and applied simultaneously across Oracle, Postgres, SQL Server, and MySQL from one console |
| **Encrypted Traffic** | SSL certificate upload and Kerberos key configuration allow Imperva to decrypt login sequences and resolve true usernames |
| **Local Session Blind Spot** | Agent captures traffic at the OS kernel level, including IPC and BEQ local socket connections that are completely invisible to network monitoring |

---

## 5. Architecture Overview

Imperva DAM uses a **three-tier decoupled architecture** that separates traffic capture, analysis, and management across three distinct layers. This design ensures the database server's performance is never impacted.

```
┌─────────────────────┐         ┌──────────────────────┐         ┌─────────────────────────┐
│   DATABASE SERVER   │         │      GATEWAY          │         │    MX MANAGEMENT SERVER │
│                     │         │                       │         │                         │
│  ┌───────────────┐  │  SSL    │  ┌─────────────────┐ │  Mgmt   │  ┌───────────────────┐  │
│  │  R-Agent      │──┼─────────┼─▶│  SQL Parser &   │─┼─────────┼─▶│  Web UI Console   │  │
│  │  (OS Kernel)  │  │ Port 443│  │  Policy Engine  │ │         │  │  Policy Store     │  │
│  └───────────────┘  │         │  └─────────────────┘ │         │  │  Audit Log Vault  │  │
│                     │         │                       │         │  │  Report Engine    │  │
│  Captures:          │         │  Performs:            │         │  └───────────────────┘  │
│  - TCP traffic      │         │  - Real-time parsing  │         │                         │
│  - Local IPC        │         │  - Policy matching    │         │  DBAs have NO access    │
│  - Oracle BEQ       │         │  - Active blocking    │         │  to this server         │
│  - MSSQL IPC        │         │  - Alert generation   │         │                         │
└─────────────────────┘         └──────────────────────┘         └─────────────────────────┘
```

### Tier 1 — The R-Agent (Sensor)

- Deployed directly on the database host operating system at the **kernel level**
- Intercepts database traffic **before** it reaches the database engine
- **Does zero analysis locally** — only collects and forwards encrypted traffic
- Captures traffic from ALL connection types:
  - **TCP** — remote application and user connections over the network
  - **Oracle BEQ / IPC** — local Oracle connections from the same server (e.g., a DBA using `sqlplus` in a terminal)
  - **MSSQL IPC** — local SQL Server connections (e.g., a DBA using SSMS locally)
- On Linux systems, uses **KABI (Kernel Application Binary Interface)** to hook into the OS without causing instability
- Near-zero CPU and memory footprint on the database host

### Tier 2 — The Gateway (Brain)

- A dedicated appliance or virtual machine — physically separate from the database server
- Receives all agent traffic over an **SSL-encrypted tunnel on port 443**
- Parses every SQL query in real time — understanding the query structure, the user identity, the target database objects, and the session context
- Checks each query against configured security policies
- **Active mitigation**: if a blocking rule is triggered, the Gateway issues a TCP reset command that severs the connection **before** the database executes the malicious SQL
- One Gateway can serve multiple databases and multiple agents simultaneously
- Requires dedicated CPU and RAM due to the intensity of real-time SQL parsing

### Tier 3 — The MX Management Server (Command Centre)

- The central nervous system — does not process live traffic
- Hosts the **Web UI console** where all configuration, policy management, and monitoring happens
- **Tamper-proof audit log vault** — DBAs have no credentials to this server; they cannot touch the logs
- Compiles audit data from Gateways into dashboards, analytics, and compliance reports
- Manages policy deployment, agent updates, and Gateway configuration centrally
- Triggers **Action Sets** (email, Syslog, SMS) when critical events occur

---

## 6. Deployment at DPE — Installation Procedure

The deployment was performed on a **designated test database environment** rather than the live IPEMIS production servers. This approach allows policy tuning, baseline establishment, and verification before any production change.

### Phase 0 — Agent Installation on the Database Server

**Step 1: Download the reconnaissance script**
```bash
# Access Imperva FTP server
# Navigate to: /misc/
# Download: which_ragent.sh
```

**Step 2: Identify the correct agent package**
```bash
chmod +x which_ragent.sh
./which_ragent.sh
# Script analyses OS version, kernel version, and architecture
# Outputs the exact agent package name compatible with this system
```

**Step 3: Retrieve the agent package**
```
# Return to Imperva FTP
# Download: the recommended .bsx binary package
# Download: the corresponding KABI file (if required by kernel version)
```

**Step 4: Execute the installer**
```bash
# Run as root
./install.sh
# OR
./agent_package.bsx
# Provide the KABI file path when prompted (Linux/Oracle deployments)
# The installer hooks the agent into the OS kernel
```

**Step 5: Configure the agent to point to the Gateway**
```bash
# During or after installation, configure:
# Gateway IP address: <gateway_ip>
# Gateway Port: 443
# SSL: enabled
# This establishes the secure forwarding tunnel
```

---

### Phase 1 — Logical Infrastructure Setup in MX Console

**Navigation:** `Main > Setup > Sites`

1. Create a new **Site** — the top-level container (e.g., `DPE_Datacenter`)
2. Under the Site, create a **Server Group** — organise by OS/DB type:
   - `windows_server_db` — for Windows-hosted databases (MS SQL)
   - `linux_oracle_db` — for Linux-hosted databases (Oracle)
3. Under the Server Group, add a **Server**:
   - Provide the exact **IP address** of the database host
   - Provide the exact **Hostname** of the database host
4. Under the Server, add a **Database Service**:
   - Select the database type: `MsSql` / `Oracle` / `MySql` / `Postgres`
   - Define the listening port: `1433` (MS SQL) or `1521` (Oracle)

> **Pro-Tip:** If the database sits behind a load balancer, configure the **Forwarded Connections** section in the Service's Operation tab. Use `X-Forwarded-For` for F5/Cisco load balancers, or `True-Client-IP` for Akamai.

---

### Phase 2 — Gateway Listener Configuration

**Navigation:** `Main > Setup > Gateways`

1. Select your active Gateway from the list
2. Confirm an **Agent Listener** is active
3. Standard configuration:
   - **Port:** `443`
   - **SSL:** Enabled
4. All traffic from agents to the Gateway is encrypted in transit

---

### Phase 3 — Agent Assignment & Service Mapping

**Navigation:** `Main > Setup > Agents`

1. Locate the newly installed agent — it appears **automatically** once the OS-level installation points to the Gateway (this confirms the network path is open)
2. Select the agent → go to the **Settings tab**
3. **Assign** the agent to the Server Group created in Phase 1
4. **Enable Agent Monitoring** specifically for the Database Service created in Phase 1

---

### Phase 4 — Data Interface Binding *(Critical Step)*

**Navigation:** `Main > Setup > Agents > [Your Agent] > Data Interfaces tab`

This is the most important configuration step. Without it, local DBA sessions are completely invisible.

1. Select your agent and click the **Data Interfaces** tab
2. On the right side, view **Discovered Data Interfaces** — the agent automatically detects active protocols on the server

3. **For Windows / MS SQL Server:**

   | Interface | Purpose | Action |
   |---|---|---|
   | `MssQL IPC` | Local SQL Server Management Studio (SSMS) sessions | Un-ignore ✔ |
   | `TCP` | Remote application and user connections | Un-ignore ✔ |

4. **For Linux / Oracle:**

   | Interface | Purpose | Action |
   |---|---|---|
   | `Oracle BEQ` | Local `sqlplus` terminal sessions (same server) | Un-ignore ✔ |
   | `Oracle IPC` | Local inter-process communication sessions | Un-ignore ✔ |
   | `TCP` | Remote application and user connections | Un-ignore ✔ |

5. For every un-ignored interface, set the **Service** drop-down to match the exact Database Service created in Phase 1
6. Click **Save**

> **Why this matters:** Network firewalls only see TCP connections from remote hosts. A DBA who logs directly into the database server via a local terminal uses IPC/BEQ — completely bypassing the network layer. Without binding these interfaces, that entire category of administrative activity is a blind spot.

---

### Phase 5 — Live Verification

**Navigation:** `Main > Audit > DB Audit Data`

1. Select the **Data view** in the left-hand menu
2. Execute test queries directly on the database:
   - Remote query via application simulation
   - Local query directly on the server as an administrator
3. Set **Time Frame** to `Last Hour`
4. Click **Update**
5. Both query types should appear in the audit log ✔

**What this confirms:**
- The agent is successfully capturing and forwarding traffic
- The Gateway is successfully parsing SQL and identifying users
- The MX is successfully storing tamper-proof audit records
- Both remote AND local admin sessions are visible

---

### Verifying Database Site Objects

After deployment, verify foundational settings before enforcing policies:

**Navigation:** `Main > Setup > Sites > [Your Server Group]`

- **Definitions tab → Operation Mode:** Confirm it is set to `Simulation` (never start in Active)
- **Protected IP Addresses:** Confirm the correct database IP is listed
- **Database Service → Definitions tab → Ports:** Confirm the correct listening port is listed (e.g., `1433`, `1521`)

---

## 7. Features & Functionalities

---

### 7.1 UI Customisation & Workspace

#### Customising the Default Home Page
**Navigation:** `Preferences > Preferences > Customize User Interface Settings > Default Home Page`

Each user role can be configured to land on the most relevant dashboard immediately after login — for example:
- **Security Analyst** → Monitor > Alerts
- **Auditor** → Audit > DB Audit Data
- **DBA** → Main > Setup > Sites

#### Toggling Workspace Views (Hanging Tabs)
**Navigation:** `Main > Monitor > Alerts (or Violations / System Events)`

- **Left hanging tab (Filter):** Show/hide the filter criteria pane
- **Right hanging tab (Details):** Show/hide the deep-dive details pane for a selected log entry
- Use these to maximise screen real estate during active incident investigation

#### Customising Display Columns
**Navigation:** `Main > Monitor > Alerts (or Violations) > Action > Select Columns`

1. Click the **Action** menu above the filter pane
2. Select **Select Columns**
3. Move desired fields from the left pane to the display pane using the `>>` button
4. Click **OK**

Recommended columns for security operations: Source IP, Server Group, DB User, Alert Name, Severity, Timestamp, Number of Events.

---

### 7.2 Log Filtering & Search

#### Basic Filters
**Navigation:** `Main > Monitor > System Events (or Alerts / Violations)`

1. Ensure the Filter pane is visible (left hanging tab)
2. Expand categories such as **By Severity**, **By SubSystem**
3. Check the desired criteria boxes
4. Click **Apply**
5. To save: click **Save** at the top of the Filter pane and provide a standardised name (e.g., `Team_Name-Filter_Name`)

#### Advanced Filters
**Navigation:** `Main > Monitor > System Events > Advanced >>`

1. Click **Advanced >>** at the bottom of the Basic Filter pane
2. Select criteria from **Available Fields** and move to **Enabled Fields** using the Up Arrow
3. For each field, set the **Operation** (Equals, Not Equals, Contains, etc.)
4. Set **Values** from the Predefined Values list
5. Click **Apply** to run or **Save** to store

#### Quick Filter (Text Search)
Located in the text box directly above any log list:

| Syntax | Example | Behaviour |
|---|---|---|
| Literal text | `login failed` | Exact phrase match |
| IP attribute | `ip:192.168.1.10` | Filter by source IP |
| User attribute | `user:admin` | Filter by DB username |
| Session attribute | `session:xyz123` | Filter by session ID |

- Press **Enter** or click the magnifying glass icon to execute
- Click the **red X** icon to clear the filter

---

### 7.3 System Administration & Security Definitions

#### Password Strength Requirements
**Navigation:** `Admin > System Definitions > Security and Authentication > Password Settings`

Configurable parameters:
- Password validity period (days)
- Minimum password length
- Required: capital letters
- Required: numbers
- Required: non-alphanumeric (special) characters

Click the **blue Save** button after making changes.

> **Default:** 7-character minimum with numbers and lowercase letters — upgrade this immediately for any production deployment.

#### Enabling Policy Change Comments
**Navigation:** `Admin > System Definitions > Policy Settings`

1. Check **Enable comments for security policy changes**
2. Save

**Effect:** Every time an administrator modifies a security policy, they are forced to provide a written reason. This creates an **Activity Log tab** on each policy that records: who changed it, what they changed, when, and why — essential for compliance auditing.

#### Scheduling ADC Content Updates
**Navigation:** `Admin > ADC`

- Configure an automated **Daily** update schedule (maximum interval: every 2 weeks)
- Imperva's Application Defense Center (ADC) researches new vulnerabilities and releases updated signatures
- **Prerequisite:** MX requires DNS resolution and internet access
- If the environment uses an authenticated proxy: `Admin > System Definitions > Management Server Settings > External HTTP Settings`

---

### 7.4 Identity & Access Management

#### Creating Local User Accounts
**Navigation:** `Admin > Users & Permissions > Users & Roles > Create > Create New User`

1. Enter a **User Name**
2. Set **Authenticator** to `SecureSphere` (for a local account)
3. Enter a **temporary password** — the system forces a change on first login
4. Select the appropriate **role** from the Assigned Roles box
5. Click **Create**

#### Building Custom Roles & Permissions (RBAC)
**Navigation:** `Admin > Users & Permissions > Users & Roles > Create > Create Role`

1. Provide a role name
2. **Navigation tab:** Grant access to specific Workspaces, Tabs, and Menus (e.g., allow Monitor but not Setup)
3. **Permissions tab:** Grant `View`, `Edit`, or `Create` rights per object category
4. Click the **yellow pencil (Edit Object)** icon next to a category for granular object-level restrictions

Example role configurations:

| Role | Access Level |
|---|---|
| **Read-Only Auditor** | View access to Monitor, Audit, Reports — no Setup or Policy access |
| **Security Analyst** | View + Edit access to Monitor, Policies — no Admin access |
| **DBA Observer** | View access to Setup > Sites only — no policy or audit access |

#### Configuring External Authentication (Active Directory / LDAP)
**Navigation:** `Admin > System Definitions > Management Server Settings > External Systems`

**Step 1 — Create the connection profile:**
1. Create a new External System
2. Set Type to `LDAP Authentication & Authorization`
3. Configure: Primary Server IP, Port, Access Mode, Account DN, Search Base, Search Filter
4. Click **Test Connection** to validate before saving

**Step 2 — Apply the profile:**
1. Navigate to `Security and Authentication > Authentication & Authorization Configuration`
2. Change **User Authentication** to `External` (or `User Specific` for mixed mode)
3. Select the newly created External System from the drop-down

#### Personal Information Masking (PIM)
**Navigation:** MX CLI → `Admin > Users & Permissions` → `Preferences`

PIM is a secondary masking layer that hashes personal fields in the UI using HMAC-SHA256 (base64-encoded), so even the Imperva console operators cannot see raw user identities.

**Fields masked by PIM:**
- Username
- OS Hostname
- Source IP Address

**Enable via CLI:**
```bash
# SSH into the MX Management Server
# Edit the properties file:
vi /opt/SecureSphere/server/SecureSphere/jakarta-tomcat-secsph/webapps/SecureSphere/WEB-INF/properties/dam.properties

# Change:
dam.personal.info.masking.activated=false
# To:
dam.personal.info.masking.activated=true

# Restart the server:
impctl server restart
```

**Authorise users to unmask:**
`Admin > Users & Permissions > [User or Role] > Global Settings tab`
→ Check: *"The user is authorized to unmask personal info"*

**To unmask for investigation:**
`Preferences > Preferences > Data Masking section > Unmask`
(Effect lasts for the current session only)

---

### 7.5 Infrastructure Setup — Global Objects & Sites Tree

#### Managing Global Objects
**Navigation:** `Main > Setup > Global Objects`

Global Objects are centrally managed containers that can be referenced across multiple server groups and policies — set them once, use them everywhere.

| Object Type | Purpose |
|---|---|
| **IP Groups** | Named collections of IP addresses (e.g., "DBA Workstations", "Application Servers") |
| **Windows Domains** | Active Directory domain definitions for user resolution |
| **Stored Procedure Groups** | Collections of stored procedures for audit mapping |
| **Agent Monitoring Rules** | Rules for excluding trusted connections at the agent level |

**Shortcut:** When working inside any configuration menu (e.g., a Server Group), click the **yellow pencil (Edit Object)** button next to a drop-down field to jump directly to the relevant Global Object configuration.

#### Building the Sites Tree Hierarchy
**Navigation:** `Main > Setup > Sites`

The Sites Tree is the organisational backbone of the entire deployment:

```
Site (e.g., DPE_Datacenter)
└── Server Group (e.g., windows_server_db)
    ├── Server (IP: 192.168.1.50, Hostname: SQLSRV01)
    └── Database Service (Type: MsSql, Port: 1433)
        └── Application (Default MsSql Application)
```

**Step-by-step:**
1. Create a **Site** — organise by datacenter, environment (prod/test), or business unit
2. Create a **Server Group** — represents physical or clustered servers
3. Add a **Service** — defines the monitored protocol and ports
   - Load balancer configuration: set **X-Forwarded-For** or **True-Client-IP** in the Forwarded Connections section under the Service's Operation tab
4. The system auto-generates a default **Application** — rename it or create additional applications for granular policy control

---

### 7.6 Asset Discovery & Operation Modes

#### Running Service Discovery Scans
**Navigation:** `Main > Discovery & Classification > Scans Management`

Actively probes network subnets to detect unknown, unauthorised, or shadow databases that are currently unprotected (e.g., a test database spun up by a developer without IT knowledge).

**Configuration:**
1. Define the scan scope: specific IPs, subnets, IP ranges
2. Include checks for **non-standard ports** (databases not on their default ports)
3. Configure **New Entities Configuration** naming templates using placeholders:
   - `$SITE` — the site name
   - `$IP` — the discovered server IP
   - `$SERVICE_TYPE` — the detected database type
4. Choose: auto-add discovered servers to Sites Tree, or leave for manual review

#### Reviewing and Protecting Discovered Servers
**Navigation:** `Main > Discovery & Classification > Discovered Servers`

1. Review results in **Summary View** or **Pending Servers** view
2. **Accept** valid servers → creates the Server Group/Service and adds the server to the Servers tab
3. **Reject** servers → hides them from future scan results (confirmed non-targets)
4. Navigate to `Main > Setup > Sites > [Server Group] > Servers tab`
5. Click the discovered server row → click **Add to Protected IPs**
6. Verify: a **blue checkmark** appears in the Protected column

#### Managing Server Group Operation Modes
**Navigation:** `Main > Setup > Sites > [Server Group] > Definitions tab > Operation`

| Mode | Behaviour |
|---|---|
| **Simulation** | Inspect all traffic and log violations — but never drop any packets. **Default for all new Server Groups.** Use this during policy tuning. |
| **Active** | Actively drop traffic that violates blocking rules. Switch to this only after simulation review. |
| **Disabled** | Completely bypass all inspection and logging for IPs in this group. Use only for maintenance windows. |

> **Best Practice:** Always start in Simulation. Run for a sufficient period (including nightly batch jobs and scheduled tasks), review Alert Reports with DBAs to eliminate false positives, then switch to Active.

---

### 7.7 Agent Configuration & Tuning

#### Tuning Agent Settings
**Navigation:** `Main > Setup > Agents > [Agent] > Settings tab`

- **Enable CPU Usage Restraining:** Check this and set a maximum CPU percentage limit — prevents the agent from overloading the database host server
- **Blocking:** Check this if you want the agent to directly block traffic based on policies (only effective when the Server Group is in Active mode)
- **Advanced Configuration:** Input specific tuning parameters as directed by Imperva Support (e.g., enabling DHE encrypted traffic decryption)

#### Configuring Data Interfaces
**Navigation:** `Main > Setup > Agents > [Agent] > Data Interfaces tab`

- **Right pane (Discovered):** Auto-detected interfaces — Oracle BEQ, Oracle IPC, MSSQL IPC, TCP
- **Left pane (User Defined):** Manually define interfaces not auto-discovered (e.g., Oracle BEQ/IPC on Windows)
- For each interface to monitor: **uncheck the red Ignore box** and set the **Service** drop-down to the correct Database Service

#### Creating Agent Exclusion Rules
**Navigation:** `Main > Setup > Global Objects > Scope: Agent Monitoring Rules`

Use to exclude high-volume, trusted connections from being sent to the Gateway — saving network bandwidth and Gateway CPU. Common example: nightly database backup jobs.

**Configuration:**
1. Set Scope Selection to `Agent Monitoring Rules`
2. Create a new rule, set action to `Exclude from Monitoring`
3. In Match Criteria, **you must use criteria starting with "Agent Criteria -"**:
   - `Agent Criteria - Destination IP`
   - `Agent Criteria - Process Details`
   - `Agent Criteria - Application Name`

> **Warning:** Exclusion rules operate at the TCP connection level. Once a connection is excluded, **all activity within that session is invisible** — even if the same session later accesses sensitive tables. Use exclusions only for completely trusted, well-defined processes.

---

### 7.8 Deep Database Interactivity

#### Configuring Direct Access Credentials
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Direct Access Information`

Imperva requires dedicated database credentials to log directly into the database for internal operations:
- Reading stored procedure definitions
- Running Data Classification scans
- Running Vulnerability Assessment scans
- Running User Rights Management scans

**Setup:**
1. Enter a dedicated **User Name** and **Password**
2. Best practice: use a **dedicated privileged account created specifically for Imperva** — never a shared DBA account
3. Click **Test Connection** to validate before saving

#### Applying Stored Procedure Groups
**Navigation:** `Main > Setup > Global Objects > Scope: Stored Procedure Groups`

Stored procedures are precompiled — when called, the DB engine executes internal SQL that Imperva cannot natively intercept. Loading Stored Procedure Groups allows Imperva to map those internal SQL statements and audit them.

1. Create or review Stored Procedure Groups in Global Objects
2. Navigate to `Main > Setup > Sites > [DB Application]`
3. Apply the relevant Stored Procedure Group to the application

#### Manually Configuring Kerberos Keys
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Encryption Support > Manually Configured Kerberos Keys`

MS SQL Server in Active Directory environments uses Kerberos to encrypt domain user logins. Without the Kerberos key, Imperva cannot resolve domain usernames.

**Setup:**
1. Expand **Encryption Support** on the DB Service's Definitions tab
2. Click the **yellow pencil** next to Manually Configured Kerberos Keys
3. Click **Create New (+)**
4. Enter the domain and username: `DOMAIN\username`
5. Enter the exact password in both Password and Verify fields
6. Save → close the window
7. Select the newly created credentials from the drop-down in the Kerberos Keys table
8. Click **Save**

---

### 7.9 Traffic Decryption & Session Diagnostics

#### Resolving "Hashed User" (SSL-Encrypted Logins)
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Encryption Support`

**Symptom:** Audit logs show `Hashed User (unknown server SSL certificate)` instead of the actual username.  
**Cause:** The database login is encrypted with SSL — Imperva sees the encrypted handshake but cannot read the credentials inside it.  
**Fix:** Upload the database server's SSL certificate.

**Steps:**
1. On the DB Service Definitions tab, expand **Encryption Support**
2. Click **Create New (green +)**
3. Enter a key name
4. Select certificate format: `PKCS12/PFX`
5. Click **Browse** and upload the certificate file
6. Enter the certificate password
7. Click **Upload** → **Save**

#### Resolving "Connected User" (Missed Login Phase)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Connection Settings`

**Symptom:** Audit logs show `Connected User` instead of the actual username.  
**Cause:** Imperva missed the initial login packet — the session was already established when the Gateway or Agent came online, or the connection timeout mismatch caused Imperva to lose session context.  
**Fix:** Match Imperva's connection timeout to the database server's session timeout.

**Steps:**
1. Navigate to your Database Service → **Operation tab**
2. Expand **Connection Settings**
3. Adjust **Connection Timeout (Seconds)** to match the database server's exact session timeout value

#### Text Replacement (Query Normalisation)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Text Replacement`

**Problem:** Highly dynamic applications (e.g., ORM frameworks) generate thousands of slightly different query structures that bloat the profile with thousands of unique Query Groups.  
**Solution:** Text Replacement strips out dynamic variable portions and replaces them with static text before the query reaches the profiling engine.

**Steps:**
1. Review profile bloat at `Main > Profiles > Profile Overview`
2. Navigate to the relevant DB Service → Operation tab → expand **Text Replacement**
3. Configure replacement rules targeting:
   - Normalised Query patterns
   - User Name patterns
   - Application User Name patterns
4. After saving, go to the DB Application Profile and **manually delete** the old bloated query groups
5. Allow Imperva to relearn clean, normalised query structures

---

### 7.10 Behavioural Profiling (SQL Profile Engine)

Behavioural profiling is Imperva's **whitelist-based machine learning engine**. It observes all database activity over time and builds a precise model of what "normal" looks like for each user. Any deviation from that model becomes a security violation.

#### How Learning Works

The moment a Database Application is created and the SQL Profile Policy is applied (automatically), the engine begins passively watching. For every database user observed, it builds a profile containing:

| Profile Component | What Is Learned |
|---|---|
| **Source IPs** | Which client IP addresses this user connects from |
| **OS Hostnames** | Source machine hostnames |
| **Source Applications** | Application names passing the connection (JDBC, SSMS, app name) |
| **Query Groups** | Normalised SQL structures — `SELECT * FROM t WHERE id = ?` |
| **Tables & Operations** | Which tables the user accesses and with what operation type |
| **Privileged Operations** | Whether the user executes DDL: CREATE, DROP, ALTER, GRANT |

All learning happens in **Learning Mode** — zero alerts, zero blocks, pure observation.

#### Query Normalisation

Imperva replaces literal values with `?` to create a canonical query structure:

```sql
-- These three queries become ONE Query Group:
SELECT * FROM students WHERE id = 5;
SELECT * FROM students WHERE id = 102;
SELECT * FROM students WHERE id = 9847;

-- Normalised to:
SELECT * FROM students WHERE id = ?
```

This prevents the profile from exploding into millions of entries for parameterised queries.

#### Enforcement Mode Lifecycle

```
Learning Mode ──(time threshold met)──▶ Dynamic Protect ──(time threshold met)──▶ Static Protect
    │                                          │                                          │
No alerts                              Alerts on new queries                    Alerts on ANY
No blocks                              Profile can still grow                   deviation — zero
Pure observation                       slightly                                 tolerance
```

Time thresholds are configurable at:  
`Admin > System Definitions > Dynamic Profiling > Switching to "Protect" Mode Thresholds (SQL)`

#### Viewing Profiles

**Navigation:** `Main > Profiles > Profile Overview`

Aggregate statistics across all sites: total profiled users, query groups, tables.

**Navigation:** `Main > Profiles > Application View > [DB Application] > Users`

Drill into a specific user's profile to see all learned components.

#### Profile Detail Tabs

**Learned Tab**
`Main > Profiles > Application View > [User] > Learned tab`

- Review: Destination, Tables & Operations, Source categories
- **Add (+):** Manually insert an acceptable attribute (e.g., a new trusted IP)
- **Delete (x):** Remove an entry from the learned profile
- **Configure (wrench):** Set `Allow Any Value` or `Allow Empty Value` for highly dynamic fields
- **Padlock icon:** Lock a category — new values will trigger violations instead of being absorbed

**Query Groups Tab**
`Main > Profiles > Application View > [User] > Query Groups tab`

- **Expand (+):** Read the literal SQL statements within a Query Group
- **Enforced Mode drop-down:** Manually override the mode: `Static Protect`, `Dynamic Protect`, `Learning`
- **Filter:** Select a specific Table + Operation to isolate matching Query Groups

**Restrictions Tab**
`Main > Profiles > Application View > [User] > Restrictions tab`

- **Blacklist Table Groups:** Click `+` to add restricted table groups — the user can never access these tables
  - Prerequisite: the Table Group must already be applied to the DB Application under Setup > Sites
- **Time Restrictions:** Click and drag over the weekly grid to paint permitted access hours purple — access outside these hours triggers a violation

**Privileged Operations Tab**
`Main > Profiles > Application View > [User] > Privileged Operations tab`

Check the boxes for operations this user is authorised to perform: `ALTER`, `DROP`, `GRANT`, `BACKUP`, `CREATE`, etc. Unchecked operations trigger alerts when executed.

**Learning Preferences Tab**
`Main > Profiles > Application View > [User] > Learning Preferences tab`

| Setting | Options |
|---|---|
| **Profiling policy** | No profiling / Profile only sensitive tables / Profile all Query Groups |
| **Unprofiled queries** | Open mode (ignore) / Closed mode (immediately flag as violation) |
| **Query count thresholds** | Override global settings for this specific user/group |

#### User Group Profiles

**Navigation:** `Main > Profiles > Application View > User Groups`

Group users with identical job functions (e.g., `DBA_Group`, `App_Service_Accounts`, `Reporting_Users`) so they share the exact same profile. Individual users moved into a group automatically inherit all shared attributes.

This drastically reduces management overhead in environments with dozens or hundreds of database users.

#### Tuning Protect Mode Thresholds (Global)

**Navigation:** `Admin > System Definitions > Dynamic Profiling > Switching to "Protect" Mode Thresholds (SQL)`

Adjust duration parameters (in hours) for:
- Closing a user's query group list
- Locking source/schema pairs
- Switching query groups from Dynamic to Static Protect mode

#### Freezing the Learning Engine

**Navigation:** `Main > Policies > Security > SQL Profile Policy > Advanced tab`

Once the environment is stable and the profile is mature:

1. Select the SQL Profile Policy applied to your database
2. Click the **Advanced tab**
3. Check **Disable learning engine**
4. Click **Save**

Effect: the profile is frozen exactly as-is. New queries are no longer absorbed. CPU usage on the Gateway drops. Violations fire for any never-before-seen query structure.

#### Reverting Profile Enforcement

**Navigation:** `Main > Profiles > Application View > [User/Group] > Learning Preferences tab`

If an application undergoes significant changes or the environment becomes too dynamic for strict profiling:

1. First, disable the learning engine via the SQL Profile Policy Advanced tab
2. Navigate to the relevant User or User Group profile
3. Click **Learning Preferences tab**
4. Click **Switch all Query Groups to Learning** → **Apply** → **OK**

All Query Groups revert to Learning mode — no violations are invoked. The profile starts rebuilding from scratch.

#### Actions Triggered by Profile Violations

When a deviation from the learned profile is detected, the following chain occurs:

```
Deviation Detected
        │
        ▼
Violation Logged (Main > Monitor > Violations)
  - Exact SQL statement
  - Source IP
  - DB Username
  - Matched policy rule
        │
        ▼
Alert Aggregated (Main > Monitor > Alerts)
  - Groups up to 30 identical violations
  - Tracks hit count for up to 12 hours
        │
        ▼
Action Set Fires (if configured)
  - Syslog to SIEM
  - Email to DBA team
  - Terminate DB Session
  - User Block (lock DB account)
  - IP Block (drop all traffic from source IP)
        │
        ▼
(If Server Group is Active + Rule Action = Block)
Gateway issues TCP reset → connection severed
```

> **Critical Note:** Profile-based blocking only works when Query Groups are in **Static Protect** or **Dynamic Protect** mode. Groups in Learning mode will **never** trigger a block, even if the SQL Profile Policy rule is set to Block.

---

### 7.11 Data Masking & Personal Information Masking

#### Configuring Data Masking (Sensitive Dictionaries)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Data Masking`

Masks sensitive data values with asterisks `***` **before** they are stored in the MX's internal database. This means even Imperva console users cannot see raw sensitive values in audit logs.

**Setup:**
1. Select the Database Service → **Operation tab**
2. Expand **Data Masking**
3. Under **Sensitive Dictionaries**, select predefined dictionaries:
   - MasterCard Credit Card Numbers
   - Visa Credit Card Numbers
   - US Social Security Numbers
   - Custom dictionaries for Bangladesh NID etc.
4. Use the drop-downs to apply dictionaries to:
   - **Requests** (Alerts, Violations, DB Audit logs)
   - **DB Audit Responses**
5. Optionally: apply specific dictionaries to specific table groups under **Specific Table Groups**

---

### 7.12 Data Classification

DB Data Classification is Imperva's **sensitive data discovery engine**. It actively logs into the database and scans table columns — either by column name or by actual content — to create a complete map of where sensitive data lives.

#### Step 1 — Ensure Direct Access Credentials Are Configured

See [Section 7.8 — Direct Access Credentials](#78-deep-database-interactivity). Classification requires Imperva to log into the database.

#### Step 2 — Define Data Types

**Navigation:** `Main > Discovery & Classification > Scans Management > Scope: Data Types Configuration`

Data Types are the labels for categories of sensitive data to discover:

| Predefined Data Types | Examples |
|---|---|
| Email Address | Columns containing email format values |
| Credit Card Numbers | PAN data matching Luhn algorithm patterns |
| Phone Numbers | Various national phone number formats |
| Address | Street address pattern matching |
| Account Numbers | Financial account number patterns |

**To create a custom Data Type:**
1. Set Scope Selection to `Data Types Configuration`
2. Click **Create New**
3. Name it (e.g., `Bangladesh_NID`, `DPE_Teacher_ID`)
4. Assign classification rules to it

#### Step 3 — Build a Scan Profile

**Navigation:** `Scans Management > Scope: Scan Profiles > Create New`

A Scan Profile is reusable configuration defining how to scan:

- **Data Types tab:** Select which Data Types to search for
- **Settings tab:**

| Parameter | Description | Default |
|---|---|---|
| Data Sample Accuracy | Percentage of rows sampled per table | 75% (0.75) |
| Delay Between Queries | Pause between scan queries (milliseconds) | Configurable |
| Max Concurrent Connections | Parallel scan threads | Configurable |
| Include/Exclude schemas | Scope the scan to specific databases or schemas | All |

#### Step 4 — Write Custom Classification Rules (Optional)

**Navigation:** `Scans Management > [Specific scan] > Settings tab > Custom Classification Rules > +`

For proprietary or organisation-specific data formats:

**Name-Based Rules (Column/Table Name Matching):**
```
Pattern: nid
Matches columns named: national_id, nid, student_nid, nid_number
```

**Content-Based Rules (Regex on Actual Data):**
```regex
# Bangladesh NID (13 digits)
^\d{13}$

# Bangladesh NID (10 digits — older format)
^\d{10}$

# Email address pattern
^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$
```

**Steps:**
1. Click **+** in Custom Classification Rules section
2. Name the rule and assign to a Data Type
3. Click the **yellow pencil** to open the Custom Patterns window
4. Enable **Custom Name Based Patterns** — enter column name keywords
5. Enable **Custom Content Based Patterns** — write your Regex
6. Save

#### Step 5 — Create and Run the Scan

**Navigation:** `Scans Management > Create New > DB Data Classification Scan`

1. Select Data Types to scan for
2. Apply the Scan Profile
3. Go to **Apply To tab**
4. Select the **Direct Access Connection credentials** (links to the DB Service)
5. Save and **Run**

The scan executes against the Protected IPs defined under the selected DB Service.

#### Step 6 — Analyse Scan Results

**Navigation:** `Scans Management > [Your Scan] > Scan Results`

**Summary View:**
- Coloured distribution graphs showing where sensitive data was found
- Breakdown by database, schema, table, data type
- Click any coloured band → adds that data slice to your active filter

**DB Data Classification Results View:**
- Row-by-row table of every single finding
- Click **Column Options** to toggle columns: DB, Table Group, Schema, Rule, Discovery Method, Accuracy
- Expand individual rows to see:
  - Exact column that matched
  - Discovery method: Name-Based vs Content-Based
  - Discovery accuracy percentage

#### Step 7 — Accept Results & Generate Table Groups

**Navigation:** `Scan Results > Pending Tables View`

1. Navigate to **Pending Tables View** — all findings awaiting approval
2. Click a link in the **Data Type column** (e.g., `Email Address`)
3. Select **Perform Action**:
   - **This Field** — Accept all instances of this data type across all tables
   - **This Row** — Accept only this specific table
4. Click **Accept All New Tables**

**What happens automatically:**
- A **Global Database Table Group** is created (named after the Data Type)
- It is linked to your Database Application
- It appears in `Main > Setup > Sites > [DB Application] > Definitions tab > Table Groups`

#### Step 8 — Use Table Groups in Policies, Profiles & Masking

Once Table Groups exist, they can be used across the entire platform:

| Feature | How to Use the Table Group |
|---|---|
| **Security Policy** | Create a rule that fires whenever anyone queries this Table Group |
| **Behavioural Profile** | Add to a user's Blacklist Tables — they can never access these tables |
| **Data Masking** | Apply a masking dictionary specifically to this Table Group |
| **Audit Policy** | Scope audit capture to only queries touching these sensitive tables |

#### Verification After Accepting

**Navigation:** `Main > Setup > Sites > [DB Application] > Definitions tab`

Confirm the newly created Table Group appears in the **Table Groups** list on the Definitions tab.

---

### 7.13 Action Interfaces & Action Sets

#### Configuring Action Interfaces
**Navigation:** `Main > Setup > Action Interfaces > Create`

Action Interfaces define the technical communication channels Imperva uses for notifications and archiving.

| Interface Type | Use Case | Key Parameters |
|---|---|---|
| **Email (SMTP)** | Alert notifications to DBA/Security team | SMTP server, port, sender, auth |
| **Syslog** | Forward logs to SIEM (Splunk, QRadar, etc.) | Syslog server IP, port, CEF format |
| **FTP Archive** | Transfer audit archives to FTP server | Server IP, credentials, path |
| **SCP Archive** | Secure file transfer for audit archives | Server IP, SSH key/credentials, path |
| **SNMP Trap** | Network management system alerts | SNMP community string, OID |
| **SMS** | Critical alert notifications | SMS gateway API details |

#### Building Action Sets
**Navigation:** `Main > Policies > Action Sets > Create`

Action Sets group one or more Action Interfaces and define when they fire.

1. Provide a descriptive name
2. Define the **Event Scope** (what triggers this Action Set):
   - Security Violations
   - System Events
   - Audit events
   - Archiving completion
   - Assessment completion
3. Define the **Application Level** at which it applies:
   - Service Level
   - Application Level
   - DB Application Level
4. Add the Action Interface(s) to the set
5. Save

> **Note:** If an Action Set does not appear in a policy rule's Followed Action drop-down, check that its **Type** was set to `Security Violations` during creation.

#### Attaching Followed Actions to Security Rules
**Navigation:** `Main > Policies > Security > [Policy] > Policy Rules tab`

1. Select the specific rule
2. Locate the **Followed Action** column for that rule
3. Select a pre-configured Action Set from the drop-down
4. Click **Save**

---

### 7.14 Archiving & Retention

#### Applying Followed Actions to System Jobs
**Navigation:** System Administration / Archiving Settings

1. Navigate to the specific system job (MX Database Backup, Audit Archiving)
2. Locate the **Followed Action** section
3. Select an Action Set containing an FTP or SCP archive interface
4. Save

> **Troubleshooting:** If a file transfer fails (FTP server down), Imperva stores the file locally at:  
> `/var/tmp/failed_delivery/`  
> Retrieve manually when the external server is back online.

#### Report Retention (Reports Archive Job)
**Navigation:** System Administration / Archiving Jobs

1. Locate the **Reports Archive** job
2. Configure an Archiving Action Set (FTP/SCP)
3. Enable **Purge Job**
4. Set the **retention timeframe** — reports older than this threshold are purged from the MX after successful archiving

#### Configuring Audit Archiving Locations
**Navigation:** `Main > Policies > Audit > [Policy] > Archiving tab`

1. Assign an **Archiving Action Set** (FTP, SCP, or NFS mount)
2. Set the recurring **Schedule** (e.g., daily at 04:30)
3. Set the **Purge records threshold**

> **Important:** The global Audit Purge job will only delete records from the Gateway if they are both:
> - Older than the purge threshold, **AND**
> - Successfully transferred to the archive destination

---

### 7.15 Audit Framework

#### Viewing DB Audit Data
**Navigation:** `Main > Audit > DB Audit Data`

1. In the Scope pane, select the **Audit Policy** to review
2. Set the **Time Frame** (Last 1 Hour, Last 3 Months, or custom range)
3. **Fast View** — data the MX has pre-fetched from Gateways and cached locally; loads instantly
4. **Live Fetch** — for time ranges outside the Fast View cache, click **Update** to force the MX to query Gateways and merge real-time data

#### Reviewing Parsed Queries
**Navigation:** `Audit > DB Audit Data > Default Rule - All Events > Data view > Select Columns`

1. Select the **Default Rule - All Events** policy
2. Switch to **Data view**
3. Click **Select Columns**
4. Replace a less-needed column (e.g., Service) with **Parsed Query**
5. Click Save

**Parsed Query format:** Variables are replaced with `?`, reducing millions of parameterised queries into a manageable number of unique structural patterns.

#### Creating Custom Audit Policies
**Navigation:** `Main > Policies > Audit > Create New`

Replace the resource-intensive "Default Rule - All Events" with targeted policies:

1. Select policy level: **DB Service** or **DB Application**
2. Configure **Match Criteria**:
   - Specific Event Types (SELECT, INSERT, DDL, login events)
   - Specific Database Users
   - Specific Tables (using Table Groups from Classification)
3. Save and apply to the relevant DB objects

**Benefit:** Auditing only what matters reduces Gateway CPU usage and MX disk consumption significantly.

#### Tuning Audit Aggregation & Fast View
**Navigation:** `Main > Policies > Audit > [Policy] > Settings tab`

| Setting | Description | Example |
|---|---|---|
| **Event aggregation time** | Window in minutes — identical queries within this window are collapsed into one row with a hit counter | 30 minutes: 100 identical queries → 1 row, count = 100 |
| **Fast View days** | Number of days of audit data pre-fetched to MX for instant loading | 7 days (monitor MX disk space when increasing) |

#### Enabling Extended Data Collection
**Navigation:** `Audit > [Policy] > Settings tab > Data Collection`

| Option | What It Captures | Impact |
|---|---|---|
| **Extended Collection Events** | Unparsed (literal) SQL queries, exact response sizes, detailed timestamps | Increased storage per event |
| **DB Responses** | The actual data returned by the database to the user | Significant storage increase — scope carefully |

If DB Responses are enabled, go to **DB Response Auditing** and explicitly define which tables/columns to Include or Exclude to prevent storage from being overwhelmed.

---

### 7.16 SIEM Integration & Analytics

#### SIEM Integration via External Logger
**Navigation:** `Main > Policies > Audit > [Policy] > External Logger tab`

1. Select the pre-configured **Audit Syslog Action** from the drop-down
2. **Default behaviour:** MX sends Syslog to the SIEM
3. For lower latency: check **Enable using gateway configuration if exists** — the Gateway sends directly to the SIEM (requires Gateway Group to have an External Logger IP/Port defined under `Setup > Gateways`)

**Supported format:** Syslog CEF (Common Event Format) — compatible with Splunk, IBM QRadar, ArcSight, and most enterprise SIEM platforms.

#### Using Audit Analytics Views
**Navigation:** `Main > Audit > DB Audit Data > Views section (bottom-left)`

Predefined analytical views for rapid investigation:

| View | What It Shows |
|---|---|
| **Source Analysis** | Traffic breakdown by source IP, application, OS hostname |
| **Privileged Operations** | DDL and administrative command activity |
| **Time Based Analysis** | Activity volume over time — detect unusual after-hours spikes |
| **Data Access Patterns** | Table and column access frequency analysis |
| **User Activity** | Per-user query volume and type breakdown |

**Persistent Filter workflow:**
1. Right-click any data point in one view (e.g., a suspicious IP address) → **Add To Filter**
2. Switch to a completely different view (e.g., Data Access Patterns)
3. The filter persists — you now see that specific IP's behaviour from a different analytical angle

#### Converting Audit Views to Reports
**Navigation:** `Main > Audit > DB Audit Data > Save as Report`

1. Customise any Data view: arrange columns, set filters, apply sorts
2. Click **Save as Report**
3. Provide a name
4. Imperva automatically maps the current layout into a Report definition
5. Navigate to `Main > Reports > Manage Reports` to attach a schedule and email distribution

#### Reviewing Audit Management Statistics
**Navigation:** `Main > Audit > Management Statistics`

| Report Tab | Content |
|---|---|
| **Summary Report** | Global totals on audited events; top 5 Gateways/Policies by disk utilisation |
| **Gateways Report** | Per-Gateway statistics: event volume, disk usage, connection counts |
| **Policies Report** | Per-policy disk footprint and event collection rate |
| **Management Server Report** | MX-level indexing status and archive job status |

---

### 7.17 Security Policies & Blocking

#### Reviewing Default Applied Policies
**Navigation:** `Main > Setup > Sites > [DB Service or Application] > Applied Policies tab`

Every new database object automatically receives three default policies:

| Policy | Purpose |
|---|---|
| **SQL Profile Policy** | Whitelist-based behavioural profiling and enforcement |
| **SQL Protocol Policy** | Detects protocol-level violations (malformed packets, excessively long queries) |
| **SQL Correlation Policy** | Detects patterns across multiple events (e.g., repeated failed logins followed by a successful login) |

#### Tuning and Customising Policy Rules
**Navigation:** `Main > Policies > Security > [Policy] > Policy Rules tab`

For each rule within a policy:

| Control | Options |
|---|---|
| **Enabled** | Toggle the rule on or off |
| **Severity** | Informative / Low / Medium / High |
| **Action** | None (alert only) / Block (drop traffic) |
| **Followed Action** | Select an Action Set for extended response |

Click the **+ expand icon** next to a rule to access advanced rule-specific parameters (e.g., threshold values, whitelist exceptions).

#### Creating Custom Security Policies
**Navigation:** `Main > Policies > Security > Create New`

1. Select the policy level: **DB Service** or **DB Application**
2. Choose: **From Scratch** or **Use Existing** (clone an existing policy)
3. If from scratch, select the **Policy Type**:
   - Custom
   - Protocol Validation
   - Signatures (known attack signatures)
4. Configure **Match Criteria** to define trigger conditions:
   - Specific Columns accessed (e.g., `ccnumber`, `nid`)
   - Event Type (SELECT, INSERT, DDL)
   - Database User (specific accounts)
   - Source IP or IP Group
5. Save

#### Setting a Custom Policy as the New Default
**Navigation:** `Main > Policies > Security > [Policy] > Apply To tab`

1. Check **Automatically apply this policy to new services**
2. Click **Save**

This policy becomes the automatic baseline for any future database services or applications added to the Sites Tree — replacing the system default.

#### Enabling Blocking — The Two-Lever Model

For traffic to actually be dropped, **both conditions must be true simultaneously:**

```
Condition 1: Server Group Operation Mode = Active
             (Main > Setup > Sites > [Server Group] > Definitions tab)
                        AND
Condition 2: Policy Rule Action = Block
             (Main > Policies > Security > [Policy] > Policy Rules tab)
```

Neither condition alone is sufficient.

#### Extended Blocking via Followed Actions
**Navigation:** `Main > Policies > Security > [Policy] > Policy Rules tab > Followed Action column`

| Extended Action | Effect |
|---|---|
| **Terminate DB Session** | Severs the entire database session — not just the single query |
| **User Block** | Locks the database user account — all further connections refused |
| **IP Block** | Drops all network traffic from the attacker's source IP for a configurable duration |

#### Applying Followed Actions to Rules
**Navigation:** `Main > Policies > Security > [Policy] > Policy Rules tab`

1. Select the specific rule
2. In the **Followed Action** column, click the drop-down
3. Select the desired Action Set
4. Click **Save**

> **Prerequisite:** The Action Set must have been created with Type = `Security Violations`.

#### The Simulation-First Safe Deployment Workflow

```
Step 1: Keep Server Group in Simulation mode
          ↓
Step 2: Configure all desired Blocking Actions on policies
          ↓
Step 3: Run for sufficient time (include nightly jobs, weekly tasks)
          ↓
Step 4: Run Alert Report filtered for your new blocking policies
          (Main > Reports > Manage Reports)
          ↓
Step 5: Review report with Data Owners and DBAs
        → Identify any legitimate traffic being flagged (false positives)
          ↓
Step 6: Tune policies to eliminate false positives
          ↓
Step 7: Only when report is clean of legitimate traffic:
        Change Server Group Operation Mode to Active
```

---

### 7.18 Monitoring & Incident Response

#### Global Alert Dashboard
**Navigation:** `Main > Monitor > Dashboard`

Real-time single-pane view of the entire environment:
- **Top panels:** Gateway CPU load, ThreatRadar status, Connections/sec, Throughput graphs
- **Alerts per Severity graph:** Current threat level distribution
- **Latest Alerts list:** Most recent critical security events — click any to open full details
- **Latest System Events list:** Recent system health events

#### Understanding Violations vs. Alerts

| Item | Description |
|---|---|
| **Violation** | A single individual security event — contains: violated policy name, severity, source IP, DB username, exact SQL statement that triggered the rule |
| **Alert** | A smart container that groups similar, repetitive violations together — aggregates up to 30 full violation details, tracks total hit count for up to 12 hours. Reduces alert fatigue. |

#### Investigating Security Violations
**Navigation:** `Main > Monitor > Violations`

1. Use the Filter pane (left) to narrow by: Severity, Event Type, Server Groups
2. Select a Violation to open the **Details Pane** (right)
3. Review **Key-Value pairs** at the top for quick policy summary
4. Scroll to **Event Details** for:
   - Granular connection information
   - Matched policy patterns
   - The **literal SQL statement** that caused the violation
   - Database error messages (if any)

#### Analysing Aggregated Alerts
**Navigation:** `Main > Monitor > Alerts`

1. Select an Alert
2. Review **Actions and Policy** sections — what automated responses fired and which policy triggered
3. Check **"Alert aggregated by:"** table — understand the grouping criteria (e.g., Source IP + Rule + DB User)
4. Expand **Violations section** — read deep-dive details for the first 30 individual violations

#### Alert Triage Workflow (Status Flags)
**Navigation:** `Main > Monitor > Alerts > [Right-click on alert]`

| Flag | Icon | Meaning |
|---|---|---|
| **Mark as Important** | `!` | High-priority — requires immediate investigation |
| **Mark as Acknowledged** | `✓` (green) | Currently being investigated by a team member |
| **Mark as Dismissed** | `×` | Confirmed false positive or resolved — hidden from active view |
| **Mark as Unread** | **Bold** text | Reverts to unreviewed state |

To clear a flag: right-click the alert → **Clear Mark**

---

### 7.19 Risk Management & Vulnerability Assessment

#### Reviewing DB Assessment Policies
**Navigation:** `Main > Risk Management > DB Assessment Policies`

Built-in benchmark policies containing standardised tests:

| Policy Type | Standards Covered |
|---|---|
| **CIS Benchmarks** | Center for Internet Security hardening guidelines |
| **DISA-STIG** | Defense Information Systems Agency security technical implementation guides |
| **PCI-DSS** | Payment Card Industry Data Security Standard |
| **Known Vulnerabilities** | CVE database checks for specific DB version vulnerabilities |
| **User Rights Review** | Permission and privilege audit tests |

**To filter by database type:**
1. Open the Filter hanging tab (left side)
2. Check your DB Type (e.g., `MsSql`, `Oracle`)
3. Click **Apply**
4. Select any individual test to read its **Overview**, **Recommended Fix**, and **Technical Details**

#### Tagging Assessment Policies
**Navigation:** `Main > Risk Management > DB Assessment Policies > [Policy] > Details tab`

1. Locate the **Policy Tags** text box
2. Type a custom tag string and press Enter (e.g., `DPE-DB`, `PCI-Scope`)
3. Click **Save**

Tags allow you to run multiple policies simultaneously via Tag-Based Scans. Remove tags by clicking the `×` next to them.

#### Configuring Policy-Based Assessment Scans
**Navigation:** `Main > Risk Management > Assessment Scans > Create New > Policy Based Scan`

1. Select target DB Type
2. Select the specific DB Assessment Policy to execute
3. **Apply To tab:** Select the Direct Access Connection credentials
4. Save and Run

#### Configuring Tag-Based Assessment Scans
**Navigation:** `Main > Risk Management > Assessment Scans > Create New > Tag Based Scan`

1. Provide a descriptive name
2. **Settings tab:** Select the combination of Policy Tags and ADC Tags to target
3. **Apply To tab:** Select Direct Access Connection credentials
4. Save, then schedule or run

#### Manually Updating the Risk Score
**Navigation:** `Admin > Jobs Status`

The Risk Calculator runs nightly by default at 05:00. To force immediate recalculation after an ad-hoc scan:

1. Open the **Filter pane** → select the `Risk Update` filter
2. Locate the **Risk Calculator** job
3. Click **Action > Run Now**
4. Wait for progress bar to reach 100% → "Risk was calculated successfully"

Risk scores use the **CVSS (Common Vulnerability Scoring System)** standard.

#### Analysing Risk via the Risk Console
**Navigation:** `Main > Risk Management > Risk Console`

| Scope / View | Content |
|---|---|
| **Risk Management Scope → Network View** | Which Server Groups carry the highest risk scores |
| **Risk Management Scope → Data View** | Which sensitive data types are associated with highest-risk servers |
| **Assessment Results Scope** | Pass/fail rates per server and per individual test |
| **Comparison View** | Side-by-side of recent scan vs. baseline scan — proves remediation progress |

For the most granular itemised list of failed tests:  
`Main > Reports > Manage Reports > Create > Assessment Report`

---

### 7.20 User Rights Management (URM)

#### Configuring Database OS for URM Scans
**Navigation:** `Main > Setup > Sites > [Server Group] > Servers tab`

Before running URM scans, the host OS must be defined:

1. In the **OS column** for your target database server, use the drop-down
2. Select the correct OS: `Windows` or `Linux`
3. Click **Save**

This provides the OS-level context required to map Active Directory groups and OS-level permissions accurately.

#### Creating and Running DB User Rights Scans
**Navigation:** `Main > Discovery & Classification > Scans Management > Create New > DB User Rights Scan`

1. **Settings tab:**
   - Define **Source of User Organisational Information** (e.g., Windows Domain configured on the Server Group)
   - Expand **Advanced Configuration**:
     - **Ignore DBA Users:** Uncheck this to see if application service accounts have improper DBA-level access — a common security misconfiguration
2. **Apply To tab:** Select Direct Access Connection credentials
3. Save → Run

#### Analysing Effective Permissions & Access Paths
**Navigation:** `Main > Discovery & Classification > DB User Rights`

1. In the Scope pane, select your URM Scan and DB Connection
2. Use analytical views:
   - **User Breakdown** — per-user privilege summary
   - **Effective Permissions (Tabular)** — complete permission matrix for all users

**Tracing privilege lineage:**
1. Select any specific row in the Raw Data table
2. Click **Show Paths**
3. A visual map opens showing the complete chain: individual account → nested AD group → inherited role → database object

This answers: *How exactly did this user get this permission?*

#### Managing Tablespace Usage
**Navigation:** `Main > Discovery & Classification > DB User Rights > Tablespace Usage`

URM scans can consume significant storage in the MX internal database.

1. Review **tablespace graphs** showing total MB usage across all historical scans
2. To purge: click the specific Scan Name in the table → click **Purge**
3. Alternatively: click **Show DB Connections** → select specific credentials → **Purge**

#### Approving or Rejecting Role and Permission Grants
**Navigation:** `Discovery & Classification > DB User Rights > Scope: Manage Role Grants (or Manage Permission Grants)`

1. Change Scope drop-down to `Manage Role Grants` or `Manage Permission Grants`
2. Select rows with **Unmanaged** status
3. Right-click → **Manage Status** → choose:

| Status | Meaning | Action Required |
|---|---|---|
| **Approve** | Access is legitimate and expected | No further action |
| **Reject** | Access is unauthorised — should be removed | DBA must revoke in the database |
| **Review** | Needs further business owner confirmation | Assign to a Data Owner |

4. Optionally: click **Derived Rights** to see every underlying sub-privilege that a role provides

---

### 7.21 Reporting Engine

#### Global Report Settings
**Navigation:** `Admin > System Definitions > Report Settings / Keyword Settings`

- Configure default output formats: PDF, HTML, CSV
- Create **Report Keywords** — dynamic text replacements that swap system terminology for business-friendly terms in generated reports

#### Creating Custom Reports
**Navigation:** `Main > Reports > Manage Reports > Create`

Select the data category:

| Category | Report Type |
|---|---|
| **Alerts** | Top 10 attacks, violation trends, attack source analysis |
| **Audit** | DB Audit Data reports, query frequency, user activity |
| **Configuration** | Security policy documentation, applied policy audit |
| **DB Profile** | User and Group profile detailed reports for DBA review |
| **Assessment** | Vulnerability scan results, failed test itemisation |
| **DB User Rights** | Permission grants, role assignments, entitlement reports |

**Data tab:** Set time frame (Last 7 Days, Last 30 Days, custom range) and apply filters.

#### Customising Report Views
**Navigation:** `Manage Reports > [Report] > View tab`

- **Charts:** Define X and Y axes — e.g., Alert Name (X) vs Number of Events (Y) for a Top 10 attack pie chart
- **Tables:** Select exact columns to include in tabular output

#### Scheduling & Distributing Reports
**Navigation:** `Manage Reports > [Report] > Scheduling tab`

1. Enable scheduling and set frequency: Daily / Weekly / Monthly
2. Set output format: PDF / HTML / CSV
3. Under **Distribution**, assign an Email Action Interface to auto-send to stakeholders

#### Viewing Historical Report Results
**Navigation:** `Main > Reports > View Results`

- Filter by report name or date range
- Click any report instance to download or view
- **Shortcut:** Select any report in Manage Reports → click its individual **Results tab**

#### Compliance-Specific Report Types

| Report | Compliance Use |
|---|---|
| **Security Policy Configuration** | Documents all active policies, rules, severity levels, and protected assets — for quarterly security audits |
| **DB Profile User/Group Detailed** | Documents what the ML engine has learned — for DBA hygiene reviews |
| **Top 10 DB Violations** | Executive-level daily/weekly attack summary |
| **DB User Rights** | Entitlement review showing who has access to what — for access recertification |
| **Assessment Reports** | Granular vulnerability findings — for remediation tracking |

---

## 8. Risk Elimination Summary

| Risk | How Imperva DAM Eliminates It |
|---|---|
| **SQL Injection** | SQL Protocol & Profile policies detect malformed queries and excessively long SQL; Gateway issues TCP reset before the DB executes the malicious statement |
| **Privilege Abuse / Insider Threat** | Behavioural profiling flags any query or table access outside the user's learned whitelist; tamper-proof logs on MX — DBAs cannot delete their own audit trail |
| **Encrypted Credentials (Hashed User)** | SSL certificate upload and Kerberos key configuration reveals true usernames in every audit record — zero blind spots in the identity trail |
| **Bulk Data Exfiltration** | Extended DB Response auditing, anomalous row-count SIEM alerts, and real-time blocking stop data theft before it leaves the network perimeter |
| **Compliance & Audit Gaps** | CIS / DISA-STIG / PCI-DSS built-in assessments with CVSS scoring; automated quarterly compliance reports distributed to DPE auditors |
| **Rogue / Shadow Databases** | Service Discovery Scans actively probe all subnets; any unprotected database spun up without IT knowledge is detected and added to the protection scope |
| **No Segregation of Duties** | Audit logs stored on separate MX server with no DBA access — absolute separation between those being audited and the audit system itself |
| **Local DBA Terminal Sessions** | IPC/BEQ interface binding ensures terminal sessions on the physical server are as visible as any remote network connection |

---

## 9. Conclusion & Production Readiness

### What Was Accomplished

The deployment on the DPE test database environment successfully validated all operational components of Imperva SecureSphere DAM:

- ✅ All 5 installation phases completed: Agent → Gateway → MX Logical Hierarchy → Interface Binding → Live Verification
- ✅ Both remote application traffic AND local DBA terminal sessions captured and logged
- ✅ Tamper-proof audit trail established — completely independent of database DBA access
- ✅ SQL Profile Policy learning engine active — building behavioural baselines
- ✅ Data Classification scan capability confirmed with Direct Access credentials
- ✅ Security Policy baseline (SQL Profile, Protocol, Correlation) applied and operational
- ✅ SIEM forwarding path configured and validated

### What Native Security Was Replaced With

| Before (Native Only) | After (Imperva DAM) |
|---|---|
| Siloed per-vendor audit logs | Unified single-pane audit across all DB types |
| DBAs can delete their own logs | Tamper-proof logs on separate hardened MX server |
| Reactive — detect after the fact | Real-time blocking before the DB executes |
| Performance-heavy native auditing | Zero-impact OS kernel agent |
| No visibility into local sessions | IPC/BEQ capture covers all connection types |
| No sensitive data location map | Classification scans map all PII/PCI columns |

### Production Deployment Roadmap

| Phase | Activity |
|---|---|
| **Phase A — Baseline Period** | Run in Simulation mode for minimum 2 weeks; allow all scheduled jobs (nightly backups, weekly reports) to execute and be profiled |
| **Phase B — Policy Review** | Run Alert Reports filtered for blocking policy violations; review with DBA team; eliminate false positives |
| **Phase C — Classification** | Run DB Data Classification scans; accept results; generate Table Groups; apply to masking and policies |
| **Phase D — Profile Maturity** | Review profile completeness for all DB users; lock critical attributes; freeze learning engine for stable accounts |
| **Phase E — Go Active** | Change production Server Groups from Simulation to Active mode; enable blocking rules; activate SIEM forwarding |
| **Phase F — Ongoing** | Quarterly compliance report distribution; periodic URM scans; annual assessment scan review; profile hygiene reviews with DBA team |

---

## References

- Imperva SecureSphere DAM Official Documentation
- CIS Benchmark Guides for Oracle, MS SQL Server, MySQL, PostgreSQL
- DISA STIG Database Security Technical Implementation Guides
- PCI-DSS v4.0 Requirements 10 (Audit Logs) and Requirement 8 (User Access)
- OWASP SQL Injection Prevention Cheat Sheet
- NIST SP 800-86 Guide to Integrating Forensic Techniques into Incident Response

---

*Documentation prepared by Infosonik Systems Limited — Professional Services Division*  
*Imperva SecureSphere DAM | DPE Bangladesh | 2025*
