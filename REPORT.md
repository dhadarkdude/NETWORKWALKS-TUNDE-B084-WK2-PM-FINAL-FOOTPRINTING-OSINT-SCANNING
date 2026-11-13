# PENETRATION TESTING REPORT
### Footprinting, OSINT & Network Scanning Phases
**W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

| | |
|---|---|
| **Pentester Name (Cybersecurity Professional)** | Tunde Oduola |
| **Program/Batch** | B084-Networkwalks |
| **Date** | 13 November 2026 |
| **Modules Completed** | W2-PM1 (Multiple Kali Tools), W2-PM4 (theHarvester), W2-PM5 (Zenmap Scanning) |
| **Client/Target** | Networkwalks (networkwalks.com) — secured written permission already |
| **Permission secured from client?** | Yes |
| **Phases covered** | Phase 1: Reconnaissance & Footprinting · Phase 2: Scanning & Network Discovery · Phase 3-5: In Progress |

---

## 1. Liability Disclaimer

I have performed these activities only on the systems and devices where I had secured written permission, or the devices/systems that I own myself. All materials in this report are for education and research purposes only. Do not use anything from here to break the law. The instructor, the authors, and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 2. Introduction

This report covers three Week 2 practical modules of my Cybersecurity and Ethical Hacking internship at Networkwalks: footprinting the networkwalks.com domain using six Kali Linux tools (W2-PM1), OSINT based subdomain and host enumeration using theHarvester (W2-PM4), and network scanning the same domain using Zenmap (W2-PM5). Together these modules show how an attacker moves from gathering public information, to aggregating OSINT across public sources, to actively mapping live services on the target host.

All activities were performed in Kali Linux against networkwalks.com, with written permission already secured as part of the internship's engagement scope. Every step below includes the exact command used, the result observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Operating system used for all reconnaissance, OSINT, and scanning activities |
| WHOIS | Find domain registration details (registrar, name servers, DNSSEC status) |
| WhatWeb | Fingerprint web technologies (server, CMS, plugins, IP) |
| Nslookup | Resolve the domain name to its IP address using DNS |
| Curl -I | Read the HTTP response headers of the website |
| Wafw00f | Detect whether a Web Application Firewall protects the site |
| DNSRecon | Enumerate DNS records (SOA, NS, MX, TXT/SPF, SRV) |
| theHarvester | OSINT aggregation across public sources to enumerate subdomains, hosts, emails, and IPs |
| Zenmap (Nmap GUI) | Scan the target host to identify open ports, running services, versions, OS, and network topology |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance (W2-PM1)

I performed reconnaissance against the networkwalks.com domain using six Kali Linux tools: WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon. Each tool was used to collect a different type of information about the target.

I first used **WHOIS** to obtain publicly available domain registration information. The result identified the domain's name servers (NS6135/NS6136.HOSTGATOR.COM and NS29/NS30.DOMAINCONTROL.COM), confirmed DNSSEC is unsigned, and showed the registrar's contact resolution routed through GoDaddy.

![WHOIS lookup](whois%20networkwalks.png)
*Figure 1: WHOIS lookup showing name servers and domain registration details.*

I then used **WhatWeb** to identify technologies used by the website. The results identified Apache as the web server, WordPress 7.1.4, and WP Download Manager 3.3.76, along with the resolved IP 192.232.216.135, country (United States), and the site title "Networkwalks Academy".

![WhatWeb results](whatweb%20result.png)
*Figure 2: WhatWeb fingerprinting results showing CMS, plugin versions, and server details.*

Using **Nslookup**, I resolved the domain name to its IP address. The result confirmed networkwalks.com resolves to 192.232.216.135.

![Nslookup result](nslookup%20result.png)
*Figure 3: Nslookup resolving networkwalks.com to its IP address.*

I used **Curl** with the `-I` option to inspect the HTTP response headers. This returned a 200 OK response and exposed the WordPress REST API endpoint (`/wp-json/`), caching headers (`X-Endurance-Cache-Level`, `X-nginx-cache`), and a Permissions-Policy header, along with a session cookie being set.

![Curl headers](curl%20result.png)
*Figure 4: HTTP response headers returned by curl -I, exposing the WordPress REST API link and caching metadata.*

Next, I used **Wafw00f** to determine whether a Web Application Firewall was protecting the website. The result identified ModSecurity (SpiderLabs) as the active WAF.

![Wafw00f result](wafw00f%20result.png)
*Figure 5: Wafw00f identifying ModSecurity (SpiderLabs) as the site's WAF.*

Finally, I used **DNSRecon** to enumerate DNS records. The results identified the SOA and NS records, the MX record, an SPF record, a Google site-verification TXT record, and eight SRV records routing Outlook autodiscovery to cpanelemaildiscovery.cpanel.net across multiple IPs.

