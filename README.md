# wazuh-threat-hunting-lab16-powershell-download-investigation
## Overview

This lab demonstrates how a SOC analyst investigates PowerShell download activity using Sysmon telemetry collected by Wazuh.

Instead of analyzing a single event, this investigation correlates multiple Sysmon events to reconstruct the attack timeline, including process creation, network communication, and file creation.

The objective is to simulate a common attacker technique where PowerShell is used to download content from the Internet and investigate the activity using Wazuh Threat Hunting.

---

# Lab Objectives

- Generate PowerShell download activity
- Detect Sysmon Process Creation events
- Detect Network Connection events
- Detect File Creation events
- Correlate events inside Wazuh
- Build an investigation timeline
- Practice SOC alert triage techniques

---

# Technologies Used

- Wazuh SIEM
- Sysmon
- Windows 11
- PowerShell
- Windows Event Logs

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Execution | PowerShell | T1059.001 |
| Command and Control | Application Layer Protocol | T1071.001 |
| Ingress Tool Transfer | Ingress Tool Transfer | T1105 |

---

# Lab Environment

**Host Machine**
- Windows 11

**Monitoring**
- Wazuh Agent
- Sysmon

**SIEM**
- Wazuh Dashboard

---

# Attack Simulation

A PowerShell command downloaded a file from the Internet using Invoke-WebRequest.

The activity generated multiple Sysmon events that were forwarded to Wazuh for investigation.

---

# Investigation Workflow

1. Execute PowerShell download.
2. Verify downloaded file.
3. Review Sysmon Event ID 1.
4. Review Sysmon Event ID 3.
5. Review Sysmon Event ID 11.
6. Search events in Wazuh.
7. Correlate Process ID and Process GUID.
8. Build the attack timeline.

---

# Wazuh Queries

## Process Creation

```
data.win.system.eventID:1
```

---

## Network Connections

```
data.win.system.eventID:3
```

---

## File Creation

```
data.win.system.eventID:11
```

---

# Investigation Summary

The investigation confirmed that PowerShell initiated a download operation from an external website. Sysmon successfully captured the process execution, network communication, and resulting file creation. Wazuh ingested these events, enabling analysts to correlate related telemetry and reconstruct the activity timeline.

---

# Skills Demonstrated

- Threat Hunting
- PowerShell Investigation
- Event Correlation
- Timeline Analysis
- Process Investigation
- Network Investigation
- Wazuh SIEM
- Sysmon Analysis
- MITRE ATT&CK Mapping

---

# Learning Outcome

This lab demonstrates how multiple Sysmon events can be correlated to investigate PowerShell download activity, providing experience similar to real-world SOC investigations where analysts reconstruct attacker behavior across several event sources.
