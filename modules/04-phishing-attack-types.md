# 04 — Phishing Attack Types

> **Timestamp:** [3:19:30](https://www.youtube.com/watch?v=56NDgBOSpUg&t=11970s)

---

## Overview

Phishing attacks come in many forms. Understanding the different types helps us correctly classify incidents, apply the right analysis methodology, and take appropriate defensive actions.

---

## Attack Type Reference

### 1. Information Gathering
**Goal:** Confirm mailbox is active, collect user data via email signature, perform reconnaissance for future attacks.

**Indicators:**
- No URLs to click
- No attachments
- No request for sensitive information
- Generic greeting like "Hi, are you available?"

**Techniques used:**
- Bounce-back analysis (email to non-existent mailbox → confirms valid addresses)
- **Tracking pixels** — 1×1 transparent image embedded in body; when opened, logs the recipient's IP address, timestamp, user agent, and device info

---

### 2. Credential Harvesting
**Goal:** Steal usernames and passwords via a fake login page.

**Indicators:**
- Button or link leading to a fake login page
- Page closely mimics a legitimate service (Microsoft, Google, banking)
- URL does not match the legitimate domain
- POST request sends credentials to attacker-controlled server

**Most common impersonation targets:** Microsoft 365 login, banking portals, cryptocurrency exchanges

---

### 3. Malware Delivery
**Goal:** Deliver a malicious file to infect the victim's machine.

**Common attachment types:**
| Extension | Risk |
|---|---|
| `.exe` | Direct executable |
| `.iso` | Disk image containing malware |
| `.xlsm` / `.docm` | Macro-enabled Office documents |
| `.pdf` | Contains malicious URLs or embedded scripts |
| `.zip` / `.rar` | Archives hiding malicious files |

**Drive-by download:** Malware automatically downloads when a user visits a compromised website — no interaction required.

---

### 4. Spear Phishing
**Goal:** Targeted attack against a specific individual or group.

Requires prior reconnaissance — attacker personalizes the email with:
- Victim's name and role
- References to real colleagues or projects
- Timing aligned with real events (e.g., during a merger)

---

### 5. Whaling
Spear phishing specifically targeting **high-profile individuals** — executives, C-suite, finance directors.

These targets have elevated access and authority to approve financial transactions or access sensitive systems.

---

### 6. Business Email Compromise (BEC)
Also known as **CEO Fraud** or **Email Account Compromise**.

**Two variants:**
- **Spoofed domain** — attacker registers lookalike domain, emails from it
- **Compromised mailbox** — attacker gains access to a real executive mailbox and uses it directly

**Common BEC scenarios:**
- CEO requests urgent gift card purchase
- Finance team instructed to wire funds
- Vendor invoice fraud (fake payment details)

---

### 7. Vishing (Voice Phishing)
Social engineering conducted **over the phone**. Attacker impersonates IT support, banks, or government agencies to extract credentials or perform actions.

---

### 8. Smishing (SMS Phishing)
Phishing via **text messages**. Contains a malicious link or prompts the recipient to reply with sensitive information.

---

### 9. Quishing (QR Code Phishing)
Attacker embeds a malicious URL in a **QR code** distributed via email, physical flyers, or fake posters. Victim scans and is redirected to a credential capture page.

QR codes can be generated in seconds using free online tools — trivial for attackers to produce.

---

### 10. Spam
Unsolicited bulk email — not always directly malicious but can:
- Contain information-gathering pixels
- Serve as a precursor to targeted attacks
- Deliver low-sophistication malware

---

## Classification Summary

| Type | Method | Primary Goal |
|---|---|---|
| Information Gathering | Blank/generic email | Reconnaissance |
| Credential Harvesting | Fake login page link | Steal credentials |
| Malware Delivery | Malicious attachment | Compromise endpoint |
| Spear Phishing | Targeted, personalized | Any of the above |
| Whaling | Targets executives | Financial fraud / data access |
| BEC | Spoofed/compromised exec account | Wire transfer fraud |
| Vishing | Phone call | Credential theft / action |
| Smishing | SMS | Credential theft / malware |
| Quishing | QR code | Redirect to malicious site |
| Spam | Bulk unsolicited email | Advertising / recon |

---

*← [03 — Lab Setup](./03-lab-setup-and-config.md) | [Next: Attack Techniques →](./05-phishing-attack-techniques.md)*