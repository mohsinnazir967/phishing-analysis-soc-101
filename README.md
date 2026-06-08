# 🎣 Phishing Analysis — SOC 101 (TCM Security)

> A complete, hands-on phishing analysis reference built from the TCM Security SOC 101 curriculum. Covers email investigation methodology, header analysis, URL and attachment analysis, defense strategies, and documentation — using only Ubuntu Linux with Firefox and a text editor.

![Course](https://img.shields.io/badge/Course-TCM%20Security%20SOC%20101-blue?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Ubuntu%20Linux-orange?style=flat-square)
![Tools](https://img.shields.io/badge/Tools-Thunderbird%20%7C%20Sublime%20Text%20%7C%20Firefox-lightgrey?style=flat-square)

---

## 📋 About This Repository

This repo documents the full **Phishing Analysis** domain from the TCM Security SOC 101 course. It is structured as a practical reference for SOC analysts — covering everything from understanding how email works, through to writing investigation reports and implementing defensive measures.

> **Original course by:** [TCM Security Academy](https://academy.tcm-sec.com)  
> **Instructor:** Andrew Prince

---

## 🛠️ Lab Requirements

All analysis in this section is performed on **Ubuntu Linux** using:

| Tool | Purpose | Install |
|---|---|---|
| Mozilla Thunderbird | Open and view `.eml` email files | `sudo apt install thunderbird` |
| Sublime Text | Inspect raw email source and headers | Via apt repository (see Module 03) |
| Firefox | Web-based analysis tools (URLScan, VirusTotal, etc.) | Pre-installed |
| Terminal | CLI decoding, hashing, and extraction | Pre-installed |

> No Windows VM is required for this section.

---

## 📂 Repository Structure

```
phishing-analysis-soc-101/
├── README.md
├── .gitignore
├── docs/
│   ├── email-analysis-methodology.md    ← Full 7-step investigation methodology
│   ├── report-template.md               ← Copy-paste investigation report template
│   └── phishing-resources.md            ← Tools, sample repos, and practice resources
└── modules/
    ├── 01-introduction-to-phishing.md
    ├── 02-email-fundamentals.md
    ├── 03-lab-setup-and-config.md
    ├── 04-phishing-attack-types.md
    ├── 05-phishing-attack-techniques.md
    ├── 06-email-analysis-methodology.md
    ├── 07-header-and-sender-analysis.md
    ├── 08-email-authentication-methods.md
    ├── 09-email-content-analysis.md
    ├── 10-url-anatomy.md
    ├── 11-email-url-analysis.md
    ├── 12-email-attachment-analysis.md
    ├── 13-dynamic-analysis-sandboxing.md
    ├── 14-static-maldoc-analysis.md
    ├── 15-static-pdf-analysis.md
    ├── 16-automated-analysis-phishtool.md
    ├── 17-reactive-phishing-defense.md
    ├── 18-proactive-phishing-defense.md
    ├── 19-documentation-and-reporting.md
    └── 20-additional-practice-resources.md
```

---

## 📅 Module Curriculum

### 🔵 Foundations

| Module | Topic | Timestamp |
|---|---|---|
| [01](./modules/01-introduction-to-phishing.md) | Introduction to Phishing | [2:46:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=10007s) |
| [02](./modules/02-email-fundamentals.md) | Email Fundamentals | [3:00:51](https://www.youtube.com/watch?v=56NDgBOSpUg&t=10851s) |
| [03](./modules/03-lab-setup-and-config.md) | Lab Setup & Configuration | [3:13:25](https://www.youtube.com/watch?v=56NDgBOSpUg&t=11605s) |
| [04](./modules/04-phishing-attack-types.md) | Phishing Attack Types | [3:19:30](https://www.youtube.com/watch?v=56NDgBOSpUg&t=11970s) |
| [05](./modules/05-phishing-attack-techniques.md) | Phishing Attack Techniques | [3:35:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=12947s) |

### 🔵 Email Investigation Methodology

| Module | Topic | Timestamp |
|---|---|---|
| [06](./modules/06-email-analysis-methodology.md) | Email Analysis Methodology | [3:50:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=13847s) |
| [07](./modules/07-header-and-sender-analysis.md) | Header & Sender Analysis | [3:56:27](https://www.youtube.com/watch?v=56NDgBOSpUg&t=14187s) |
| [08](./modules/08-email-authentication-methods.md) | Email Authentication (SPF / DKIM / DMARC) | [4:17:51](https://www.youtube.com/watch?v=56NDgBOSpUg&t=15471s) |
| [09](./modules/09-email-content-analysis.md) | Email Content Analysis | [4:35:18](https://www.youtube.com/watch?v=56NDgBOSpUg&t=16518s) |

### 🔵 URL & Attachment Analysis

| Module | Topic | Timestamp |
|---|---|---|
| [10](./modules/10-url-anatomy.md) | The Anatomy of a URL | [4:48:07](https://www.youtube.com/watch?v=56NDgBOSpUg&t=17287s) |
| [11](./modules/11-email-url-analysis.md) | Email URL Analysis | [4:57:36](https://www.youtube.com/watch?v=56NDgBOSpUg&t=17856s) |
| [12](./modules/12-email-attachment-analysis.md) | Email Attachment Analysis | [5:18:27](https://www.youtube.com/watch?v=56NDgBOSpUg&t=19107s) |
| [13](./modules/13-dynamic-analysis-sandboxing.md) | Dynamic Analysis & Sandboxing | [5:33:06](https://www.youtube.com/watch?v=56NDgBOSpUg&t=19986s) |
| [14](./modules/14-static-maldoc-analysis.md) | Static MalDoc Analysis | [5:56:06](https://www.youtube.com/watch?v=56NDgBOSpUg&t=21366s) |
| [15](./modules/15-static-pdf-analysis.md) | Static PDF Analysis | [6:03:00](https://www.youtube.com/watch?v=56NDgBOSpUg&t=21780s) |
| [16](./modules/16-automated-analysis-phishtool.md) | Automated Analysis with PhishTool | [6:13:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=22427s) |

### 🔵 Defense & Documentation

| Module | Topic | Timestamp |
|---|---|---|
| [17](./modules/17-reactive-phishing-defense.md) | Reactive Phishing Defense | [6:19:58](https://www.youtube.com/watch?v=56NDgBOSpUg&t=22798s) |
| [18](./modules/18-proactive-phishing-defense.md) | Proactive Phishing Defense | [6:47:24](https://www.youtube.com/watch?v=56NDgBOSpUg&t=24444s) |
| [19](./modules/19-documentation-and-reporting.md) | Documentation & Reporting | [7:00:43](https://www.youtube.com/watch?v=56NDgBOSpUg&t=25243s) |
| [20](./modules/20-additional-practice-resources.md) | Additional Practice Resources | [7:12:35](https://www.youtube.com/watch?v=56NDgBOSpUg&t=25955s) |

---

## 🧰 Key Tools Reference

| Tool | Purpose | URL |
|---|---|---|
| PhishTool | Automated email analysis | [app.phishtool.com](https://app.phishtool.com) |
| MXToolbox | Header analysis, SPF/DKIM lookup | [mxtoolbox.com](https://mxtoolbox.com) |
| URLScan.io | URL sandboxing and screenshot | [urlscan.io](https://urlscan.io) |
| VirusTotal | File hash and URL reputation | [virustotal.com](https://virustotal.com) |
| CyberChef | Decoding (Base64, URL, entities) | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef) |
| DomainTools WHOIS | Domain age and ownership | [whois.domaintools.com](https://whois.domaintools.com) |
| Hybrid Analysis | Dynamic malware sandboxing | [hybrid-analysis.com](https://www.hybrid-analysis.com) |
| Fish.tank | Phishing URL database | [phishtank.com](https://www.phishtank.com) |
| URL2PNG | Safe URL screenshot | [url2png.com](https://www.url2png.com) |
| Cisco Talos | Threat intelligence | [talosintelligence.com](https://talosintelligence.com) |

---

## 📚 Reference Docs

- [Email Analysis Methodology](./docs/email-analysis-methodology.md) — The full 7-step investigation framework
- [Investigation Report Template](./docs/report-template.md) — Copy-paste template for tickets
- [Phishing Resources](./docs/phishing-resources.md) — Sample emails, URLs, and tools for practice

---

## 🔗 My Other Security Projects

- 🔍 [KC7 KQL Query Repository](https://github.com/mohsinnazir967/kc7-kql-queries) — 79 KQL queries across 4 investigations, mapped to MITRE ATT&CK
- 🛡️ [30-Day SOC Analyst Challenge](https://github.com/mohsinnazir967/30-day-soc-analyst-challenge) — ELK Stack SIEM lab with brute force detection, C2 simulation, and IR workflows

---

## 👤 About

SOC Analyst building hands-on skills through structured labs and cybersecurity courses.

- 📍 Islamabad, Pakistan
- 🔗 [github.com/mohsinnazir967](https://github.com/mohsinnazir967)

---

*Based on the TCM Security SOC 101 course — all lab work performed for educational purposes.*
