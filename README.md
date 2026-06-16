<div align="center">

# Imperva SecureSphere DAM
### Database Activity Monitoring — Deployment & Operations Reference

**Deployed at:** Directorate of Primary Education (DPE), Bangladesh  
**Deployed by:** Infosonik Systems Limited — Professional Services Division  
**Environment:** Test Database (Pre-Production Validation)  
**Year:** 2026

---

[![License: MIT](https://img.shields.io/badge/License-MIT-teal.svg)](./LICENSE)
[![Vendor: Imperva](https://img.shields.io/badge/Vendor-Imperva-blue.svg)](https://www.imperva.com)
[![Status: Test Deployment](https://img.shields.io/badge/Status-Test%20Deployment-orange.svg)]()
[![Docs](https://img.shields.io/badge/Docs-Complete-brightgreen.svg)]()

</div>

---

## What Is This Repository?

This repository is the **complete technical reference** for the Imperva SecureSphere Database Activity Monitoring (DAM) solution deployed for the Directorate of Primary Education (DPE) of Bangladesh by Infosonik Systems Limited.

It covers everything from first principles (what a database is, why it needs external monitoring) through the full 5-phase production-grade installation procedure, and documents all 95 operational features of the Imperva SecureSphere platform in structured, navigable detail.

This documentation is intended to serve as:

- A **knowledge base** for the Infosonik engineering and professional services team
- A **handover reference** for DPE IT administrators and DBAs
- A **study guide** for team members pursuing database security expertise
- A **compliance record** showing what was deployed, how, and why

---

## Project Context

The **Directorate of Primary Education (DPE)** is a government agency under the Ministry of Primary and Mass Education, Bangladesh. DPE manages the primary education infrastructure nationwide, including the **IPEMIS** (Integrated Primary Education Management Information System) platform, which stores the Personal Identifiable Information (PII) of millions of students, teachers, and administrative staff.

Securing this data is not a technical preference — it is a **national necessity**.

Imperva SecureSphere DAM was selected to:

- Provide **real-time, independent monitoring** of all database activity
- Eliminate the **insider threat and DBA blind spot** that native database controls cannot address
- Create a **tamper-proof audit trail** that satisfies compliance requirements
- Enable **active blocking** of malicious queries before data leaves the network

---

## Repository Structure

```
imperva-dam-dpe/
│
├── README.md                          ← You are here — project overview & navigation
├── LICENSE                            ← MIT License
│
├── docs/
│   ├── 01-database-fundamentals.md    ← What is a database, types, security pillars
│   ├── 02-risks-and-vulnerabilities.md← Threats, attack surfaces, native security gaps
│   ├── 03-how-dam-works.md            ← DAM concept, architecture, three-tier model
│   ├── 04-installation.md             
│   │
│   └── features/
│       ├── 05-ui-and-administration.md        ← UI, user management, RBAC, LDAP, PIM
│       ├── 06-infrastructure-and-discovery.md ← Sites Tree, Global Objects, Discovery
│       ├── 07-agent-configuration.md          ← Agent tuning, interfaces, exclusions
│       ├── 08-db-interactivity.md             ← Direct Access, Stored Procs, Kerberos
│       ├── 09-traffic-decryption.md           ← Hashed User, Connected User, Normalisation
│       ├── 10-behavioural-profiling.md        ← SQL Profile engine, learning, blocking
│       ├── 11-data-masking.md                 ← Data Masking, PIM
│       ├── 12-data-classification.md          ← Classification scans, Data Types, Table Groups
│       ├── 13-action-sets.md                  ← Action Interfaces, Action Sets, Notifications
│       ├── 14-archiving-and-retention.md      ← Audit archiving, report retention
│       ├── 15-audit-framework.md              ← Audit policies, aggregation, extended capture
│       ├── 16-siem-integration.md             ← SIEM, Syslog CEF, Analytics Views
│       ├── 17-security-policies.md            ← Policies, blocking, simulation workflow
│       ├── 18-monitoring-and-alerting.md      ← Dashboard, violations, alerts, triage
│       ├── 19-risk-and-assessment.md          ← CIS, DISA-STIG, PCI-DSS, Risk Console
│       ├── 20-user-rights-management.md       ← URM scans, access paths, entitlement governance
│       └── 21-reporting.md                    ← Report engine, scheduling, compliance reports
│
└── presentations/
    └── Imperva_DAM_DPE_30Slides.pptx  ← Executive/client-facing presentation deck
```

---

## Quick Navigation

| I want to… | Go to |
|---|---|
| Understand what a database is and why it needs DAM | [Database Fundamentals](./docs/01-database-fundamentals.md) |
| Understand the risks native DB security cannot address | [Risks & Vulnerabilities](./docs/02-risks-and-vulnerabilities.md) |
| Understand how Imperva DAM works architecturally | [How DAM Works](./docs/03-how-dam-works.md) |
| Install the agent and configure the MX console | [Installation Guide](./docs/04-installation.md) |
| Configure user accounts, roles, LDAP, PIM | [UI & Administration](./docs/features/05-ui-and-administration.md) |
| Set up the Sites Tree and run Discovery Scans | [Infrastructure & Discovery](./docs/features/06-infrastructure-and-discovery.md) |
| Tune agent CPU, bind IPC/BEQ interfaces | [Agent Configuration](./docs/features/07-agent-configuration.md) |
| Set up Direct Access credentials and Kerberos | [DB Interactivity](./docs/features/08-db-interactivity.md) |
| Fix "Hashed User" or "Connected User" in audit logs | [Traffic Decryption](./docs/features/09-traffic-decryption.md) |
| Understand and manage behavioural profiling | [Behavioural Profiling](./docs/features/10-behavioural-profiling.md) |
| Mask PII in audit logs | [Data Masking](./docs/features/11-data-masking.md) |
| Find where sensitive data lives in the database | [Data Classification](./docs/features/12-data-classification.md) |
| Configure email/Syslog/SNMP notifications | [Action Sets](./docs/features/13-action-sets.md) |
| Set up long-term audit archiving | [Archiving & Retention](./docs/features/14-archiving-and-retention.md) |
| Configure audit policies and SIEM forwarding | [Audit Framework](./docs/features/15-audit-framework.md) + [SIEM Integration](./docs/features/16-siem-integration.md) |
| Create policies, enable blocking, simulate safely | [Security Policies](./docs/features/17-security-policies.md) |
| Investigate violations and triage alerts | [Monitoring & Alerting](./docs/features/18-monitoring-and-alerting.md) |
| Run CIS/DISA-STIG/PCI-DSS vulnerability scans | [Risk & Assessment](./docs/features/19-risk-and-assessment.md) |
| Map who has access to what in the database | [User Rights Management](./docs/features/20-user-rights-management.md) |
| Schedule and distribute compliance reports | [Reporting Engine](./docs/features/21-reporting.md) |

---

## Technology Stack

| Component | Details |
|---|---|
| **DAM Platform** | Imperva SecureSphere |
| **Management Server** | Imperva MX (Management eXtension) |
| **Gateway** | Imperva SecureSphere Gateway (physical or virtual appliance) |
| **Agent** | Imperva R-Agent (OS-level kernel agent) |
| **Protected Databases** | Oracle, Microsoft SQL Server, EDB PostgreSQL, MySQL |
| **Authentication Integration** | Active Directory / LDAP |
| **SIEM Integration** | Syslog CEF (compatible with Splunk, QRadar, ArcSight) |
| **Archive Transport** | FTP, SCP, NFS |
| **Assessment Standards** | CIS Benchmarks, DISA-STIG, PCI-DSS, CVE Database |

---

## Deployment Summary

| Item | Detail |
|---|---|
| **Deployment Type** | Test / Pre-Production |
| **Target Environment** | DPE Test Database Server |
| **Agent Communication Port** | 443 (SSL encrypted) |
| **Operation Mode at Deployment** | Simulation (no blocking — for tuning) |
| **Traffic Captured** | TCP (remote) + IPC/BEQ (local terminal sessions) |
| **Verification Method** | Live audit data visible in MX within Last Hour filter |
| **Next Stage** | Production go-live after policy simulation review |

---

## License

This documentation is released under the [MIT License](./LICENSE).  
The Imperva SecureSphere product is proprietary software owned by Imperva, Inc. This repository contains only operational documentation, procedures, and reference material — not any Imperva software or proprietary source code.

---

## Contributing

This is an internal professional services reference maintained by Infosonik Systems Limited. If you are a team member and identify errors, missing procedures, or updated navigation paths following a SecureSphere version update, please open a pull request with a clear description of what changed and why.

---

<div align="center">

*Infosonik Systems Limited — Professional Services Division*  
*Imperva SecureSphere DAM | 2026*

</div>
