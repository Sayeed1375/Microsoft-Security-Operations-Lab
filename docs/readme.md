![Azure](https://shields.io)
![Microsoft Sentinel](https://shields.io)
![Microsoft Entra](https://shields.io)
![KQL](https://shields.io)
![SC-200](https://shields.io)

# Microsoft Entra ID Log Ingestion & Custom Workbook Dashboard

## Overview

Microsoft Entra ID (formerly Azure Active Directory) identity data is the first line of defense in a modern cloud architecture. By default, raw sign-in traffic and administrative tracking are not routed to an Azure Log Analytics workspace. 

As part of my Microsoft Security Operations (SOC) Home Lab, I configured a real-time diagnostic pipeline to stream security events from Microsoft Entra ID into Microsoft Sentinel. To conclude this deployment phase, I shifted away from default templates to engineer a custom visual monitoring dashboard workbook powered entirely by my own Kusto Query Language (KQL) scripts.

---

## Objectives

- Stream raw identity diagnostic logs to the SIEM repository
- Deploy and configure the official Microsoft Entra ID Sentinel connector
- Generate live simulated user authentication and brute-force traffic
- Verify telemetry pipeline operations inside the advanced KQL engine
- Build a custom monitoring workbook containing tailored visualization charts

---

## Deployment Workflow

```text
Microsoft Entra ID
        │
        ▼
Diagnostic Settings Policy
        │
        ▼
Log Analytics Workspace
        │
        ▼
Sentinel Content Hub
        │
        ▼
Data Connectors Ingestion
        │
        ▼
KQL Pipeline Verification
        │
        ▼
Custom Workbook Dashboard
```

## Environment

| Resource | Configuration |
|----------|---------------|
| Cloud Platform | Microsoft Azure |
| Identity Provider | Microsoft Entra ID |
| SIEM | Microsoft Sentinel |
| Workspace | SentinelLogWorkspace |
| Primary Tables | SigninLogs, AuditLogs |
| Dashboard Engine | Custom Sentinel Workbooks |

---

# Step 1 – Identify Ingestion Gap

Initially, executing advanced hunting scripts or reviewing workbook templates against identity schemas returned empty results. The database tables did not exist on the back-end infrastructure because a continuous platform logging policy had not been established.

### Screenshot

<img src="Screenshots/Sentinel/1 Empty Workbook Query Window.png" width="100%" alt="Empty Workbook Query Window">

---

# Step 2 – Create Diagnostic Setting Policy

To bridge this data visibility gap, a diagnostic streaming rule was created within the identity directory to forward raw logs down to the Sentinel engine workspace.

1. Navigated to **Microsoft Entra Admin Center** > **Identity** > **Monitoring & health** > **Diagnostic settings**.
2. Clicked **Add diagnostic setting**.

### Screenshot

<img src="Screenshots/Sentinel/2 Unconfigured Diagnostic Settings.png" width="100%" alt="Unconfigured Diagnostic Settings">

3. Provided a descriptive policy tracking name: `Entra-to-loganalytic`.
4. Checked the vital monitoring category logs: `SignInLogs` and `AuditLogs`.
5. Under Destination details, selected **Send to Log Analytics workspace** and bound the configuration directly to `SentinelLogWorkspace`.

### Screenshot

<img src="Screenshots/Sentinel/6 Active Diagnostic Setting Policy.png" width="100%" alt="Active Diagnostic Setting Policy">

---

# Step 3 – Deploy Solution Package

Microsoft Sentinel must be trained to parse and classify incoming Entra ID identity schemas. The solution container was sourced and unpacked from the cloud catalog.

1. Opened **Microsoft Sentinel** > **Content management** > **Content hub**.
2. Located the core **Microsoft Entra ID** standalone solution pack.
3. Initiated the installation sequence.

### Screenshot

<img src="Screenshots/Sentinel/3 Finding the Microsoft Entra ID Solution.png" width="100%" alt="Finding the Microsoft Entra ID Solution">

4. Monitored the automation window until deployment configurations completely finished building.

### Screenshots

<img src="Screenshots/Sentinel/4 Installation Progress Bar.png" width="100%" alt="Installation Progress Bar">

<img src="Screenshots/Sentinel/5 Installation Success Notification.png" width="100%" alt="Installation Success Notification">

---

# Step 4 – Configure Data Connector Ingestion

With the solution components properly compiled, the specific logs were activated inside the connector profile to complete the pipeline handshake.

1. Navigated to **Configuration** > **Data connectors** and highlighted **Microsoft Entra ID**.

### Screenshot

<img src="Screenshots/Sentinel/7 Sentinel Data Connectors Menu.png" width="100%" alt="Sentinel Data Connectors Menu">

2. Launched the **Open connector page** pane.
3. Enabled collection rules for **Sign-In Logs**, **Audit Logs**, **User Risk Events**, and **Risky Users**, then applied changes.

### Screenshot

<img src="Screenshots/Sentinel/8 Enabling Entra Log Types.png" width="100%" alt="Enabling Entra Log Types">

---

# Step 5 – Verify Pipeline Operations via KQL

Log Analytics creates database tables dynamically upon the arrival of their first payload packet. To wake up the logging mechanism, an **Incognito web browser tab** was opened to generate real-time authentication traffic against the environment portal.

To confirm the data route was fully operational:
1. Navigated to **Microsoft Sentinel** > **General** > **Logs**.
2. Toggled the editor interface module dropdown to **KQL mode**.
3. Executed an exploratory data testing command:

```kql
SigninLogs
| take 5
```

The workspace successfully populated rows displaying precise event times, logged-in user profiles, targeted cloud application names, and public IP networks.

### Screenshot

<img src="Screenshots/Sentinel/9 Successful KQL Query Log Results.png" width="100%" alt="Successful KQL Query Log Results">

---

# Step 6 – Create Custom Workbook Dashboard

Rather than deploying standard pre-designed metrics layouts, a custom monitoring dashboard was developed from scratch inside Sentinel Workbooks to practice structuring security data into tailored charts.

1. Selected **Threat management** > **Workbooks** and initialized a **New workbook**.
2. Switched into layout editing mode, generated a fresh query widget card, and wrote a specialized script to aggregate identity trends:

```kql
SigninLogs
| summarize Total = count() by AppDisplayName
```

3. Altered the presentation properties dropdown from a simple text table to a visual **Bar Chart**.
4. Saved the customized telemetry template components directly to the active resource group.

### Screenshot



---

## 🎯 Key Takeaways & Lab Lessons
* **Ingestion Delay:** Brand-new diagnostic pipelines can take anywhere from **15 to 45 minutes** to spin up across the Microsoft cloud architecture before logs populate the schema.
* **Forward-Looking Only:** Diagnostic configurations do not capture history retroactively. They only monitor events starting from the exact minute the configuration policy is saved.
* **KQL Mode Matters:** Switching the log query window from Simple mode to KQL mode gives you complete query control to hunt, analyze, and build custom visual steps.
