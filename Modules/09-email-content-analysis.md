# 09 — Email Content Analysis

> **Timestamp:** [4:35:18](https://www.youtube.com/watch?v=56NDgBOSpUg&t=16518s)

---

## Overview

After examining headers, we shift focus to the **body content** of the email. Content analysis helps identify social engineering tactics, encoding tricks, and visual red flags that indicate a phishing attempt.

---

## MIME Structure

Emails are structured using **MIME** (Multipurpose Internet Mail Extensions), which allows emails to contain different content types.

### Key MIME Headers

| Header | Purpose | Common Values |
|---|---|---|
| `MIME-Version` | MIME version in use | `1.0` |
| `Content-Type` | Type of content in the body | `text/plain`, `text/html`, `multipart/alternative` |
| `Content-Transfer-Encoding` | How content is encoded | `7bit`, `base64`, `quoted-printable` |

### Boundary
Separates different MIME parts. The presence of two boundary strings means the email has two parts (e.g., plain text + HTML version of the same content).

### Viewing in Sublime Text
```bash
subl sample1.eml
```
Scroll past the headers to see the raw HTML/text content of the body.

### Switching Views in Thunderbird
View → Message Body As → **Plain Text** or **Original HTML**

---

## Red Flags in Email Content

### Generic Greetings
```
Dear Customer,
Dear User,
Hi Dear; Customer,    ← awkward punctuation = likely automated/spoofed
```
Legitimate organizations typically address you by name.

### Grammar & Spelling Errors
- Misspelled company names: `TrustWallet` vs `Trustwallet`
- Incorrect possessives: `NFT's` instead of `NFTs`
- Misspelled words: `assistent` instead of `assistant`
- Awkward sentence structure

> With AI tools like ChatGPT now widely available, grammar errors are becoming less reliable as the only indicator — but they are still useful.

### Urgency / Fear Language
```
"Your account will be SUSPENDED within 24 hours"
"Immediate action required — verify your account NOW"
"Unauthorized login detected from Russia"
```

### Mismatch in Branding
- Company logo present but name spelled differently
- Colour scheme slightly off
- Font inconsistencies
- Button styling doesn't match legitimate brand

### Suspicious Calls to Action
- "Click here to verify your account"
- "Download the attached invoice"
- "Reply with your login credentials"

---

## Encoding in Email Bodies

### Base64 Encoding
When `Content-Transfer-Encoding: base64` is set, the body is encoded as a long Base64 string.

**Decode in terminal:**
```bash
echo "SGVsbG8gV29ybGQ=" | base64 -d
# Output: Hello World
```

**Decode with CyberChef:**
1. Go to [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef)
2. Input: paste the Base64 string
3. Operations: drag **From Base64** into the recipe
4. Output: decoded HTML content

---

### HTML Entity Encoding
Used to represent special HTML characters or obfuscate keywords:

```html
&#66;&#105;&#116;&#99;&#111;&#105;&#110;  →  Bitcoin
&lt;  →  <
&amp; →  &
```

**Decode with CyberChef:** drag **From HTML Entity** into recipe

---

### URL Encoding
Characters replaced with `%` + hex value:

```
%68%74%74%70%73  →  https
%3A%2F%2F       →  ://
```

**Decode with CyberChef:** drag **URL Decode** into recipe

---

### Quoted-Printable Encoding
Lines are wrapped and special characters are encoded with `=XX`. Common in multi-layer encoded URLs.

**Decode with CyberChef:** drag **From Quoted Printable** into recipe

---

## Multi-Layer Decoding Example

A URL inside an email may be encoded with **three layers** simultaneously:

1. Quoted-Printable → From Quoted Printable
2. HTML Entity → From HTML Entity
3. URL Encoded → URL Decode

**CyberChef recipe order:**
```
From Quoted Printable → From HTML Entity → URL Decode
```

---

## Practical Workflow

1. Open email in Thunderbird — note any visual red flags
2. Open same file in Sublime Text — examine raw body content
3. If body is Base64 encoded → decode with CyberChef
4. If body contains encoded URLs → decode multi-layer with CyberChef
5. Document all social engineering tactics observed
6. Extract any URLs found in the body for URL analysis (next module)

---

## Sample Analysis Notes

**Email:** Amazon Prime account suspension notice  
**Content observations:**
- Urgency: "Your account will be suspended by [date]"
- Authority: Impersonating Amazon Prime Support
- Convincing branding: correct logos, fonts, formatting
- Generic greeting: "Dear Customer"
- Call to action: "Verify Account" button linking to suspicious URL
- Content was done well — **cannot rely on content inspection alone**

**Conclusion:** Content shows urgency + authority tactics. Requires header and URL analysis to confirm verdict.

---

*← [08 — Authentication Methods](./08-email-authentication-methods.md) | [Next: URL Anatomy →](./10-url-anatomy.md)*
