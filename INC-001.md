# Incident Report — INC-001

| Field | Details |
|---|---|
| Incident ID | INC-001 |
| Date/Time Detected | 2026-03-08 |
| Severity | Medium |
| Alert Name | PowerShell Encoded Command Detected |
| Affected Host | DESKTOP-UTEILEI |
| Affected User | endpointuser |
| Detection Source | Splunk saved alert, Windows Security Log EventCode 4688 |
| MITRE Technique | T1059.001 - Command and Scripting Interpreter: PowerShell |
| Determination | True Positive - activity was intentionally generated via Atomic Red Team test T1059.001-17 for lab validation purposes |
| Analyst | Sam Germanson |

---

## Evidence

**Detection Query:**
```
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
New_Process_Name=*powershell.exe*
(Process_Command_Line=*EncodedCommand* OR Process_Command_Line=*-enc *)
```

**Result:** 1 event returned showing powershell.exe launched with -EncodedCommand parameter on host DESKTOP-UTEILEI under account endpointuser.

---

## Timeline

| Time | Event |
|---|---|
| 2026-03-08 | Atomic Red Team test T1059.001-17 executed on DESKTOP-UTEILEI |
| 2026-03-08 | powershell.exe launched with -EncodedCommand parameter |
| 2026-03-08 | Encoded command decoded to harmless Write-Host execution |
| 2026-03-08 | Process exited cleanly with exit code 0 |
| 2026-03-08 | Splunk alert fired on next scheduled run |

---

## Analysis

The alert fired on a PowerShell process creation event where the command line included the -EncodedCommand parameter. This parameter is used to pass base64-encoded commands to PowerShell and is rarely used by legitimate administrative scripts. It is frequently abused by attackers to obfuscate malicious payloads and evade signature-based detection.

In this case the parent process was the Atomic Red Team test framework running under the endpointuser account during authorized lab testing. The encoded command decoded to a benign Write-Host statement with no malicious payload.

In a production environment this alert would require immediate triage. Key questions: what account ran it, what was the parent process, what did the encoded command decode to, and has this host generated other alerts recently.

---

## Recommendation

- Decode and inspect any base64 payload on future hits before closing
- Review parent process — unexpected parents such as Office applications or browser processes indicate higher severity
- If parent process is legitimate admin tooling and user is known, document and close as false positive
- If parent process is unexpected or user has no administrative function, escalate to Tier 2
- Consider tuning the rule to exclude known admin scripts by adding exceptions for specific account names or parent process paths

---

## Disposition

Closed — True Positive, authorized lab simulation. No further action required.
