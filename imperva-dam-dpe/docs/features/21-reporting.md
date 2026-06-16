# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
