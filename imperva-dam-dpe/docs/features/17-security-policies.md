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

[← Back to README](../../README.md)
