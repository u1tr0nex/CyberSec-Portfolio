# Security Portfolio
Analyst: Abhimanyu Singh Rawat
Program: GraySentinel Cyber Training
Goal: Hands-on cybersecurity portfolio documenting daily missions,
investigations, and security analysis.

Contact: mannurawat2010@gmail.com
LinkedIn: linkedin.com/in/abhimanyu-rawat-3a4754161

---

## Progress Tracker

| Day | Topic                                          | Status |
|-----|------------------------------------------------|--------|
| Day 1 | Phishing Analysis, OSINT, Digital Footprint | Done   |
| Day 2 | Network Recon, OSINT, File Integrity, IOCs  | Done   |
| Day 3 | Persistence, Process Trees, Hidden Files, ADS | Done |
| Day 4 | Alert Triage, Log Analysis, Correlation, IR        | Done   |
| Day 5 | Vulnerability Scanning, Password Analysis, Recon, Pentest | Done |
| Capstone | Full IR Simulation — Nexus Solutions Ransomware | Done |

---

## Day 1 – Operation First Contact

Category: Phishing Analysis, OSINT, Network Recon

Tasks Completed:
- D1-01B: Social Media Privacy Check
- D1-02B: Public and Private IP Discovery
- D1-03B: File Extension Double Extension Test (invoice.pdf.exe)
- D1-04B: VirusTotal Website Safety Check
- D1-05B: Phishing Email Analysis – Coinbase Impersonation
- D1-05: Incident Report – Phishing Click Simulation
- D1-06B: Digital Footprint Reflection

Key Finding:
Analyzed a real phishing email impersonating Coinbase from
esev9586@esev.ipv.pt — identified spoofed domain, financial
lure tactic ($36,824.44 withdrawal), and known spam
infrastructure (ESEVIPV.onmicrosoft.com).
Mapped to MITRE ATT&CK T1566.

---

## Day 2 – Operation Silent Trace

Category: Network Recon, OSINT, File Integrity, Incident Response

Tasks Completed:
- D2-01L: Advanced Digital Footprint – Sherlock scan on
  username "ultronex" across 31 platforms including GitHub,
  Pastebin, Cracked Forum, and HudsonRock breach database
- D1-02B: Network Port Scan – Nmap SYN scan on VMware gateway
  (192.168.153.2) and Windows host (192.168.1.6)
- D1-03B: File Hash Verification – SHA-256 hash of putty-0.83.tar.gz
  matched official vendor hash, confirming file integrity
- D1-04B: Domain Reputation Check – webcatalog.io analyzed via
  WHOIS and URLScan. No malicious flags. Risk: Low
- D1-05B: Timeline-Based Incident Report – Suspicious outbound
  C2 connection to 185.130.5.253 port 4444 at 3:00 AM
- D1-06B: SOC Analyst Role-Play Reflection – Ethical dilemma
  of user monitoring vs privacy

Key Findings:

Sherlock OSINT:
31 accounts found under username "ultronex". High-risk hits
on Pastebin, Cracked Forum, and HudsonRock breach database.
Indicates significant credential stuffing attack surface.

Nmap Windows Host Scan (192.168.1.6):
Ports 135, 139, 445 (SMB stack) open — historically exploited
by EternalBlue/WannaCry (CVE-2017-0144).
Ports 902, 912 confirm VMware Workstation running.
Risk Assessment: Medium. SMB should be firewalled in
enterprise environments.

File Integrity:
SHA-256: 718777c13d63d0dff91fe03162bc2a05b4dfc8b0827634cd60b51cefdff631c6
Verified against official PuTTY vendor hash. Match confirmed.
File authentic and untampered.

Incident Response:
Identified port 4444 outbound TCP as Metasploit reverse shell
indicator (C2 activity). Containment steps executed.
Mapped to MITRE ATT&CK T1571 (Non-Standard Port).

---

## Day 3 – Operation Phantom Hunt

Category: Persistence Analysis, Malware Investigation, Detection

Tasks Completed:
- D3-01: Persistence Analysis (Startup) – Audited system startup
  locations using Task Manager to identify auto-run programs
- D3-02: Process Tree Mapping – Analyzed parent-child relationships
  in msedge.exe (21 child processes) to understand modern browser
  architecture and isolation
- D3-03: Hidden File Detection – Used dir /a:h to audit hidden
  system files and scan for suspicious naming patterns like
  double extensions or triple dots
- D3-04: Alternate Data Streams (ADS) – Successfully created and
  detected a hidden data stream (test.txt:hidden.txt) and identified
  Zone.Identifier tags on downloaded files
- D3-05: Malware Behavior Report – Authored a formal incident report
  for a Medium-severity browser hijack simulation, documenting
  IOCs and containment steps
- D3-06: Persistence Reflection – Evaluated why attackers utilize
  registry run keys and startup folders to maintain long-term
  access to compromised systems

Key Findings:

Persistence Hunting:
Identified Update.vbs and a generic "Program" entry in the startup
list. As legitimate software rarely uses VBScripts as direct startup
entries, this was flagged as a high-risk persistence mechanism
often used for silent execution.

Forensic Detection:
Successfully uncovered Alternate Data Streams (ADS) using dir /r.
Verified that metadata like Zone.Identifier is used by Windows to
track file origins, but can be abused by attackers to hide malicious
payloads within legitimate-looking files.

Incident Response:
The simulation of a browser hijack highlighted the necessity of
monitoring non-standard startup entries and hidden attributes when
antivirus (AV) fails to trigger.
Mapped to MITRE ATT&CK T1547.001 (Boot or Logon Autostart Execution).

