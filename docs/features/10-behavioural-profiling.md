# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
