# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.18 Monitoring & Incident Response

#### Global Alert Dashboard
**Navigation:** `Main > Monitor > Dashboard`

Real-time single-pane view of the entire environment:
- **Top panels:** Gateway CPU load, ThreatRadar status, Connections/sec, Throughput graphs
- **Alerts per Severity graph:** Current threat level distribution
- **Latest Alerts list:** Most recent critical security events — click any to open full details
- **Latest System Events list:** Recent system health events

#### Understanding Violations vs. Alerts

| Item | Description |
|---|---|
| **Violation** | A single individual security event — contains: violated policy name, severity, source IP, DB username, exact SQL statement that triggered the rule |
| **Alert** | A smart container that groups similar, repetitive violations together — aggregates up to 30 full violation details, tracks total hit count for up to 12 hours. Reduces alert fatigue. |

#### Investigating Security Violations
**Navigation:** `Main > Monitor > Violations`

1. Use the Filter pane (left) to narrow by: Severity, Event Type, Server Groups
2. Select a Violation to open the **Details Pane** (right)
3. Review **Key-Value pairs** at the top for quick policy summary
4. Scroll to **Event Details** for:
   - Granular connection information
   - Matched policy patterns
   - The **literal SQL statement** that caused the violation
   - Database error messages (if any)

#### Analysing Aggregated Alerts
**Navigation:** `Main > Monitor > Alerts`

1. Select an Alert
2. Review **Actions and Policy** sections — what automated responses fired and which policy triggered
3. Check **"Alert aggregated by:"** table — understand the grouping criteria (e.g., Source IP + Rule + DB User)
4. Expand **Violations section** — read deep-dive details for the first 30 individual violations

#### Alert Triage Workflow (Status Flags)
**Navigation:** `Main > Monitor > Alerts > [Right-click on alert]`

| Flag | Icon | Meaning |
|---|---|---|
| **Mark as Important** | `!` | High-priority — requires immediate investigation |
| **Mark as Acknowledged** | `✓` (green) | Currently being investigated by a team member |
| **Mark as Dismissed** | `×` | Confirmed false positive or resolved — hidden from active view |
| **Mark as Unread** | **Bold** text | Reverts to unreviewed state |

To clear a flag: right-click the alert → **Clear Mark**


---

[← Back to README](../../README.md)
