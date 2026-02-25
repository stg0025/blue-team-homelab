# Blue Team SOC Home Lab Report

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
