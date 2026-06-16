# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.14 Archiving & Retention

#### Applying Followed Actions to System Jobs
**Navigation:** System Administration / Archiving Settings

1. Navigate to the specific system job (MX Database Backup, Audit Archiving)
2. Locate the **Followed Action** section
3. Select an Action Set containing an FTP or SCP archive interface
4. Save

> **Troubleshooting:** If a file transfer fails (FTP server down), Imperva stores the file locally at:  
> `/var/tmp/failed_delivery/`  
> Retrieve manually when the external server is back online.

#### Report Retention (Reports Archive Job)
**Navigation:** System Administration / Archiving Jobs

1. Locate the **Reports Archive** job
2. Configure an Archiving Action Set (FTP/SCP)
3. Enable **Purge Job**
4. Set the **retention timeframe** — reports older than this threshold are purged from the MX after successful archiving

#### Configuring Audit Archiving Locations
**Navigation:** `Main > Policies > Audit > [Policy] > Archiving tab`

1. Assign an **Archiving Action Set** (FTP, SCP, or NFS mount)
2. Set the recurring **Schedule** (e.g., daily at 04:30)
3. Set the **Purge records threshold**

> **Important:** The global Audit Purge job will only delete records from the Gateway if they are both:
> - Older than the purge threshold, **AND**
> - Successfully transferred to the archive destination


---

[← Back to README](../../README.md)
