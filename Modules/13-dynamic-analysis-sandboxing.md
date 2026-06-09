# 13 — Dynamic Attachment Analysis & Sandboxing

> **Timestamp:** [5:33:06](https://www.youtube.com/watch?v=56NDgBOSpUg&t=19986s)

---

## Overview

When a file hash returns no results or you need deeper behavioral insight, **dynamic analysis** (sandboxing) lets you safely observe what a file *actually does* when executed — without risking your own machine.

---

## What to Look For in Dynamic Analysis

| Category | What to Observe |
|---|---|
| **Process Activity** | What processes are spawned? What are parent-child relationships? |
| **Registry Changes** | Are persistence mechanisms being written to the registry? |
| **Network Connections** | Is the file calling home to a C2 server? What IPs/domains? |
| **File Activity** | Is it dropping files to disk? Modifying existing files? |

---

## Sandboxing Tools

### 1. Hybrid Analysis (Recommended — Free & Accessible)
Powered by the **CrowdStrike Falcon Sandbox**.

**URL:** [hybrid-analysis.com](https://www.hybrid-analysis.com)

**Steps:**
1. Drag and drop file OR click **Upload Sample**
2. Select OS (Windows 10 64-bit recommended)
3. Click **Generate Public Report**
4. Wait for analysis to complete

**Key report sections:**
- **Antivirus results** — multi-engine detection
- **Malicious indicators** — behavioural findings with severity
- **Process tree** — parent-child process relationships
- **Network analysis** — DNS queries, HTTP requests, contacted IPs
- **Screenshots** — what the sandbox machine looked like during execution
- **Extracted files** — any files dropped during execution

> ⚠️ Public reports are visible to anyone. Do not submit files containing sensitive organizational data.

---

### 2. Joe Sandbox
Advanced analysis engine supporting Windows, macOS, and Android.

**URL:** [joesandbox.com](https://www.joesandbox.com)

**Requirements:** Business email required for free tier registration.

**Features over Hybrid Analysis:**
- **Live interaction** — interact with the sandbox in real time during execution
- Useful when a file requires user interaction (clicking a button, entering a password)
- Deep malware analysis report with full behaviour graph

**Steps:**
1. Upload file → select Windows 10
2. Check **Live Interaction** (optional)
3. Check **Generate Deep Analysis Report**
4. Click **Analyze**
5. Interact with sandbox if needed → click Stop when done
6. Review full report — signatures, IPs, domains, URLs, file drops

---

### 3. Any.Run
Interactive sandbox — requires business email for free tier.

**URL:** [any.run](https://any.run)

**Key feature:** Real-time visual sandbox you can interact with live.

**Steps:**
1. Click **Analyze Files**
2. Upload file → select **Pro Mode**
3. Configure OS, network, privacy settings
4. Click **Run Public Analysis**
5. Interact with sandbox during execution
6. Review IoCs, DNS requests, network connections, behaviour graph

---

## Reading a Sandbox Report — Key Indicators

### Malicious Process Patterns
```
winword.exe → cmd.exe           ← Word spawning command prompt (suspicious)
winword.exe → powershell.exe    ← Word spawning PowerShell (very suspicious)
explorer.exe → mshta.exe        ← HTA application execution
svchost.exe → wscript.exe       ← VBScript execution
```

### Persistence Indicators
```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run  ← registry run key
Scheduled task creation via schtasks.exe
Startup folder file drops
```

### Network Indicators
```
DNS query to: tt.vg             ← URL shortener used as C2
HTTP GET to: 192.168.x.x/.exe  ← downloading second stage payload
POST to: attacker-domain.com    ← data exfiltration
```

### File Drop Indicators
```
%TEMP%\crypted_dll.bin          ← encrypted payload dropped
%APPDATA%\malware.exe           ← persistence executable
C:\Windows\Temp\update.ps1      ← PowerShell script dropped
```

---

## Worked Example — Malicious DOCX Analysis

**File submitted:** `bank_payment.docx`  
**Platform:** Hybrid Analysis

**Findings:**
- CVE matched: office document with OLE2 embedded link object
- `winword.exe` → HTTP GET request to `tt.vg` (URL shortener) → fetches fake RTF file
- Fake RTF contains embedded malicious script
- `winword.exe` spawns `mshta.exe` → executes embedded script
- Scheduled task created for persistence
- `powershell.exe` spawned with AV exclusion commands
- C2 domain contacted: `tt.vg` (identified as URL shortener abused for C2)

**IoCs extracted:**
```
File hash (SHA256): [hash]
C2 domain (defanged): tt[.]vg
Dropped file: crypted_dll[.]bin
Scheduled task name: [name]
```

---

## When to Use Dynamic Analysis

| Situation | Action |
|---|---|
| Hash has no VirusTotal results | Submit to sandbox |
| File behaviour needed for full IR | Submit to sandbox |
| Attachment came from Google Drive | Download + sandbox |
| Password-protected archive | Submit with password to sandbox |
| Need live interaction (click button) | Use Joe Sandbox or Any.Run |
| Macro-enabled Office doc | Static analysis first (Module 14), then sandbox |

---

## Important Safety Rules

- **Never** run suspected malware on your host machine
- **Always** use an isolated VM or online sandbox
- Be aware that **public sandbox reports** expose file content and screenshots
- Sandboxing is a **powerful supplemental tool** — not a replacement for hash checks and static analysis

---

*← [12 — Attachment Analysis](./12-email-attachment-analysis.md) | [Next: Static MalDoc Analysis →](./14-static-maldoc-analysis.md)*
