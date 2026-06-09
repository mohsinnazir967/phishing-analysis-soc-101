# 01 — Introduction to Phishing

> **Course:** TCM Security — SOC 101  
> **Section:** Phishing Analysis  
> **Timestamp:** [2:46:47](https://www.youtube.com/watch?v=56NDgBOSpUg&t=10007s)

---

## What Is Phishing?

Phishing is an attempt to steal information from victims through the use of **social engineering techniques** and various communication channels including:

- Email
- SMS (smishing)
- Phone calls (vishing)
- Web pages

The attacker impersonates a legitimate organization or entity to trick recipients into:
- Revealing sensitive information (passwords, credit card numbers)
- Downloading malware
- Visiting infected sites

---

## Why Phishing Matters to SOC Analysts

Phishing is one of the most **consistent, prevalent threats** across organizations regardless of size or industry. Key reasons:

- **Low barrier of entry** for attackers
- **High success rate** — targets the human element
- **Easy to scale** — send to thousands simultaneously
- One successful fish can lead to full organizational compromise

As SOC analysts, we are the **frontline defenders** against phishing — we must be quick, efficient, and methodical in spotting, analyzing, and responding to suspicious emails.

---

## Real-World Impact — Notable Attacks

| Attack | Year | Vector | Impact |
|---|---|---|---|
| Colonial Pipeline | 2021 | Phishing email → ransomware | US East Coast fuel shortage, $4.4M ransom |
| Levitas Capital | 2020 | Whaling (fake Zoom link) | $8.5M fraudulent invoices, fund closure |
| Ubiquity Networks | 2015 | Business Email Compromise | $46.7M wired to overseas accounts |
| Ukraine Power Grid | 2015 | Spear phishing → malware | 230,000 people without power |

---

## Social Engineering Principles Exploited

Phishing attacks exploit fundamental human psychology:

| Principle | Description | Example |
|---|---|---|
| **Authority** | Impersonating executives or IT staff | "CEO requests gift cards urgently" |
| **Trust** | Using legitimate logos, official language | Fake bank email with real branding |
| **Urgency** | Time pressure to act without thinking | "Your account will be locked in 24 hours" |
| **Scarcity** | Fear of missing out | "Limited-time offer — act now" |
| **Familiarity** | Impersonating known contacts | Reply to existing email thread from compromised account |
| **Intimidation** | Threats of legal or financial consequences | "Pay this invoice or face legal action" |
| **Social Proof** | Implying widespread adoption | "Thousands of users have already verified" |

---

## Domain Objectives

By the end of the Phishing Analysis section, you will be able to:

- ✅ Understand phishing fundamentals and its prevalence
- ✅ Apply a structured methodology for analyzing suspicious emails
- ✅ Analyze email headers and trace original senders
- ✅ Identify and analyze malicious URLs
- ✅ Safely investigate email attachments
- ✅ Implement reactive and proactive defense measures
- ✅ Document findings clearly in reports and tickets

---

## Tools Required (Ubuntu Only)

| Tool | Purpose | Install |
|---|---|---|
| Mozilla Thunderbird | Open `.eml` email files | `sudo apt install thunderbird` |
| Sublime Text | Inspect raw email source / headers | Via apt repository |
| Firefox | Access web-based analysis tools | Pre-installed |
| Terminal | Run CLI commands for decoding | Pre-installed |

> **Note:** All phishing analysis in this course is performed on **Ubuntu Linux** using only Firefox and a text editor. No Windows VM is required for this section.

---

*← Previous: Lab Setup | [Next: Email Fundamentals →](./02-email-fundamentals.md)*
