← [Back to project overview](README.md)

# Full Email-by-Email Breakdown — Case File CA-ETI-01

All 13 recovered emails, shown exactly as investigated, with the verdict and reasoning for each.

---

### 🟢 #1 — Password Expiry Reminder — SAFE
![Email 1](screenshots/email-01.jpg)

Real IT helpdesk, matching sender/reply-to, all checks pass. Nothing asked beyond a routine password change. No links, no urgency.

---

### 🔴 #2 — "Mailbox Storage is FULL" — SUSPICIOUS
![Email 2](screenshots/email-02.jpg)

Sent from a fake copycat domain, every security check failed, and it pressures you to "verify" your login on a fake page. Classic credential theft.

---

### 🟠 #3 — Invoice & "Updated Bank Details" — MEDIUM RISK
![Email 3](screenshots/email-03.jpg)

Passes SPF/DKIM/DMARC for its own domain, so it's not an obvious fake. But it asks to change where a real invoice gets paid, and replies are quietly redirected to a different domain. This is how real companies lose real money — the fraud is in the *content*, not the headers.

---

### 🟢 #4 — Revised Leave & WFH Policy — SAFE
![Email 4](screenshots/email-04.jpg)

Real HR broadcast to all staff, fully authenticated, no links, no urgency.

---

### 🔴 #5 — "Urgent Wire Transfer" from the "MD" — SUSPICIOUS
![Email 5](screenshots/email-05.jpg)

Actually sent from a free look-alike email address with zero authentication. Impersonates a senior executive, demands secrecy and a same-day payment. Classic CEO fraud.

---

### 🟢 #6 — Google Drive File Share — SAFE
![Email 6](screenshots/email-06.jpg)

Genuinely from Google's real infrastructure, shared by a real named colleague referencing a plausible internal event.

---

### 🔴 #7 — "Unusual Microsoft Sign-in" — SUSPICIOUS
![Email 7](screenshots/email-07.jpg)

From a fake Microsoft domain (a zero swapped in for the letter "o"), every check failed. Same credential-theft trick as #2, dressed up as Microsoft.

---

### 🟠 #8 — Unsolicited Recruiter Offer — MEDIUM RISK
![Email 8](screenshots/email-08.jpg)

The domain is technically valid, but a legitimate recruiter would never ask for a PAN card and salary slip before even scheduling an interview — and the attached `.zip` file is a common way to smuggle malware to a first-time contact.

---

### 🟢 #9 — Software Subscription Renewal — SAFE
![Email 9](screenshots/email-09.jpg)

Real vendor, fully authenticated, and the email itself explicitly confirms no bank details have changed.

---

### 🔴 #10 — "You Won the Lottery" — SUSPICIOUS
![Email 10](screenshots/email-10.jpg)

A mass scam blast (recipient was Bcc'd), every check failed, asks for a "fee" to release a prize that doesn't exist.

---

### 🔴 #11 — "Quick Favor Before My Flight" (CFO) — SUSPICIOUS
![Email 11](screenshots/email-11.jpg)

The cleverest one in the set. The sender address is completely real and passes every check. The only giveaway is that any reply gets secretly redirected to a personal email address. It asks for Amazon gift cards, urgently, and tells the recipient not to call to confirm — dangerous precisely *because* the headers look fine.

---

### 🟢 #12 — Mandatory Security Training Reminder — SAFE
![Email 12](screenshots/email-12.jpg)

Real internal notice from InfoSec, points to the known company learning portal, fully authenticated.

---

### 🔴 #13 — "Pending Invoice" with Attachment — SUSPICIOUS
![Email 13](screenshots/email-13.jpg)

The domain failed every check, and the attachment is a Word file that asks you to "enable macros" to view it — one of the most common ways attackers get malware to run on your computer.

---

## Indicators of Compromise (IOCs)

**Malicious / suspicious domains**
```
solvex-industries-helpdesk.com        - typosquat of solvexindustries.com
mail-secure-verify.net                - mismatched Reply-To / Return-Path domain
verify-account-secure.com             - credential phishing landing page
gmail-corpmail.com                    - look-alike free-mail domain (CEO fraud)
micros0ft-online.com                  - typosquat of microsoft-online (0 for o)
apexsteel-billing.com                 - mismatched Reply-To on vendor fraud email
outlook-verify.info                   - mismatched Reply-To on lottery scam
yandex-mailer.com                     - lottery scam sending domain
shreeganesh-logistics.in.invoice-view-secure.com - wrapped look-alike domain
```

**Suspicious IPs / hosts**
```
193.41.77.108   unknown-host-193.41.77.108.static-cloud.ru   (Email 2)
45.137.22.9     smtp-relay-88.freemailhost.io, Netherlands   (Email 5)
77.91.134.6     host-77-91-134-6.vpn-exit.net                (Email 7)
185.220.101.44  unknown-relay-node4.freehosting-mail.com     (Email 10)
41.203.88.17    webmail-host-4.sharedhosting-cluster.com     (Email 13)
```

**Malicious links (defanged)**
```
hxxp://solvex-industries-helpdesk.verify-account-secure.com/mailbox/confirm?user=aditya.rao
hxxp://login.micros0ft-online.com/secure/verify-identity?ref=aditya.rao
hxxp://claim-your-prize-now.lottery-verify.info/claim?ref=44921
hxxp://shreeganesh-logistics.in.invoice-view-secure.com/download?file=inv2207
```

**Risky attachments**
```
JD_SeniorFinanceManager_Client.zip     - unsolicited first-contact archive (Email 8)
Pending_Invoice_Challan_Details.docm   - macro-enabled malware delivery (Email 13)
```

## Recommended organization-wide actions

- Enforce a strict DMARC `reject` policy and monitor for overrides.
- Require out-of-band (phone) verification for any bank-detail change or unusual payment request.
- Disable macros by default for external documents; sandbox `.docm`/`.zip` from first-time senders.
- Train staff to inspect `Reply-To` specifically — the only indicator that caught the CFO gift-card BEC (#11).
- Block all IOCs listed above at the email gateway/firewall.

---
← [Back to project overview](README.md)
