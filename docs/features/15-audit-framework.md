# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
