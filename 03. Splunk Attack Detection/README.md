# 03 — Splunk SIEM

Splunk is the detection engine for this lab. It runs on Ubuntu Server and receives logs from both Windows machines through Universal Forwarders. Sysmon on the endpoints provides detailed process, network, and file activity logs on top of standard Windows Event Logs.

---

## Setup overview

**Splunk server (Ubuntu 192.168.106.136)**
- Installed Splunk Enterprise
- Configured to listen on port 9997 for incoming forwarder data
- Set up indexes and dashboards for log analysis

**Splunk Universal Forwarder (Windows Server 2022 + Windows 10)**
- Installed on both Windows machines
- Configured to forward Windows Event Logs and Sysmon logs to 192.168.10.10:9997

**Sysmon (Windows Server 2022 + Windows 10)**
- Installed using the SwiftOnSecurity config
- Captures: process creation, network connections, file creation, registry changes

---

## Screenshots

| File | What it shows |
|---|---|
| `01-splunk-forwarder-and-sysmon-running.png` | Forwarder and Sysmon services active on endpoint |
| `02-splunk-SIEM-dashboard.png` | Splunk receiving events from both machines |
| `03-account-creation-detection.png` | Splunk alert triggered by a new account being created in AD |

---

## Key event IDs monitored

| EventID | Description |
|---|---|
| 4625 | Failed logon attempt |
| 4624 | Successful logon |
| 4726 | User account created |

---

## Notes

The account creation detection screenshot shows Splunk catching EventID 4726 when a new user was added in Active Directory — this is a real detection rule used in SOC environments to spot unauthorized account creation.
