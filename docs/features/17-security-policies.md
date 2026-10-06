# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

### Appendix — Policy Engineering Best Practices

This appendix distills Imperva's official Data Security Policy Engineering best-practice guidance for turning a fresh service object into a tuned, use-case-driven policy set.

#### Required Information for Successful Policy Engineering

Before writing a single policy, gather:
- A classification of all Sensitive and Critical Data
- Identification and classification of Privileged Users
- Identification and classification of Application Users
- Classification of the network segment(s) used by Application Servers
- Classification of the network segment(s) used by Privileged Users / DBAs

#### Building the Whitelist Foundation (Lookup Data Sets)

Effective policies start from a **whitelist approach**. Under `Main > Setup > Global Objects`, build the following Lookup Data Sets so they can be referenced from Match Criteria across every policy, rather than hard-coding values into individual rules:

- **Privileged Users** — a named list/data set of DBA and admin accounts
- **Application Users** — a named list/data set of the service accounts applications use to connect
- **IP Groups** — at minimum, separate groups for **Application Servers** and **Middleware Servers**

#### End-to-End Deployment-to-Policy Workflow

1. Add the DB Service within the Sites Tree
2. Add the target DB Server's IP address to the Server Group's Protected IP Address List
3. Install and register an Agent to a Gateway (or Gateway Cluster)
4. From the Management Server's Agent Workbench, assign the Agent to its Server Group and Service, and confirm all required Data Interfaces are assigned (see [07 — Agent Configuration](07-agent-configuration.md))
5. Verify the Agent is collecting data using the Default Audit Policy
6. Once audit data is visible under the default policy, disable it
7. Apply the targeted use-case policies (below) to the new service object

#### Database Activity Monitoring — Use-Case Breakdown

The following 21 use cases are Imperva's standard starting catalogue for a DAM deployment, each mapped to the event it watches for, its scope of users, and whether it should feed an Audit Report, a Real-Time Security Alert, or both. (Reconstructed from a scanned reference table — treat exact thresholds as a starting point to tune for your environment.)

| # | Use Case | Triggering Event | Threshold | Scope of Users | Audit Report | Real-Time Alert |
|---|---|---|---|---|---|---|
| 1 | Logging of all connection activities | Login | N/A | All | ✓ | |
| 2 | Failed login monitoring | Failed Login | > 5 in 5 minutes | All | ✓ | ✓ |
| 3 | Login from an unauthorized zone/segment | Login outside approved IP Group | N/A | All | ✓ | ✓ |
| 4 | PCI Compliance — sensitive data access | `SELECT` on Credit Card / Customer Data | N/A | All | ✓ | ✓ |
| 5 | PCI Compliance — default account usage | Usage of default/built-in DB accounts | N/A | Privileged Accounts | ✓ | |
| 6 | Account lifecycle changes | Account creation, deletion, modification, disablement | N/A | Privileged Accounts | ✓ | |
| 7 | Schema/code changes (DDL) | `CREATE`, `DROP`, `ALTER` | N/A | Privileged Accounts | ✓ | |
| 8 | SOX Compliance — data changes (DML) | `INSERT`, `UPDATE`, `DELETE` | N/A | Application Users & Privileged Accounts | ✓ | |
| 9 | Monitor large response data | Data dump / bulk export detection | Response size > 10 (MB or rows, per policy) | All | ✓ | ✓ |
| 10 | Service account access monitoring | Service account connecting from an unauthorized zone | Outside approved segment | All | ✓ | ✓ |
| 11 | Privileged-operation monitoring on critical DBs | Create/modify an account to Privileged (DCL) | N/A | Management Account, Privileged Accounts | ✓ | ✓ |
| 12 | DCL operations by non-privileged users | Any non-Privileged-Account rights amendment (DCL) | N/A | Management Account, Privileged Accounts | ✓ | |
| 13 | Maintenance-operation monitoring on critical DBs | System configuration changes | Outside approved change window | Privileged Accounts | ✓ | ✓ |
| 14 | Database Agent availability monitoring | Agent Start/Stop | N/A | All Databases | Critical System Event | |
| 15 | Users and privileges modification | Usage of admin accounts | N/A | Restricted Accounts, All | | |
| 16 | Inactive-user reporting | Inactive user account audit (requires a URM license) | > 90 days | All | ✓ | |
| 17 | Unauthorized user-agent tracking | Track unauthorized client/user agents | N/A | All, Privileged Accounts | | ✓ |
| 18 | Restricted command usage | Execution of explicitly restricted commands | N/A | Restricted Commands scope | ✓ | |

