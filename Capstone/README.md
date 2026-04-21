# Final Capstone – Operation Phoenix Rising
Date: April 21, 2026
Analyst: Abhimanyu Singh Rawat
Category: Full Incident Response Simulation — Investigation,
Containment, Eradication, Recovery, Reporting

---

## Scenario

A mid-sized company (Nexus Solutions) reports employee Alex's
machine is behaving abnormally. Pop-ups appear, files have
strange names, and antivirus cannot remove the threat. The
analyst is tasked with full incident response from detection
to closure.

---

## Tasks Completed

CAP-01 – Alert Log Investigation

Analyzed simulated alert log. Three key findings identified:

Suspicious Process:
powershell.exe (PID 4421) spawned by winword.exe (PID 1288).
A VBA macro in a Word document executed a Base64 encoded
PowerShell IEX downloader command to pull a remote payload
into memory without writing to disk.

Malicious IP:
185.130.5.253 port 4444 — Metasploit default reverse shell
port. PowerShell established outbound C2 connection to this
IP seconds after the macro fired.

Malware — svcboost.exe:
Dropped to C:\Users\Alex\AppData\Roaming\svcboost.exe.
Registry persistence added at:
HKLM\Software\Microsoft\Windows\CurrentVersion\Run\svcboost.
Ransomware popup displayed: "Pay 0.5 BTC to recover."
File encryption confirmed on .docx and .xlsx files.

IP Reputation (VirusTotal): Clean at time of investigation.
Classified malicious based on behavioral context — port 4444
outbound from a PowerShell/Word macro chain is sufficient
evidence regardless of vendor reputation score. This is a
known "reputation gap" with newly stood-up C2 infrastructure.

---

CAP-02 – Containment Steps

Step 1 – Block malicious IP at Windows firewall:
netsh advfirewall firewall add rule name="Block C2 185.130.5.253" ^
dir=out action=block remoteip=185.130.5.253

Step 2 – Kill malicious processes:
taskkill /PID 4421 /F
taskkill /IM svcboost.exe /F

Step 3 – Remove registry persistence:
reg delete "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" ^
/v svcboost /f

Additional: Endpoint physically isolated from network to
prevent lateral movement to other Nexus Solutions systems.

---

CAP-03 – Eradication and Recovery Plan

Verify system is clean:
Run full EDR scan. Check all startup locations (registry run
keys, scheduled tasks, AppData\Roaming). Run netstat -ano to
confirm no active C2 connections. Review PowerShell Event ID
4104 logs for remaining encoded commands. Reimage if any doubt.

Restore files from:
OneDrive/SharePoint version history prior to 2025-04-19 14:23
UTC. If unavailable, check VSS shadow copies (vssadmin list
shadows). Do not pay ransom — payment does not guarantee
decryption and funds future attacks.

Lesson for employee Alex:
Never enable macros in Word or Excel documents received via
email, even from known senders. Legitimate business documents
do not require macros. When in doubt — contact IT before
opening any attachment.

---

CAP-04 – Incident Closure Report

Incident ID: CAP-2025-001
Severity: Critical
Time to resolve (simulated): 2 hours

Executive Summary:
Employee Alex opened a malicious Word document containing a
VBA macro that spawned PowerShell, established C2 to
185.130.5.253:4444, and dropped ransomware (svcboost.exe).
Documents folder files were encrypted and 0.5 BTC ransom was
demanded. Incident was contained by blocking C2, terminating
processes, removing persistence, and isolating the endpoint.
Files restored from clean backup.

IOCs:
Process:      powershell.exe (PID 4421, parent winword.exe 1288)
File:         C:\Users\Alex\AppData\Roaming\svcboost.exe
IP:           185.130.5.253:4444 (TCP outbound C2)
Registry:     HKLM\...\CurrentVersion\Run\svcboost
Command:      powershell.exe -EncodedCommand SQBFAFgA... (IEX)

Recommendations:
1. Disable Office macros via Group Policy — allow only
   digitally signed macros from trusted publishers.
2. Deploy EDR rule to auto-block powershell.exe spawned
   by any Office application.
3. Block port 4444 outbound at network firewall. Enforce
   DNS filtering and run quarterly phishing awareness training.

---

## Key Finding

The full attack chain from phishing document to ransomware
deployment completed in under 60 seconds — macro fired at
14:23:11, files encrypted by 14:24:05. This demonstrates
why endpoint behavioral detection is critical. VirusTotal
showed the C2 IP as clean, highlighting the "reputation gap"
problem — newly stood-up attacker infrastructure evades
reputation-based tools. Behavioral context (port 4444,
PowerShell spawned by Word, dropper + registry persistence)
was sufficient to classify the incident as critical without
any vendor detection.

---

## MITRE ATT&CK Mapping
T1566.001 – Phishing: Spearphishing Attachment
T1059.001 – Command and Scripting Interpreter: PowerShell
T1105    – Ingress Tool Transfer
T1547.001 – Boot or Logon Autostart: Registry Run Keys
T1486    – Data Encrypted for Impact
T1071.001 – Application Layer Protocol: C2 over TCP

---

## Screenshots
CAP-01 – CAP01_research (VirusTotal IP result)
CAP-02 – CAP02_containment (containment commands)
CAP-03 – CAP03_recovery (recovery plan)
CAP-04 – CAP04_report (incident closure report)
