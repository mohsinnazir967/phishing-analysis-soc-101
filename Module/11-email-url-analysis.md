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
