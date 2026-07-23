# SOC-Forensics-Hybrid-Lab
Integrated SOC &amp; DFIR lab featuring Wazuh SIEM/XDR, Sysmon telemetry, and Velociraptor for centralized threat detection, MITRE ATT&amp;CK mapping, and live forensics

## Project Overview
Developed by **SOC + Forensics Hybrid Lab**, this project demonstrates an enterprise-grade Security Operations Center (SOC) and Digital Forensics / Incident Response (DFIR) pipeline. By combining **Sysmon** (granular endpoint logging), **Wazuh** (centralized SIEM/XDR & threat mapping), and **Velociraptor** (advanced forensic artifact collection and threat hunting), this setup provides full-spectrum detection, analysis, and investigation capabilities across multi-node environments.

## Team Roles & Responsibilities

| Team Member | Role | Primary Focus |
| :--- | :--- | :--- |
| **Member A** | **Network Infrastructure Provider** | Configured network topology, pfSense routing, firewall policies, and Tailscale mesh network connectivity. |
| **Member B** | **Team Lead / SOC & SIEM Analyst** | Managed project deployment, tuned Wazuh SIEM detection rules, and analyzed MITRE ATT&CK alerts. |
| **Member C** | **Victim Endpoint Operations** | Configured target Windows endpoints, deployed Sysmon telemetry collectors, and maintained agent status. |
| **Member D** | **Red Team / Attacker** | Conducted controlled threat simulations, executed test payloads, and verified alerting pathways. |


## Architecture & Component Integration

| Tool | Role & Functionality |
| :--- | :--- |
| **Sysmon** | Installed on Windows endpoints to generate detailed system event logs (Process Creation, Network Connections, Registry modifications). |
| **Wazuh Manager** | Centralized SIEM engine running on Ubuntu Linux. Processes Sysmon & system logs, maps threats to MITRE ATT&CK, and enforces compliance standards (PCI DSS). |
| **Velociraptor** | On-demand Digital Forensics & Incident Response (DFIR) framework used to hunt endpoints using VQL (Velociraptor Query Language) and inspect live host artifacts. |
| **pfSense & Tailscale** | Provides secure network segmentation, routing, and remote mesh connectivity between nodes. |


## Verification & Proof of Concept

### 1. Threat Detection & MITRE ATT&CK Mapping (Wazuh)
*Wazuh dashboard visualizing real-time alerts categorized by MITRE ATT&CK tactics (Execution, Defense Evasion, Persistence, Privilege Escalation).*
![Wazuh MITRE Dashboard](screenshots/Screenshot%20(143).png)

### 2. Event Analysis & PCI DSS Logging (Wazuh + Sysmon)
*Correlation of Sysmon events (e.g., Suspicious Process Detection - Rule 61640) alongside Windows logon and service status events.*
![Wazuh PCI DSS Alerts](screenshots/Screenshot%20(142).png)

### 3. Velociraptor Server Configuration & Setup
*Configuring the Velociraptor server daemon and GUI portal on Ubuntu via terminal (`velociraptor config generate`).*
![Velociraptor Setup](screenshots/Screenshot%20(124).png)

### 4. DFIR Host Artifact Hunting (Velociraptor)
*Querying connected Windows endpoints (`DESKTOP-5HK5798`) to extract PowerShell command history, logged-in user accounts, and real-time client metrics.*
![Velociraptor Forensics](screenshots/Screenshot%20(139).png)
