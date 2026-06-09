# 08 — Email Authentication Methods (SPF, DKIM, DMARC)

> **Timestamp:** [4:17:51](https://www.youtube.com/watch?v=56NDgBOSpUg&t=15471s)

---

## Overview

Three email authentication protocols work together to verify that an email genuinely came from the domain it claims. These are critical checks in every phishing investigation — but **none of them is a silver bullet on its own**.

---

## SPF — Sender Policy Framework

### What It Does
Allows domain owners to publish a list of **authorized mail servers** allowed to send email on their behalf, via DNS TXT records.

### How It Works
1. Email arrives claiming to be from `@chase.com`
2. Receiving server queries DNS: "What IPs are authorized to send for chase.com?"
3. Compares the actual sending IP against the SPF record
4. **Pass** = IP is authorized | **Fail** = IP is NOT authorized

### Manual SPF Check
```bash
# Using dig
dig TXT chase.com | grep spf

# Using nslookup
nslookup -type=txt chase.com | grep spf
```

**Example SPF record:**
```
v=spf1 ip4:192.168.1.0/24 include:_spf.google.com -all
```

| Mechanism | Meaning |
|---|---|
| `ip4:x.x.x.x` | This IP is authorized |
| `include:domain` | Include that domain's SPF record |
| `-all` | Hard fail — reject unauthorized senders |
| `~all` | Soft fail — treat unauthorized with suspicion |

### Limitation
SPF only checks the `Return-Path` domain, not the visible `From:` address. An attacker can register their own domain, set up valid SPF records, and still pass — while spoofing a different `From:` address.

---

## DKIM — DomainKeys Identified Mail

### What It Does
Uses **public key cryptography** to sign outgoing emails, allowing receivers to verify the message hasn't been tampered with in transit.

### How It Works
1. Sending server signs the email with a **private key**
2. The corresponding **public key** is published in DNS
3. Receiving server retrieves the public key and verifies the signature
4. **Pass** = signature valid, content unchanged | **Fail** = tampered or forged

### Manual DKIM Check
```bash
# Syntax: <selector>._domainkey.<domain>
nslookup -type=txt s1._domainkey.namecheap.com
```

### Key DKIM Header Fields
```
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
  d=namecheap.com;    ← signing domain
  s=s1;               ← selector (used to locate public key in DNS)
  bh=<body hash>;     ← hash of the message body
  b=<signature>       ← the actual cryptographic signature
```

### Limitation
DKIM confirms the domain that **signed** the email is legitimate. It doesn't guarantee the visible `From:` domain is the same as the signing domain. An attacker using a lookalike domain with valid DKIM will still pass.

---

## DMARC — Domain-Based Message Authentication, Reporting & Conformance

### What It Does
Builds on SPF and DKIM by adding a **policy layer** and **alignment check** — ensuring the authenticated domain matches the visible `From:` address.

### DMARC Policies
| Policy | Action |
|---|---|
| `p=none` | Monitor only — no action taken, collect reports |
| `p=quarantine` | Send failing emails to spam/quarantine folder |
| `p=reject` | Outright reject emails that fail DMARC |

### Manual DMARC Check
```bash
dig TXT _dmarc.chase.com
```

### Alignment
DMARC alignment ensures the domain in the `From:` header matches the domain used in SPF/DKIM checks — closing the gap where attackers could pass SPF/DKIM with their own domain while spoofing a different `From:`.

---

## Authentication Results Header

The `Authentication-Results:` header is added by the receiving server and summarizes all three checks:

```
Authentication-Results: mx.google.com;
  dkim=pass header.i=@namecheap.com header.s=s1;
  spf=pass (google.com: domain of mailout1.namecheap.com designates 149.72.142.11 as permitted sender);
  dmarc=pass (p=REJECT) header.from=namecheap.com
```

---

## Important Caveat

These checks are **not a silver bullet**:

| Scenario | Result |
|---|---|
| Attacker registers own domain + valid SPF/DKIM/DMARC | All checks **PASS** — still malicious |
| Legitimate mailbox is compromised | All checks **PASS** — still malicious |
| Attacker uses Gmail/Yahoo to send | All checks **PASS** — still malicious |
| Legitimate email with misconfigured DNS | Checks may **FAIL** — but benign |

> Always combine authentication results with IP reputation, domain age, content analysis, and URL/attachment investigation before reaching a verdict.

---

## Worked Example — Namecheap Renewal (Legitimate)

```
Received-SPF: pass (google.com: 149.72.142.11 is authorized)
DKIM-Signature: d=namecheap.com; s=s1; a=rsa-sha256
Authentication-Results: dkim=pass; spf=pass; dmarc=pass (p=REJECT)
```

- SPF: Sending IP `149.72.142.11` is within SendGrid's authorized range for `namecheap.com` ✅
- DKIM: Signature valid, domain matches ✅
- DMARC: Aligned and passes with reject policy ✅
- **Verdict: Legitimate email**

---

## Header Analysis Tool

Use **MXToolbox Email Header Analyzer** to automatically run all three checks:

1. Copy full email source (Thunderbird: View → Message Source)
2. Paste at [mxtoolbox.com/EmailHeaders.aspx](https://mxtoolbox.com/EmailHeaders.aspx)
3. Review SPF, DKIM, and DMARC results

---

*← [07 — Header Analysis](./07-header-and-sender-analysis.md) | [Next: Content Analysis →](./09-email-content-analysis.md)*
