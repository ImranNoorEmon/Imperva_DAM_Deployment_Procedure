## 3. Risks & Vulnerabilities

### Active Threats Against Databases

#### SQL Injection (SQLi)
Attackers insert malicious SQL code into web forms or API parameters. The application passes this unsanitised input to the database, which executes it blindly. This can result in full data extraction, authentication bypass, data deletion, or OS command execution. Native databases generally cannot distinguish a legitimate application query from a SQLi attack — they execute whatever SQL they receive.

#### Privilege Abuse
DBAs require high-level access to maintain systems. If a DBA's credentials are stolen via phishing, or if a DBA themselves goes rogue, native security cannot stop them — they *are* the gatekeeper. They have access to both the data and the audit logs that would record their theft.

#### Unpatched Software Vulnerabilities
Every DBMS version contains CVEs (Common Vulnerabilities and Exposures). A Zero-Day vulnerability in Oracle or SQL Server can be exploited by attackers before the vendor even releases a fix. Organisations running outdated DB versions are permanently exposed.

#### Misconfigurations
Databases are complex. Common misconfigurations include:
- Default vendor passwords left active
- `PUBLIC` access granted to sensitive tables
- Unnecessary network ports left open
- Over-privileged application service accounts
- DEBUG or diagnostic features left enabled in production

### Limitations of Native Database Security

| Limitation | Explanation |
|---|---|
| **Performance Penalty** | Granular native auditing (logging every single SELECT query) consumes massive CPU and disk I/O. In production environments, this can degrade application performance to unacceptable levels, so organisations either disable it or use it minimally |
| **No Segregation of Duties** | DBAs manage the database AND the audit logs. A malicious DBA can steal data and then delete the audit log that recorded the theft. This is a critical compliance failure — there is no independent, tamper-proof witness |
| **Reactive, Not Proactive** | Native logs tell you what happened *after the fact*. They do not block a user who is extracting 50,000 student records right now. Detection comes hours, days, or never |
| **Siloed Management** | Oracle, SQL Server, and Postgres each use completely different audit systems, log formats, and management tools. There is no single unified security view across a mixed database environment |
| **Encrypted Traffic Blind Spot** | When database logins use SSL/Kerberos encryption, native audit logs cannot resolve the actual username — it appears as unknown or hashed |
| **Local Session Blind Spot** | Network firewalls only see remote connections. A DBA logging directly into the physical server via a terminal is invisible to any network-level monitoring |


---

[← Back to README](../README.md)
