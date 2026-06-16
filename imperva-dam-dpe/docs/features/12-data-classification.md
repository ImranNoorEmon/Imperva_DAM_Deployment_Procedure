# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
