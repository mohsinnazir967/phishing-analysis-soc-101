# 17 — Reactive Phishing Defense

> **Timestamp:** [6:19:58](https://www.youtube.com/watch?v=56NDgBOSpUg&t=22798s)

---

## Overview

Reactive defense covers the actions taken **after** a phishing email has been identified and classified as malicious. The goal is to contain the threat, remove it from the environment, and prevent further impact.

---

## Step 1 — Determine Scope

Before taking any blocking action, determine how many users received the email.

**Methods:**
- Check `To:`, `CC:`, `BCC:` headers (BCC recipients won't appear — search the gateway)
- Perform a **message trace** at the email gateway using:
  - Sender address
  - Subject line
  - Timestamp
  - Message-ID

**In Microsoft 365:**
- Security & Compliance Center → Message Trace
- Or use PowerShell: `Get-MessageTrace`

**In Google Workspace:**
- Admin Console → Reports → Email Log Search

---

## Step 2 — Quarantine Malicious Emails

Isolate the phishing email so no further users can interact with it.

- **Microsoft 365:** Security & Compliance → Threat Management → Review → Quarantine
- **Google Workspace:** Admin Console → Gmail → Manage quarantine
- Remove from all affected inboxes once quarantined

---

## Step 3 — Block Sender Artifacts

Prevent future emails from the same campaign reaching your users.

### Block by Email Address
Most specific — but attackers change addresses frequently.

### Block by Domain
Effective against campaigns using a dedicated malicious domain.
> ⚠️ Never block legitimate platforms like gmail.com or outlook.com even if abused.

### Block by IP Address
Block the originating mail server IP.
> ⚠️ Attackers can change IPs easily — use alongside other blocks.

### Block by Subject Line
Useful for campaigns with a distinctive subject.
> ⚠️ Easy for attackers to change — use as supplemental only.

---

## Step 4 — Block Web Artifacts

Prevent users from accessing malicious URLs identified during analysis.

### At the Email Gateway
Create a mail flow rule to block incoming emails containing the malicious URL:
- Exchange / M365: Mail Flow → Rules → Block if body contains [URL]

### At the EDR
Block the malicious domain at the endpoint level so no device on the network can reach it.

### At the Web Proxy / Firewall
If organization uses a web proxy → add domain to blocklist.

**Before blocking a domain, always ask:**
- Is this a legitimate service being abused (e.g., Google Drive)? → Block the specific path, not the whole domain
- Will blocking this disrupt business operations?
- Is the domain confirmed malicious with sufficient evidence?

---

## Step 5 — Block File Artifacts

Prevent malicious files from being downloaded or executed on endpoints.

### Block by File Hash (EDR)
Submit MD5 / SHA1 / SHA256 hashes to your EDR platform → it will auto-quarantine any matching file detected on endpoints.

### Block by File Name
Less reliable — easy to rename. Use hash blocking instead when possible.

---

## Step 6 — Eradication

Remove all traces of the phishing campaign from your environment.

- **Delete all instances** of the phishing email from all mailboxes (gateway-level content search + purge)
- **Remove downloaded attachments** from any endpoints where the file was opened
- **Leverage EDR** to quarantine and delete malicious files remotely

**Microsoft 365 PowerShell purge:**
```powershell
New-ComplianceSearchAction -SearchName "PhishingSearch" -Purge -PurgeType SoftDelete
```

---

## Step 7 — Report Malicious Domains

Report phishing domains to their registrar and relevant abuse channels.

**How to find abuse contact:**
1. Look up domain on [whois.domaintools.com](https://whois.domaintools.com)
2. Find **Registrar Abuse Contact Email** field
3. Or search: `[registrar name] abuse report form`

**GoDaddy example:**
1. Search "GoDaddy abuse report"
2. Select **Phishing** as abuse type
3. Submit: full URL, brand being impersonated, evidence

**Other reporting channels:**
- [Google Safe Browsing](https://safebrowsing.google.com/safebrowsing/report_phish/)
- [PhishTank — Submit](https://www.phishtank.com/add_web_phish.php)
- [APWG eCrime](https://apwg.org/reportphishing/)

---

## Step 8 — Credential Reset (If User Was Compromised)

If a user clicked a credential capture link and submitted credentials:

- Immediately **disable the account**
- **Reset password** and rotate any associated tokens / API keys
- Review **login history** for unauthorized access
- Check for **email forwarding rules** added by attacker
- Scan the endpoint for malware
- Consider **full reimaging** if malware was executed

---

## Step 9 — Communicate With Users and Stakeholders

Notify affected users — clearly and without blame:

```
Subject: Phishing Email Notification — Action Required

Hi [Name],

You recently received a phishing email with the subject "[Subject]" sent on [Date].
This email has been identified as malicious and removed from your inbox.

[If they clicked/submitted]: Please [action required — password reset, etc.]

If you have any questions, contact the security team at [contact].
```

Escalate to management or stakeholders if:
- A high-value user was targeted (executive, finance, IT admin)
- Credentials were submitted or malware was executed
- The incident meets your organization's escalation threshold

---

## Step 10 — Recovery

Restore affected systems and users to normal operations:

- Restore from clean backups if systems were compromised
- Reimage endpoints where malware executed
- Re-enable accounts after credential reset and validation
- Conduct post-incident review — what happened, how far did it go, what can be improved?

---

## Reactive Defense Checklist

```
[ ] Scope determined — how many users received it?
[ ] Email quarantined from all affected inboxes
[ ] Sender address / domain / IP blocked at gateway
[ ] Malicious URL / domain blocked at EDR and proxy
[ ] Malicious file hashes blocked at EDR
[ ] Malicious domain reported to registrar
[ ] All instances purged from email server
[ ] Affected users notified
[ ] Credentials reset (if user submitted to credential capture page)
[ ] Endpoint scanned / reimaged (if malware executed)
[ ] Stakeholders notified (if threshold met)
[ ] Post-incident review scheduled
[ ] All actions documented in ticket
```

---

*← [16 — PhishTool](./16-automated-analysis-phishtool.md) | [Next: Proactive Defense →](./18-proactive-phishing-defense.md)*