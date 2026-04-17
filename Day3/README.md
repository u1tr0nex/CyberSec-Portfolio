# Day 3 – Operation Phantom Hunt
Date: April 17, 2026
Analyst: Abhimanyu Singh Rawat
Category: Persistence Analysis, Malware Investigation, Detection

---

## Tasks Completed

### D3-01 – Persistence Analysis (Startup)
Audited startup locations using Task Manager. 
- **Flagged:** `Update.vbs` (No publisher, high risk for script-based malware).
- **Flagged:** `Program` (Indicates a potentially malicious or broken registry path).

### D3-02 – Process Tree Mapping
Mapped the process hierarchy of **Microsoft Edge**.
- **Observation:** Identified 21 child processes.
- **Analysis:** This multi-process architecture isolates tabs for security, but can be used by hijackers to hide malicious sub-tasks.

### D3-03 – Hidden File Detection
Used `dir /a:h` to audit hidden files in the user directory.
- **Finding:** Identified `NTUSER.DAT` and system log files. No suspicious hidden executables found.

### D3-04 – Alternate Data Streams (ADS)
Created and detected a hidden data stream.
- **Command:** `dir /r` revealed `test.txt:hidden.txt:$DATA`.
- **Note:** Also observed `Zone.Identifier` on downloaded PDFs, confirming how Windows tracks web-sourced files.

### D3-05 – Malware Behavior Report
Completed a formal incident report on a **Browser Hijacker** scenario. Rated **Medium** severity due to AV evasion and persistent system changes.

---

## Key Finding
The discovery of `Update.vbs` in the startup list is a classic indicator of a persistence mechanism. Attackers use these scripts because they are often overlooked by basic security scans.

---

## MITRE ATT&CK Mapping
- **T1547.001** – Boot or Logon Autostart Execution (Registry Run Keys/Startup Folder)
- **T1564.004** – Hide Artifacts: Alternate Data Streams
- **T1059.005** – Command and Scripting Interpreter: Visual Basic
