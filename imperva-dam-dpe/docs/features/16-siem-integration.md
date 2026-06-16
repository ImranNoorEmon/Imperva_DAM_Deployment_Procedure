# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.16 SIEM Integration & Analytics

#### SIEM Integration via External Logger
**Navigation:** `Main > Policies > Audit > [Policy] > External Logger tab`

1. Select the pre-configured **Audit Syslog Action** from the drop-down
2. **Default behaviour:** MX sends Syslog to the SIEM
3. For lower latency: check **Enable using gateway configuration if exists** — the Gateway sends directly to the SIEM (requires Gateway Group to have an External Logger IP/Port defined under `Setup > Gateways`)

**Supported format:** Syslog CEF (Common Event Format) — compatible with Splunk, IBM QRadar, ArcSight, and most enterprise SIEM platforms.

#### Using Audit Analytics Views
**Navigation:** `Main > Audit > DB Audit Data > Views section (bottom-left)`

Predefined analytical views for rapid investigation:

| View | What It Shows |
|---|---|
| **Source Analysis** | Traffic breakdown by source IP, application, OS hostname |
| **Privileged Operations** | DDL and administrative command activity |
| **Time Based Analysis** | Activity volume over time — detect unusual after-hours spikes |
| **Data Access Patterns** | Table and column access frequency analysis |
| **User Activity** | Per-user query volume and type breakdown |

**Persistent Filter workflow:**
1. Right-click any data point in one view (e.g., a suspicious IP address) → **Add To Filter**
2. Switch to a completely different view (e.g., Data Access Patterns)
3. The filter persists — you now see that specific IP's behaviour from a different analytical angle

#### Converting Audit Views to Reports
**Navigation:** `Main > Audit > DB Audit Data > Save as Report`

1. Customise any Data view: arrange columns, set filters, apply sorts
2. Click **Save as Report**
3. Provide a name
4. Imperva automatically maps the current layout into a Report definition
5. Navigate to `Main > Reports > Manage Reports` to attach a schedule and email distribution

#### Reviewing Audit Management Statistics
**Navigation:** `Main > Audit > Management Statistics`

| Report Tab | Content |
|---|---|
| **Summary Report** | Global totals on audited events; top 5 Gateways/Policies by disk utilisation |
| **Gateways Report** | Per-Gateway statistics: event volume, disk usage, connection counts |
| **Policies Report** | Per-policy disk footprint and event collection rate |
| **Management Server Report** | MX-level indexing status and archive job status |


---

[← Back to README](../../README.md)
