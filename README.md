# wazuh-threat-hunting-lab23-lolbas-certutil-investigation
## Overview

This lab demonstrates how to investigate the execution of a Living-off-the-Land Binary (LOLBin) using Wazuh and Sysmon.

Rather than analyzing malware, the investigation focuses on `certutil.exe`, a legitimate Microsoft utility that attackers frequently abuse for payload downloads, Base64 encoding/decoding, and defense evasion.

In this scenario, `certutil.exe` is used safely to calculate the SHA256 hash of a local executable. The objective is to investigate the process execution, collect forensic evidence, correlate telemetry from multiple sources, and determine whether the activity is legitimate or suspicious.

---

## Executive Summary

The investigation began after the execution of `certutil.exe` generated endpoint telemetry captured by Sysmon and forwarded to Wazuh.

The analyst verified the legitimacy of the executed command, examined process execution details, identified the parent process, confirmed the user context, validated the generated SHA256 hash, and correlated evidence across PowerShell, Sysmon, and Wazuh.

Although the investigation involved a commonly abused LOLBin, the analysis determined that the activity represented legitimate administrative usage because the binary was only used to calculate a file hash and exhibited no indicators of malicious behavior.

---

## Investigation Scenario

SOC analysts received an alert indicating execution of `certutil.exe`.

Since attackers frequently abuse LOLBins to evade security controls, the analyst was tasked with determining:

- Which LOLBin executed
- What command was executed
- Which user initiated the process
- Parent process
- Execution timestamp
- Whether the activity appeared legitimate or suspicious

---

## Skills Demonstrated

- Living-off-the-Land Binary (LOLBAS) investigation
- Windows process execution analysis
- Process tree analysis
- Parent-child process correlation
- Command-line investigation
- SHA256 hash verification
- Windows DFIR methodology
- IOC identification and validation
- Evidence correlation across multiple telemetry sources
- Sysmon Event ID 1 analysis
- Wazuh Threat Hunting
- Security event validation
- Incident documentation

---

## Tools Used

- Wazuh
- Sysmon
- PowerShell
- Windows Command Prompt
- Windows Event Viewer
- File Explorer

---

## Lab Environment

| Component | Details |
|------------|---------|
| Host OS | Windows 11 |
| SIEM | Wazuh |
| Endpoint Monitoring | Sysmon |
| Shell | PowerShell & Command Prompt |
| Investigation Target | certutil.exe |
| Test File | notepad.exe |

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Defense Evasion | System Binary Proxy Execution | T1218 |
| Defense Evasion | Signed Binary Proxy Execution | T1218 |
| Execution | Command and Scripting Interpreter | T1059 |
| Discovery | File and Directory Discovery (supporting analysis) | T1083 |

---

## Investigation Workflow

```
Verify Monitoring
        ↓
Locate certutil.exe
        ↓
Execute Safe Hash Command
        ↓
Generate Process Creation Event
        ↓
Investigate Sysmon
        ↓
Investigate Wazuh
        ↓
Correlate Evidence
        ↓
Validate Activity
        ↓
Document Findings
```

---

## Evidence Collected

- certutil.exe execution
- Process Creation (Sysmon Event ID 1)
- Parent Process
- Command Line
- User Context
- Execution Timestamp
- SHA256 Hash
- Wazuh Process Event
- PowerShell Hash Output

---

## Evidence Correlation

The investigation correlated evidence from four independent sources:

| Evidence Source | Validation |
|----------------|------------|
| Command Prompt | Verified executed command |
| PowerShell | Confirmed SHA256 hash |
| Sysmon | Verified process execution details |
| Wazuh | Confirmed ingestion of endpoint telemetry |

The consistency across these sources validated the accuracy of the investigation timeline and confirmed the legitimacy of the observed activity.

---

## Investigation Findings

- `certutil.exe` executed successfully.
- The command calculated the SHA256 hash of a local executable.
- No download, decoding, or file transfer activity occurred.
- Sysmon recorded the process creation event.
- Wazuh successfully ingested and displayed the telemetry.
- The parent process matched expected user activity.
- No suspicious child processes or persistence mechanisms were identified.
- No additional indicators of compromise were observed.

---

## Key Takeaways

- Legitimate Windows binaries can generate security alerts.
- LOLBins require contextual analysis rather than immediate classification as malicious.
- Process execution telemetry provides valuable forensic evidence.
- Correlating Sysmon and Wazuh improves investigation confidence.
- Evidence validation is an essential DFIR skill.
