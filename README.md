# SOC Automation Home Lab: End-to-End Mimikatz Detection

[![Project Type: Blue Team](https://img.shields.io/badge/Type-Blue%20Team-blue)](https://github.com/razal369/SOC-Automation-Lab)
[![MITRE ATT&CK: T1003](https://img.shields.io/badge/MITRE-T1003-red)](https://attack.mitre.org/techniques/T1003/)
[![Project Status: Completed](https://img.shields.io/badge/Status-Completed-green)](https://github.com/razal369/SOC-Automation-Lab)
[![Tools Used](https://img.shields.io/badge/Tools-Wazuh%20%7C%20Shuffle%20%7C%20TheHive%20%7C%20VirusTotal-orange)](https://github.com/razal369/SOC-Automation-Lab)

## 📌 Project Overview
This laboratory environment demonstrates a complete **Security Operations Center (SOC) automation pipeline** designed to detect, enrich, and respond to **Mimikatz** credential dumping attacks. By leveraging best-in-class open-source security tools, the system automatically identifies threats, extracts critical IOCs, and documents the incident within a case management platform—all without manual analyst intervention.

## 🏗️ System Architecture
The pipeline follows a structured, high-fidelity orchestration flow:

1.  **Detection**: A monitored Windows 11 endpoint triggers a high-severity alert in **Wazuh** via Sysmon telemetry.
2.  **Ingestion**: **Shuffle SOAR** receives the alert via a secure webhook integration.
3.  **Processing**: Custom regex nodes extract SHA256 file hashes and metadata from the raw alert.
4.  **Enrichment**: The SOAR workflow queries the **VirusTotal API** for automated reputation scoring.
5.  **Response**: A detailed case is automatically generated in **TheHive**, populated with all enrichment data for analyst review.

## 🛠️ Technical Stack
| Component | Function |
| :--- | :--- |
| **Wazuh** | SIEM/XDR - Detection engine & log aggregator |
| **Shuffle** | SOAR - Workflow orchestration |
| **TheHive** | Case Management - Incident tracking platform |
| **VirusTotal** | Threat Intelligence - Automated enrichment |
| **Sysmon** | Telemetry - Deep Windows event visibility |
| **Infrastructure** | Ubuntu 24.04 LTS, Docker, ngrok |

## 🔍 Detection Logic
The core detection relies on high-severity custom rulesets:

*   **Rule ID**: `100002`
*   **Severity**: `15` (Critical)
*   **MITRE ATT&CK**: [T1003 (Credential Dumping)](https://attack.mitre.org/techniques/T1003/)
*   **Signature**: Identification of `mimikatz.exe` via `OriginalFileName` field in Sysmon events.

## 🧪 Laboratory Testing
Validation was performed using a simulated JSON payload to trigger the full SOAR orchestration chain:

```powershell
Invoke-RestMethod `
  -Uri "https://shuffler.io/api/v1/hooks/webhook_xxx" `
  -Method POST `
  -Body '{"rule":{"id":"100002","level":15,"description":"Mimikatz Usage Detected"},"agent":{"name":"razal"},"data":{"win":{"eventdata":{"originalFileName":"mimikatz.exe","hashes":"SHA256=61c0810a..."}}}}' `
  -ContentType "application/json"
```

## ✅ Results & Validation
| Component | Status | Details |
| :--- | :--- | :--- |
| **Wazuh Detection** | ✅ SUCCESS | Rule 100002 triggered on attack signature |
| **Shuffle Workflow** | ✅ FINISHED | End-to-end orchestration nodes completed |
| **Enrichment** | ✅ SUCCESS | VirusTotal API identified malicious hash |
| **Case Management** | ✅ CREATED | Detailed alert successfully logged in TheHive |

## 📁 Repository Structure
```text
SOC-Automation-Lab/
├── README.md              # Project overview and technical details
├── report/                # Final SOC Automation Report
│   └── SOC_Automation_Report.pdf
└── configs/               # SIEM & SOAR configuration files
    ├── ossec.conf         # Wazuh integration settings
    └── local_rules.xml    # Custom detection rules
```

---
**Maintained by [MUHAMMED RAZAL TM](https://github.com/razal369)**  
[LinkedIn](https://www.linkedin.com/in/muhammed-razal-tm) | [Professional Portfolio](https://razal369.github.io/me/)
