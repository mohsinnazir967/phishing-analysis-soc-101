# 16 — Automated Email Analysis with PhishTool

> **Timestamp:** [6:13:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=22427s)

---

## Overview

PhishTool is a web-based phishing analysis platform that automates much of the manual artifact extraction and enrichment work covered in previous modules. It is a powerful triage accelerator — though it does not replace the need to understand the underlying methodology.

---

## What PhishTool Does

- Automatically extracts key email headers
- Performs reverse DNS lookup on the originating IP
- Flags `Reply-To` / `Return-Path` inconsistencies
- Displays received header hops in a clean table
- Extracts X-headers
- Runs SPF / DKIM / DMARC authentication checks
- Extracts URLs from the email body
- Extracts attachment names and hashes
- Optionally integrates with VirusTotal API for automated reputation checks
- Generates a structured analysis report
- Allows analysts to assign a verdict and flag IoCs

---

## Access

**Free tier available** — no business email required.

```
URL: https://app.phishtool.com
Sign up with any Gmail, Yahoo, or Hotmail account
```

---

## Step-by-Step Walkthrough

### Step 1 — Upload an Email
1. Log in to PhishTool
2. Click **Analyze** or the blue **Analyze** button
3. Drag and drop an `.eml` file OR click **Choose File**
4. PhishTool automatically processes the email

---

### Step 2 — Review the Analysis Dashboard

**Right panel — Email Preview:**
- **HTML** tab — rendered email view
- **Source** tab — full raw email source

**Left panel — Extracted Artifacts:**

| Field | What to Check |
|---|---|
| Subject | Document for gateway searches |
| From | Claimed sender — is it spoofed? |
| To | Recipient scope |
| Date/Time | Timeline documentation |
| Reply-To | Does it match From? ⚠️ if not |
| Return-Path | Does domain match From? ⚠️ if not |
| Originating IP | True sender IP |
| RDNS | Reverse DNS of originating IP |

> **Exclamation marks (⚠️)** next to Reply-To or Return-Path indicate inconsistency with the From domain — automatic red flag detection.

---

### Step 3 — Review Received Lines Tab
Clean table view of all MTA hops with timestamps, sending server, and receiving server — eliminates the need to manually parse the received chain.

---

### Step 4 — Review X-Headers Tab
Automatically extracts all custom / extended headers including `X-Sender-IP`, `X-Originating-IP`, and any security appliance headers.

---

### Step 5 — Review Security Tab
Summarizes authentication results:
- **SPF:** Pass / Fail / Softfail
- **DKIM:** Pass / Fail
- **DMARC:** Pass / Fail + policy (none / quarantine / reject)

---

### Step 6 — Review Attachments Tab
For emails with attachments, PhishTool automatically extracts:
- File name
- MD5 hash
- SHA1 hash
- SHA256 hash

If VirusTotal API is configured → automatic reputation lookup is shown here.

---

### Step 7 — Review Message URLs Tab
All URLs extracted from the email body — no manual CyberChef extraction needed.

If VirusTotal API is configured → automatic URL reputation shown.

---

## Configuring VirusTotal Integration

1. Go to [virustotal.com](https://virustotal.com) → Sign in → API Key (free tier available)
2. In PhishTool → Settings → paste your VirusTotal API key
3. All future attachment hashes and URLs will be automatically checked

---

## Assigning a Verdict

Once analysis is complete:

1. Click **Resolve** in the top right
2. Set **Email Disposition:** Malicious / Benign / Spam / Inconclusive
3. Click **Flagged Artifacts** — select which artifacts caused the determination:
   - Reply-To mismatch
   - Return-Path mismatch
   - Malicious URL
   - Malicious attachment
4. Under **Classification Codes** — select techniques used:
   - Spoofing
   - Credential harvesting
   - Malware delivery
   - BEC
5. Click **Resolve** to close the case

---

## PhishTool vs. Manual Analysis

| Task | Manual | PhishTool |
|---|---|---|
| Header extraction | Sublime Text + reading | Automatic |
| rDNS lookup | whois / DomainTools | Automatic |
| Received chain parsing | Manual bottom-up reading | Clean table view |
| SPF/DKIM/DMARC check | MXToolbox | Automatic |
| URL extraction | CyberChef | Automatic |
| Hash extraction | email-dump.py + sha256sum | Automatic |
| VirusTotal check | Manual copy-paste | Automatic (with API key) |
| Report generation | Manual template | Built-in |

---

## When to Use PhishTool

- **High alert volume** — speeds up triage significantly
- **Standardised reporting** — consistent format across all analysts
- **Junior analyst support** — guided workflow reduces errors

**Still do manual analysis when:**
- PhishTool misses something (encoding tricks, deeply nested MIME)
- You need to perform deeper URL or attachment investigation
- You are learning — manual analysis builds the fundamental skills

---

## Important Reminder

PhishTool is a **tool** — it automates the *collection* of artifacts, but the *analysis and verdict* still require human judgment. A clean PhishTool report does not mean the email is safe.

---

*← [15 — Static PDF Analysis](./15-static-pdf-analysis.md) | [Next: Reactive Phishing Defense →](./17-reactive-phishing-defense.md)*