![DNSRecon result](dnsrecon%20result.png)
*Figure 6: DNSRecon output showing SOA, NS, MX, TXT/SPF, and SRV records for networkwalks.com.*

### 4.2 OSINT & Subdomain Enumeration with theHarvester (W2-PM4)

For this module, I ran theHarvester against networkwalks.com, aggregating results across multiple public OSINT sources (certspotter, crtsh, dnsdumpster, duckduckgo, hackertarget, otx, rapiddns, sublist3r, threatcrowd, urlscan, waybackarchive, and yahoo) with a result limit of 500.

![theHarvester command](theHarvester%20command.png)
*Figure 7: theHarvester launched against networkwalks.com across multiple OSINT sources.*

The scan found 1 IP address (192.232.216.135), no public emails, and no people, but enumerated 17 distinct hosts and subdomains, including autodiscover, cpanel, cpcalendars, cpcontacts, mail, webdisk, and webmail, each resolving to the same hosting IP or its associated IPv6 address.

![theHarvester summary](theHarvester%20result1.png)
*Figure 8: theHarvester summary showing IPs found, and no emails or people found.*

![theHarvester hosts found](theHarvester%20result.png)
*Figure 9: theHarvester's full list of 17 enumerated hosts and subdomains for networkwalks.com.*

### 4.3 Network Scanning with Zenmap (W2-PM5)

For the scanning module, I used Zenmap to scan networkwalks.com directly, since written permission for footprinting and network scanning against this domain had already been secured as part of the internship's engagement scope.

I first ran a **Quick Scan**, which completed in under 17 seconds and identified 13 open TCP ports: 21 (ftp), 22 (ssh), 26 (rsftp), 53 (domain), 80 (http), 110 (pop3), 143 (imap), 443 (https), 465 (smtps), 993 (imaps), 995 (pop3s), 3306 (mysql), and 2222 (ssh, running OpenSSH 9.9).

![Zenmap quick scan](Zenmap%20quickscan.png)
*Figure 10: Zenmap Quick Scan output listing open ports on networkwalks.com.*

![Quick scan ports](Zenmap%20quickscan%20port.png)
*Figure 11: Quick Scan Ports/Hosts tab showing all 13 open ports and their services.*

![Quick scan host details](Zenmap%20quickscan%20host%20details.png)
*Figure 12: Quick Scan Host Details showing 13 open, 88 filtered, and 0 closed ports.*

I then ran an **Intense Scan** to gather service versions, OS detection, and a traceroute. This identified specific service versions behind each open port, including Pure-FTPd (ftp), OpenSSH 9.9 (ssh), Exim smtpd 4.100.1 (smtp), ISC BIND 9.16.23 on RedHat Linux (domain/DNS), Apache httpd (http/https), Dovecot (pop3/imap), and MySQL 5.7.44-48 (3306). Ports 25, 514, and 587 were filtered rather than open.

![Zenmap intense scan](Zenmap%20intense%20scan.png)
*Figure 13: Zenmap Intense Scan output showing OS guesses, network distance, and traceroute.*

![Intense scan ports](Zenmap%20port%20result.png)
*Figure 14: Intense Scan Ports/Hosts tab with full service and version detection.*

![Intense scan service detail](Zenmap%20open%20ports%20example.png)
*Figure 15: Intense Scan service detection detail for the identified open ports.*

![Intense scan host details](Zenmap%20host%20details.png)
*Figure 16: Intense Scan Host Details showing the full port breakdown (13 open, 3 filtered, 984 closed).*

The Intense Scan's SSL certificate check (run against the mail/IMAP services) also revealed that the certificate presented for `*.networkwalks.com` (and its SAN entries) was valid only from 22 September 2025 to 21 December 2025, meaning **it had already expired** at the time of this assessment. OS fingerprinting returned no confident exact match, with the scan noting "test conditions non-ideal", and a low-confidence guess of an Actiontec router device, which is not treated as reliable given the low accuracy score.

![OS details](Zenmap%20OS%20details.png)
*Figure 17: Intense Scan Host Details OS section, showing a low-confidence OS guess.*

Finally, I opened the **Topology** tab to visualize the network path to the target. The topology confirmed a 2-hop distance from my Kali VM (via its local gateway at 10.0.0.2) to the networkwalks.com host at 192.232.216.135.

![Zenmap topology](Zenmap%20topology.png)
*Figure 18: Zenmap Topology view showing the 2-hop path from the local host to networkwalks.com.*

## 5. Risk Analysis / Impact