---

## Day 4 – Operation Alarm: Investigating Real Alerts

Category: Alert Triage, Log Analysis, Correlation, Incident Response

Tasks Completed:
- D4-01: Alert triage — powershell.exe spawned by winword.exe
  confirmed as true positive via VBA macro research
- D4-02: Failed login analysis — journalctl on Kali, 7 real
  auth failures found from local terminal, risk low
- D4-03: Network correlation — netstat + lsof cross-platform
  check, C2 IP 185.130.5.253 not active, SMB noted
- D4-04: False positive analysis — svchost.exe DNS queries
  confirmed benign via Dnscache service verification
- D4-05: SOC triage report — cmd.exe download at 2 AM,
  no user session, true positive, endpoint isolation recommended

Key Finding:

Two critical discoveries stand out across Day 4. First, the
correlation of cmd.exe running at 2 AM with no active user
session and an outbound connection to a .exe download URL
confirmed a true positive — demonstrating why time-based
behavioral baselining is essential in SOC operations.
Second, a real authentication failure pattern was found in
Kali Linux logs — 4 failed su/password attempts across two
days targeting both root and kali accounts, discovered using
journalctl after legacy tools like lastb were unavailable
in Kali 2026.1.
MITRE: T1105 – Ingress Tool Transfer, T1078 – Valid Accounts.

---

## Day 5 – Operation Phantom Entry: Thinking Like an Attacker

Category: Vulnerability Scanning, Password Analysis,
Phishing Simulation, Reconnaissance, Penetration Testing

Tasks Completed:
- D5-01: Localhost vulnerability scan — nmap -sV 127.0.0.1
  on Kali Linux. All 1000 ports closed. Clean and hardened
  configuration confirmed. CVE-2023-38408 (OpenSSH RCE)
  documented as simulated service reference.
- D5-02: Password strength analysis — tested password123,
  administrator, admin (all pwned) vs Youngdumbroke@6497
  (not pwned) on haveibeenpwned.com/Passwords. Confirmed
  that weak default credentials appear in millions of
  credential dumps and are prime targets for spraying attacks.
- D5-03: Canarytoken creation — generated URL token on
  canarytokens.com and embedded in fake invoice document.
  Analyzed how attackers use tracking tokens to silently
  harvest victim IP, browser, and OS data before deploying
  a real payload.
- D5-04: OSINT reconnaissance — Google dorks against fictional
  target "Socialasto" to simulate attacker recon. Identified
  3 attack-ready data points: employee names via LinkedIn,
  credential exposure via paste sites, internal infrastructure
  via public PDFs.
- D5-05: Penetration test summary report — documented open
  SSH port 22 with admin:admin credentials (Critical) and
  Apache 2.4.29 with CVE-2021-41773 RCE vulnerability (High).
  Full remediation timeline provided.

Key Finding:
Google dork reconnaissance against a fictional target
demonstrated that an attacker can map employee names,
exposed credentials, and internal infrastructure without
sending a single packet to the target — zero footprint,
full intel. Combined with the password analysis showing
"admin" appearing in millions of breach dumps, this
illustrates why default credentials and poor OSINT hygiene
represent the lowest-effort, highest-impact attack surface.
MITRE: T1590 – Gather Victim Network Information,
T1593 – Search Open Websites/Domains,
T1110.001 – Brute Force: Password Guessing.

---

## Final Capstone – Operation Phoenix Rising

Category: Full Incident Response — Investigation, Containment,
Eradication, Recovery, Reporting

Scenario:
Employee Alex at Nexus Solutions opened a malicious Word document.
A VBA macro spawned PowerShell, connected to C2 at
185.130.5.253:4444, dropped svcboost.exe, established registry
persistence, and encrypted Documents files with a 0.5 BTC ransom
demand — all within 60 seconds.

Tasks Completed:
- CAP-01: Alert log analysis — identified powershell.exe spawned
  by winword.exe, C2 IP 185.130.5.253:4444, and ransomware
  dropper svcboost.exe. VirusTotal showed clean — classified
  malicious via behavioral context (reputation gap noted).
- CAP-02: Containment — blocked C2 IP via netsh firewall rule,
  force-killed powershell.exe and svcboost.exe, deleted registry
  run key, isolated endpoint from network.
- CAP-03: Recovery — full EDR scan, VSS shadow copy check,
  file restoration from OneDrive version history, endpoint
  reimaged, macro awareness training for employee Alex.
- CAP-04: Incident Closure Report — full timeline, IOC table,
  containment and eradication steps, 3 prevention recommendations.

Key Finding:
The complete attack chain from macro execution to file encryption
took under 60 seconds, demonstrating why behavioral EDR detection
is more reliable than reputation-based tools. The C2 IP appeared
clean on VirusTotal — a real-world "reputation gap" scenario where
analyst judgment and behavioral context determined the verdict.
MITRE: T1566.001, T1059.001, T1105, T1547.001, T1486

## About Me
Cybersecurity professional with 2+ years in endpoint security
and EDR/XDR at Sophos. Transitioning into SOC Analysis and
Threat Detection. Currently pursuing SC-200 (Microsoft Security
Operations Analyst).

Core Skills: Sophos Central, Intercept X, EDR/XDR, Wireshark,
Splunk, Microsoft Sentinel, KQL, MITRE ATT&CK, Nmap, OSINT

LinkedIn: linkedin.com/in/abhimanyu-rawat-3a4754161
