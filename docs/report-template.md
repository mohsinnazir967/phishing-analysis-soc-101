# Phishing Investigation Report Template

> Copy this template into your ticketing system for every phishing investigation.  
> Fill in each field as you work through the analysis — do not leave sections blank.

---

```
╔══════════════════════════════════════════════════════════════╗
║           PHISHING INVESTIGATION REPORT                      ║
╚══════════════════════════════════════════════════════════════╝

TICKET ID:        [Ticket number from your TMS]
ANALYST:          [Your name]
DATE OPENED:      [YYYY-MM-DD HH:MM UTC]
DATE CLOSED:      [YYYY-MM-DD HH:MM UTC]
SEVERITY:         [ ] Low   [ ] Medium   [ ] High   [ ] Critical
REPORTED BY:      [User name / automated rule / gateway alert]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 1 — EMAIL HEADERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Date/Time:          
From:               
Reply-To:           
Return-Path:        
To:                 
CC / BCC:           
Subject:            
Message-ID:         
Originating IP:     
Resolved Host:      

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 2 — AUTHENTICATION RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SPF:    [ ] Pass   [ ] Fail   [ ] Softfail   [ ] Neutral   [ ] None
DKIM:   [ ] Pass   [ ] Fail   [ ] None
DMARC:  [ ] Pass   [ ] Fail   [ ] None       Policy: [ ] none  [ ] quarantine  [ ] reject

Notes:
[Any inconsistencies or notable findings from authentication checks]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 3 — URLs (all defanged)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

URL 1:
URL 2:
URL 3:
[add more as needed]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 4 — ATTACHMENTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

File Name:
True File Type (file cmd):
MD5:
SHA1:
SHA256:
VirusTotal Result:
[add more rows as needed]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 5 — EMAIL DESCRIPTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Describe what the email is claiming, what it asks the user to do,
and which social engineering tactics are being used:]

What the email claims:


What it asks the user to do:


Social engineering tactics identified:
[ ] Urgency / time pressure
[ ] Authority impersonation
[ ] Fear / intimidation
[ ] Trust building
[ ] Scarcity / FOMO
[ ] Familiarity / known contact
[ ] Other: _______________

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 6 — ANALYSIS NOTES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SENDER ANALYSIS:
[Document your header investigation findings. What did the rDNS
reveal? Does the originating IP match the claimed sender domain?
What do SPF/DKIM/DMARC results indicate?]


URL ANALYSIS:
[For each URL: what tool was used, what was found, what is the
domain age, was it flagged? Include detection counts.]


ATTACHMENT ANALYSIS:
[Hash results, VirusTotal detections, sandbox findings, static
analysis findings if performed.]


SCOPE:
[How many users received this email? Did anyone click a link
or open an attachment? Did anyone submit credentials?]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 7 — VERDICT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VERDICT:  [ ] Malicious   [ ] Benign   [ ] Spam   [ ] Inconclusive

JUSTIFICATION:
[State clearly why you reached this verdict. Reference specific
evidence — originating IP mismatch, VirusTotal detections,
credential capture page identified, etc. Be thorough enough
that another analyst can verify your conclusion.]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 8 — DEFENSE ACTIONS TAKEN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] Email quarantined from all affected inboxes
[ ] Sender address blocked at email gateway
[ ] Sender domain blocked at email gateway
[ ] Originating IP blocked at email gateway
[ ] Malicious URL blocked at EDR / proxy
[ ] Malicious domain blocked at EDR / proxy
[ ] Malicious file hash blocked at EDR
[ ] All instances purged from mail server
[ ] Malicious domain reported to registrar
[ ] Affected users notified (see communication log below)
[ ] Credentials reset — [list affected accounts]
[ ] Endpoint scanned — [list affected endpoints]
[ ] Endpoint reimaged — [list affected endpoints]
[ ] Stakeholders notified — [list names/roles]
[ ] Post-incident review scheduled

ACTION NOTES:
[Detail every action taken with justification and timestamps.
Example: "2024-03-15 14:32 UTC — Domain malicious[.]com blocked
at email gateway (rule ID: PHI-2024-031). Evidence: VirusTotal
43/61, URLScan screenshot attached (US-001)."]


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 9 — SCREENSHOTS / EVIDENCE ATTACHED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] VT-001 — VirusTotal hash result
[ ] VT-002 — VirusTotal URL result
[ ] US-001 — URLScan.io report screenshot
[ ] US-002 — URLScan.io page screenshot
[ ] HA-001 — Hybrid Analysis sandbox report
[ ] WH-001 — WHOIS domain age result
[ ] MX-001 — MXToolbox header analysis
[ ] PT-001 — PhishTool analysis dashboard
[ ] Other: _______________

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 10 — USER COMMUNICATION LOG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Date/Time] — Notified [user] of verdict via [email/phone/ticket]
[Date/Time] — [User] confirmed [action taken / no action needed]
[Date/Time] — Ticket closed and resolved

```

---

## IoC Defanging Quick Reference

| Type | Original | Defanged |
|---|---|---|
| URL | `https://malicious.com/phish` | `hxxps://malicious[.]com/phish` |
| Domain | `malicious.com` | `malicious[.]com` |
| IP Address | `185.220.101.45` | `185[.]220[.]101[.]45` |
| Email address | `attacker@evil.com` | `attacker[@]evil[.]com` |
| File hash | `d41d8cd98f00b204...` | No defanging needed |

---

## Verdict Definitions

| Verdict | When to Use |
|---|---|
| **Malicious** | Clear evidence of phishing intent — spoofed sender, credential capture page, malicious attachment confirmed |
| **Benign** | All checks pass — email is confirmed legitimate |
| **Spam** | Unsolicited bulk email — not directly malicious but unwanted |
| **Inconclusive** | Insufficient evidence to confirm either way — escalate or continue monitoring |

---

*Part of the [SOC 101 Phishing Analysis Repository](../README.md)*
