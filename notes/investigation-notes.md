# Investigation Notes

## Lab Summary

This investigation examined the execution of `certutil.exe`, a Windows LOLBin commonly abused by attackers.

The objective was to determine whether the observed execution represented legitimate administrative activity or potential malicious behavior by collecting and correlating evidence from multiple endpoint telemetry sources.

---

## Analyst Methodology

The investigation followed a structured DFIR workflow:

1. Verify endpoint monitoring services.
2. Identify the executed LOLBin.
3. Execute a safe administrative command.
4. Collect process execution evidence.
5. Validate SHA256 hash generation.
6. Investigate Sysmon process creation logs.
7. Investigate Wazuh telemetry.
8. Correlate evidence across multiple sources.
9. Assess the legitimacy of the activity.
10. Document findings and conclusions.

---

## Investigation Steps

### Monitoring Verification

Verified that both Sysmon and Wazuh services were operational.

---

### LOLBin Identification

Confirmed that `certutil.exe` was located in the Windows System32 directory.

---

### Safe Command Execution

Executed:

```
certutil -hashfile C:\LOLBASLab\notepad.exe SHA256
```

This generated a SHA256 hash without performing any network or decoding operations.

---

### Hash Verification

Validated the generated hash using PowerShell's `Get-FileHash` cmdlet.

---

### Sysmon Analysis

Reviewed Event ID 1 and extracted:

- Process Name
- Parent Process
- Command Line
- User
- Timestamp

---

### Wazuh Investigation

Verified that Wazuh successfully ingested the process execution event and displayed the associated telemetry.

---

### Evidence Correlation

Compared:

- Command Prompt
- PowerShell
- Sysmon
- Wazuh

All evidence sources consistently described the same activity.

---

## Analyst Observations

- The executed LOLBin was digitally signed by Microsoft.
- The command line matched a legitimate administrative operation.
- The generated SHA256 hash was consistent across multiple tools.
- No suspicious child processes were observed.
- No network activity was generated.
- No persistence mechanisms were identified.
- No additional suspicious processes were launched.

---

## Conclusion

Although `certutil.exe` is frequently abused during cyber attacks, the observed activity represented legitimate administrative usage.

The investigation demonstrated the importance of contextual analysis when investigating LOLBins and reinforced the value of correlating endpoint telemetry before classifying an event as malicious.
