# Atomic Red Team — Active Directory Password Guessing

## Overview

This lab uses [Atomic Red Team] to simulate controlled password-guessing activity against a dedicated Active Directory test account.

The purpose is to understand how authentication failures appear in Windows Security logs and how as a SOC analyst to investigate and detect suspicious activity using Splunk.

- **MITRE ATT&CK:** [T1110.001 — Brute Force: Password Guessing]
- **Testing framework:** Atomic Red Team
- **Target environment:** Isolated Active Directory lab
- **Detection platform:** Splunk Enterprise
- **Objective:** Generate and investigate authentication telemetry

## Lab Prerequisites

- A Windows Server VM configured as an Active Directory Domain Controller.
- A Windows client joined to the test domain.
- Atomic Red Team and Invoke-AtomicRedTeam installed on the test machine.
- A dedicated, non-privileged test account.
- Windows Security auditing configured appropriately.
- Domain Controller Security logs forwarded to Splunk.
- VM snapshots and a review of the account-lockout policy.

## 1. Identify the Atomic Test

powershell => 
Invoke-AtomicTest T1110.001 -ShowDetailsBrief

powershell => 
Invoke-AtomicTest T1110.001 -ShowDetails

powershell => 
Invoke-AtomicTest T1110.001 -TestNumbers 1 -CheckPrereqs

Test 1 covers password guessing against an Active Directory user over SMB.  

 
## 2. Execute the Controlled Simulation

Only proceed after confirming that the test targets your lab environment, the required inputs are correct, and the potential account-lockout impact is understood.

powershell => 
Invoke-AtomicTest T1110.001 -TestNumbers 1 -InputArgs @{user="LabBruteTest"}


## 3. Investigate Windows Security Events

After the simulation, inspect the Domain Controller's Security log.

| Event ID | Description |
|---|---|
| 4625 | Failed logon |
| 4776 | NTLM credential validation |
| 4771 | Kerberos pre-authentication failure |
| 4740 | Account locked out |


Finally Verify the Events in Splunk