> Use cases 4/5, 8/9, 11/12, and 13/14 above were split across a page break in the source material and have been merged here where the split was unambiguous — validate the exact thresholds and scopes against your current Imperva documentation before relying on them for a compliance attestation.

#### Audit and Security Policy Match Criteria

Both Audit Policies and Security Policies use the same underlying **Match Criteria** taxonomy to translate a use case into a concrete rule. The available fields are organized into six groups:

| Event Criteria | Destination | Users (WHO) | Source | Result | Defined |
|---|---|---|---|---|---|
| Operations | Table Groups | Application User | Proxy IP Addresses | Affected Rows | Signatures |
| Command Groups | Columns | Data Set: Attribute Lookup | Source Applications | Authentication Result | Violations |
| Event Type | Data Type | Database User Groups | Source IP Addresses | Cumulative Query Response Size | Custom Violations |
| Privileged Operation | Database and Schema | Database Usernames | Source URL | Generic Dictionary Search | |
| Time Of Day | Destination Tables | Lookup Data Set Search | Source of Activity | Number of Occurrences | |
| Sensitive Data Access | OS Host Names | | Web Source IP | Query Response Size | |
| | Stored Procedure | OS Usernames | Originated in Agent | Query Response Time | |
| | Unanalyzed Objects | Subject of Privileged Operation | Originated in Log Collectors | SQL Exception Strings | |
| | | | | SQL Exceptions | |
| | | | | Sensitive Dictionary Search | |
| | | | | Ticket Assigned | |
| | | | | Enrichment Data | |

Use this taxonomy as a checklist when a use case doesn't map cleanly to an existing rule: most gaps are solved by combining an **Event Criteria** field (what happened) with a **Users** field (who did it) and either a **Destination** or **Result** field (what was affected, or how big the blast radius was) — the **Defined** column (Signatures/Violations/Custom Violations) is reserved for referencing a pre-built detection object rather than raw match conditions.

#### A Note on Newer "Data Security Fabric" Terminology

Imperva's more recent product literature (2023+) rebrands the same architecture as the **Data Security Fabric**, replacing or supplementing some names used elsewhere in this repository:

| Data Security Fabric term | Classic term used elsewhere in this repo |
|---|---|
| Agentless Gateway(s) | Gateway performing network-based (SPAN/TAP or Bridge) monitoring, no agent involved |
| Agent Gateway Cluster(s) / Agent Gateway | The Gateway(s) that agent-based traffic (R-Agent) registers to |
| MX | MX Management Server (unchanged) |
| SOM / Agent Collection Configuration | Security Operations Manager — coordinates agent-facing configuration across a large-scale, multi-MX deployment |
| USC | Unified Security Console — the newer, consolidated management/reporting front end |
| Fabric Applications | Applications layered on top of the Data Security Fabric Hub (reporting, workflow, etc.) |
| SonarW | The Fabric Hub's own audit-data warehouse/analytics store |
| DRA Admin / DRA Analytics | Data Risk Analytics administration console and analytics engine |

The deployment documented in this repository uses the classic **MX + Gateway + Agent** model throughout — these newer names are provided here purely as a Rosetta Stone in case future Imperva documentation, support engagements, or licensing references the Data Security Fabric branding instead.

---

[← Back to README](../../README.md)
