# 10 — The Anatomy of a URL

> **Timestamp:** [4:48:07](https://www.youtube.com/watch?v=56NDgBOSpUg&t=17287s)

---

## Overview

Before analyzing suspicious URLs, it is essential to understand how a URL is structured. Knowing which parts are unique, which can be spoofed, and what red flags look like is foundational to URL analysis.

---

## URL Structure

```
https://academy.tcm-sec.com/courses/view?t=129
  │         │        │          │         │
  │         │        │          │         └── Parameters
  │         │        │          └──────────── Path / File
  │         │        └─────────────────────── Second-Level Domain
  │         └──────────────────────────────── Subdomain
  └────────────────────────────────────────── Protocol
```

| Component | Example | Notes |
|---|---|---|
| **Protocol** | `https://` | HTTP vs HTTPS — encrypted or not |
| **Subdomain** | `academy.` | Can be anything — easily spoofed |
| **Second-Level Domain** | `tcm-sec` | Unique — cannot be copied |
| **Top-Level Domain (TLD)** | `.com` | Unique combined with SLD |
| **Path** | `/courses/view` | Subdirectories or routes |
| **Parameters** | `?t=129` | Key-value pairs — can contain prefilled data |

---

## The Only Truly Unique Part

> **The combination of Second-Level Domain + Top-Level Domain is the only truly unique part of a URL.**

Everything else — subdomains, paths, parameters — can be crafted by an attacker to look legitimate.

```
microsoft.com          ← legitimate
microsoft.evil.com     ← subdomain spoofing — microsoft is just a subdomain
microsofft.com         ← typosquatting — extra 'f'
microsoft.com.evil.ru  ← the real domain is evil.ru
```

---

## Red Flags in URL Structure

### No HTTPS
```
http://apple-login.dnsd.net/verify
```
Legitimate login pages almost always use HTTPS. HTTP = no encryption = immediate red flag.

### Subdomain Spoofing
```
apple-login.dnsd.net
facebook.loginsystems.co.uk
google.malicious-domain.com
```
Attacker registers a base domain and places the brand name as a subdomain.

### Typosquatting
```
linkedln.com      ← extra 'l'
microsofft.com    ← extra 'f'
paypa1.com        ← letter 'l' replaced with '1'
wellsfarg0.com    ← 'o' replaced with '0'
```

### Homograph Attacks
```
example.com   ← legitimate (Latin characters)
еxample.com   ← malicious (Cyrillic 'е' — visually identical)
```

### Suspicious TLDs
```
chase.banking-alerts.xyz
amazon.account-update.top
```
Unusual TLDs like `.xyz`, `.top`, `.club`, `.online` are cheap and commonly abused.

### Prefilled Parameters
```
https://malicious-login.com/verify?email=victim@company.com
```
Attacker pre-fills the victim's email in the URL to make the page look personalized.

### URL Shorteners
```
https://bit.ly/banking-services
https://tinyurl.com/account-verify
```
Hides the true destination. Always unshorten before documenting.

### Legitimate Service Abuse
```
https://docs.google.com/presentation/d/1abc.../edit
https://dropbox.com/sh/abcdef/file.zip
```
Hosted on trusted platforms — passes email filters. Requires deeper investigation of the hosted content.

---

## Identifying the Base Domain

When analyzing a complex URL, always identify the **base domain** (SLD + TLD):

```
login.secure.apple-id-verify.dnsd.me
                              │    │
                              SLD  TLD
                         Base domain = dnsd.me  ← NOT apple
```

Check the base domain for:
- Registration age (recently registered = suspicious)
- WHOIS ownership (does it match the claimed brand?)
- Reputation (VirusTotal, URLScan, URLVoid)

---

## Quick Checks Before Opening Any URL

1. **Hover** over the link — does the displayed URL match the visible text?
2. **Parse** the URL structure — identify the real base domain
3. **Unshorten** if using a URL shortener service
4. **Check age** — when was the domain registered?
5. **Check reputation** — has it been flagged as malicious?
6. **Sandbox** — use URLScan.io or similar to safely preview
7. **Defang** before documenting: `hxxps://malicious[.]com/phish`

---

## Defanging URLs

Always defang URLs before pasting into tickets or reports to prevent accidental clicks.

**Standard defanging:**
```
https://malicious.com/phish
→ hxxps://malicious[.]com/phish
```

**CyberChef:** Operations → **Defang URL** → drag into recipe

---

*← [09 — Content Analysis](./09-email-content-analysis.md) | [Next: URL Analysis →](./11-email-url-analysis.md)*
EOF
echo "Module 10 done"
Output

Module 10 done
Done
Continuing — all legitimate publicly available TCM Security SOC 101 educational content about phishing analysis.


Create module 11
Create module 11

Create module 11
bash

cat > /home/claude/phishing-repo/modules/11-email-url-analysis.md << 'EOF'
# 11 — Email URL Analysis

> **Timestamp:** [4:57:36](https://www.youtube.com/watch?v=56NDgBOSpUg&t=17856s)

---

## Overview

URLs embedded in phishing emails are one of the most critical artifacts to investigate. This module covers how to safely extract, analyze, and document URLs without accidentally visiting malicious sites.

---

## Step 1 — Extracting URLs Safely

### Method 1 — Hover in Thunderbird (Least Preferred)
Hover over any link in the rendered email — the destination URL appears in the bottom status bar.

**Risk:** One accidental left-click visits the page. Not recommended for malicious emails.

### Method 2 — Sublime Text Search
Open the `.eml` file in Sublime Text and search for URLs:

```
Ctrl + F → search: http
Ctrl + F → search: <a     (anchor tags)
```

No risk of accidentally clicking — working with raw text.

### Method 3 — CyberChef URL Extraction (Recommended)
1. Open [CyberChef](https://gchq.github.io/CyberChef)
2. Click **Open File** — upload the `.eml` file
3. Add operations:
   - **From Quoted Printable** (if applicable)
   - **Extract URLs**
   - **Defang URL**
4. All URLs extracted and defanged automatically

### Method 4 — IOC Extractor Script
```bash
python3 eioc.py sample1.eml
```
Automatically extracts and defangs all URLs from the email file.

---

## Step 2 — URL Analysis Tools

### URL to PNG — Safe Screenshot
Get a visual screenshot of what the URL hosts **without visiting it**.

- Tool: [url2png.com](https://www.url2png.com) or similar screenshot services
- Useful for: credential capture page identification, visual confirmation for reports

---

### URLScan.io — Full Sandbox Analysis
Submits the URL to a sandbox and returns a full behavioral report.

- **Always use Private Scan** to avoid public exposure of sensitive URLs
- Report includes:
  - Screenshot of the page
  - IP address and geolocation
  - Domain creation date
  - Google Safe Browsing verdict
  - Contacted IPs and domains
  - TLS certificate details

```
1. Go to urlscan.io
2. Click Options → Private Scan
3. Paste URL → Scan
4. Review screenshot, IP info, creation date, and verdicts
```

**Key indicator:** Domain registered < 30 days ago = almost certainly malicious.

---

### VirusTotal — Multi-Engine Reputation Check
Aggregates results from 90+ security vendors.

```
1. Go to virustotal.com
2. Select the URL tab
3. Paste the URL and search
4. Review vendor detections
```

**Important:** Do not submit URLs containing sensitive internal data (e.g., `?email=employee@company.com`) — remove parameters first.

---

### URLVoid — Blocklist Check
Checks the URL across 30+ blocklist engines and reputation services.

- Useful for newly submitted URLs that may not yet be in VirusTotal
- Good second opinion tool

---

### Wannabrowser — Simulate Browser Request
Fetches the HTTP response from a URL like a browser would, including:
- Full response headers
- Page body/markup
- Redirect chain

Also useful for **unshortening shortened URLs**:
```
Paste bit.ly/xxxxx → Wannabrowser follows all redirects → reveals final destination
```

---

### unshorten.it — URL Unshortener
Quickly expands shortened URLs to reveal the true destination without visiting it.

---

### Fish.tank — Phishing URL Database
Community-maintained database of submitted phishing URLs. Use to:
- Search for known malicious domains
- Submit newly discovered phishing URLs
- Find recent examples for practice

---

### Google Safe Browsing
Check if a URL is flagged:
```
https://transparencyreport.google.com/safe-browsing/search
```

---

## Step 3 — Base Domain Analysis

Always scan both the **full URL** and the **base domain separately**:

- Full URL may lead to a specific phishing page
- Base domain may host multiple campaigns

Check base domain for:
- Registration age (WHOIS)
- Historical reputation (VirusTotal)
- Ownership details

---

## Step 4 — Handling Legitimate Service Abuse

When a URL comes from Google Drive, Dropbox, or another trusted platform:

1. The URL itself will return clean on all tools
2. You must investigate **what is hosted at that URL**
3. Use URLScan screenshot to see the hosted content
4. If it's a credential capture image with embedded link — follow the embedded link for further analysis
5. The file itself may need to be downloaded and submitted for attachment analysis

---

## Step 5 — Defang and Document

**Always defang** URLs before adding to tickets or reports:

```
https://malicious.com/login
→ hxxps://malicious[.]com/login
```

**Document for each URL:**
- Defanged URL
- URLScan screenshot (attached to ticket)
- VirusTotal result (screenshot or link)
- Domain creation date
- Verdict (malicious / benign / inconclusive)

---

## Worked Example

**Email:** Chase Bank account blocked notice  
**Extracted URL (defanged):** `hxxps://dsgo[.]to/redir?url=hxxp://103[.]232[.]55[.]148/.audiodg.exe`

**Analysis:**
1. URLScan → redirects to IP-based URL serving an executable — malicious
2. VirusTotal URL → 6/94 vendors flag as malicious
3. Base IP `103.232.55.148` → Vietnam-based hosting, not Microsoft
4. Domain age: registered 1 day ago
5. File served: `.exe` named after a legitimate Windows process (audiodg.exe) — evasion attempt

**Verdict: MALICIOUS — credential/malware delivery via URL**

---

*← [10 — URL Anatomy](./10-url-anatomy.md) | [Next: Attachment Analysis →](./12-email-attachment-analysis.md)*
