# Day 1 – Operation First Contact

Date: April 15, 2026
Analyst: Abhimanyu Singh Rawat
Category: Phishing Analysis, OSINT, Network Recon

---

## Tasks Completed

D1-01B – Social Media Privacy Check
Opened Instagram profile in incognito mode. Identified 3
things visible to a stranger:
- Profile bio and photo
- Public posts and reels
- Followers and following count

D1-02B – Public and Private IP Discovery
Used whatismyip.com for public IP and ipconfig on Windows
for private IP. Documented both and labeled accordingly.

D1-03B – File Extension Double Extension Test
Created a fake file named invoice.pdf.exe. Enabled file
extensions in Windows Explorer to reveal the double
extension trick used by attackers to disguise malware
as legitimate documents.

D1-04B – VirusTotal Website Safety Check
Submitted a news website URL to virustotal.com.
Result: No malicious detections. Green across all vendors.
Risk Level: Low.

D1-05B – Phishing Email Analysis
Analyzed a real phishing email from junk folder
impersonating Coinbase.

Sender: esev9586@esev.ipv.pt

3 Red Flags Identified:
1. Invalid sender domain – no connection to coinbase.com
2. Too-good-to-be-true financial lure – $36,824.44
   ready to withdraw
3. Known spam infrastructure – ESEVIPV.onmicrosoft.com
   previously flagged as spam

D1-05 – Incident Report – Phishing Click Simulation
Completed full incident report based on the Coinbase
phishing email. Documented initial actions, evidence
collected, containment steps, and recommendations.

D1-06B – Digital Footprint Reflection
The most surprising discovery was how much of my digital
footprint is publicly visible without logging in — profile
picture, bio, follower count, and recent posts on social
media are accessible to anyone. Even basic OSINT requires
zero authentication. This reinforced that privacy settings
are the first line of personal defense.

---

## Key Finding

Analyzed a real phishing email impersonating Coinbase from
esev9586@esev.ipv.pt. Identified spoofed domain, financial
lure tactic ($36,824.44 withdrawal), and known spam
infrastructure (ESEVIPV.onmicrosoft.com).

---

## MITRE ATT&CK Mapping

T1566   – Phishing
T1598   – Phishing for Information
T1204   – User Execution (clicking malicious link)
