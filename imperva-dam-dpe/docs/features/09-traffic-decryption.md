# Imperva SecureSphere DAM — Features

> Part of the *Features & Functionalities* reference. See the [master navigation](../../README.md).

---

### 7.9 Traffic Decryption & Session Diagnostics

#### Resolving "Hashed User" (SSL-Encrypted Logins)
**Navigation:** `Main > Setup > Sites > [DB Service] > Definitions tab > Encryption Support`

**Symptom:** Audit logs show `Hashed User (unknown server SSL certificate)` instead of the actual username.  
**Cause:** The database login is encrypted with SSL — Imperva sees the encrypted handshake but cannot read the credentials inside it.  
**Fix:** Upload the database server's SSL certificate.

**Steps:**
1. On the DB Service Definitions tab, expand **Encryption Support**
2. Click **Create New (green +)**
3. Enter a key name
4. Select certificate format: `PKCS12/PFX`
5. Click **Browse** and upload the certificate file
6. Enter the certificate password
7. Click **Upload** → **Save**

#### Resolving "Connected User" (Missed Login Phase)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Connection Settings`

**Symptom:** Audit logs show `Connected User` instead of the actual username.  
**Cause:** Imperva missed the initial login packet — the session was already established when the Gateway or Agent came online, or the connection timeout mismatch caused Imperva to lose session context.  
**Fix:** Match Imperva's connection timeout to the database server's session timeout.

**Steps:**
1. Navigate to your Database Service → **Operation tab**
2. Expand **Connection Settings**
3. Adjust **Connection Timeout (Seconds)** to match the database server's exact session timeout value

#### Text Replacement (Query Normalisation)
**Navigation:** `Main > Setup > Sites > [DB Service] > Operation tab > Text Replacement`

**Problem:** Highly dynamic applications (e.g., ORM frameworks) generate thousands of slightly different query structures that bloat the profile with thousands of unique Query Groups.  
**Solution:** Text Replacement strips out dynamic variable portions and replaces them with static text before the query reaches the profiling engine.

**Steps:**
1. Review profile bloat at `Main > Profiles > Profile Overview`
2. Navigate to the relevant DB Service → Operation tab → expand **Text Replacement**
3. Configure replacement rules targeting:
   - Normalised Query patterns
   - User Name patterns
   - Application User Name patterns
4. After saving, go to the DB Application Profile and **manually delete** the old bloated query groups
5. Allow Imperva to relearn clean, normalised query structures


---

[← Back to README](../../README.md)
