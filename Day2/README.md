# Day 2 – Operation Silent Trace

Date: April 16, 2026
Analyst: Abhimanyu Singh Rawat
Category: Network Recon, OSINT, File Integrity, Incident Response

---

## Tasks Completed

D2-01L – Advanced Digital Footprint (Sherlock)
Ran Sherlock tool on Kali Linux against username "ultronex"
across 100+ platforms.

Command used: sherlock ultronex

Result: 31 accounts found across platforms including:
- GitHub, YouTube, Twitch, Steam, Snapchat, Pinterest
- Pastebin (potential credential dump exposure)
- Cracked Forum (hacking/combolist community)
- HudsonRock (breach database hit)
- BabyRu, Velomania (unexpected/forgotten accounts)

Summary:
Sherlock identified 31 accounts linked to the username
"ultronex" across platforms including GitHub, Steam, Pastebin,
Cracked Forum, and HudsonRock breach database. The Pastebin
and HudsonRock results are most concerning — potential data
leak exposure. Dormant accounts with reused credentials
represent a serious credential stuffing attack surface.

---

D1-02B – Network Port Scan (Nmap)
Performed SYN stealth scan on VMware gateway and Windows host.

Command 1: nmap -sS 192.168.153.2
Target: VMware NAT Gateway
Result: Port 53 (DNS) filtered. 999 ports closed.

Command 2: nmap -sS -Pn 192.168.1.6
Target: desktop-38u3i3p (Windows Host)
Scan Duration: 30 minutes (full SYN scan)

Open Ports Found:
Port 135  – MSRPC        (Windows Remote Procedure Call)
Port 139  – NetBIOS-SSN  (Windows Network File Sharing)
Port 445  – Microsoft-DS (SMB File and Printer Sharing)
Port 902  – VMware       (VMware Authentication Daemon)
Port 912  – VMware       (VMware Remote Console)

Risk Assessment: Medium
SMB ports 135/139/445 open. Port 445 is historically linked
to EternalBlue exploit used in WannaCry ransomware (2017).
Should be firewalled in enterprise environments.

---

D1-03B – File Hash Verification (SHA-256)
Downloaded putty-0.83.tar.gz and verified integrity against
official vendor hash.

Command used: sha256sum putty-0.83.tar.gz

Generated Hash:
718777c13d63d0dff91fe03162bc2a05b4dfc8b0827634cd60b51cefdff631c6

Official Vendor Hash:
718777c13d63d0dff91fe03162bc2a05b4dfc8b0827634cd60b51cefdff631c6

Result: MATCH CONFIRMED
File is authentic and has not been tampered with or corrupted.

---

D1-04B – Domain Reputation Check (WHOIS + URLScan)
Target domain: webcatalog.io

WHOIS Findings:
Creation Date:  May 8, 2021
Registrar:      Cloudflare, Inc.
Nameservers:    clay.ns.cloudflare.com / zelda.ns.cloudflare.com
Expiry:         May 8, 2027
Registrant:     Redacted for privacy (standard practice)

URLScan Findings:
IP:             104.26.12.175 (Cloudflare)
Verdict:        No classification
Google Safe Browsing: Clean
HTTPS:          100%
Scanned:        342 times – no malicious flags

20-Word Risk Assessment:
Domain created 2021, registered via Cloudflare, no malicious
flags on URLScan, Google Safe Browsing clean. Risk level: Low.

---

D1-05B – Timeline-Based Incident Report
Scenario: Employee laptop showing unusual outbound connections
at 3:00 AM to unknown IP while employee was not working.

IOCs Identified:
Destination IP:   185.130.5.253
Destination Port: 4444 (Metasploit default C2 port)
Protocol:         TCP

Key Actions Taken:
- Isolated endpoint from network
- Preserved memory dump and logs
- Blocked IP at firewall
- Reimaged affected laptop

Risk: High – Port 4444 is the default Metasploit reverse
shell listener, indicating likely C2 compromise.

---

D1-06B – SOC Analyst Role-Play Reflection
As a junior SOC analyst, one ethical dilemma I might face
is monitoring employee activity to detect insider threats
versus respecting individual privacy.

Imagine receiving an alert that an employee is uploading
large volumes of data to a personal cloud drive late at
night. My job requires me to investigate — but doing so
means reading through personal files, messages, or browsing
activity that may have no connection to the threat.

I would handle this by strictly following the organization's
Acceptable Use Policy and escalating to my team lead before
accessing any personal data. I would document every action
taken, limit my scope to only the evidence relevant to the
alert, and avoid drawing conclusions beyond what the data
shows.

The gray zone exists between protection and surveillance.
A good analyst operates with transparency, works within
legal boundaries, and remembers that the goal is to protect
— not to spy.

---

## Key Findings

1. Sherlock OSINT revealed 31 accounts under username
   "ultronex" with high-risk hits on Pastebin, Cracked
   Forum, and HudsonRock breach database.

2. Nmap scan of Windows host revealed SMB stack open
   (ports 135/139/445) — historically exploited by
   EternalBlue/WannaCry (CVE-2017-0144).

3. SHA-256 hash verification confirmed PuTTY file
   integrity against official vendor hash.

4. Port 4444 outbound TCP identified as Metasploit
   reverse shell indicator in incident report.

---

## MITRE ATT&CK Mapping

T1595   – Active Scanning (Nmap recon)
T1590   – Gather Victim Network Information
T1566   – Phishing (incident report scenario)
T1571   – Non-Standard Port (port 4444 C2)
T1071   – Application Layer Protocol (C2 communication)
