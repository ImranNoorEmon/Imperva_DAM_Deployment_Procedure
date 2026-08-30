# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.7 Agent Configuration & Tuning

#### Tuning Agent Settings
**Navigation:** `Main > Setup > Agents > [Agent] > Settings tab`

- **Enable CPU Usage Restraining:** Check this and set a maximum CPU percentage limit — prevents the agent from overloading the database host server
- **Blocking:** Check this if you want the agent to directly block traffic based on policies (only effective when the Server Group is in Active mode)
- **Advanced Configuration:** Input specific tuning parameters as directed by Imperva Support (e.g., enabling DHE encrypted traffic decryption)

**Agent blocking modes** — once Blocking is enabled on an agent, it operates in one of two modes that mirror the Gateway's own Sniffing vs. Bridge (Inline) behaviour (see [06 — Gateway Deployment (Capture) Modes](06-infrastructure-and-discovery.md#gateway-deployment-capture-modes)):

| Agent Mode | Behaviour | Trade-off |
|---|---|---|
| **Sniffing Mode** | The agent allows all traffic to reach the DB server and sends a copy to the Gateway for review; if the Gateway signals a block, the agent sends an RST to the protected server | Lower CPU usage on the DB host, but some malicious traffic may reach the database before the block signal arrives |
| **Inline Mode** | The agent holds traffic, forwards it to the Gateway, and waits for a response before allowing it through or blocking it | Tighter security (nothing reaches the DB before a verdict), at the cost of a small amount of added latency and slightly higher CPU usage on the DB host |

#### Configuring Data Interfaces
**Navigation:** `Main > Setup > Agents > [Agent] > Data Interfaces tab`

- **Right pane (Discovered):** Auto-detected interfaces — Oracle BEQ, Oracle IPC, MSSQL IPC, TCP
- **Left pane (User Defined):** Manually define interfaces not auto-discovered (e.g., Oracle BEQ/IPC on Windows)
- For each interface to monitor: **uncheck the red Ignore box** and set the **Service** drop-down to the correct Database Service

#### Creating Agent Exclusion Rules
**Navigation:** `Main > Setup > Global Objects > Scope: Agent Monitoring Rules`

Use to exclude high-volume, trusted connections from being sent to the Gateway — saving network bandwidth and Gateway CPU. Common example: nightly database backup jobs.

**Configuration:**
1. Set Scope Selection to `Agent Monitoring Rules`
2. Create a new rule, set action to `Exclude from Monitoring`
3. In Match Criteria, **you must use criteria starting with "Agent Criteria -"**:
   - `Agent Criteria - Destination IP`
   - `Agent Criteria - Process Details`
   - `Agent Criteria - Application Name`

> **Warning:** Exclusion rules operate at the TCP connection level. Once a connection is excluded, **all activity within that session is invisible** — even if the same session later accesses sensitive tables. Use exclusions only for completely trusted, well-defined processes.


---

[← Back to README](../../README.md)
