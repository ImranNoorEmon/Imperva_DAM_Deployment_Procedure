# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.8 Deep Database Interactivity

#### Configuring Direct Access Credentials
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Direct Access Information`

Imperva requires dedicated database credentials to log directly into the database for internal operations:
- Reading stored procedure definitions
- Running Data Classification scans
- Running Vulnerability Assessment scans
- Running User Rights Management scans

**Setup:**
1. Enter a dedicated **User Name** and **Password**
2. Best practice: use a **dedicated privileged account created specifically for Imperva** — never a shared DBA account
3. Click **Test Connection** to validate before saving

#### Applying Stored Procedure Groups
**Navigation:** `Main > Setup > Global Objects > Scope: Stored Procedure Groups`

Stored procedures are precompiled — when called, the DB engine executes internal SQL that Imperva cannot natively intercept. Loading Stored Procedure Groups allows Imperva to map those internal SQL statements and audit them.

1. Create or review Stored Procedure Groups in Global Objects
2. Navigate to `Main > Setup > Sites > [DB Application]`
3. Apply the relevant Stored Procedure Group to the application

#### Manually Configuring Kerberos Keys
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Encryption Support > Manually Configured Kerberos Keys`

MS SQL Server in Active Directory environments uses Kerberos to encrypt domain user logins. Without the Kerberos key, Imperva cannot resolve domain usernames.

**Setup:**
1. Expand **Encryption Support** on the DB Service's Definitions tab
2. Click the **yellow pencil** next to Manually Configured Kerberos Keys
3. Click **Create New (+)**
4. Enter the domain and username: `DOMAIN\username`
5. Enter the exact password in both Password and Verify fields
6. Save → close the window
7. Select the newly created credentials from the drop-down in the Kerberos Keys table
8. Click **Save**


---

[← Back to README](../../README.md)
