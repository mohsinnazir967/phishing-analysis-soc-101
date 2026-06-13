# 20 — Additional Practice Resources

> **Timestamp:** [7:12:35](https://www.youtube.com/watch?v=56NDgBOSpUg&t=25955s)

---

## Overview

The best way to build phishing analysis skills is consistent hands-on practice with real samples. This module compiles the three best resources for finding sample emails, live phishing URLs, and malware attachments to investigate.

---

## Resource 1 — Phishing Pot (Sample Emails)

A GitHub repository containing thousands of real phishing emails collected via honeypot mailboxes — freely available for research, training, and detection development.

**URL:** [github.com/rf-peixoto/phishing_pot](https://github.com/rf-peixoto/phishing_pot)

**What it contains:**
- Thousands of real `.eml` phishing samples
- Organized in the `email/` directory
- Collected from live honeypot accounts

**How to use:**

```bash
# Download a single sample
wget https://raw.githubusercontent.com/rf-peixoto/phishing_pot/main/email/sample.eml

# Clone the entire repository
git clone https://github.com/rf-peixoto/phishing_pot.git
```

**Practice activities:**
- Open samples in Thunderbird and Sublime Text
- Run through the full 7-step analysis methodology (Module 06)
- Extract headers, identify originating IP, run rDNS lookup
- Extract and analyze embedded URLs
- Document findings using the report template

---

## Resource 2 — PhishTank (Live Phishing URLs)

A community-maintained database of live and recently submitted phishing URLs — useful for practicing URL analysis with fresh, real-world samples.

**URL:** [phishtank.com](https://www.phishtank.com)

**What it contains:**
- Recently submitted suspected phishing URLs
- Community verification status
- Submission timestamps and target brand information

**How to use:**
1. Browse recent submissions on the main page
2. Click any submission ID to view the full URL
3. Copy the URL and run it through your URL analysis workflow:
   - URLScan.io (private scan)
   - VirusTotal
   - URL2PNG for screenshot
   - WHOIS for domain age
   - unshorten.it if shortened
4. Practice identifying credential capture pages, subdomain spoofing, and legitimate service abuse

> ⚠️ Never visit phishing URLs directly in your browser. Always use sandboxing tools.

---

## Resource 3 — MalwareBazaar (Malicious Attachments)

A public database of malware samples submitted by the security research community — useful for practicing attachment analysis with real malicious files.

**URL:** [bazaar.abuse.ch](https://bazaar.abuse.ch)

**What it contains:**
- Real malware samples with hashes, tags, and file types
- Search by file type, signature, tag, or SHA256
- All samples downloadable in password-protected archives

**How to search by file type:**
```
# In the search bar use:
file_type:pdf
file_type:doc
file_type:xlsm
file_type:iso
```

**How to download a sample:**
1. Find a sample of interest — click its SHA256 hash
2. Click **Download Sample**
3. File downloads as a password-protected `.zip`
4. Password is always: `infected`
5. Extract only inside an isolated VM or sandbox

**Practice activities:**
- Collect MD5, SHA1, SHA256 hashes
- Submit hash to VirusTotal — review detections and family names
- Submit to Hybrid Analysis for dynamic analysis
- For `.xlsm` / `.doc` files → run `oledump.py` (Module 14)
- For `.pdf` files → run `pdfid.py` and `pdf-parser.py` (Module 15)
- Document findings using the report template

---

## Additional Tools for Continued Practice

| Tool | Purpose | URL |
|---|---|---|
| **Any.Run Public Tasks** | Browse public sandbox analyses | [app.any.run](https://app.any.run) |
| **URLhaus** | Database of malicious URLs used for malware distribution | [urlhaus.abuse.ch](https://urlhaus.abuse.ch) |
| **OpenPhish** | Phishing URL feed | [openphish.com](https://openphish.com) |
| **CyberChef** | Decoding, encoding, extraction practice | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef) |
| **MXToolbox** | SPF/DKIM/DMARC lookup practice | [mxtoolbox.com](https://mxtoolbox.com) |
| **DNSTwist** | Typosquatting and lookalike domain discovery | [dnstwist.it](https://dnstwist.it) |

---

## Suggested Practice Workflow

Use this structure each time you practice with a new sample:

```
1. TRIAGE
   → What type of email is this? What does it claim?
   → Who is the target? What is being asked?

2. HEADER ANALYSIS
   → Open in Sublime Text with Email Header syntax
   → Identify originating IP from Received chain
   → Run rDNS lookup on originating IP
   → Check SPF / DKIM / DMARC results

3. CONTENT ANALYSIS
   → Identify social engineering tactics used
   → Note any grammar/spelling errors or branding issues
   → Decode any Base64 or encoded content with CyberChef

4. URL ANALYSIS
   → Extract all URLs with CyberChef or eioc.py
   → Defang all URLs
   → Run each through URLScan.io and VirusTotal
   → Check domain age with WHOIS

5. ATTACHMENT ANALYSIS (if present)
   → Extract with email-dump.py or Thunderbird
   → Collect SHA256, SHA1, MD5 hashes
   → Submit hash to VirusTotal
   → Submit to Hybrid Analysis if no results

6. VERDICT
   → Malicious / Benign / Spam / Inconclusive
   → Write clear justification

7. DOCUMENT
   → Fill in the report template
   → List all IoCs (defanged)
   → Note what defensive actions would be taken
```

*← [19 — Documentation & Reporting](./19-documentation-and-reporting.md) | [Back to README →](../README.md)*
