# Troubleshooting Notes

## Issue 1

### Invoke-WebRequest returned HTTP 429

**Cause**

The remote server temporarily rate-limited repeated download requests.

**Resolution**

Retry after a short delay or use a different publicly accessible URL such as:

```
https://example.com
```

---

## Issue 2

### No Sysmon Event ID 3 in Wazuh

**Cause**

The event may not have been indexed yet or the selected time range did not include the activity.

**Resolution**

- Increase the dashboard time range to Last 24 Hours.
- Refresh the Threat Hunting page.
- Verify Sysmon Network Connection logging is enabled.

---

## Issue 3

### No Process Creation event found

**Cause**

The query window was too small or the PowerShell event had already scrolled beyond the latest results.

**Resolution**

Search:

```
data.win.system.eventID:1
```

Increase the time range and filter by:

```
powershell.exe
```

or

```
Invoke-WebRequest
```

---

## Issue 4

### File Creation event not visible

**Cause**

The file may not have been created successfully or Sysmon Event ID 11 was not generated yet.

**Resolution**

Verify the file exists:

```powershell
Get-ChildItem C:\SOCLab
```

Then refresh Wazuh and search:

```
data.win.system.eventID:11
```

---

## Issue 5

### PowerShell event not found in Sysmon

**Cause**

Sysmon had not yet written the event or the search returned only recent unrelated process creation events.

**Resolution**

Search the last 50–100 Event ID 1 records:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -FilterXPath "*[System[(EventID=1)]]" -MaxEvents 100
```

Filter results for:

- powershell.exe
- Invoke-WebRequest
- example.html

---

## Issue 6

### Wazuh events delayed

**Cause**

Agent synchronization delay between the endpoint and Wazuh Manager.

**Resolution**

- Confirm the Wazuh Agent service is running.
- Wait 30–60 seconds.
- Refresh the Threat Hunting page.
- Verify the correct time range is selected.

---

# Lessons Learned

- Correlating Process Creation, Network Connection, and File Creation events provides much richer visibility than analyzing a single event.
- Process GUID is a reliable field for linking related Sysmon events.
- Time range selection in Wazuh is critical when hunting recent activity.
- Benign PowerShell downloads closely resemble attacker behavior and should always be investigated in production environments.
