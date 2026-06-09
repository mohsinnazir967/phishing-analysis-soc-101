# 15 — Static PDF Analysis

> **Timestamp:** [6:03:00](https://www.youtube.com/watch?v=56NDgBOSpUg&t=21780s)

---

## Overview

PDFs are a commonly trusted file format — and therefore a common phishing attachment. This module covers how to safely analyze PDF files, extract embedded URLs, and identify suspicious components without opening the file in a PDF viewer.

---

## Why PDFs Are Used in Phishing

- Widely trusted by end users and organizations
- Email gateways often allow PDF attachments by default
- Can contain clickable URLs that bypass email-level URL filtering
- Can contain embedded scripts (JavaScript) or even embedded files
- Users are accustomed to receiving PDFs for invoices, policies, and forms

---

## Most Common PDF Phishing Technique

The **simplest and most common** approach: a PDF containing a clickable URL that leads to a phishing page or drive-by download.

The attacker avoids embedding malware directly — the PDF itself is clean. The malicious content lives at the URL destination.

**Why this works:**
- Email security tools may not extract and scan URLs embedded inside PDFs
- Users trust PDFs and click links without scrutiny
- The PDF can be visually convincing (blurred preview, "click to unlock" prompt)

---

## Tools — Didier Stevens Suite

### pdf-parser.py
Parses the internal structure of a PDF and extracts objects, streams, and embedded content.

### pdfid.py
Lightweight tool that gives a high-level overview of a PDF's structure — quickly identifies suspicious components.

**Install / locate:**
```bash
# Available in the course tools/ directory, or download:
wget https://raw.githubusercontent.com/DidierStevens/DidierStevensSuite/master/pdf-parser.py
wget https://raw.githubusercontent.com/DidierStevens/DidierStevensSuite/master/pdfid.py
```

---

## Step 1 — Quick Overview with pdfid.py

```bash
python3 pdfid.py statement.pdf
```

**Example output:**
```
PDFiD 0.2.8 statement.pdf
 PDF Header: %PDF-1.4
 obj                   14
 endobj                14
 stream                 3
 endstream              3
 xref                   1
 trailer                1
 startxref              1
 /Page                  2
 /Encrypt               0
 /ObjStm                0
 /JS                    1    ← JavaScript present
 /JavaScript            1    ← JavaScript present
 /AA                    0
 /OpenAction            1    ← action runs on open
 /AcroForm              0
 /JBIG2Decode           0
 /RichMedia             0
 /Launch                1    ← can launch external software
 /EmbeddedFile          1    ← embedded file present
 /XFA                   0
 /Colors > 2^24         0
```

### Suspicious Fields to Flag

| Field | Risk |
|---|---|
| `/JS` or `/JavaScript` | Embedded JavaScript — can steal data, make connections, execute programs |
| `/OpenAction` | Action triggered when PDF is opened |
| `/Launch` | Can launch external software or commands |
| `/EmbeddedFile` | File embedded inside the PDF |
| `/AA` | Additional actions — triggered by various PDF events |

---

## Step 2 — Extract URLs with pdf-parser.py

### Basic Parse
```bash
python3 pdf-parser.py statement.pdf | more
```
Scrolls through all objects in the PDF. Look for URI objects.

### Search for URI Objects
```bash
python3 pdf-parser.py -s /URI statement.pdf
```

**Example output:**
```
obj 7 0
 Type: /Annot
 Contains stream
  /URI (https://docs.google.com/presentation/d/1abc.../malicious-link)
```

The URI is extracted directly — copy it for URL analysis (Module 11).

---

## Step 3 — Extract Embedded Files

### Find the Embedded File Object
```bash
python3 pdf-parser.py statement.pdf | more
# Look for /EmbeddedFile or /Filespec objects
```

### Extract the Embedded File
```bash
python3 pdf-parser.py \
  --object 8 \        # object number containing the embedded file
  --filter \          # decode the stream
  --raw \             # output raw bytes
  --dump eicar_dropper.doc \  # save to file
  statement.pdf
```

### Verify the Extracted File
```bash
file eicar_dropper.doc
# Output: Composite Document File V2 Document
# → Older Word format — may contain macros
```

### Hash and Check Reputation
```bash
sha256sum eicar_dropper.doc
# → Submit hash to VirusTotal
```

---

## Step 4 — Examine Embedded JavaScript

```bash
python3 pdf-parser.py -s /JavaScript statement.pdf
```

Look for:
- `this.exportDataObject` — extracts embedded files
- `app.launchURL` — opens a URL
- `util.printd` — can be abused for code execution
- Obfuscated or encoded script content

---

## Worked Example 1 — Simple URL in PDF

**File:** `amazon-alert.pdf`  
**Scenario:** PDF claims unusual activity on Amazon account — "Click here to verify"

```bash
# Hash check
sha256sum amazon-alert.pdf
# → No VirusTotal results (new file)

# Extract URL
python3 pdf-parser.py -s /URI amazon-alert.pdf
# Output: /URI (https://docs.google.com/...)

# URL analysis
# → URLScan shows Google Docs hosting credential capture image
# → Image contains link to actual phishing page
# → VirusTotal: 1/94 flagged
```

**Verdict: MALICIOUS** — credential harvesting via legitimate service abuse.

---

## Worked Example 2 — Embedded Malicious Document

**File:** `eicar_test.pdf`  
**Scenario:** PDF with embedded Word document and JavaScript that auto-extracts and launches it

```bash
# pdfid overview
python3 pdfid.py eicar_test.pdf
# → /JS: 1, /JavaScript: 1, /OpenAction: 1, /Launch: 1, /EmbeddedFile: 1

# Find embedded file object
python3 pdf-parser.py eicar_test.pdf | grep -i "filespec\|embedded"
# → Object 8 contains EmbeddedFile

# Extract
python3 pdf-parser.py --object 8 --filter --raw --dump dropper.doc eicar_test.pdf

# Verify
file dropper.doc
# → Composite Document File V2 Document

# Now run oledump.py on the extracted doc (Module 14)
python3 oledump.py dropper.doc
# → Stream 7: M (macro present)
```

---

## Practical Scope in the SOC

Most PDF phishing investigations follow this simple path:

```
1. Hash → VirusTotal → no result
2. pdf-parser → extract URL → URL analysis (Module 11)
3. URL flagged → verdict: MALICIOUS
```

Deep static analysis (extracting embedded files, analyzing JavaScript) is needed only when:
- No URL is embedded and hash returns nothing
- Sandbox results are inconclusive
- You need to fully understand the attack chain for IR purposes

---

## IoCs to Document

```
File name:         statement.pdf
SHA256:            [hash]
Suspicious fields: /JS, /OpenAction, /EmbeddedFile
Embedded URL:      hxxps://docs[.]google[.]com/presentation/d/1abc.../  ← defanged
Embedded file:     dropper.doc (SHA256: [hash])
Verdict:           MALICIOUS
```

---

*← [14 — Static MalDoc Analysis](./14-static-maldoc-analysis.md) | [Next: Automated Analysis with PhishTool →](./16-automated-analysis-phishtool.md)*
