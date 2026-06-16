# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.11 Data Masking & Personal Information Masking

#### Configuring Data Masking (Sensitive Dictionaries)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Data Masking`

Masks sensitive data values with asterisks `***` **before** they are stored in the MX's internal database. This means even Imperva console users cannot see raw sensitive values in audit logs.

**Setup:**
1. Select the Database Service → **Operation tab**
2. Expand **Data Masking**
3. Under **Sensitive Dictionaries**, select predefined dictionaries:
   - MasterCard Credit Card Numbers
   - Visa Credit Card Numbers
   - US Social Security Numbers
   - Custom dictionaries for Bangladesh NID etc.
4. Use the drop-downs to apply dictionaries to:
   - **Requests** (Alerts, Violations, DB Audit logs)
   - **DB Audit Responses**
5. Optionally: apply specific dictionaries to specific table groups under **Specific Table Groups**


---

[← Back to README](../../README.md)
