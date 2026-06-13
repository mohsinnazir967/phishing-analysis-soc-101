# Email Analysis Methodology — Quick Reference

> The complete 7-step phishing investigation framework used throughout the SOC 101 Phishing Analysis section.

---

## The 7-Step Framework

```
STEP 1 → TRIAGE
STEP 2 → HEADER & SENDER ANALYSIS
STEP 3 → CONTENT EXAMINATION
STEP 4 → URL ANALYSIS
STEP 5 → ATTACHMENT ANALYSIS
STEP 6 → HOLISTIC ANALYSIS & SCOPE
STEP 7 → DEFENSE ACTIONS & DOCUMENTATION
```

---

## Step 1 — Triage

Quickly assess severity and prioritise investigation effort before diving deep.

**Ask:**
- Who received it? One user or an entire department?
- Is the recipient high-value? (Executive, finance, IT admin)
- What is the email claiming? Any obvious urgency indicators?
- Has a similar email been reported recently?

**Output:** Severity rating — Low / Medium / High / Critical

---

## Step 2 — Header & Sender Analysis

Determine the **true origin** of the email — not what it claims.

**Checklist:**
```
[ ] Open email in Sublime Text with Email Header syntax highlighting
[ ] Identify originating IP from the Received chain (read bottom-up)
[ ] Run reverse DNS (rDNS) lookup on originating IP
[ ] Check if rDNS matches the claimed sender domain
[ ] Note X-Sender-IP / X-Originating-IP if present
[ ] Check Reply-To — does it match the From domain?
[ ] Check Return-Path — does it match the From domain?
[ ] Document Message-ID domain vs. claimed sender domain
[ ] Run SPF check (dig TXT <domain> | grep spf)
[ ] Run DKIM check (MXToolbox or manual DNS lookup)
[ ] Check DMARC policy (dig TXT _dmarc.<domain>)
[ ] Review Authentication-Results header summary
```

