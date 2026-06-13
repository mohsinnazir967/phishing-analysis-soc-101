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
