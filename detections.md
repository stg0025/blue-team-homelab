# SPL Detection Queries

All queries use index=windows_logs and EventCode 4688 from Windows Security logs with command-line auditing enabled.

---

## T1003.002 - Credential Dumping
```
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
New_Process_Name=*reg.exe*
(Process_Command_Line=*save* AND (Process_Command_Line=*hklm\\sam* OR Process_Command_Line=*hklm\\system*))
| table _time, ComputerName, SubjectUserName, New_Process_Name, Process_Command_Line
```

---

## T1053.005 - Scheduled Task Persistence
```
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
New_Process_Name=*schtasks.exe*
Process_Command_Line=*/create*
| table _time, ComputerName, SubjectUserName, New_Process_Name, Process_Command_Line
```

---

## T1059.001 - PowerShell Encoded Command Execution
```
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
New_Process_Name=*powershell.exe*
(Process_Command_Line=*EncodedCommand* OR Process_Command_Line=*-enc *)
| table _time, ComputerName, SubjectUserName, New_Process_Name, Process_Command_Line
```

---

## T1087.001 - Local Account Discovery
```
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
(New_Process_Name=*net.exe* OR New_Process_Name=*net1.exe*)
Process_Command_Line=*user*
| table _time, ComputerName, SubjectUserName, New_Process_Name, Process_Command_Line
```
