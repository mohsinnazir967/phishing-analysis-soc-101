# 03 — Phishing Analysis Lab Setup

> **Timestamp:** [3:13:25](https://www.youtube.com/watch?v=56NDgBOSpUg&t=11605s)

---

## Requirements

All phishing analysis is performed on **Ubuntu Linux** using two tools:

- **Mozilla Thunderbird** — email client for opening `.eml` files
- **Sublime Text** — text editor for inspecting raw email source and headers

> No Windows VM is needed for this section.

---

## Step 1 — Install Mozilla Thunderbird

```bash
sudo apt install thunderbird
```

### First-Time Setup
1. Open Thunderbird from the Applications menu
2. When prompted for email setup — click **Cancel**
3. You do NOT need to configure an email account
4. Thunderbird will automatically open `.eml` files

### Enable Remote Content
1. Click the **hamburger menu** (top right)
2. Go to **Settings → Privacy & Security**
3. Enable **"Allow remote content in messages"**
   - This allows linked images to load when opening sample emails

### Opening Email Files
- Double-click any `.eml` file — it opens in Thunderbird automatically
- Or right-click → **Open With → Thunderbird Mail**

---

## Step 2 — Install Sublime Text

```bash
# Install GPG key
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | \
  gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null

# Add stable channel repository
echo "deb https://download.sublimetext.com/ apt/stable/" | \
  sudo tee /etc/apt/sources.list.d/sublime-text.list

# Update and install
sudo apt-get update
sudo apt-get install sublime-text
```

### Install Email Header Syntax Highlighting Plugin
This plugin (by **13Cubed**) colour-codes email headers for much easier reading.

1. Open Sublime Text
2. Go to **Tools → Install Package Control**
3. Press `Ctrl + Shift + P` → type **Install Package**
4. Search for **`EmailHeader`** → install the 13Cubed plugin
5. Open any `.eml` file → click the syntax selector (bottom right) → select **Email Header**

### Opening Email Files in Sublime Text
```bash
subl sample1.eml
```

---

## Step 3 — Verify Tools

```bash
# Verify Thunderbird
thunderbird --version

# Verify Sublime Text
subl --version
```

---

## Course File Structure (TCM Repository)

The TCM SOC 101 course repository contains sample email files organized by analysis type:

```
phishing-analysis/
├── 01-header-analysis/
│   ├── sample1.eml      ← Chase Bank spoof
│   ├── sample2.eml      ← CIBC Bank spoof
│   └── sample3.eml      ← Namecheap (legitimate)
├── 02-content-analysis/
│   ├── sample1.eml      ← TrustWallet spoof
│   └── sample2-5.eml
├── 03-url-analysis/
│   └── sample1.eml
├── 04-attachment-analysis/
│   ├── sample1.eml      ← Invoice with .ISO attachment
│   └── malware-samples/
├── 05-maldoc-analysis/
│   └── sample.xlsm
├── 06-pdf-analysis/
│   └── statement.pdf
└── tools/
    ├── report-template.txt
    ├── eioc.py           ← IOC extractor script
    ├── email-dump.py     ← Attachment extractor
    ├── oledump.py        ← Office document analyzer
    └── pdf-parser.py     ← PDF structure analyzer
```

---

## Quick Reference: Key Commands

```bash
# Open email in Sublime Text
subl sample1.eml

# Open email in Thunderbird
thunderbird sample1.eml

# View raw headers in terminal
cat sample1.eml | grep -i "^from\|^to\|^subject\|^received"

# Decode base64 content
echo "BASE64STRING" | base64 -d

# Get file hash
sha256sum filename.iso
md5sum filename.iso
sha1sum filename.iso
```

---

*← [02 — Email Fundamentals](./02-email-fundamentals.md) | [Next: Attack Types →](./04-phishing-attack-types.md)*
