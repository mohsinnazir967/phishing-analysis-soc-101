# 12 — Email Attachment Analysis

> **Timestamp:** [5:18:27](https://www.youtube.com/watch?v=56NDgBOSpUg&t=19107s)

---

## Overview

Malicious email attachments are one of the most common phishing delivery mechanisms. This module covers how to safely extract attachments, collect file hashes, and perform reputation checks — without ever executing the file.

---

## File Hashes — The Core Artifact

A **file hash** is a unique fixed-length fingerprint generated from a file's content. Even a one-byte change produces a completely different hash.

| Algorithm | Length | Use Case |
|---|---|---|
| MD5 | 32 chars | Quick identification (not collision-proof) |
| SHA1 | 40 chars | Legacy compatibility |
| SHA256 | 64 chars | Preferred for reputation checks |

**Collect all three** — it takes seconds and covers all reputation databases.

---

## Step 1 — Extract the Attachment

### Method 1 — Thunderbird GUI
1. Right-click the attachment in the email
2. Click **Save As**
3. Save to your working directory
4. **Do NOT open or execute the file**

### Method 2 — email-dump.py (Command Line)
```bash
# List all MIME parts in the email
python3 email-dump.py sample1.eml

# Output example:
# Index 1: headers
# Index 2: HTML body
# Index 3: text/plain body
# Index 4: quotation.iso  ← attachment

# Extract the attachment (index 4)
python3 email-dump.py -s 4 -d sample1.eml > quotation.iso
```

### Method 3 — IOC Extractor Script
```bash
python3 eioc.py sample1.eml
# Automatically lists attachment names and hashes — no manual extraction needed
```

---

## Step 2 — Collect File Hashes

### On Linux / Ubuntu
```bash
# Individual hashes
sha256sum quotation.iso
sha1sum quotation.iso
md5sum quotation.iso

# All three at once
sha256sum quotation.iso && sha1sum quotation.iso && md5sum quotation.iso
```

### On Windows (PowerShell)
```powershell
# SHA256 (default)
Get-FileHash quotation.iso

# MD5
Get-FileHash quotation.iso -Algorithm MD5

# SHA1
Get-FileHash quotation.iso -Algorithm SHA1

# All three at once
Get-FileHash quotation.iso; Get-FileHash quotation.iso -Algorithm MD5; Get-FileHash quotation.iso -Algorithm SHA1
```

---

## Step 3 — File Reputation Checks

### VirusTotal — Primary Check
```
1. Go to virustotal.com
2. Click the Search tab
3. Paste the SHA256 hash
4. Review vendor detections
```

**Upload the file vs. hash:**
- Prefer **hash submission** — does not expose file contents to third parties
- If hash returns no results → consider file upload after confirming no sensitive content
- Anyone with a VirusTotal Enterprise account can download submitted files

**Key fields to document:**
- Detection ratio (e.g., `43/61 vendors`)
- Malware family names (e.g., Trojan.Backdoor, Lokibot)
- First submission date
- Associated file names from other submissions

---

### Cisco Talos Intelligence
```
1. Go to talosintelligence.com
2. Paste SHA256 hash into the search bar
3. Review verdict and detection aliases
```

Good secondary check — often provides different context than VirusTotal.

---

## Step 4 — Verify File Type

Never trust the file extension — attackers use misleading names. Verify the true file type:

```bash
file quotation.iso
# Output: quotation.iso: ISO 9660 CD-ROM filesystem data

file invoice.pdf
# Output: invoice.pdf: PDF document, version 1.4

file malware.doc
# Output: malware.doc: Composite Document File V2 Document
```

---

## Common Malicious Attachment Types

| Extension | True Type | Risk |
|---|---|---|
| `.iso` | Disk image | Contains hidden executable — auto-mounts on Windows |
| `.exe` | Executable | Direct malware execution |
| `.xlsm` / `.docm` | Macro-enabled Office | VBA macros execute on open |
| `.pdf` | PDF document | Embedded URLs, JavaScript, or files |
| `.lnk` | Windows shortcut | Can execute PowerShell payloads |
| `.js` | JavaScript | Executes in Windows Script Host |
| `.vbs` | VBScript | Executes malicious scripts |
| `.hta` | HTML Application | Executes scripts via mshta.exe |
| `.zip` / `.rar` | Archive | Contains malicious files inside |

---

## Documentation Checklist

For every attachment investigated:

```
Attachment name:    quotation.iso
File type (file):   ISO 9660 CD-ROM filesystem
MD5:                [hash]
SHA1:               [hash]
SHA256:             [hash]
VirusTotal result:  43/61 — Trojan.Backdoor (screenshot attached)
Cisco Talos:        Malicious (screenshot attached)
Verdict:            MALICIOUS
```

---

## Important Notes

- **Never open or execute** a suspected malicious file on your host machine
- Use a **sandboxed VM** or online sandbox service for any dynamic analysis
- Hashes do **not need to be defanged** — they are just hex strings
- If hash returns no results, the file may be new/novel — escalate and use dynamic sandbox analysis (covered in Module 13)

---

*← [11 — URL Analysis](./11-email-url-analysis.md) | [Next: Dynamic Analysis & Sandboxing →](./13-dynamic-analysis-sandboxing.md)*
