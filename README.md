# Week 2 Project: Footprinting, OSINT & Network Scanning (Networkwalks)

Reconnaissance and scanning engagement against networkwalks.com, completed as part of the Networkwalks Cybersecurity and Ethical Hacking internship, Week 2. Covers three modules: footprinting with six Kali Linux tools (W2-PM1), OSINT based subdomain enumeration with theHarvester (W2-PM4), and network scanning with Zenmap (W2-PM5), written up as a full penetration testing style report (W2-PM-FINAL).

## Project Overview

This engagement simulates the first two phases of a real penetration test: reconnaissance and scanning. All testing was performed against networkwalks.com with written permission already secured as part of the internship's engagement scope, the goal being to see how much a target's infrastructure reveals before any active exploitation is attempted.

## Modules Completed

| Module | Focus |
|---|---|
| W2-PM1 | Footprinting with 6 Kali tools (WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon) |
| W2-PM4 | OSINT subdomain/host enumeration with theHarvester |
| W2-PM5 | Network scanning with Zenmap (Quick Scan + Intense Scan) |
| W2-PM-FINAL | Full penetration testing report covering all three modules above |

## Key Findings

- **Expired SSL certificate.** Zenmap's intense scan pulled the certificate presented by the target and it had already expired, valid only through 21 December 2025. This was the one confirmed, fixable defect in the whole assessment, everything else below is information exposure, not a vulnerability.
- **Database port open to the internet.** Port 3306 (MySQL) was reachable directly from outside the network, running MySQL 5.7.44-48. Database services generally shouldn't be internet facing at all.
- **17 subdomains enumerated via OSINT alone.** theHarvester pulled cpanel, webdisk, webmail, autodiscover, and several other admin/mail subdomains from public sources, without ever touching the target's network directly.
- **Full service fingerprint from a single scan.** Zenmap's intense scan identified the exact software and version behind every open port (Apache, Dovecot, Exim, ISC BIND, Pure-FTPd, OpenSSH), enough to go hunting for known CVEs against each one.

## Tools Used

- WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon (Kali Linux)
- [theHarvester](https://github.com/laramies/theHarvester)
- [Zenmap](https://nmap.org/zenmap/) (Nmap GUI)

## Report

The full penetration testing report (Liability Disclaimer, Introduction, Tools Used, Activities Performed, Risk Analysis, Recommendations, Conclusion, Evidence) is viewable directly here: [REPORT.md](./REPORT.md).

A Word version is also included for submission purposes: [W2_Final_Report_Tunde_Oduola.docx](./W2_Final_Report_Tunde_Oduola.docx) (this one downloads rather than previews, since GitHub doesn't render .docx inline).

## Author

Tunde Oduola
[LinkedIn](https://www.linkedin.com/in/tunde-oduola-2855a5239)

## Project Information

- **Internship/Program:** Networkwalks, Cybersecurity and Ethical Hacking Class
- **Week:** 2
- **Project:** Footprinting, OSINT & Network Scanning (W2-PM1, W2-PM4, W2-PM5, W2-PM-FINAL)
- **Organization:** Networkwalks
