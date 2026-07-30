![Azure](https://img.shields.io/badge/Microsoft-Azure-0078D4?logo=microsoftazure&logoColor=white)
![Microsoft Sentinel](https://img.shields.io/badge/Microsoft-Sentinel-5E5E5E)
![Microsoft Entra](https://shields.io)
![KQL](https://img.shields.io/badge/KQL-Kusto-green)
![SC-200](https://img.shields.io/badge/Certification-SC--200-blue)

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

![Empty Workbook Query Window](../../Screenshots/Sentinel/01-create-log-analytics-workspace.png.png)

---

# Step 2 – Create Diagnostic Setting Policy

To bridge this data visibility gap, a diagnostic streaming rule was created within the identity directory to forward raw logs down to the Sentinel engine workspace.

1. Navigated to **Microsoft Entra Admin Center** > **Identity** > **Monitoring & health** > **Diagnostic settings**.
2. Clicked **Add diagnostic setting**.

### Screenshot

![Unconfigured Diagnostic Settings](../../Screenshots/Sentinel/2 Unconfigured Diagnostic Settings.png.png)

3. Provided a descriptive policy tracking name: `Entra-to-loganalytic`.
4. Checked the vital monitoring category logs: `SignInLogs` and `AuditLogs`.
5. Under Destination details, selected **Send to Log Analytics workspace** and bound the configuration directly to `SentinelLogWorkspace`.

### Screenshot

![Active Diagnostic Setting Policy](../../Screenshots/Sentinel/6 Active Diagnostic Setting Policy.png.png)

---

# Step 3 – Deploy Solution Package

Microsoft Sentinel must be trained to parse and classify incoming Entra ID identity schemas. The solution container was sourced and unpacked from the cloud catalog.

1. Opened **Microsoft Sentinel** > **Content management** > **Content hub**.
2. Located the core **Microsoft Entra ID** standalone solution pack.
3. Initiated the installation sequence.

### Screenshot

![Finding the Microsoft Entra ID Solution](../../Screenshots/Sentinel/3 Finding the Microsoft Entra ID Solution.png.png)

4. Monitored the automation window until deployment configurations completely finished building.

### Screenshots

![Installation Progress Bar](../../Screenshots/Sentinel/4 Installation Progress Bar.png.png)

![Installation Success Notification](../../Screenshots/Sentinel/5 Installation Success Notification.png.png)

---

# Step 4 – Configure Data Connector Ingestion

With the solution components properly compiled, the specific logs were activated inside the connector profile to complete the pipeline handshake.

1. Navigated to **Configuration** > **Data connectors** and highlighted **Microsoft Entra ID**.

### Screenshot

![Sentinel Data Connectors Menu](../../Screenshots/Sentinel/7 Sentinel Data Connectors Menu.png.png)

2. Launched the **Open connector page** pane.
3. Enabled collection rules for **Sign-In Logs**, **Audit Logs**, **User Risk Events**, and **Risky Users**, then applied changes.

### Screenshot

![Enabling Entra Log Types](../../Screenshots/Sentinel/8 Enabling Entra Log Types.png.png)

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

![Successful KQL Query Log Results](../../Screenshots/Sentinel/9 Successful KQL Query Log Results.png.png)

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

![Populated Identity Dashboard Workbook](../../Screenshots/Sentinel/10 Populated Identity Dashboard Workbook.png.png)

---

# Outcome

The Microsoft Entra ID data integration and visual dashboard deployment were executed successfully. The workspace actively records critical access events across the domain, enabling:

- Visualization of identity log peaks via custom workspace dashboards
- Granular tracking of user sign-in parameters and geographic IP anomalies
- Preparation for multi-tier authentication abuse threat hunting scenarios
- Auditing of tenant directory modifications and administrative roles

## Next Steps

The next phase of the project includes:

- Constructing a scheduled Analytics Rule to flag potential credential stuffing attacks
- Simulating persistent malicious brute-force authentication streams via an external test client
- Plotting real-time sign-in traffic locations across an interactive geographic world map widget

## Skills Demonstrated

- Identity Telemetry Pipeline Ingestion
- Advanced KQL Query Engineering
- Custom Microsoft Sentinel Workbook Construction
- Data Transformation & Dashboard Visualization
- Cloud SIEM Directory Optimization
- Infrastructure Integration (Entra ID to Log Analytics)
- SC-200 Certification Blueprint Practical Application
