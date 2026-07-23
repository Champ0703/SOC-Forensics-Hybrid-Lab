 Endpoint Security & Telemetry Environment (Victim Machine Setup)

Documentation and setup details for building and deploying a fully instrumented **Windows 10 Victim Machine** on VMware, designed for endpoint telemetry collection, threat monitoring, and forensic artifact acquisition using **Wazuh Agent**, **System Monitor (Sysmon)**, and **Velociraptor**.

---

##  Role & Responsibilities

* **Virtualization & OS Deployment:** Provisioned and configured a baseline Windows 10 environment from scratch using VMware Workstation.
* **Telemetry Agent Integration:** Deployed Sysmon for deep system logging and integrated Sysmon Event ID streams into the Wazuh Agent log collector (`ossec.conf`).
* **Live Forensics Setup:** Configured and registered the Velociraptor endpoint agent to enable real-time VQL artifact collection and remote triage.

---

##  Technical Implementation & Contributions

### 1. Endpoint Provisioning (VMware & Windows 10)
* Built a clean **Windows 10 Enterprise/Pro** virtual machine from ISO on VMware.
* Configured isolated network adapters (NAT/Host-Only) and tailored security settings (disabling unnecessary updates while preserving auditing controls).
* Optimized system performance and snapshots for quick rollback during threat simulation runs.

### 2. Sysmon Setup & Audit Logging
* Installed **Microsoft System Monitor (Sysmon v14+)** on the victim host.
* Configured advanced logging for key behavioral indicators:
  * **Event ID 1:** Process Creation (with command-line arguments and parent process tracking).
  * **Event ID 3:** Network Connection initiation.
  * **Event ID 7:** Image / DLL Loading.
  * **Event ID 11 / 23:** File creation and file deletion tracking.
  * **Event ID 12/13/14:** Registry key modifications and key renames.

### 3. Wazuh Agent & Sysmon Integration
* Deployed and registered the **Wazuh Agent** service pointing to the central Wazuh Manager.
* Modified `ossec.conf` on the victim machine to ingest Windows Event Viewer channels, specifically binding the Sysmon operational log stream:
  ```xml
  <localfile>
    <location>Microsoft-Windows-Sysmon/Operational</location>
    <log_format>eventchannel</log_format>
  </localfile>