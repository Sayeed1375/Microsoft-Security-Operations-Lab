# Building a Home SOC Lab: Ingesting Microsoft Entra ID Logs into Microsoft Sentinel

An empty `SigninLogs` table is a common roadblock when setting up a Microsoft Sentinel home lab. By default, Microsoft Entra ID (Azure AD) activity data is not routed to your Log Analytics workspace. 

This repository documents how I configured a streaming diagnostic pipeline, connected the raw data stream to Microsoft Sentinel, verified ingestion using Kusto Query Language (KQL), and built my own custom visual monitoring dashboard workbook from scratch.

---

## 🛠️ Architecture & Log Flow Pipeline
`Microsoft Entra ID (Identity Source)` ➡️ `Diagnostic Settings Policy` ➡️ `Log Analytics Workspace` ➡️ `Microsoft Sentinel (SIEM Engine)` ➡️ `Custom Sentinel Workbooks (My KQL Dashboard)`

---

## 🚶‍♂️ Step-by-Step Implementation Guide

### Step 1: Discovering the Ingestion Gap
Initially, when I attempted to run advanced hunting queries or build widgets against identity logs, the workspace yielded zero entries. The tables were completely unpopulated because no active log routing policy was established out of the box.
![Sentinel Activated](../../Screenshots/Sentinel/04-microsoft-sentinel-activated.png.png)

![Empty Workbook Query Window](../../Screenshots/Sentinel/01-empty-workbook-query-window.png.png)

---

### Step 2: Configuring My Diagnostic Setting Policy
To fix this, I instructed Microsoft Entra ID to broadcast its platform telemetry directly to my SIEM database.
1. I navigated to the **Microsoft Entra Admin Center** > **Identity** > **Monitoring & health** > **Diagnostic settings**.
2. I clicked **Add diagnostic setting**.

![Unconfigured Diagnostic Settings](../../Screenshots/Sentinel/02-unconfigured-diagnostic-settings.png)

3. I named my configuration policy `Entra-to-loganalytic`.
4. Under **Logs**, I selected the core monitoring categories I wanted to capture: `SignInLogs` and `AuditLogs`.
5. Under **Destination details**, I checked **Send to Log Analytics workspace**.
6. I linked it to my subscription and saved the policy.

![Active Diagnostic Setting Policy](../../Screenshots/Sentinel/06-active-diagnostic-setting-policy.png)

---

### Step 3: Deploying the Sentinel Solution Package
Next, I needed to make sure Microsoft Sentinel was fully trained to recognize and structure the incoming Entra tables.
1. I opened the **Microsoft Defender / Sentinel Portal**, went to **Content management**, and opened the **Content hub**.
2. I searched for the official **Microsoft Entra ID** solution pack and clicked **Install**.

![Finding the Microsoft Entra ID Solution](../../Screenshots/Sentinel/03-finding-the-microsoft-entra-id-solution.png)

3. I monitored the deployment sidebar until the system completely provisioned the underlying infrastructure.

![Installation Progress Bar](../../Screenshots/Sentinel/04-installation-progress-bar.png)
![Installation Success Notification](../../Screenshots/Sentinel/05-installation-success-notification.png)

---

### Step 4: Activating Log Ingestion
With the package successfully deployed, I had to explicitly turn on the data connector page to map the incoming log logs to the Sentinel database schema.
1. I went to **Configuration** > **Data connectors** and selected **Microsoft Entra ID**.

![Sentinel Data Connectors Menu](../../Screenshots/Sentinel/07-sentinel-data-connectors-menu.png)

2. I clicked **Open connector page**.
3. Under the configuration panel, I checked the boxes for **Sign-In Logs** and **Audit Logs**, along with advanced telemetry like `User Risk Events` and `Risky Users`.
4. I clicked **Apply Changes**.

![Enabling Entra Log Types](../../Screenshots/Sentinel/08-enabling-entra-log-types.png)

---

### Step 5: Forcing and Verifying the KQL Data Pipeline
Because Log Analytics does not import historical data retroactively, it needs a new event to initialize a table. I opened a completely fresh **Incognito browser tab** and logged into my Azure portal to generate real-time authentication traffic.

To verify that my data pipeline was successfully flowing into Sentinel:
1. I opened **Microsoft Sentinel** > **General** > **Logs**.
2. I switched my query environment settings from **Simple mode** to **KQL mode**.
3. I ran my verification script:
   ```kql
   SigninLogs
   | take 5
   ```

The database successfully returned live telemetry rows detailing my authentication time, application name, resource IDs, and public IP address.

![Successful KQL Query Log Results](../../Screenshots/Sentinel/09-successful-kql-query-log-results.png)

---

### Step 6: Creating My Custom Workbook Dashboard Using KQL
Instead of using a generic pre-made layout, I wanted to build my own visual monitoring view from scratch inside Sentinel Workbooks to get comfortable working with custom KQL visualizations.

1. I navigated to **Threat management** > **Workbooks** and created a **New workbook**.
2. I went into **Edit mode**, added a new query step widget, and wrote custom KQL to calculate and group my log telemetry data.
3. I used this optimized script to parse out successful versus failed events:
   ```kql
   SigninLogs
   | summarize Total = count() by AppDisplayName
   ```
4. I changed the **Visualization** parameter dropdown from a default text grid to a **Bar Chart** to create a clean visual layout.

My custom workbook dashboard updated automatically, giving my home SOC lab a visual, real-time breakdown of identity actions.

![Populated Identity Dashboard Workbook](../../Screenshots/Sentinel/10-populated-identity-dashboard-workbook.png)

---

## 🎯 Key Takeaways & Lab Lessons
* **Ingestion Delay:** Brand-new diagnostic pipelines can take anywhere from **15 to 45 minutes** to spin up across the Microsoft cloud architecture before logs populate the schema.
* **Forward-Looking Only:** Diagnostic configurations do not capture history retroactively. They only monitor events starting from the exact minute the configuration policy is saved.
* **KQL Mode Matters:** Switching the log query window from Simple mode to KQL mode gives you complete query control to hunt, analyze, and build custom visual steps.
