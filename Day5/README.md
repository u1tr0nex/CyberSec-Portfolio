# Day 5 – Operation Phantom Entry: Thinking Like an Attacker
Date: April 19, 2026
Analyst: Abhimanyu Singh Rawat
Category: Vulnerability Scanning, Password Analysis,
Phishing Simulation, Reconnaissance, Penetration Testing

---

## Tasks Completed

D5-01 – Localhost Vulnerability Scan

Tool: Nmap 7.98
Command: nmap -sV 127.0.0.1
Target: localhost (127.0.0.1) — Kali Linux 2026.1

Result:
All 1000 scanned TCP ports are in closed/reset state.
No open ports or running services detected on localhost.

Analysis:
A clean localhost scan confirms minimal attack surface on
this Kali VM. In a real enterprise environment, open ports
would require immediate service version research and CVE
lookup before any further action.

Risk Level: Low — clean and hardened configuration confirmed.
MITRE: T1046 – Network Service Discovery

---

D5-02 – Password Strength Analysis

Tool: haveibeenpwned.com/Passwords
Method: SHA-1 k-anonymity (password never sent in full)

Passwords Tested:

1. password123        – PWNED (found in breach databases)
2. administrator      – PWNED (found in breach databases)
3. admin              – PWNED (found in breach databases)
4. Youngdumbroke@6497 – NOT PWNED (no breach records found)

Conclusion:
Weak passwords like "admin" and "password123" appear in
millions of credential dumps and are the first entries
tried in any brute force or credential stuffing attack.
A strong password combining uppercase, lowercase, numbers,
and special characters — such as Youngdumbroke@6497 — has
no recorded breach exposure and resists dictionary and
brute force attacks.

MITRE: T1110.003 – Brute Force: Password Spraying
       T1110.001 – Brute Force: Password Guessing

---

D5-03 – Canarytoken: Attacker Tracking Simulation

Tool: canarytokens.com
Token Type: URL token
Lure used: "Click here to view your $10,000 invoice."

Token embedded in a fake invoice document. When the link
is clicked, the attacker silently receives the victim's
IP address and device information — without any malware
being installed on the target machine.

How attackers abuse this:
Attackers embed Canarytoken-style tracking URLs inside
fake invoice PDFs or phishing emails. When the target
opens the document or clicks the link, the attacker
receives the victim's IP address and device information
silently — no malware required. Used for initial recon
to confirm an active target before deploying a real payload.

MITRE: T1598 – Phishing for Information

---

D5-04 – Reconnaissance: Google Dork Simulation

Fictional Target: Socialasto

Google Dorks Used:
1. site:linkedin.com "Socialasto"
2. "Socialasto" password
3. "Socialasto" filetype:pdf

3 Pieces of Information Useful to an Attacker:

1. Employee names and job titles via LinkedIn
   Enables targeted spearphishing by identifying the IT
   admin, CEO, or finance team by name for personalized
   lure emails.

2. Password leaks from forums and Pastebin
   Searching "Socialasto" + "password" may surface
   credential dumps from prior breaches — ready for
   direct credential stuffing attacks.

3. Public documents revealing internal infrastructure
   PDFs like reports or technical docs may expose internal
   software names, email formats, vendor names, or server
   addresses useful for building a targeted attack.

MITRE: T1590 – Gather Victim Network Information
       T1591 – Gather Victim Org Information
       T1593 – Search Open Websites/Domains

---

D5-05 – Penetration Test Summary Report

Client: Small Company (fictional)
Scope: External network assessment (authorized)
Date: April 19, 2026

Finding 1 – Weak SSH Credentials
Port:             22
Service:          OpenSSH
Credential found: admin:admin
Severity:         Critical
Impact:           Full system compromise possible. Attacker
                  gains full shell access and can install
                  malware, exfiltrate data, or pivot internally.
Fix:              Enforce strong passwords. Disable root login.
                  Implement SSH key-based authentication.

Finding 2 – Outdated Web Server
Port:    80
Service: Apache 2.4.29
CVE:     CVE-2021-41773 – Path Traversal and RCE
CVSS:    9.8 (Critical)
Severity: High
Impact:  Unauthenticated attacker can traverse directories,
         read arbitrary files, and execute remote commands
         on the server without any credentials required.
Fix:     Update Apache to latest stable version immediately.
         Apply all OS and web server patches regularly.

Overall Risk Level: Critical

Both findings together create a severe attack chain — an
attacker can identify the Apache version via recon, then
use SSH with default credentials to gain full shell access.
No advanced exploitation required.

Remediation Timeline:
Weak SSH credentials      – Fix within 24 hours
Outdated web server CVE   – Fix within 1 week

MITRE: T1110.001 – Brute Force: Password Guessing
       T1190    – Exploit Public-Facing Application
       T1021.004 – Remote Services: SSH

---

## Key Finding
Google dork reconnaissance against fictional target Socialasto
demonstrated that an attacker can map employee names, exposed
credentials, and internal infrastructure documents without
sending a single packet — zero footprint, full intel. Combined
with password analysis confirming "admin" exists in millions
of breach dumps, this illustrates why default credentials and
poor OSINT hygiene represent the lowest-effort, highest-impact
attack surface for any threat actor.
MITRE: T1590, T1593, T1110.001

---

## MITRE ATT&CK Mapping
T1046    – Network Service Discovery
T1110.001 – Brute Force: Password Guessing
T1110.003 – Brute Force: Password Spraying
T1598    – Phishing for Information
T1590    – Gather Victim Network Information
T1591    – Gather Victim Org Information
T1593    – Search Open Websites/Domains
T1190    – Exploit Public-Facing Application
T1021.004 – Remote Services: SSH

---

## Documents
D5-01 – D5-01.docx (nmap -sV 127.0.0.1 output)
D5-02 – D5-02.docx (haveibeenpwned password results)
D5-03 – D5-03.docx (Canarytoken creation + fake invoice)
D5-04 – D5-04.docx (Google dork search results)
D5-05 – D5-05.docx (pentest summary report)
