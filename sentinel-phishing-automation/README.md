# SOC Phishing Autoclose

An Azure Logic App workflow for Microsoft Sentinel that automatically triages and closes low-risk phishing incidents based on recipient count and URL click activity.

---

## Overview

This solution automates the investigation of phishing incidents generated in Microsoft Sentinel.

When a phishing incident is created, the Logic App:

* Retrieves the Network Message ID from the incident entities.
* Queries Microsoft Defender XDR / Log Analytics for email details.
* Determines how many recipients received the email.
* Checks whether any recipients clicked URLs contained in the email.
* Automatically updates the incident based on the results.

### Workflow Logic

| Condition                           | Action                                                                                                                            |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Multiple recipients                 | Keep the incident **Active** and set **Severity = Medium**                                                                        |
| Single recipient with URL clicks    | Keep the incident **Active** and set **Severity = High**                                                                          |
| Single recipient with no URL clicks | Automatically **Close** the incident as **True Positive - Suspicious Activity**, tag the incident, and send an email notification |

---

## Architecture

The deployment uses the following Azure resources:

* Microsoft Sentinel
* Azure Logic Apps
* Log Analytics Workspace
* Microsoft Defender XDR Advanced Hunting tables
* User Assigned Managed Identity
* Microsoft Sentinel API Connection
* Azure Monitor Logs API Connection
* Office 365 Outlook API Connection

---

## Prerequisites

Before deploying this solution, ensure the following resources already exist:

* Microsoft Sentinel enabled Log Analytics Workspace
* User Assigned Managed Identity
* Microsoft Sentinel API Connection
* Azure Monitor Logs API Connection
* Office 365 Outlook API Connection

The managed identity must have permissions to:

* Read the Log Analytics workspace
* Update Microsoft Sentinel incidents

The Office 365 API connection must be authorized after deployment if it is not already authenticated.

---

## Deployment

Click the button below to deploy the Logic App into your Azure subscription.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpaulsonsunny19%2Fsentinel-phishing-automation%2Fmain%2FSOC-Phishing-Autoclose-consolidated.json)

---

## Deployment Parameters

During deployment you will be prompted for:

| Parameter                          | Description                                          |
| ---------------------------------- | ---------------------------------------------------- |
| Logic App Name                     | Name of the Logic App                                |
| Region                             | Azure region for the deployment                      |
| Subscription ID                    | Azure Subscription containing the resources          |
| Resource Group Name                | Resource Group containing the existing resources     |
| Log Analytics Workspace Name       | Sentinel Log Analytics Workspace                     |
| Managed Identity Name              | Existing User Assigned Managed Identity              |
| Sentinel Connection Name           | Existing Microsoft Sentinel API Connection           |
| Azure Monitor Logs Connection Name | Existing Azure Monitor Logs API Connection           |
| Office 365 Connection Name         | Existing Office 365 Outlook API Connection           |
| Notification Email                 | Email address that receives auto-close notifications |

---

## Repository Structure

```text
.
├── README.md
└── SOC-Phishing-Autoclose-consolidated.json
```

---

## Required Permissions

The User Assigned Managed Identity requires:

* Microsoft Sentinel Contributor (or equivalent permissions to update incidents)
* Log Analytics Reader

The Office 365 connection requires permission to send email.

---

## Post Deployment

After deployment:

1. Open the Logic App.
2. Verify the Microsoft Sentinel connection is healthy.
3. Verify the Azure Monitor Logs connection is healthy.
4. Verify the Office 365 connection is authorized.
5. Enable the Logic App if it is disabled.
6. Trigger a test phishing incident to validate the workflow.

---

## Notes

* This deployment assumes all Azure resources are located in the same subscription and resource group.
* Existing API Connections are reused.
* No additional Azure resources are created other than the Logic App workflow.

---

## Author

Sunny Paulson

---

## License

This project is provided as-is without warranty. Modify and use it according to your organization's requirements.
