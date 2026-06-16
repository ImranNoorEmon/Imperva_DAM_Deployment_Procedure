# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
