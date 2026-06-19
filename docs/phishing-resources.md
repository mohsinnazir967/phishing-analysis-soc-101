# Phishing Analysis — Tools & Practice Resources

> A curated reference of all tools, databases, and practice resources used throughout the SOC 101 Phishing Analysis section.

---

## 🔧 Analysis Tools

### Email Analysis
| Tool | Purpose | URL |
|---|---|---|
| **Mozilla Thunderbird** | Open and view `.eml` email files | `sudo apt install thunderbird` |
| **Sublime Text** | Inspect raw email source and headers | [sublimetext.com](https://www.sublimetext.com) |
| **PhishTool** | Automated email analysis platform | [app.phishtool.com](https://app.phishtool.com) |
| **MXToolbox Header Analyzer** | Parse email headers, SPF/DKIM/DMARC checks | [mxtoolbox.com/EmailHeaders.aspx](https://mxtoolbox.com/EmailHeaders.aspx) |
| **Message Header Analyzer** | Client-side header parser (open source) | [mha.azurewebsites.net](https://mha.azurewebsites.net) |

---

### Decoding & Extraction
| Tool | Purpose | URL |
|---|---|---|
| **CyberChef** | Base64, URL, HTML entity decoding + URL extraction + defanging | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef) |
| **eioc.py** | Automated IOC extractor from `.eml` files | TCM course repository / tools folder |
| **email-dump.py** | Extract attachments from `.eml` via CLI | Didier Stevens suite |

---

### URL Analysis
| Tool | Purpose | URL |
|---|---|---|
| **URLScan.io** | URL sandboxing, screenshot, behavioral report | [urlscan.io](https://urlscan.io) |
| **VirusTotal** | Multi-engine URL and file hash reputation | [virustotal.com](https://virustotal.com) |
| **URLVoid** | Blocklist check across 30+ engines | [urlvoid.com](https://www.urlvoid.com) |
| **Wannabrowser** | Simulate browser request, follow redirects | [wannabrowser.net](https://www.wannabrowser.net) |
| **URL2PNG** | Safe screenshot of URL without visiting | [url2png.com](https://www.url2png.com) |
| **unshorten.it** | Expand shortened URLs safely | [unshorten.it](https://unshorten.it) |
| **Google Safe Browsing** | Check if URL is flagged by Google | [transparencyreport.google.com/safe-browsing/search](https://transparencyreport.google.com/safe-browsing/search) |

---

### Domain & IP Investigation
| Tool | Purpose | URL |
|---|---|---|
| **DomainTools WHOIS** | Domain age, registrar, ownership | [whois.domaintools.com](https://whois.domaintools.com) |
| **MXToolbox** | Reverse DNS, MX, SPF, DKIM, DMARC lookups | [mxtoolbox.com](https://mxtoolbox.com) |
| **AbuseIPDB** | IP address abuse reputation check | [abuseipdb.com](https://www.abuseipdb.com) |
| **DNSTwist** | Typosquatting and lookalike domain discovery | [dnstwist.it](https://dnstwist.it) |
| **Cisco Talos** | IP, domain, and file hash threat intelligence | [talosintelligence.com](https://talosintelligence.com) |

**CLI alternatives:**
```bash
# Reverse DNS / WHOIS lookup
whois 185.220.101.45

# SPF record check
dig TXT domain.com | grep spf

# DKIM record check
nslookup -type=txt selector._domainkey.domain.com

# DMARC record check
dig TXT _dmarc.domain.com
```

---

### Attachment & Malware Analysis
| Tool | Purpose | URL |
|---|---|---|
| **VirusTotal** | File hash and file upload reputation | [virustotal.com](https://virustotal.com) |
| **Cisco Talos** | File hash reputation | [talosintelligence.com](https://talosintelligence.com) |
| **Hybrid Analysis** | Free dynamic sandbox (CrowdStrike Falcon) | [hybrid-analysis.com](https://www.hybrid-analysis.com) |
| **Joe Sandbox** | Advanced dynamic sandbox with live interaction | [joesandbox.com](https://www.joesandbox.com) |
| **Any.Run** | Interactive sandbox — real-time visual | [app.any.run](https://app.any.run) |
| **oledump.py** | Static analysis of Office documents / macros | Didier Stevens suite |
| **pdfid.py** | High-level PDF structure overview | Didier Stevens suite |
| **pdf-parser.py** | Deep PDF object and URL extraction | Didier Stevens suite |

**CLI hashing:**
```bash
# Linux / Ubuntu
sha256sum file.iso
sha1sum file.iso
md5sum file.iso

# Windows PowerShell
Get-FileHash file.iso
Get-FileHash file.iso -Algorithm MD5
Get-FileHash file.iso -Algorithm SHA1
```

---

## 🗃️ Sample Repositories & Databases

### Email Samples
| Resource | What It Contains | URL |
|---|---|---|
| **Phishing Pot** | Thousands of real phishing `.eml` files from honeypot accounts | [github.com/rf-peixoto/phishing_pot](https://github.com/rf-peixoto/phishing_pot) |

```bash
# Clone entire repository
git clone https://github.com/rf-peixoto/phishing_pot.git

# Download single sample
wget https://raw.githubusercontent.com/rf-peixoto/phishing_pot/main/email/[filename].eml
```

---

### Live Phishing URLs
| Resource | What It Contains | URL |
|---|---|---|
| **PhishTank** | Community-submitted phishing URLs with verification status | [phishtank.com](https://www.phishtank.com) |
| **OpenPhish** | Phishing URL feed | [openphish.com](https://openphish.com) |
| **URLhaus** | Malicious URLs used for malware distribution | [urlhaus.abuse.ch](https://urlhaus.abuse.ch) |

> ⚠️ Never visit phishing URLs directly in your browser. Always use URLScan.io, URL2PNG, or a sandboxed VM.

---

### Malware Samples
| Resource | What It Contains | URL |
|---|---|---|
| **MalwareBazaar** | Real malware samples — searchable by file type, tag, hash | [bazaar.abuse.ch](https://bazaar.abuse.ch) |
| **Any.Run Public Tasks** | Browse public interactive sandbox analyses | [app.any.run](https://app.any.run) |

**MalwareBazaar search syntax:**
```
file_type:pdf        ← search PDF samples
file_type:xlsm       ← search macro-enabled Excel
file_type:doc        ← search Word documents
file_type:iso        ← search ISO disk images
```

**Download instructions:**
1. Find sample → click SHA256 hash → **Download Sample**
2. File downloads as password-protected `.zip`
3. Password is always: `infected`
4. Extract **only** inside an isolated VM or sandbox

---

## 📋 Phishing Reporting Channels

Use these to report malicious domains and URLs discovered during investigations:

| Channel | Purpose | URL |
|---|---|---|
| **Google Safe Browsing** | Report phishing URLs | [safebrowsing.google.com/safebrowsing/report_phish](https://safebrowsing.google.com/safebrowsing/report_phish/) |
| **PhishTank Submit** | Submit phishing URL to community database | [phishtank.com/add_web_phish.php](https://www.phishtank.com/add_web_phish.php) |
| **APWG eCrime** | Anti-Phishing Working Group reporting | [apwg.org/reportphishing](https://apwg.org/reportphishing/) |
| **Registrar Abuse** | Report to domain registrar directly | Find abuse email via WHOIS lookup |

**Finding registrar abuse contact:**
```bash
# Look up the domain
whois malicious-domain.com

# Find field:
# Registrar Abuse Contact Email: abuse@registrar.com
```

---

## 🔗 Further Learning

| Topic | Resource |
|---|---|
| Full SOC 101 curriculum | [TCM Security Academy](https://academy.tcm-sec.com) |
| Practical Malware Analysis & Triage | [TCM Security — PMAT course](https://academy.tcm-sec.com) |
| KQL threat hunting | [KC7 Security Analyst Programme](https://kc7cyber.com) |
| Microsoft Sentinel / SC-200 | [Microsoft Learn](https://learn.microsoft.com/en-us/certifications/exams/sc-200/) |
| Blue team labs and challenges | [CyberDefenders](https://cyberdefenders.org) |
| Blue team CTF and labs | [LetsDefend](https://letsdefend.io) |
| Hands-on SOC labs | [TryHackMe — SOC Level 1](https://tryhackme.com/path/outline/soclevel1) |

---

*Part of the [SOC 101 Phishing Analysis Repository](../README.md)*







