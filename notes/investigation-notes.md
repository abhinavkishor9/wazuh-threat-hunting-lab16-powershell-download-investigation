# Investigation Notes

## Incident Summary

A PowerShell process initiated a download from an external website. The activity generated multiple Sysmon events that were forwarded to Wazuh for analysis.

---

# Initial Detection

PowerShell was executed using Invoke-WebRequest to download a file into the local SOCLab directory.

---

# Evidence Collected

## Sysmon Event ID 1

Observed:

- Process Creation
- powershell.exe
- CommandLine
- Parent Process
- Process ID
- Process GUID
- User Account

Purpose:

Confirmed that PowerShell initiated the download activity.

---

## Sysmon Event ID 3

Observed:

- Network Connection
- Destination IP
- Destination Port
- Protocol
- Image
- Process GUID

Purpose:

Verified outbound network communication associated with the PowerShell process.

---

## Sysmon Event ID 11

Observed:

- File Created
- Target Filename
- Image
- User
- Process GUID

Purpose:

Confirmed successful creation of the downloaded file.

---

# Timeline

1. PowerShell executed.
2. Network connection established.
3. File downloaded successfully.
4. Sysmon generated telemetry.
5. Wazuh collected all events.
6. Events were correlated using Process GUID and timestamps.

---

# MITRE ATT&CK

Execution

- T1059.001 – PowerShell

Command and Control

- T1071.001 – Application Layer Protocol

Ingress Tool Transfer

- T1105 – Ingress Tool Transfer

---

# SOC Assessment

The observed activity is consistent with techniques frequently used by attackers to download payloads from external sources. Although the file in this lab was benign, similar behavior should be investigated in production environments to determine intent and assess potential risk.

---

# Conclusion

The investigation successfully demonstrated how correlated Sysmon events provide visibility into PowerShell-based download activity and how Wazuh supports end-to-end incident analysis.
