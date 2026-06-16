# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.5 Infrastructure Setup — Global Objects & Sites Tree

#### Managing Global Objects
**Navigation:** `Main > Setup > Global Objects`

Global Objects are centrally managed containers that can be referenced across multiple server groups and policies — set them once, use them everywhere.

| Object Type | Purpose |
|---|---|
| **IP Groups** | Named collections of IP addresses (e.g., "DBA Workstations", "Application Servers") |
| **Windows Domains** | Active Directory domain definitions for user resolution |
| **Stored Procedure Groups** | Collections of stored procedures for audit mapping |
| **Agent Monitoring Rules** | Rules for excluding trusted connections at the agent level |

**Shortcut:** When working inside any configuration menu (e.g., a Server Group), click the **yellow pencil (Edit Object)** button next to a drop-down field to jump directly to the relevant Global Object configuration.

#### Building the Sites Tree Hierarchy
**Navigation:** `Main > Setup > Sites`

The Sites Tree is the organisational backbone of the entire deployment:

```
Site (e.g., DPE_Datacenter)
└── Server Group (e.g., windows_server_db)
    ├── Server (IP: 192.168.1.50, Hostname: SQLSRV01)
    └── Database Service (Type: MsSql, Port: 1433)
        └── Application (Default MsSql Application)
```

**Step-by-step:**
1. Create a **Site** — organise by datacenter, environment (prod/test), or business unit
2. Create a **Server Group** — represents physical or clustered servers
3. Add a **Service** — defines the monitored protocol and ports
   - Load balancer configuration: set **X-Forwarded-For** or **True-Client-IP** in the Forwarded Connections section under the Service's Operation tab
4. The system auto-generates a default **Application** — rename it or create additional applications for granular policy control

---

### 7.6 Asset Discovery & Operation Modes

#### Running Service Discovery Scans
**Navigation:** `Main > Discovery & Classification > Scans Management`

Actively probes network subnets to detect unknown, unauthorised, or shadow databases that are currently unprotected (e.g., a test database spun up by a developer without IT knowledge).

**Configuration:**
1. Define the scan scope: specific IPs, subnets, IP ranges
2. Include checks for **non-standard ports** (databases not on their default ports)
3. Configure **New Entities Configuration** naming templates using placeholders:
   - `$SITE` — the site name
   - `$IP` — the discovered server IP
   - `$SERVICE_TYPE` — the detected database type
4. Choose: auto-add discovered servers to Sites Tree, or leave for manual review

#### Reviewing and Protecting Discovered Servers
**Navigation:** `Main > Discovery & Classification > Discovered Servers`

1. Review results in **Summary View** or **Pending Servers** view
2. **Accept** valid servers → creates the Server Group/Service and adds the server to the Servers tab
3. **Reject** servers → hides them from future scan results (confirmed non-targets)
4. Navigate to `Main > Setup > Sites > [Server Group] > Servers tab`
5. Click the discovered server row → click **Add to Protected IPs**
6. Verify: a **blue checkmark** appears in the Protected column

#### Managing Server Group Operation Modes
**Navigation:** `Main > Setup > Sites > [Server Group] > Definitions tab > Operation`

| Mode | Behaviour |
|---|---|
| **Simulation** | Inspect all traffic and log violations — but never drop any packets. **Default for all new Server Groups.** Use this during policy tuning. |
| **Active** | Actively drop traffic that violates blocking rules. Switch to this only after simulation review. |
| **Disabled** | Completely bypass all inspection and logging for IPs in this group. Use only for maintenance windows. |

> **Best Practice:** Always start in Simulation. Run for a sufficient period (including nightly batch jobs and scheduled tasks), review Alert Reports with DBAs to eliminate false positives, then switch to Active.


---

[← Back to README](../../README.md)
