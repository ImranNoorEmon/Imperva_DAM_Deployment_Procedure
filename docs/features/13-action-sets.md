# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.13 Action Interfaces & Action Sets

#### Configuring Action Interfaces
**Navigation:** `Main > Setup > Action Interfaces > Create`

Action Interfaces define the technical communication channels Imperva uses for notifications and archiving.

| Interface Type | Use Case | Key Parameters |
|---|---|---|
| **Email (SMTP)** | Alert notifications to DBA/Security team | SMTP server, port, sender, auth |
| **Syslog** | Forward logs to SIEM (Splunk, QRadar, etc.) | Syslog server IP, port, CEF format |
| **FTP Archive** | Transfer audit archives to FTP server | Server IP, credentials, path |
| **SCP Archive** | Secure file transfer for audit archives | Server IP, SSH key/credentials, path |
| **SNMP Trap** | Network management system alerts | SNMP community string, OID |
| **SMS** | Critical alert notifications | SMS gateway API details |

#### Building Action Sets
**Navigation:** `Main > Policies > Action Sets > Create`

Action Sets group one or more Action Interfaces and define when they fire.

1. Provide a descriptive name
2. Define the **Event Scope** (what triggers this Action Set):
   - Security Violations
   - System Events
   - Audit events
   - Archiving completion
   - Assessment completion
3. Define the **Application Level** at which it applies:
   - Service Level
   - Application Level
   - DB Application Level
4. Add the Action Interface(s) to the set
5. Save

> **Note:** If an Action Set does not appear in a policy rule's Followed Action drop-down, check that its **Type** was set to `Security Violations` during creation.

#### Attaching Followed Actions to Security Rules
**Navigation:** `Main > Policies > Security > [Policy] > Policy Rules tab`

1. Select the specific rule
2. Locate the **Followed Action** column for that rule
3. Select a pre-configured Action Set from the drop-down
4. Click **Save**


---

[← Back to README](../../README.md)
