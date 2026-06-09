# 19 — Documentation and Reporting

> **Timestamp:** [7:00:43](https://www.youtube.com/watch?v=56NDgBOSpUg&t=25243s)

---

## Overview

Documentation is not the last step in phishing analysis — it runs **throughout every stage** of the investigation. Strong documentation justifies your actions, enables other analysts to follow your work, and builds the organization's institutional knowledge over time.

---

## Why Documentation Matters

- **Justification** — if you block a domain or disable an account, you need a record of why
- **Continuity** — another analyst must be able to pick up your ticket and reach the same verdict
- **Legal / compliance** — incident records may be required for regulatory reporting
- **Improvement** — post-incident reviews depend on accurate timelines and findings
- **Metrics** — management uses ticket data to measure SOC performance

> If it is not documented, it did not happen.

---

## What to Document — At Every Stage

### During Triage
- Timestamp the ticket was created and when you picked it up
- Who reported the email (user, automated rule, gateway alert)
- Initial severity assessment

### During Header Analysis
- Date / Time from headers
- From address (claimed sender)
- Reply-To address (if different from From)
- Return-Path address
- Originating IP address (from Received chain or X-Sender-IP)
- Resolved hostname (rDNS result)
- SPF / DKIM / DMARC results

### During Content Analysis
- Social engineering tactics identified (urgency, authority, fear, etc.)
- Description of what the email is asking the user to do
- Any encoding observed (Base64, URL encoding, HTML entities)

### During URL Analysis
- All URLs extracted — **defanged**
- URLScan.io result (screenshot)
- VirusTotal URL result (screenshot or detection count)
- Domain registration age
- Whether it was a legitimate service being abused

### During Attachment Analysis
- Attachment file name
- File type (from `file` command — not just extension)
- MD5, SHA1, SHA256 hashes
- VirusTotal hash result (screenshot or detection count)
- Sandbox result if submitted (Hybrid Analysis / Joe Sandbox)

### During Defense Actions
- Every block applied (sender, domain, IP, URL, hash) with justification
- Scope — how many users received the email
- Whether any users clicked links or opened attachments
- Whether credentials were submitted
- Whether credentials were reset
- Whether endpoint was scanned or reimaged
- Communications sent to users and stakeholders

---

## The Investigation Report Template

See [`/docs/report-template.md`](../docs/report-template.md) for the full copy-paste template.

**Minimum structure:**

```
==============================
PHISHING INVESTIGATION REPORT
==============================

TICKET ID:      [ID]
ANALYST:        [Your name]
DATE OPENED:    [Date]
DATE CLOSED:    [Date]
SEVERITY:       [Low / Medium / High / Critical]

--- REPORTED EMAIL DETAILS ---
Date/Time:        
From:             
Reply-To:         
Return-Path:      
To:               
Subject:          
Message-ID:       
Originating IP:   
Resolved Host:    

--- AUTHENTICATION ---
SPF:    [ ] Pass  [ ] Fail  [ ] Softfail  [ ] None
DKIM:   [ ] Pass  [ ] Fail  [ ] None
DMARC:  [ ] Pass  [ ] Fail  [ ] None   Policy: 

--- URLS (defanged) ---
[list all URLs found]

--- ATTACHMENTS ---
File name:
SHA256:
VirusTotal:

--- DESCRIPTION ---
[What is the email claiming? What does it ask the user to do?
What social engineering tactics are used?]

--- ANALYSIS NOTES ---
Sender analysis:
[findings]

Authentication analysis:
[findings]

URL analysis:
[findings]

Attachment analysis:
[findings]

--- VERDICT ---
[ ] Malicious   [ ] Benign   [ ] Spam   [ ] Inconclusive

Justification:
[Why did you reach this verdict? What evidence supports it?]

--- DEFENSE ACTIONS TAKEN ---
[ ] Sender blocked at gateway
[ ] Domain blocked at gateway
[ ] URL blocked at EDR / proxy
[ ] File hash blocked at EDR
[ ] Emails purged from all inboxes
[ ] Domain reported to registrar
[ ] Affected users notified
[ ] Credentials reset
[ ] Endpoint scanned / reimaged
[ ] Stakeholders notified

Notes:
[Detail every action with justification]
```

---

## Screenshots — Best Practice

Always capture and attach screenshots of:

- VirusTotal results (file hash and/or URL)
- URLScan.io report and screenshot of the page
- Hybrid Analysis / sandbox report
- WHOIS domain age result
- MXToolbox header analysis
- PhishTool analysis dashboard

Screenshots serve as evidence that cannot be reconstructed if the malicious site goes down or VirusTotal results change over time.

---

## Writing Clear Analysis Notes

**Bad example:**
```
URL was malicious. Blocked it.
```

**Good example:**
```
URL Analysis:
The call-to-action button in the email linked to hxxps://dsgo[.]to/redir?url=hxxp://103[.]232[.]55[.]148/
URLScan.io (private scan) confirmed the URL redirects to an IP-based endpoint serving a Windows 
executable file. The IP 103.232.55.148 resolves to a Vietnamese hosting provider with no affiliation 
to the claimed sender (Chase Bank). VirusTotal flagged the URL 6/94 vendors as malicious (phishing).
Domain was registered 1 day prior to email receipt.
Action: URL and base domain blocked at email gateway and EDR. Screenshots attached (VT-001, US-001).
```

---

## Ticket Status Updates

Keep your ticket status current throughout the investigation:

| Status | When to Use |
|---|---|
| **Open / New** | Just assigned, not yet started |
| **In Progress** | Actively investigating |
| **Pending** | Waiting on user response, escalation, or additional info |
| **Resolved** | Investigation complete, all actions taken |
| **Closed** | Confirmed resolved, user notified |

---

## Communicating Verdicts to Users

When closing a phishing report ticket, always notify the user who reported it:

**If malicious:**
```
Hi [Name],

Thank you for reporting this email. Our investigation confirmed it was a phishing attempt.
The email has been removed from all inboxes and the sender has been blocked.

[If they clicked/interacted]: Please [specific action required].

Your vigilance helps protect the organization — thank you.

Security Team
```

**If benign:**
```
Hi [Name],

Thank you for reporting this email. After investigation, we have confirmed it is legitimate.
No further action is required on your part.

Security Team
```

---

*← [18 — Proactive Defense](./18-proactive-phishing-defense.md) | [Next: Additional Practice Resources →](./20-additional-practice-resources.md)*