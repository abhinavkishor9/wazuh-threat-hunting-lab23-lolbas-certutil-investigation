# Troubleshooting Notes

## Issue 1 – No Sysmon Event ID 1

### Cause

Sysmon service was not running or process creation logging was unavailable.

### Resolution

Verify:

```powershell
Get-Service Sysmon64
```

Restart if necessary.

---

## Issue 2 – No Wazuh Results

### Cause

Telemetry had not yet been indexed.

### Resolution

- Wait 30–60 seconds.
- Refresh Threat Hunting.
- Expand the time range to "Last 15 Minutes".

---

## Issue 3 – certutil.exe Not Found

### Cause

Incorrect PATH or command syntax.

### Resolution

Verify:

```cmd
where certutil
```

Expected:

```
C:\Windows\System32\certutil.exe
```

---

## Issue 4 – Hash Values Do Not Match

### Cause

Different files were hashed or the file was modified between executions.

### Resolution

Ensure both `certutil` and `Get-FileHash` reference the same file path.

---

## Issue 5 – No Search Results for certutil.exe

### Cause

Search query too restrictive.

### Resolution

Search using:

```
certutil.exe
```

or

```
data.win.system.eventID:1
```

and adjust the time range.

---

## Lessons Learned

- Always verify monitoring services before beginning an investigation.
- Allow sufficient time for Wazuh to ingest endpoint telemetry.
- Validate findings using multiple evidence sources.
- Legitimate LOLBins require contextual investigation before classification.
