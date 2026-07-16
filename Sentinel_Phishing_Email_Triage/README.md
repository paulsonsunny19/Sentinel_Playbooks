# Microsoft Sentinel - Phishing Email Triage

This Microsoft Sentinel playbook automates the enrichment of phishing-related incidents by querying Microsoft Defender XDR advanced hunting tables and adding the results back into the incident.

The playbook is designed to provide analysts with immediate context during phishing investigations, reducing the need for manual hunting across multiple Defender XDR tables.

---

## Features

When a new Microsoft Sentinel incident is created, the playbook automatically:

* Extracts the **Network Message ID** from the incident entities.
* Retrieves the list of email recipients.
* Retrieves sender authentication information including:

  * SPF
  * DKIM
  * DMARC
  * Composite Authentication
* Retrieves email attachment information.
* Retrieves users that clicked embedded URLs.
* Updates the Sentinel incident description with the enrichment results.
* Adds an HTML formatted comment containing the recipient information.

---

## Data Sources

The playbook queries the following Microsoft Defender XDR Advanced Hunting tables:

| Table               | Purpose                                                         |
| ------------------- | --------------------------------------------------------------- |
| EmailEvents         | Email sender information, recipients and authentication results |
| EmailAttachmentInfo | Email attachment details                                        |
| UrlClickEvents      | Safe Links click activity                                       |

---

## Requirements

Before deployment, ensure the following resources already exist:

* Microsoft Sentinel workspace
* Log Analytics workspace containing Defender XDR data
* User Assigned Managed Identity
* Microsoft Sentinel API Connection
* Azure Monitor Logs API Connection

The managed identity requires permissions to:

* Microsoft Sentinel
* Log Analytics Workspace

---

## Deployment

Click the button below to deploy the Logic App into your Azure subscription.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpaulsonsunny19%2FSentinel_Playbooks%2Fmain%2FSentinel_Phishing_Email_Triage%2Fazuredeploy.json)

---

## Deployment Parameters

| Parameter                      | Description                                            |
| ------------------------------ | ------------------------------------------------------ |
| workflowName                   | Logic App name                                         |
| logAnalyticsWorkspaceName      | Log Analytics workspace containing Defender XDR tables |
| userAssignedIdentityName       | Existing User Assigned Managed Identity                |
| sentinelConnectionName         | Sentinel API Connection name                           |
| azureMonitorLogsConnectionName | Azure Monitor Logs API Connection name                 |

---

## How It Works

```text
Microsoft Sentinel Incident
            │
            ▼
Trigger on Incident Creation
            │
            ▼
Extract Network Message ID
            │
            ▼
───────────────────────────────────────
│ Query EmailEvents                  │
│ Query UrlClickEvents               │
│ Query EmailAttachmentInfo          │
───────────────────────────────────────
            │
            ▼
Generate HTML Tables
            │
            ▼
Update Sentinel Incident
            │
            ▼
Add Analyst Comment
```

---

## Outputs

The playbook enriches the incident with:

* Sender authentication results
* Email subject
* Sender address
* First contact status
* Recipient list
* URL click activity
* Attachment details

This enables SOC analysts to triage phishing incidents directly from the Sentinel incident without manually querying Defender XDR.

---

## Repository Structure

```text
Sentinel_Phishing_Email_Triage/
│
├── azuredeploy.json
├── azuredeploy.parameters.example.json
├── README.md
└── images/
    └── workflow.png
```

---

## Contributing

Contributions are welcome.

If you identify improvements or bug fixes, feel free to submit a pull request.

---

## Disclaimer

This project is provided as-is without warranty.

Always test the playbook in a non-production environment before deploying to production.

---

## Author

**Paul Sunny**

GitHub: https://github.com/paulsonsunny19
