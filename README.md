# Blue Team SOC Home Lab Report

**Analyst:** Sam Germanson
**Date:** March 2026  
**Status:** Complete  
**Repository:** https://github.com/stg0025/blue-team-homelab

---

## Overview

Built a functional SOC home lab environment to simulate core Tier 1 analyst workflows including SIEM deployment, endpoint telemetry collection, attack simulation, detection engineering, alert triage, and incident documentation. All tasks map directly to entry-level SOC analyst responsibilities.

---

## Environment

| Component | Details |
|---|---|
| Host Machine | Windows laptop |
| SIEM Host | Ubuntu Server 22.04 LTS — splunk-server |
| Monitored Endpoint | Windows 11 Pro — DESKTOP-UTEILEI |
| Virtualization | VirtualBox |
| Network | Host-Only adapter for lab traffic, NAT adapter for internet access during installation |
| SIEM | Splunk Enterprise 9.2.1 |
| Endpoint Agent | Sysmon with SwiftOnSecurity config + Splunk Universal Forwarder |

---

## Phase 0 — Environment Setup

Installed VirtualBox and created two virtual machines. Ubuntu Server 22.04 LTS configured as Splunk host with 4GB RAM and 50GB disk. Windows 11 Pro configured as monitored endpoint with 4GB RAM and 60GB disk.

Both VMs placed on the same Host-Only network adapter to enable direct communication. A secondary NAT adapter was added to each VM for internet access during software installation. Netplan was configured on Ubuntu to persist the NAT adapter IP across reboots. VM-to-VM connectivity verified via ICMP ping.

---

## Block 1 — Splunk SIEM Deployment

Splunk Enterprise installed on Ubuntu Server under /opt/splunk. Boot-start enabled. Splunk Web confirmed accessible at http://192.168.56.103:8000. Receiving port 9997 configured to accept Universal Forwarder connections. Dedicated index created: windows_logs.

---

## Block 2 — Endpoint Telemetry

Sysmon installed on Windows 11 VM using the SwiftOnSecurity community configuration. This config is widely used in production SOC environments as a noise-reduced baseline that captures high-value events including process creation, network connections, file creation, and registry changes.

Splunk Universal Forwarder installed and configured via inputs.conf using the WinEventLog API method to ingest Sysmon, Security, and System logs. Windows Security audit policy was configured to enable process creation logging with command-line data via auditpol and registry key. Log ingestion confirmed via Splunk search showing WinEventLog:Security and WinEventLog:System sourcetypes with parsed fields.

**Troubleshooting note:** Initial forwarder configuration using monitor commands read raw .evtx files producing unparseable binary data. Fixed by switching to WinEventLog:// API method in inputs.conf. This is the correct production method for Windows event log collection.

---

## Block 3 — Attack Simulation and Detection Rules

Atomic Red Team installed via PowerShell module. Windows Defender real-time monitoring disabled for test execution. Four MITRE ATT&CK techniques simulated and detected.

Detections built using Windows Security Log EventCode 4688 (process creation with command-line logging enabled) rather than Sysmon EventCode 1. Both capture the same attacker behavior. In production, Sysmon is preferred due to enriched fields including file hashes and parent process GUID tracking.

| Technique | ID | Tool | EventCode | Result |
|---|---|---|---|---|
| Credential Dumping | T1003.002 | reg.exe | 4688 | Detected |
| Scheduled Task Persistence | T1053.005 | schtasks.exe | 4688 | Detected |
| PowerShell Encoded Command | T1059.001 | powershell.exe | 4688 | Detected |
| Local Account Discovery | T1087.001 | net.exe | 4688 | Detected |

All SPL detection queries documented in detections.txt.

---

## Block 4 — Alert Triage and Incident Report

Saved alert created in Splunk for T1059.001 PowerShell encoded command detection. Alert configured on cron schedule. Alert triggered by re-running Atomic Red Team test. Triage performed including event review, account identification, parent process analysis, and command line inspection. Full incident report produced in INC-001.md.

---

## Tools Used

Splunk Enterprise | Splunk Universal Forwarder | Sysmon | SwiftOnSecurity Sysmon Config | Atomic Red Team | Windows Security Event Logs | SPL | Ubuntu Server 22.04 | Windows 11 Pro | VirtualBox | MITRE ATT&CK

---

## References

- MITRE ATT&CK: https://attack.mitre.org
- SwiftOnSecurity Sysmon Config: https://github.com/SwiftOnSecurity/sysmon-config
- Atomic Red Team: https://github.com/redcanaryco/atomic-red-team
- Splunk Documentation: https://docs.splunk.com# Blue Team SOC Home Lab Report

**Analyst:** Sam Germanson
**Date:** February 2026
**Status:** In Progress

---

## Overview

Built a functional SOC home lab environment to simulate Tier 1 SOC analyst workflows including SIEM deployment, endpoint telemetry collection, attack simulation, detection engineering, alert triage, and incident documentation.

---

## Environment

| Component | Details |
|---|---|
| Host Machine | Windows laptop |
| SIEM Host | Ubuntu Server 22.04 LTS — splunk-server |
| Monitored Endpoint | Windows 11 Pro — Windows-Endpoint |
| Virtualization | VirtualBox |
| Network | Host-Only adapter for lab traffic, NAT for internet access |

---

## Phase 0 — Environment Setup

Installed VirtualBox and created two virtual machines. Ubuntu Server 22.04 LTS configured as Splunk host with 4GB RAM and 50GB disk. Windows 11 Pro configured as monitored endpoint with 4GB RAM and 60GB disk. Both VMs placed on Host-Only network. NAT adapter added for installation internet access. Netplan configured to persist NAT adapter IP across reboots. VM-to-VM connectivity verified via ping.

---

## Block 1 — Splunk SIEM Deployment

Splunk Enterprise installed on Ubuntu Server under /opt/splunk. Boot-start enabled. Splunk Web confirmed accessible at http://192.168.56.103:8000. Receiving port 9997 configured. Dedicated index created: windows_logs.

---

## Block 2 — Endpoint Telemetry

Sysmon installed on Windows 11 VM using SwiftOnSecurity community configuration. This config is used in production SOC environments as a noise-reduced baseline. Sysmon events verified in Event Viewer. Splunk Universal Forwarder installed and configured to ship Sysmon, Security, and System logs to Splunk on port 9997. Log ingestion confirmed via Splunk search.

---

## Block 3 — Attack Simulation and Detection Rules

*In progress*

---

## Block 4 — Alert Triage and Incident Report

*Pending*

---

## Tools Used

Splunk Enterprise | Sysmon | SwiftOnSecurity Sysmon Config | Atomic Red Team | Splunk Universal Forwarder | Ubuntu Server 22.04 | Windows 11 Pro | VirtualBox | MITRE ATT&CK

---

## References

- MITRE ATT&CK: https://attack.mitre.org
- SwiftOnSecurity Sysmon Config: https://github.com/SwiftOnSecurity/sysmon-config
- Atomic Red Team: https://github.com/redcanaryco/atomic-red-team
