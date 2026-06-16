# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

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

[← Back to README](../../README.md)
