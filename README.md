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

## Automation Pipeline
The automated workflow ensures rapid response times and consistent data collection:
1.  **Endpoint Activity**: Mimikatz execution is captured by Sysmon.
2.  **Detection**: Wazuh Manager identifies the specific attack signature.
3.  **Webhook Trigger**: A JSON alert is forwarded to Shuffle SOAR.
4.  **Data Processing**: Shuffle parses the alert, isolating critical file metadata.
5.  **Intelligence Lookup**: VirusTotal verifies the file's malicious reputation (Verified 200 OK).
6.  **Incident Logging**: TheHive creates a new alert containing all gathered evidence (Verified 201 Created).

## Repository Structure
```text
SOC-Automation-Lab/
├── config/             # Configuration files for Wazuh & Shuffle
├── documentation/      # Step-by-step implementation guide
├── logs/               # Sample detection logs and JSON alerts
└── README.md           # Project overview and technical details
```

---
**Maintained by [MUHAMMED RAZAL TM](https://github.com/razal369)**
