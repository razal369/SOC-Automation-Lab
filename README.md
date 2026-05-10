# SOC Automation Home Lab: End-to-End Mimikatz Detection

[![Project Type: Blue Team](https://img.shields.io/badge/Type-Blue%20Team-blue)](https://github.com/razal369/SOC-Automation-Lab)
[![MITRE ATT&CK: T1003](https://img.shields.io/badge/MITRE-T1003-red)](https://attack.mitre.org/techniques/T1003/)
[![Project Status: Completed](https://img.shields.io/badge/Status-Completed-green)](https://github.com/razal369/SOC-Automation-Lab)
[![Tools Used](https://img.shields.io/badge/Tools-Wazuh%20%7C%20Shuffle%20%7C%20TheHive%20%7C%20VirusTotal-orange)](https://github.com/razal369/SOC-Automation-Lab)

## Project Overview
This laboratory environment demonstrates a complete SOC automation pipeline designed to detect, enrich, and respond to **Mimikatz** credential dumping attacks. By leveraging open-source security tools, the system automatically handles the identification and documentation of threats without manual intervention.

## System Architecture
The pipeline follows a structured flow from initial detection on a Windows endpoint to automated enrichment and case management:

1.  **Windows 11 Endpoint**: Monitored via Wazuh Agent and Sysmon for deep telemetry.
2.  **Wazuh Manager**: Correlates events and triggers Rule 100002 upon detection.
3.  **Shuffle SOAR**: Receives the alert via Webhook and orchestrates the response.
4.  **IOC Extraction**: SHA256 hashes are extracted using advanced regex.
5.  **Enrichment & Documentation**:
    *   **VirusTotal**: Automated reputation check of extracted file hashes.
    *   **TheHive**: Automatic generation of high-priority security alerts.
6.  **Analyst Review**: Final investigation and closure within TheHive dashboard.

## Technical Stack
| Component | Function |
| :--- | :--- |
| **Wazuh** | SIEM/XDR - Core detection engine and log aggregator |
| **Shuffle** | SOAR - Workflow orchestration and automation |
| **TheHive** | Case Management - Incident tracking and response platform |
| **VirusTotal API** | Threat Intelligence - Automated IOC enrichment |
| **Sysmon** | Telemetry - Advanced Windows event logging |
| **Infrastructure** | Ubuntu 24.04 LTS, ngrok (Tunneling), Docker |

## Detection Logic
The core detection relies on custom Wazuh rulesets tailored for high-fidelity credential dumping identification:

*   **Rule ID**: 100002
*   **Severity Level**: 15 (Critical)
*   **MITRE Mapping**: T1003 (Credential Dumping)
*   **Condition**: Detection of `win.eventdata.originalFileName = mimikatz.exe` via Sysmon process creation telemetry.

## 🧪 Laboratory Testing
To validate the end-to-end pipeline, a simulated Mimikatz alert was generated using PowerShell to verify the Shuffle webhook integration and downstream orchestration:

```powershell
Invoke-RestMethod `
  -Uri "https://shuffler.io/api/v1/hooks/webhook_xxx" `
  -Method POST `
  -Body '{"rule":{"id":"100002","level":15,
  "description":"Mimikatz Usage Detected"},
  "agent":{"name":"razal"},
  "data":{"win":{"eventdata":{
  "originalFileName":"mimikatz.exe",
  "hashes":"SHA256=61c0810a..."}}}}' `
  -ContentType "application/json"
```

## ✅ Results & Validation
| Component | Status | Details |
| :--- | :--- | :--- |
| **Wazuh Detection** | ✅ SUCCESS | Rule 100002 triggered correctly on original filename |
| **Shuffle Workflow** | ✅ FINISHED | All orchestration nodes executed successfully |
| **SHA256 Extraction** | ✅ SUCCESS | File hash captured via regex node |
| **VirusTotal Check** | ✅ STATUS 200 | Hash identified as malicious via API lookup |
| **TheHive Alert** | ✅ STATUS 201 | Alert generated with all enrichment data |

## 📚 Key Learnings
*   **End-to-End Orchestration**: Gained deep experience in building fully automated SOC workflows from scratch.
*   **SOAR Integration**: Mastered webhook logic and multi-node orchestration using Shuffle.
*   **Threat Intel Enrichment**: Implemented automated API-based reputation checks via VirusTotal.
*   **Incident Lifecycle**: Managed the full lifecycle of an alert within TheHive case management platform.
*   **Custom SIEM Tuning**: Developed custom Wazuh rules with direct mapping to the MITRE ATT&CK framework.

## 🔗 References
*   [MyDFIR YouTube Channel](https://www.youtube.com/@MyDFIR) - Foundational project inspiration
*   [Wazuh Documentation](https://documentation.wazuh.com)
*   [TheHive Project](https://docs.strangebee.com)
*   [Shuffle SOAR Docs](https://shuffler.io/docs)
*   [VirusTotal Developer Portal](https://developers.virustotal.com)

## Repository Structure
```text
SOC-Automation-Lab/
│
├── README.md           # Project overview and technical details
├── report/             # Final SOC Automation Report
│   └── SOC_Automation_Report.pdf
└── configs/            # Wazuh & Sysmon configuration files
    ├── ossec.conf
    └── local_rules.xml
```

---
**Maintained by [MUHAMMED RAZAL TM](https://github.com/razal369)**
[LinkedIn](https://www.linkedin.com/in/muhammed-razal-tm) | [Portfolio](https://razal369.github.io/me/)