Based on the information collected during the footprinting, OSINT, and network scanning activities, I identified the following potential risks.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Expired SSL/TLS certificate | Nmap's SSL script (intense scan) showed the certificate for *.networkwalks.com valid only from 22 Sep 2025 to 21 Dec 2025, well past its expiry at the time of this assessment | Browsers will flag the site as insecure, breaking trust in encrypted traffic and potentially the WordPress REST API and mail services relying on the same certificate | 🔴 Critical |
| 2 | Database port (MySQL, 3306) open to the internet | Zenmap identified port 3306/tcp open, running MySQL 5.7.44-48, directly reachable from outside the network | Exposes the database service to internet-wide scanning and brute force or exploitation attempts against a known MySQL version | 🟠 High |
| 3 | Multiple mail and admin subdomains enumerated | theHarvester found 17 hosts including cpanel., webdisk., webmail., cpcalendars., cpcontacts., and autodiscover.networkwalks.com | Publicly discoverable admin/control panel subdomains widen the attack surface available to an attacker during reconnaissance | 🟡 Medium |
| 4 | Web technology and plugin versions exposed | WhatWeb identified WordPress 7.1.4 and WP Download Manager 3.3.76, along with other metadata | Attackers may cross-reference exposed software versions against known CVEs to plan further attacks | 🟡 Medium |
| 5 | DNS infrastructure information exposed | DNSRecon enumerated SOA, NS, MX, SPF/TXT, and SRV records, including internal autodiscover routing to cpanelemaildiscovery.cpanel.net | DNS information helps build a broader infrastructure profile of hosting and mail provider relationships | 🟡 Medium |
| 6 | Server IP address identifiable | Nslookup and WHOIS both resolved the domain to 192.232.216.135, hosted on HostGator/Unified Layer infrastructure | Confirms hosting location and enables further IP-based reconnaissance | 🟢 Low |
| 7 | HTTP technical information exposed | Curl -I returned response headers exposing the WordPress REST API endpoint (/wp-json/) and caching/CDN details | May assist technology fingerprinting and further enumeration | 🟢 Low |
| 8 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) protecting the site | Reveals information about the web application's security architecture, though the WAF itself is a positive control | 🟢 Low |

The expired SSL certificate is the one confirmed technical defect identified in this assessment; all other findings are information exposure observations from reconnaissance and scanning, not confirmed vulnerabilities. The practical exercises primarily involved information gathering, OSINT aggregation, and host/port discovery. No exploitation or active vulnerability validation was performed as part of these three modules. Therefore, the presence of a software version, open port, or DNS record does not by itself mean that the underlying system is exploitable; further authorized security testing would be required to confirm actual exploitability for findings other than the certificate expiry.

## 6. Recommendations

1. **Renew the SSL/TLS certificate immediately.** An expired certificate undermines encrypted trust across the web and mail services sharing it and should be treated as the highest priority fix from this assessment.
2. **Restrict direct internet access to the MySQL port (3306).** Database services should sit behind a firewall or be reachable only from application servers on a private network, not the public internet.
3. **Review which subdomains need to be publicly resolvable.** Administrative and mail-management subdomains (cpanel, webdisk, webmail, cpcalendars, cpcontacts) widen the reconnaissance surface and should be restricted or placed behind additional access controls where possible.
4. **Review publicly exposed technology information.** Organizations should regularly review what information about their CMS, plugins, and versions is visible to WhatWeb-style fingerprinting.
5. **Keep software updated.** WordPress, WP Download Manager, and other web technologies should be regularly updated and reviewed against current security advisories.
6. **Review HTTP response headers.** Headers should be checked periodically to ensure no unnecessary technical information (such as REST API endpoints) is exposed beyond what is required.
7. **Review DNS records regularly.** DNS records should be checked periodically to ensure only required information and services are publicly exposed.
8. **Keep the WAF properly configured and monitored.** ModSecurity should remain enabled and tuned, since it already blocks naive attacks.
9. **Perform security testing only with authorization.** Reconnaissance, OSINT, and scanning should only be performed against systems and domains where appropriate written authorization has been secured.

## 7. Conclusion

During Week 2 of my Cybersecurity and Ethical Hacking internship, I completed three practical modules covering footprinting, OSINT enumeration, and network scanning against networkwalks.com, the domain for which written testing permission had already been secured.

In the footprinting activity, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS reveals domain registration details, WhatWeb identifies web technologies, Nslookup resolves domain names, Curl inspects HTTP headers, Wafw00f identifies a WAF, and DNSRecon surfaces additional DNS records.

In the OSINT activity, I used theHarvester to aggregate data across many public sources at once, which revealed a much broader set of subdomains than any single tool would have shown on its own, useful for understanding how much of an organization's infrastructure is discoverable without ever touching its network directly.

In the network scanning activity, I used Zenmap to identify open ports, running services and their versions, operating system clues, and the network path to the target. This activity also surfaced the most concrete finding of the assessment: an expired SSL certificate, discovered only because the scan's SSL script checked certificate validity dates directly, a reminder that active scanning can surface real, fixable issues that passive footprinting alone would miss.

Finally, I learned that reconnaissance, OSINT, and scanning must always be performed within an authorized scope. These activities were completed as part of the assigned educational cybersecurity lab, with permission already secured for the networkwalks.com domain.
