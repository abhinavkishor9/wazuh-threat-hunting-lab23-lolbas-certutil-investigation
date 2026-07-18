# Investigation Timeline

| Time | Activity | Evidence Source |
|------|----------|-----------------|
| 10:00 | Verified Wazuh and Sysmon services | PowerShell |
| 10:02 | Confirmed certutil.exe location | Command Prompt |
| 10:03 | Created investigation folder | PowerShell |
| 10:04 | Copied notepad.exe to investigation folder | PowerShell |
| 10:05 | Executed `certutil -hashfile` | Command Prompt |
| 10:05 | SHA256 hash generated | Certutil |
| 10:06 | Verified SHA256 using Get-FileHash | PowerShell |
| 10:07 | Process Creation recorded | Sysmon Event ID 1 |
| 10:08 | Endpoint telemetry ingested | Wazuh |
| 10:09 | Process execution investigated | Wazuh Threat Hunting |
| 10:11 | Evidence correlated across all sources | Analyst |
| 10:13 | Investigation completed | Analyst |

---

## Investigation Summary

The investigation confirmed the execution of `certutil.exe` using a legitimate administrative command to calculate the SHA256 hash of a local executable. Evidence collected from Command Prompt, PowerShell, Sysmon, and Wazuh was successfully correlated, and no indicators of malicious behavior were identified. The activity was assessed as expected administrative usage while demonstrating the investigative process required for LOLBin-related alerts.
