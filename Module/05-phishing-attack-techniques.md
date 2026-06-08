# 05 — Phishing Attack Techniques

> **Timestamp:** [3:35:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=12947s)

---

## Overview

Beyond the attack *types*, attackers use specific *techniques* to deceive victims and evade detection. Recognizing these techniques is essential for accurate email analysis.

---

## 1. Pretexting
Creating a **fabricated story or scenario** to gain the target's trust and cooperation.

Examples:
- Impersonating HR department requesting bank details for payroll updates
- Fake IT audit requesting credentials to verify accounts
- Urgent legal threat demanding immediate wire transfer

---

## 2. Email Address & Domain Spoofing
Forging the `From:` header to make the email appear from a trusted source.

**Types:**
- **Email address spoofing** — `From: alerts@chase.com` (easily faked)
- **Domain spoofing** — registering `microsoft-security.com` to look official

> **Key fact:** Almost all email headers can be modified by the attacker before sending. The `Received:` headers added by MTAs are the most reliable.

---

## 3. URL Manipulation Techniques

### URL Shortening
Using services like bit.ly, TinyURL, or short.io to hide the true destination of a link.

```
https://bit.ly/banking-services  →  actually leads to phishing page
```

**Unshortening tools:** unshorten.it, wannabrowser.com

---

### Subdomain Spoofing
Registering a malicious base domain and using the legitimate brand name as a **subdomain**:

```
google.malicioussite.com      ← Google is just a subdomain
facebook.loginsystems.co.uk   ← Facebook is just a subdomain
```

The companies being impersonated have no affiliation with the base domain.

---

### Homograph / IDN Attacks
Replacing characters with visually identical characters from different Unicode sets (e.g., Cyrillic):

```
example.com   ← legitimate (Latin 'c')
еxample.com   ← malicious (Cyrillic 'е' — looks identical)
```

Tools like **homoglyph attack generator** can produce these automatically.

---

### Typosquatting
Registering domains with common typing mistakes:

```
linkedin.com   → linkdin.com  (missing 'e')
microsoft.com  → mircosoft.com (transposed letters)
paypal.com     → paypa1.com   (letter 'l' replaced with '1')
```

Tool for checking: [DNSTwist](https://dnstwist.it)

---

## 4. Encoding Techniques

Used to obfuscate malicious content and evade simple spam filters.

### Base64 Encoding
The `Content-Transfer-Encoding: base64` header indicates the email body is base64 encoded.

```bash
# Decode in terminal
echo "BASE64STRING" | base64 -d
```

### HTML Entity Encoding
Characters encoded as HTML entities to hide keywords from spam filters:

```html
&#66;&#105;&#116;&#99;&#111;&#105;&#110;  →  Bitcoin
```

### URL Encoding
Characters replaced with percent-encoded hex values:

```
https%3A%2F%2Fmalicious.com  →  https://malicious.com
```

**Decoding tool:** [CyberChef](https://gchq.github.io/CyberChef/)

---

## 5. Abuse of Legitimate Services
Hosting malicious content on trusted platforms to bypass email filters:

| Platform | How Abused |
|---|---|
| Google Drive / Docs | Host credential capture images with embedded links |
| Dropbox | Distribute password-protected zip archives |
| OneDrive | Host malicious Office documents |
| Wix / Webflow | Quickly spin up fake login pages |
| Render / Netlify | Host credential harvesting backends |

> Since the URL comes from a legitimate service (e.g., `docs.google.com`), it often bypasses email security tools.

---

## 6. Pharming
A two-stage attack:

1. **Stage 1** — Attacker delivers malware OR manipulates DNS/hosts file on victim machine
2. **Stage 2** — Victim is silently redirected to fraudulent website even when typing the legitimate URL

Types:
- **Malware-based pharming** — Trojan modifies the local `hosts` file
- **DNS poisoning** — Corrupt DNS cache redirects traffic at network level

---

## Quick Reference: Red Flag Checklist

When analyzing a suspicious email, watch for:

- [ ] Spoofed `From:` address (domain doesn't match claimed sender)
- [ ] `Reply-To:` differs from `From:`
- [ ] Urgency or fear-inducing language
- [ ] Generic greeting ("Dear Customer")
- [ ] Grammar or spelling errors
- [ ] Mismatched brand name spelling
- [ ] URL shorteners or suspicious domains
- [ ] Unexpected attachments (`.iso`, `.exe`, macro-enabled docs)
- [ ] Requests for credentials, gift cards, or wire transfers
- [ ] Recently registered domain (< 30 days old)

---

*← [04 — Attack Types](./04-phishing-attack-types.md) | [Next: Analysis Methodology →](./06-email-analysis-methodology.md)*
