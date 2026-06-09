# 02 — Email Fundamentals

> **Timestamp:** [3:00:51](https://www.youtube.com/watch?v=56NDgBOSpUg&t=10851s)

---

## How Email Works

Understanding email delivery helps us trace the origin of suspicious messages.

### Basic Flow (Bob → Alice)

```
Bob (Gmail) → Gmail SMTP Server → DNS lookup → Yahoo SMTP Server → Alice's Inbox
```

1. Bob composes email and hits **Send**
2. Email client connects to **Gmail's SMTP server** (port 25)
3. Gmail queries **DNS** for Yahoo's MX record
4. Email is forwarded to **Yahoo's SMTP server**
5. Yahoo stores the email in **Alice's mailbox**
6. Alice retrieves via **POP3 or IMAP**

---

## Email Protocols

| Protocol | Port | Purpose |
|---|---|---|
| **SMTP** | 25 (465/587 encrypted) | Sending outgoing email |
| **POP3** | 110 (995 encrypted) | Retrieve email — downloads & deletes from server |
| **IMAP** | 143 (993 encrypted) | Retrieve email — syncs across devices |

---

## Email Infrastructure Agents

| Agent | Role |
|---|---|
| **MUA** (Mail User Agent) | The email client — Outlook, Gmail, Thunderbird |
| **MTA** (Mail Transfer Agent) | Routes email between mail servers |
| **MDA** (Mail Delivery Agent) | Delivers to the recipient's mailbox |

> **Key fact:** Every MTA the email passes through adds a **Received** header — this creates a traceable chain from sender to recipient.

---

## Components of an Email

### 1. Email Headers
Lines of text providing metadata about origin, routing, and handling. Mostly hidden from users but critical for investigation.

### 2. Email Body
Main content — can be plain text, HTML, or multimedia. HTML emails render logos, buttons, and styling. Viewing the raw source reveals the underlying markup.

### 3. Attachments
Files attached to the email — can range from innocent documents to malicious executables.

---

## Anatomy of an Email Address

```
bob.smith@example.com
    │         │    │
    │         │    └── Top-Level Domain (TLD): .com / .org / .net
    │         └─────── Second-Level Domain: example
    └───────────────── Local part / mailbox: bob.smith
```

The **combination of second-level domain + TLD** is the only truly unique part of a URL — subdomains and mailbox names can be spoofed.

---

## Why This Matters for Analysis

- Email headers can be **mostly spoofed** — never trust what the email client displays at face value
- The **Received headers** (added by MTAs) are the most reliable indicators of true origin
- Understanding how email travels helps us identify where spoofing occurred

---

*← [01 — Introduction](./01-introduction-to-phishing.md) | [Next: Lab Setup →](./03-lab-setup-and-config.md)*
