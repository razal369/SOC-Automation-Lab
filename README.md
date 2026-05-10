# SOC Automation Lab: Mimikatz Detection & Response Pipeline

## 🚀 Overview
This project demonstrates an end-to-end Security Operations Center (SOC) automation pipeline. It focuses on detecting **Mimikatz** credential dumping attacks on Windows endpoints and automating the enrichment and incident response process.

## 🛠️ Technology Stack
*   **Wazuh (SIEM/XDR)**: For endpoint monitoring, log collection, and initial detection.
*   **Shuffle (SOAR)**: Orchestration platform to connect various security tools.
*   **TheHive (Case Management)**: For tracking and managing security incidents.
*   **VirusTotal (Threat Intel)**: To enrich alerts with file and IP reputation data.
*   **Windows Sysmon**: For advanced endpoint telemetry.

## 🔄 Workflow
1.  **Detection**: A Mimikatz execution attempt is detected by Wazuh on a monitored Windows endpoint via Sysmon events.
2.  **Trigger**: Wazuh sends an alert to **Shuffle (SOAR)** via a custom webhook.
3.  **Enrichment**: Shuffle extracts the file hash (IOC) and queries **VirusTotal** for reputation data.
4.  **Case Creation**: Shuffle automatically creates a high-priority case in **TheHive**, attaching the enrichment data and relevant logs.
5.  **Notification**: (Optional) An automated notification is sent to security analysts (e.g., via Email or Slack).

## 📊 Key Features
*   **Automated Triage**: IOCs are automatically enriched before an analyst even sees the alert.
*   **Centralized Logging**: All telemetry is funneled into Wazuh for correlation.
*   **Scalable Architecture**: The use of Docker-based deployments for Wazuh and TheHive allows for easy scaling.

## 📸 Screenshots
*(Coming soon: Detailed screenshots of the pipeline in action)*

## 🤝 Collaboration
Feel free to fork this repository, submit PRs, or reach out if you have questions about implementing SOC automation!

---
**Designed & Built by [MUHAMMED RAZAL TM](https://github.com/razal369)**
