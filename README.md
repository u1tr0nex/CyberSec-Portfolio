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

## About Me
Cybersecurity professional with 2+ years in endpoint security
and EDR/XDR at Sophos. Transitioning into SOC Analysis and
Threat Detection. Currently pursuing SC-200 (Microsoft Security
Operations Analyst).

Core Skills: Sophos Central, Intercept X, EDR/XDR, Wireshark,
Splunk, Microsoft Sentinel, KQL, MITRE ATT&CK, Nmap, OSINT

LinkedIn: linkedin.com/in/abhimanyu-rawat-3a4754161
Website: www.socialasto.com