**Tools:**
- Sublime Text (Email Header syntax)
- [MXToolbox Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)
- [DomainTools WHOIS](https://whois.domaintools.com)
- `whois <IP>` in terminal

---

## Step 3 — Content Examination

Analyze the body of the email for social engineering tactics and encoding.

**Checklist:**
```
[ ] Open in Thunderbird — note visual red flags
[ ] Open in Sublime Text — examine raw MIME structure
[ ] Identify Content-Transfer-Encoding (7bit / base64 / quoted-printable)
[ ] Decode body if Base64 encoded (CyberChef → From Base64)
[ ] Decode URLs if multi-layer encoded (CyberChef pipeline)
[ ] Identify social engineering tactics:
    [ ] Urgency / time pressure
    [ ] Authority impersonation
    [ ] Fear / intimidation
    [ ] Trust building (logos, legitimate language)
    [ ] Scarcity
    [ ] Familiarity / known contact
[ ] Note grammar/spelling errors
[ ] Note generic greetings ("Dear Customer")
[ ] Note brand name misspellings or logo inconsistencies
[ ] Describe what the email is asking the user to do
```

**Tools:**
- Thunderbird
- Sublime Text
- [CyberChef](https://gchq.github.io/CyberChef)

---

## Step 4 — URL Analysis

Investigate all links embedded in the email safely.

**Checklist:**
```
[ ] Extract all URLs (CyberChef Extract URLs, or eioc.py)
[ ] Defang all URLs before documenting
[ ] Parse each URL — identify true base domain (SLD + TLD)
[ ] Identify any URL manipulation techniques:
    [ ] URL shorteners (unshorten with unshorten.it)
    [ ] Subdomain spoofing
    [ ] Typosquatting
    [ ] Homograph / IDN attack
    [ ] Legitimate service abuse (Google Drive, Dropbox)
[ ] Check domain age — WHOIS lookup
[ ] Run URLScan.io (private scan) — capture screenshot
[ ] Run VirusTotal URL check — note detection count
[ ] Run URLVoid for blocklist check
[ ] If shortened URL — unshorten and analyze destination
[ ] Scan both full URL and base domain separately
[ ] Document all findings with screenshots
```

**Tools:**
- [CyberChef](https://gchq.github.io/CyberChef)
- [URLScan.io](https://urlscan.io) ← private scan
- [VirusTotal](https://virustotal.com)
- [URLVoid](https://urlvoid.com)
- [unshorten.it](https://unshorten.it)
- [DomainTools WHOIS](https://whois.domaintools.com)
- `python3 eioc.py sample.eml`

---

## Step 5 — Attachment Analysis

Safely investigate any files attached to the email.

**Checklist:**
```
[ ] Extract attachment (Thunderbird right-click → Save As, or email-dump.py)
[ ] Verify true file type with: file <attachment>
[ ] Collect all three hashes:
    sha256sum <file>
    sha1sum <file>
    md5sum <file>
[ ] Submit SHA256 hash to VirusTotal — note detection count and family
[ ] Submit SHA256 hash to Cisco Talos
[ ] If no VirusTotal results → submit to Hybrid Analysis sandbox
[ ] For .xlsm / .docm → run oledump.py (Module 14)
[ ] For .pdf → run pdfid.py and pdf-parser.py (Module 15)
[ ] Document all findings with screenshots
[ ] DO NOT open or execute the file on your host machine
```

**Tools:**
- `python3 email-dump.py`
- `file <attachment>`
- `sha256sum / sha1sum / md5sum`
- [VirusTotal](https://virustotal.com)
- [Cisco Talos](https://talosintelligence.com)
- [Hybrid Analysis](https://hybrid-analysis.com)
- `python3 oledump.py`
- `python3 pdfid.py` / `python3 pdf-parser.py`

---

## Step 6 — Holistic Analysis & Scope

Consider the bigger picture before concluding.

**Checklist:**
```
[ ] Search ticket system for similar recent reports
[ ] Perform message trace at email gateway — how many users received it?
[ ] Identify if this is part of a wider campaign
[ ] Correlate IoCs — do the sender IP, domain, and URL patterns match?
[ ] Assess business impact — who was targeted and what did they have access to?
[ ] Did any user click a link or open an attachment?
[ ] Did any user submit credentials to a capture page?
```

---

## Step 7 — Defense Actions & Documentation

Take action and document everything throughout the investigation.

**Reactive actions checklist:**
```
[ ] Quarantine email from all affected inboxes
[ ] Block sender address at email gateway
[ ] Block sender domain at email gateway (if dedicated malicious domain)
[ ] Block originating IP at email gateway
[ ] Block malicious URL at EDR and proxy
[ ] Block malicious domain at EDR and proxy
[ ] Block malicious file hashes at EDR
[ ] Purge all instances of email from mail server
[ ] Report malicious domain to registrar
[ ] Notify affected users
[ ] Reset credentials (if user submitted to credential capture page)
[ ] Scan / reimage endpoint (if malware was executed)
[ ] Notify stakeholders if threshold met
```

**Documentation checklist:**
```
[ ] All IoCs collected and defanged
[ ] All tool outputs screenshotted and attached to ticket
[ ] Report template filled in completely
[ ] Verdict stated with clear written justification
[ ] Every defensive action listed with justification
[ ] Ticket status updated to Resolved
[ ] User notified of verdict
```

---

## IoC Defanging Reference

Always defang IoCs before pasting into tickets or reports.

| Type | Original | Defanged |
|---|---|---|
| URL | `https://malicious.com/phish` | `hxxps://malicious[.]com/phish` |
| Domain | `malicious.com` | `malicious[.]com` |
| IP Address | `185.220.101.45` | `185[.]220[.]101[.]45` |
| Email | `attacker@evil.com` | `attacker[@]evil[.]com` |
| File hash | `d41d8cd98f00b204e9800998ecf8427e` | No defanging needed |

**CyberChef:** Operations → **Defang URL** for automated defanging.

---

## Verdict Definitions

| Verdict | Meaning |
|---|---|
| **Malicious** | Confirmed phishing — clear evidence of malicious intent |
| **Benign** | Confirmed legitimate — no malicious indicators found |
| **Spam** | Unsolicited bulk email — not directly malicious |
| **Inconclusive** | Insufficient evidence — escalate or monitor |

---

*Part of the [SOC 101 Phishing Analysis Repository](../README.md)*