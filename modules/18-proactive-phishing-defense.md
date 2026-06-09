# 18 — Proactive Phishing Defense

> **Timestamp:** [6:47:24](https://www.youtube.com/watch?v=56NDgBOSpUg&t=24444s)

---

## Overview

Proactive defense focuses on preventing phishing emails from reaching users in the first place, and reducing the impact when they do get through. These measures supplement reactive response — defense in depth applied to email security.

---

## 1. Email Filtering & Security Appliances

Deploy dedicated email security solutions to detect and block phishing before delivery.

**Commercial options:**
- Proofpoint
- Mimecast
- Barracuda
- Microsoft Defender for Office 365
- Symantec Email Security

**Detection capabilities these solutions use:**
- Heuristic analysis and pattern matching
- Machine learning on email behavior
- Attachment sandboxing
- URL rewriting and scanning
- Threat intelligence feeds
- DMARC enforcement

---

## 2. Mark External Emails

Prepend a visual warning banner to all emails originating from outside the organization. This helps users quickly distinguish internal from external senders.

**Exchange / Microsoft 365 — Mail Flow Rule:**

**Conditions:**
- Sender is located: Outside the organization
- Recipient is located: Inside the organization

**Actions:**
- Prepend subject with: `[EXTERNAL]`
- Prepend disclaimer to message body (HTML banner)

**Example HTML disclaimer:**
```html
<div style="background:#ff4444;color:#fff;padding:8px;font-family:Arial;font-size:13px;">
  ⚠️ <strong>EXTERNAL EMAIL:</strong> This email originated from outside the organization.
  Do not click links or open attachments unless you are expecting them.
</div>
```

---

## 3. Block Recently Registered Domains

Attackers almost always use **newly registered domains** for phishing campaigns — setting up credible infrastructure takes time, so they don't bother.

**Strategy:** Block all email from domains registered within the past 30 days.

- Very few legitimate business emails come from brand-new domains
- Exceptions can be whitelisted on a case-by-case basis with documentation
- Many commercial email security appliances have this capability built in

**Check domain age manually:**
```
whois.domaintools.com → paste domain → check "Created" date
```

---

## 4. URL Scanning & Rewriting

**URL rewriting:** Commercial email security tools rewrite all URLs in incoming emails. When a user clicks, the link passes through a proxy that:
- Scans the destination in real time
- Blocks if malicious
- Allows if clean

**DNS-based blocking:** Block known malicious domains at the DNS resolver level — any device on the network trying to resolve the domain gets blocked before the connection is made.

---

## 5. Attachment Filtering

Block dangerous file types at the email gateway before they reach user inboxes.

**High-risk extensions to consider blocking:**

| Category | Extensions |
|---|---|
| Executables | `.exe`, `.com`, `.bat`, `.cmd`, `.scr` |
| Scripts | `.vbs`, `.js`, `.ps1`, `.wsf`, `.hta` |
| Disk images | `.iso`, `.img`, `.vhd` |
| Macro-enabled | `.xlsm`, `.docm`, `.pptm`, `.xlsb` |
| Shortcuts | `.lnk` |

**Recommended approach:** Use an **allowlist** of file types employees actually need (`.pdf`, `.docx`, `.xlsx`, `.png`, `.jpg`, `.zip`) rather than a denylist — the allowlist is much shorter.

---

## 6. Enforce Email Authentication (SPF / DKIM / DMARC)

Configure your own domain's email authentication to prevent attackers from spoofing your domain to target others — and to enforce rejection of spoofed inbound emails.

**DMARC rollout best practice:**
1. Start with `p=none` — monitor only, collect reports
2. Review reports — identify legitimate email sources
3. Move to `p=quarantine` — send failing emails to spam
4. Move to `p=reject` — outright reject unauthenticated email

```dns
; Example DMARC record
_dmarc.yourdomain.com TXT "v=DMARC1; p=reject; rua=mailto:dmarc@yourdomain.com"
```

---

## 7. Attachment Sandboxing

Some email security platforms automatically detonate attachments in a sandbox before delivery — flagging malicious behavior before the email reaches the inbox.

This provides automated dynamic analysis (similar to Module 13) at scale for every incoming attachment.

---

## 8. User Security Awareness Training

Humans are the last line of defense — train them to be effective at that role.

**Security awareness training platforms:**
- KnowBe4
- Proofpoint Security Awareness (Wombat)
- Huntress Security Awareness Training
- Cofense
- Barracuda PhishLine

**Training should cover:**
- How to recognize phishing emails
- What to do when you receive a suspicious email
- How to report to the security team
- Password hygiene and MFA

**Frequency:** At minimum annually — quarterly is better. New joiners should complete training within first 30 days.

---

## 9. Phishing Simulations

Run controlled phishing simulations to measure how susceptible your organization is — and identify who needs additional training.

**How it works:**
1. Security team (or awareness platform) sends fake phishing emails to employees
2. Platform tracks who opens, clicks, or submits credentials
3. Users who fall for it are automatically enrolled in targeted remedial training
4. Results are reported to management over time — track improvement

**Simulation types to run:**
- Generic credential harvest
- Spear phishing (personalized)
- Attachment delivery
- Smishing / QR code phishing (quishing)

---

## 10. Easy Reporting Mechanism for Users

Make it simple for employees to report suspicious emails — if it's difficult, they won't do it.

**Options:**
- **Report Phishing button** integrated directly into Outlook / Gmail
  - Microsoft: Phish Alert Button (KnowBe4) or built-in Microsoft Report Message add-in
  - Google: Report Phishing option in Gmail menu
- Dedicated phishing mailbox: `phishing@yourcompany.com`
- Ticket portal with a clear "Report Suspicious Email" option

**What happens after a report:**
- Email automatically forwarded as attachment to SOC mailbox
- Ticket automatically created in your ticketing system
- Analyst triages and investigates using the methodology in Module 06

---

## Proactive Defense Checklist

```
[ ] Email security appliance deployed and tuned
[ ] External email warning banners configured
[ ] Recently registered domain blocking enabled (< 30 days)
[ ] High-risk attachment types blocked at gateway
[ ] URL rewriting / scanning enabled
[ ] SPF, DKIM, and DMARC configured for your domain
[ ] DMARC policy at p=quarantine or p=reject
[ ] Attachment sandboxing enabled (if available)
[ ] Security awareness training programme in place
[ ] Phishing simulations running on a regular cadence
[ ] Easy phishing report mechanism available to all users
```

---

*← [17 — Reactive Defense](./17-reactive-phishing-defense.md) | [Next: Documentation & Reporting →](./19-documentation-and-reporting.md)*







