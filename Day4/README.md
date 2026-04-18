# Day 4 – Operation Alarm: Investigating Real Alerts
Date: April 18, 2026
Analyst: Abhimanyu Singh Rawat
Category: Alert Triage, Log Analysis, Correlation, Incident Response

---

## Tasks Completed

D4-01 – Alert Triage: powershell.exe Spawned by winword.exe

Verdict: True Positive — Investigate Immediately

winword.exe is the executable for Microsoft Word. PowerShell
execution from Word is triggered through VBA (Visual Basic for
Applications) macros. Attackers inject malicious code into macros,
causing Word to silently spawn powershell.exe as a child process
to download payloads or execute remote commands. Legitimate Word
usage rarely requires PowerShell, making this a high-severity
indicator of macro-based exploitation.

MITRE: T1566.001 – Phishing: Spearphishing Attachment
       T1059.001 – Command and Scripting Interpreter: PowerShell

---

D4-02 – Failed Login Analysis

Tool: journalctl (Kali Linux 2026.1)
Command: journalctl | grep -i "failed" | wc -l
Total lines: 345

After filtering kernel PCI hardware noise and VMware boot errors,
7 genuine authentication failures were identified:

Apr 16 01:49 – 2 failed su attempts to root (user: kali, pts/2)
Apr 16 01:49 – 2 failed password checks for root
Apr 17 13:12 – 3 failed password checks for user kali

All failures originated from local terminal sessions, not remote
or SSH sources. No brute-force pattern detected. Assessed as
mistyped passwords during normal use.

Risk Level: Low
MITRE: T1078 – Valid Accounts (monitoring context)

---

D4-03 – Network Connection Analysis

Tools: netstat -ano (Windows), lsof -i (Kali Linux)

Windows (desktop-38u3i3p / 192.168.1.3):
Searched for C2 IP 185.130.5.253 on port 4444.
Result: IP not present in netstat output.
Notable open ports: 445 (SMB), 135 (MSRPC), 902/912 (VMware).
PID 18324 showed multiple HTTPS outbound connections — browser
activity during investigation.

Kali Linux (192.168.153.128):
lsof -i result: Single connection found.
Process: NetworkManager (PID 689)
Connection: bootpc -> VMware DHCP server (192.168.153.254)
Assessment: Normal DHCP lease renewal. Benign.

C2 IP 185.130.5.253 not active on either platform.
Port 445 (SMB) remains open — medium risk noted from Day 2.

MITRE: T1571 – Non-Standard Port (monitoring context)

---

D4-04 – False Positive Analysis: svchost.exe DNS Queries

Verdict: FALSE POSITIVE — Benign

svchost.exe hosts multiple Windows background services via DLLs,
including Dnscache (DNS Client), which resolves hostnames for all
applications. DNS queries from svchost.exe are expected behavior.

However, abnormally high query volumes or queries to known
malicious domains should be escalated as potential DNS tunneling
or C2 beaconing.

Verification Command: tasklist /svc /fi "imagename eq svchost.exe"

MITRE: T1071.004 – Application Layer Protocol: DNS (if malicious)
Current verdict: False Positive — no malicious indicators present.

---

D4-05 – SOC Triage Report: cmd.exe Suspicious Download

Alert ID: SIM-ALERT-2024-001
Severity: High

Alert time        : 02:00 AM
Source process    : cmd.exe
Source host       : desktop-38u3i3p
Destination URL   : hxxp://malware-test[.]com/file.exe
User logged in    : None (system idle)

Investigation Steps:
1. Process verification: cmd.exe ran at 02:00 AM with no user
   session. Verified via Event ID 4688 (process creation log).
2. Network check: Outbound HTTP to malware-test[.]com on port 80.
   Unencrypted traffic. Destination filename is file.exe (dropper).
3. Parent process: No user trigger. Likely scheduled task or
   registry run key from prior compromise.
4. File system: Checked %TEMP%, %APPDATA%, C:\Users\Public for
   file.exe. SHA-256 hash submission to VirusTotal pending.

Evidence Collected:
Process tree      : cmd.exe at 02:00 AM, no user session
Network connection: Outbound HTTP to hxxp://malware-test[.]com/file.exe
File hash         : Pending — file not confirmed on disk

Verdict: TRUE POSITIVE – Malicious activity confirmed

Justification:
cmd.exe downloading a file at 2 AM with no active user session
is not normal behavior. The destination domain is flagged and
targets a direct .exe download consistent with dropper activity.
No user login eliminates accidental execution, pointing to an
automated persistence mechanism from a prior compromise.

Recommended Actions:
1. Isolate endpoint immediately — prevent payload execution
2. Preserve memory dump and disk image for forensic analysis
3. Hunt across all endpoints for off-hours cmd.exe activity
   using EDR telemetry and scheduled task audit

MITRE: T1059.003 – Windows Command Shell
       T1105    – Ingress Tool Transfer
       T1053.005 – Scheduled Task/Job

---

## Key Finding
The correlation of cmd.exe running at 2 AM with no user session
and an outbound connection to a suspicious .exe download URL is
a clear true positive. This scenario demonstrates why time-based
behavioral baselines are critical in SOC operations — normal
processes at abnormal times are a major detection signal.

---

## MITRE ATT&CK Mapping
T1566.001 – Phishing: Spearphishing Attachment
T1059.001 – Command and Scripting Interpreter: PowerShell
T1059.003 – Command and Scripting Interpreter: Windows Command Shell
T1071.004 – Application Layer Protocol: DNS
T1078     – Valid Accounts
T1105     – Ingress Tool Transfer
T1053.005 – Scheduled Task/Job
T1571     – Non-Standard Port

---

## Screenshots
D4-01 – D4-1.docx (winword + PowerShell research)
D4-02 – D4-2.docx (journalctl failed login output)
D4-03 – D4-3.docx (netstat -ano + lsof -i output)
D4-04 – D4-4.docx (svchost.exe services command output)
D4-05 – D4-5.docx (SOC triage report)
