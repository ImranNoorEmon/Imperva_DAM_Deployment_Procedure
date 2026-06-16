# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
