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
┌─────────────────────┐         ┌──────────────────────┐        ┌─────────────────────────┐
│   DATABASE SERVER   │         │      GATEWAY         │        │    MX MANAGEMENT SERVER │
│                     │         │                      │        │                         │
│  ┌───────────────┐  │  SSL    │  ┌─────────────────┐ │  Mgmt  │  ┌───────────────────┐  │
│  │  R-Agent      │ ─┼─────────┼─▶│  SQL Parser &   │ ┼───────┼─▶│  Web UI Console   │  │
│  │  (OS Kernel)  │  │ Port 443│  │  Policy Engine  │ │        │  │  Policy Store     │  │
│  └───────────────┘  │         │  └─────────────────┘ │        │  │  Audit Log Vault  │  │
│                     │         │                      │        │  │  Report Engine    │  │
│  Captures:          │         │  Performs:           │        │  └───────────────────┘  │
│  - TCP traffic      │         │  - Real-time parsing │        │                         │
│  - Local IPC        │         │  - Policy matching   │        │  DBAs have NO access    │
│  - Oracle BEQ       │         │  - Active blocking   │        │  to this server         │
│  - MSSQL IPC        │         │  - Alert generation  │        │                         │
└─────────────────────┘         └──────────────────────┘        └─────────────────────────┘
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

[← Back to README](../README.md)
