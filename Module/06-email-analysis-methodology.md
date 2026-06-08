# 06 — Email Analysis Methodology

> **Timestamp:** [3:50:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=13847s)

---

## Overview

Effective phishing analysis requires a **structured, repeatable methodology** that works regardless of the tools available. This framework should be applied consistently to every suspicious email you investigate.

---

## The 7-Step Methodology

### Step 1 — Initial Triage
Quickly assess the potential threat level before diving deep.

**Ask yourself:**
- Who received this email? (One person vs. entire department)
- Is the recipient a high-value target? (Executive, finance, IT admin)
- What is the email claiming? (Urgency indicators?)
- Does anything look obviously suspicious at first glance?

**Goal:** Prioritize your analysis effort. A targeted email to the CFO gets more resources than a generic spam message to one user.

---

### Step 2 — Header & Sender Analysis
Examine email headers to determine the **true origin** of the message.

**Key checks:**
- Where did the email actually originate? (IP address, mail server)
- Does the originating server match the claimed sender domain?
- What do the `Received:` headers reveal?
- Is there a suspicious `Reply-To:` or `Return-Path:`?
- Did the email pass **SPF**, **DKIM**, and **DMARC** checks?

> Covered in depth in [Module 07](./07-header-and-sender-analysis.md)

---

### Step 3 — Content Examination
Analyze the body of the email for suspicious elements.

**Look for:**
- Grammar and spelling errors
- Generic greetings ("Dear Customer")
- Urgency or fear-inducing language
- Social engineering tactics (authority, scarcity, intimidation)
- Brand name misspellings
- Calls to action (click this link, download this file, reply with credentials)

> Covered in depth in [Module 09](./09-email-content-analysis.md)

---

### Step 4 — URL Analysis
Investigate any links embedded in the email.

**Process:**
1. Extract URLs safely (without clicking)
2. Parse the URL structure — identify suspicious components
3. Check domain reputation and registration age
4. Use sandboxing tools to safely preview the destination
5. Defang URLs before documenting

> Covered in depth in [Module 11](./11-email-url-analysis.md)

---

### Step 5 — Attachment Analysis
Safely investigate any attached files.

**Process:**
1. Extract the attachment without opening it
2. Collect file hashes (MD5, SHA1, SHA256)
3. Check hash reputation on VirusTotal / Cisco Talos
4. Submit to sandbox for dynamic analysis if needed
5. Perform static analysis for office documents and PDFs if required

> Covered in depth in [Module 12](./12-email-attachment-analysis.md)

---

### Step 6 — Holistic Analysis & Scope
Consider the broader context and organizational impact.

**Ask:**
- Are there similar emails in the ticket system?
- How many users received this? (Scope determination)
- Can I identify a campaign pattern (same sender, subject, or IoCs)?
- Should I perform a mini threat hunt across the email gateway?

---

### Step 7 — Defense Actions & Documentation
Take action and document everything.

**Reactive actions (if malicious):**
- Block sender address / domain / IP at email gateway
- Block malicious URL/domain at EDR and proxy level
- Block malicious file hashes at EDR level
- Quarantine emails across the organization
- Initiate incident response if users were compromised

**Proactive actions:**
- Update email filter rules with new IoCs
- Report malicious domain to registrar
- Notify affected users
- Assign remedial security awareness training

**Documentation must be:**
- Clear enough for another analyst to reach the same verdict
- Inclusive of all IoCs collected (defanged)
- Inclusive of screenshots of tool outputs
- Updated in the ticketing system throughout the investigation

---

## Documentation Template

See the full template in [`/docs/report-template.md`](../docs/report-template.md)

**Minimum fields to capture:**

```
HEADERS
-------
Date/Time:
From:
To:
Subject:
Message-ID:
Reply-To:
Return-Path:
Originating IP:
Resolved Host (rDNS):

ARTIFACTS
---------
URLs (defanged):
Attachments (name + hashes):

ANALYSIS NOTES
--------------
Sender analysis:
Authentication results (SPF/DKIM/DMARC):
URL analysis:
Attachment analysis:

VERDICT
-------
[ ] Malicious  [ ] Benign  [ ] Inconclusive

DEFENSE ACTIONS
---------------
Actions taken:
```

---

## Key Principle

> **Tools are just tools** — they evolve and change. Focus on mastering the *methodology*. A new tool will always serve the same investigative objectives.

---

*← [05 — Attack Techniques](./05-phishing-attack-techniques.md) | [Next: Header Analysis →](./07-header-and-sender-analysis.md)*
