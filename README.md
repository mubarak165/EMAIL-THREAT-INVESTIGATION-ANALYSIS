# Email Threat Investigation — Case File CA-ETI-01

## What this project is

A finance executive's mailbox was flagged after a suspected fraud attempt. I was given 13 recovered emails and investigated each one: who really sent it, whether the technical security checks (SPF/DKIM/DMARC) passed, what tactic it was using, and whether it was safe, suspicious, or somewhere in between.

## The result, at a glance

| Verdict | Count | What it means |
|---|---|---|
| 🟢 Safe | 5 | Genuinely from who it claims to be, no manipulation |
| 🔴 Suspicious | 6 | Faked sender, broken security checks, or a clear scam |
| 🟠 Medium Risk | 2 | Passed the technical checks, but the *request* itself was the scam |

**📄 [See the full email-by-email breakdown, with all 13 recovered emails and reasoning →](full-analysis.md)**

## The one thing worth knowing

Two emails in this set passed every security check — correct domain, SPF/DKIM/DMARC all green — and were still fraud attempts. **Passing those checks proves an email really came from that domain. It does not prove the domain, or the request inside, can be trusted.** That's the finding this whole project is built around.

## The two that fooled the technical checks

### 🔴 "Quick favor before my flight" — impersonating the CFO
![CFO gift card scam](screenshots/email-11.jpg)

The sender address is completely real and passes every check. The only giveaway: any reply gets secretly redirected to a personal email address. It asks for Amazon gift cards, urgently, and tells the recipient not to call to confirm — dangerous precisely *because* the headers look fine.

### 🟠 "Updated bank details" — vendor payment fraud
![Vendor bank fraud](screenshots/email-03.jpg)

Passes SPF/DKIM/DMARC for its own domain, so it's not an obvious fake. But it asks to change where a real invoice gets paid, and replies are quietly redirected to a different domain. This is how real companies lose real money — the fraud is in the *content*, not the headers.

## How each email was judged

1. **Does the sender's address actually match who it claims to be?** — Compared From against Reply-To and Return-Path.
2. **Do SPF/DKIM/DMARC pass?** — Confirms the email really came from that domain. A fail is a strong warning sign. A pass just means "this domain is real," not "this domain is safe."
3. **What is the email actually asking me to do, and is that normal?** — A request to change a bank account, wire money urgently, or buy gift cards is unusual no matter how clean the headers look.
4. **What emotional lever is it pulling?** — Urgency, fear, authority, greed, or secrecy — used to stop the reader from pausing to check.

## Skills demonstrated

- Reading and interpreting raw email headers (From / Reply-To / Return-Path)
- Understanding what SPF, DKIM, and DMARC actually prove — and their limits
- Recognizing Business Email Compromise (BEC) and vendor payment-diversion fraud
- Spotting typosquatted / look-alike domains
- Identifying social engineering tactics in written content
- Writing clear, evidence-based security findings

---
*Analysis performed on a synthetic training email set as part of a cyber security investigation exercise.* &nbsp;|&nbsp; **[→ Full 13-email breakdown](full-analysis.md)**
