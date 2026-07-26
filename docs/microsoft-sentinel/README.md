# Microsoft Sentinel Deployment

## Overview

Microsoft Sentinel is Microsoft's cloud-native Security Information and Event Management (SIEM) and Security Orchestration, Automation, and Response (SOAR) platform. It provides centralized security monitoring, threat detection, investigation, and response across cloud and on-premises environments.

As part of my Microsoft Security Operations (SOC) Home Lab, I deployed Microsoft Sentinel to build a centralized security monitoring environment for SC-200 learning and hands-on practice.

---

## Objectives

- Deploy Azure Log Analytics Workspace
- Enable Microsoft Sentinel
- Prepare the environment for security monitoring
- Build the foundation for threat hunting and incident response

---

## Environment

| Resource | Configuration |
|----------|---------------|
| Cloud Platform | Microsoft Azure |
| SIEM | Microsoft Sentinel |
| Workspace | SentinelLogWorkspace |
| Resource Group | SentinelRG |
| Region | East US |

---

# Step 1 – Create Log Analytics Workspace

A Log Analytics Workspace was created to collect, store, and analyse security logs. This workspace serves as the data repository used by Microsoft Sentinel.

### Screenshot

![Create Log Analytics Workspace](Screenshots/Sentinel/01-create-log-analytics-workspace.png.png)

---

# Step 2 – Review Configuration

Azure validated the deployment configuration before creating the Log Analytics Workspace. This ensured that the subscription, resource group, workspace name, region, and pricing configuration were correct.

### Screenshot

![Review Configuration](../../Screenshots/Sentinel/02-review-and-create.png)

---

# Step 3 – Deployment Completed

The Log Analytics Workspace deployment completed successfully. The workspace is now ready to ingest telemetry from connected Microsoft security services.

### Screenshot

![Deployment Complete](../../Screenshots/Sentinel/03-deployment-complete.png)

---

# Step 4 – Enable Microsoft Sentinel

Microsoft Sentinel was enabled using the newly created Log Analytics Workspace.

After activation, the platform provides capabilities including:

- Security Monitoring
- Incident Management
- Threat Hunting
- Analytics Rules
- Automation
- Workbooks

### Screenshot

![Sentinel Activated](../../Screenshots/Sentinel/04-microsoft-sentinel-activated.png)

---

# Outcome

The deployment successfully established the core SIEM platform for the SOC Home Lab.

The environment is now prepared for:

- Microsoft Defender XDR integration
- Microsoft Entra ID logs
- Data Connectors
- Analytics Rules
- Threat Hunting
- Incident Investigation
- Automation Rules
- KQL Queries

---

# Skills Demonstrated

- Microsoft Sentinel Deployment
- Azure Log Analytics
- Azure Resource Management
- Security Monitoring
- SIEM Configuration
- Cloud Security
- Microsoft Azure
- SC-200 Security Operations
