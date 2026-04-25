# Home SOC Lab – Brute Force Detection (Splunk)

## Overview

This project demonstrates a hands-on Security Operations Center (SOC) lab built using Splunk, Kali Linux, and Windows 10. The lab simulates a brute force attack against a Windows system and detects failed authentication attempts using Windows Event Logs.

The goal of this project was to replicate a real-world SOC workflow: generate attack data, ingest logs into a SIEM, analyze activity, and create a detection rule.

---

## Lab Architecture

* Attacker: Kali Linux VM
* Target: Windows 10 VM
* SIEM: Splunk Enterprise
* Network: VirtualBox Host-Only Network

---

## Tools Used

* Splunk (SIEM)
* Kali Linux
* Hydra (brute force tool)
* Windows Event Logs

---

## Attack Simulation

A brute force attack was simulated from Kali Linux targeting the Windows system over RDP. Multiple failed login attempts were generated using Hydra with a common password wordlist.

---

## Log Analysis

Windows Security logs were forwarded to Splunk using the Universal Forwarder. Failed login attempts were identified using:

Event ID: 4625 (Failed Logon)

Search query used:

```
index=wineventlog EventCode=4625
```

To analyze attack patterns:

```
index=wineventlog EventCode=4625
| stats count by Account_Name, host
| sort -count
```

This revealed repeated login attempts against common accounts such as Administrator.

---

## Detection Rule

A scheduled alert was created in Splunk to detect brute force behavior.

Alert Configuration:

* Name: Brute Force Login Detection
* Trigger: Number of results > 5
* Time Range: Last 5 minutes
* Schedule: Every 5 minutes

This reduces noise while identifying repeated failed authentication attempts.

---

## MITRE ATT&CK Mapping

Technique: T1110 – Brute Force

This activity aligns with adversaries attempting to gain access through repeated authentication attempts.

---

## Outcome

* Successfully simulated a brute force attack
* Ingested Windows logs into Splunk
* Identified failed login attempts (Event ID 4625)
* Built a detection rule for brute force activity

---

## Key Takeaways

* SIEM tools can effectively detect authentication-based attacks
* Event ID 4625 is critical for identifying failed login activity
* Scheduled alerts are more effective than real-time alerts for reducing noise
* Understanding log patterns is essential for SOC analysts

---

## Screenshots

(Add screenshots here of:)

* Splunk log ingestion
* EventCode 4625 results
* Detection query output
* Alert configuration
* Kali attack execution

---

## Future Improvements

* Detect successful logins (Event ID 4624)
* Add PowerShell logging
* Build dashboards in Splunk
* Expand detection rules for additional attack techniques
