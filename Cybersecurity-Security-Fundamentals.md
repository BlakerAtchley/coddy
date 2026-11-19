# Cybersecurity: Security Fundamentals

**Coddy Cybersecurity journey, Section 1** · 7 chapters · 43 scenario lessons · 43 graded challenges · Certificate earned 2026-11-15 ([verify](https://coddy.tech/certifications/tj6FfM-cybersecurity-l0kMWP))

[← Back to portfolio summary](README.md)

> **About the commands below:** This track is a simulated security-operations job. I work as the first security analyst at *Lumen Coffee* (domain `lumen.test`) and handle tickets from the IT lead, the CEO and staff through **`seclab`**, a command-line security console. Coddy grades each ticket on the final state of the environment and does not keep a copy of what was typed. The commands below are reference solutions I wrote afterward. Each scenario generates its own IP addresses, usernames and message IDs, so those values appear as `<placeholders>` and each entry describes how I found them. The investigation steps and the remediation commands are the part that carries over to real work.

## What this course covers

Alert triage and blocking, log analysis, password resets and session revocation, MFA, the CIA triad, phishing and look-alike domains, access control (groups, least privilege, admin rights, joiners/movers/leavers, shared accounts, separation of duties), password policy (length, common-password blocking, spraying, credential stuffing, MFA fatigue and number matching), email attacks (Reply-To, attachments, headers, BEC, sender blocks, malicious mailbox rules), rules of engagement (authorization, no hacking back, vendor verification), and incident response (containment, persistence removal, recovery, reporting).

### `seclab` commands used

| Area | Commands |
| --- | --- |
| Overview | `seclab status` |
| Network | `seclab block IP` · `seclab unblock IP` |
| Logs | `seclab logs --ip IP` · `--since DATE` · `--event LOGIN_OK/WEB_POST` |
| Findings | `seclab answer NAME VALUE` (one finding per command) |
| Accounts | `seclab people [USER]` · `seclab reset USER` · `seclab sessions USER` · `seclab mfa USER on` · `seclab disable USER` · `seclab enable USER` · `seclab approve USER` |
| Groups | `seclab groups [NAME]` · `seclab add USER GROUP` · `seclab remove USER GROUP` |
| Policy | `seclab policy set min-length N` · `seclab policy set block-common on` · `seclab policy set number-matching on` |
| Email | `seclab inbox` · `seclab email ID` · `seclab headers ID` · `seclab quarantine ID` · `seclab release ID` · `seclab verdict ID phishing\|safe` · `seclab mailblock DOMAIN` |
| Mail rules | `seclab rules USER` · `seclab rule USER delete N` |
| Requests | `seclab requests ID` · `seclab request ID approve\|deny` |

## Contents

1. [The First Alert](#1-the-first-alert)
2. [Phishing](#2-phishing)
3. [Access Control](#3-access-control)
4. [Passwords](#4-passwords)
5. [Email Attacks](#5-email-attacks)
6. [Rules of the Job](#6-rules-of-the-job)
7. [Incident Response](#7-incident-response)
8. [Key takeaways](#key-takeaways)

---

## 1. The First Alert

### 1.1 What Is Cybersecurity · Easy
**Ticket:** Get a first look at the company.
```bash
seclab status
```

### 1.2 Stopping an Attack · Easy
**Ticket:** Someone is brute-forcing the mail server. Block that address and nothing else.
```bash
seclab status                 # alert shows the attacking source IP
seclab block <attacker-ip>
```
Block only the address named in the alert. Blocking more risks cutting off legitimate users.

### 1.3 Log Lines · Easy
**Ticket:** The block ended a live session. Find the sign-in that let the attacker in.
```bash
seclab logs --ip <attacker-ip>        # look for LOGIN_OK user=...
seclab answer account <user>
```

### 1.4 Stolen Passwords · Easy
**Ticket:** Make Mia's stolen password useless without locking her out.
```bash
seclab reset mia
seclab sessions mia                   # confirm open sessions ended
```
A reset replaces the password **and** ends existing sessions. Disabling the account would stop Mia from working.

### 1.5 Multi-Factor Authentication · Easy
**Ticket:** Make a stolen password useless on its own for everyone in finance.
```bash
seclab groups finance                 # list members
seclab mfa <member> on                # repeat for each finance member
```

### 1.6 The CIA Triad · Easy
**Ticket:** Record what the intruder changed and which part of the CIA triad it hit.
```bash
seclab logs --ip <attacker-ip>        # find the change event (e.g. a page edit)
seclab answer page <page-changed>
seclab answer hit integrity
```
Reading mail breaches **confidentiality**, but the ticket asks about the *change*, and an unauthorized change is an **integrity** breach.

### 1.7 Back for More · Medium
**Ticket:** A new address is spraying passwords and got into an account. Contain it.
```bash
seclab status
seclab logs --ip <new-ip>             # find the LOGIN_OK account
seclab block <new-ip>
seclab reset <user>
seclab mfa <user> on
```

---

## 2. Phishing

### 2.1 Phishing · Easy
**Ticket:** Omar got a "mailbox full" email from "IT". Confirm it's phishing and remove it.
```bash
seclab inbox
seclab email <msg-id>                 # sender and link destination are not lumen.test
seclab quarantine <msg-id>
seclab verdict <msg-id> phishing
```

### 2.2 Look-Alike Domains · Easy
**Ticket:** Three "Lumen" emails arrived. Keep the real ones and pull the fakes.
```bash
seclab inbox
seclab email <id>                     # check each sender domain
seclab quarantine <fake-id>; seclab verdict <fake-id> phishing
seclab verdict <real-id> safe
```
`lumen.payroll.test` belongs to `payroll.test`, not to `lumen.test`. Read domains right to left.

### 2.3 Who Clicked · Easy
**Ticket:** Seven people got the fake email. Reset only those who submitted their password.
```bash
seclab logs --event WEB_POST          # form submissions to the fake page
seclab reset <user>                   # only users who POSTed
```
`WEB_GET` only means someone opened the page. `WEB_POST` means they submitted the form. Resetting everyone would waste each person half an hour with IT.

### 2.4 The Second Wave · Medium
**Ticket:** A new attack is in progress. Protect the breached account before data leaves.
```bash
seclab status
seclab logs --ip <new-ip>
seclab block <new-ip>
seclab reset <user>
seclab mfa <user> on
```
Order matters: stop the source, then kill the session, then harden the account.

---

## 3. Access Control

### 3.1 Groups · Easy
**Ticket:** Mia moves from marketing to customer support.
```bash
seclab people mia
seclab groups                         # what each group allows
seclab add mia support
seclab remove mia marketing           # keep staff
```

### 3.2 Least Privilege · Easy
**Ticket:** The intern's compromised account can read payroll through a mistaken group membership.
```bash
seclab people <intern>
seclab remove <intern> finance        # keep support access
```

### 3.3 Admin Rights · Easy
**Ticket:** Malware on a developer laptop is trying to create an admin account.
```bash
seclab logs                           # find the developer
seclab remove <developer> admins      # keep the account active
```

### 3.4 The New Hire · Medium
**Ticket:** Approve the new hire with job access only, even though they asked for admin "just in case".
```bash
seclab approve <new-hire>
seclab add <new-hire> support
seclab add <new-hire> staff
```

### 3.5 Leavers · Easy
**Ticket:** A former employee's account signed in at 01:18, and a new employee has the same name.
```bash
seclab people <reed-a>; seclab people <reed-b>   # compare leaving dates
seclab disable <former-reed>                     # keeps the record
```

### 3.6 Shared Accounts · Easy
**Ticket:** Three people share a front-desk login.
```bash
seclab add <worker1> orders; seclab add <worker2> orders; seclab add <worker3> orders
seclab disable <shared-login>
```
Shared accounts destroy accountability, because nobody can tell who did what.

### 3.7 Separation of Duties · Easy
**Ticket:** One person can both change supplier bank details and approve payments.
```bash
seclab groups supplier-details
seclab groups payment-approvers
seclab remove <overlapping-user> <group-they-shouldnt-have>
```
Dana keeps supplier details and Omar keeps approvals, so no single person controls both steps.

### 3.8 The Access Review · Medium
**Ticket:** An import reintroduced four access mistakes.
```bash
seclab disable <shared-reception>                # already under attack: first
seclab remove <support-worker-1> <extra-group>
seclab remove <support-worker-2> <extra-group>
seclab remove <overlap-user> <conflicting-group>
```

---

## 4. Passwords

### 4.1 Long Passwords · Easy
**Ticket:** Set a 14-character minimum and reset only the passwords that are too short.
```bash
seclab people                         # check lengths
seclab policy set min-length 14
seclab reset <short-pw-user>          # repeat for each short one
```

### 4.2 Common Passwords · Easy
**Ticket:** Block common passwords and reset only the flagged accounts.
```bash
seclab people <user>                  # shows a common-list warning
seclab policy set block-common on     # BEFORE resetting
seclab reset <flagged-user>
```

### 4.3 Password Spraying · Easy
**Ticket:** Guesses are arriving from changing IPs. Find the breached account.
```bash
seclab logs --since 2026-10-22        # look for LOGIN_OK among the failures
seclab reset <user>                   # ends the outside session
# then turn on MFA for everyone in policy
```
Spraying rotates source IPs, so blocking addresses doesn't help. Look for the **successful** sign-in.

### 4.4 Morning Cleanup · Medium
**Ticket:** A restore rolled back the password policy and revived an old password and session.
```bash
seclab logs                           # read the restore event
seclab policy set min-length 14
seclab policy set block-common on
# re-enable MFA for everyone
seclab reset <affected-user>
seclab mfa <affected-user> on
```

### 4.5 Credential Stuffing · Easy
**Ticket:** A partner breach notice lists stolen passwords. Reset the reused ones only.
```bash
seclab email <notice-id>
seclab reset <reused-user>            # even if MFA blocked the attacker
```

### 4.6 MFA Fatigue · Easy
**Ticket:** A user denied several push prompts, then approved one.
```bash
seclab logs                           # MFA denied x N, then approved
seclab reset <user>
seclab policy set number-matching on
```
Number matching stops "just tap approve" fatigue attacks.

### 4.7 The Password Review · Medium
**Ticket:** A second breach notice arrived. Reset only the passwords confirmed as stolen.
```bash
seclab email <notice-id>
seclab logs --since <date>
seclab reset <confirmed-user>         # leave the person with a different password and a normal home sign-in alone
```

---

## 5. Email Attacks

### 5.1 Reply-To · Easy
```bash
seclab email <id>                     # From: supplier, Reply-To: different company
seclab quarantine <fake-id>; seclab verdict <fake-id> phishing
```

### 5.2 Dangerous Attachments · Easy
```bash
seclab email <id>                     # look for .exe / .html / .iso / .zip contents
seclab quarantine <id1>; seclab verdict <id1> phishing
seclab quarantine <id2>; seclab verdict <id2> phishing
```

### 5.3 Where an Email Came From · Easy
```bash
seclab headers <id>                   # trust the checks recorded by mail.lumen.test
seclab quarantine <fake-id>; seclab verdict <fake-id> phishing
seclab verdict <newsletter-id> safe
```
An unfamiliar delivery server can still be legitimate, for example a real mailing service whose SPF/DKIM checks pass.

### 5.4 The Invoice Queue · Medium
```bash
seclab email <id>; seclab headers <id>          # for each of 3 messages
seclab quarantine <fake>; seclab verdict <fake> phishing
seclab verdict <bulletin-id> safe
```

### 5.5 Business Email Compromise · Easy
**Ticket:** A "bank details changed, pay now" email passed the sender checks.
```bash
seclab quarantine <bec-id>; seclab verdict <bec-id> phishing
seclab answer verification <call-using-saved-contact-record>
```
Verify bank changes **out of band**, using contact details you already hold, never a number from the email itself.

### 5.6 Blocking a Sender · Easy
```bash
seclab mailblock <fake-domain>
seclab quarantine <id>; seclab verdict <id> phishing
```
A sender block stops new mail. Quarantine handles messages that already arrived.

### 5.7 Mailbox Rules · Easy
**Ticket:** Invoices are still being forwarded outside the company after the password resets.
```bash
seclab rules <user>
seclab rule <user> delete <n>         # the forwarding rule; keep newsletter sorting
```
A forwarding rule is **persistence**: it keeps exfiltrating mail even after the attacker is signed out.

### 5.8 The Phishing Reports · Medium
```bash
seclab rules <user>; seclab rule <user> delete <n>      # 1. stop forwarding
seclab mailblock <fake-domain>                          # 2. block new mail
seclab quarantine <id>; seclab verdict <id> phishing    # 3. clean up existing
```

---

## 6. Rules of the Job

### 6.1 Authorization · Easy
```bash
seclab requests <id>                  # owner, who approved, scope
seclab request <authorized-id> approve
seclab request <other-id> deny        # x2
```
Authorization means the owner's permission for that **specific** job.

### 6.2 No Hacking Back · Easy
```bash
seclab request <take-down-server> deny
seclab request <save-logs-and-report> approve
```
The attacker's server may itself be a stolen victim's machine. Preserve evidence and report it instead.

### 6.3 The Vendor's Call · Medium
```bash
seclab requests <id>                  # how each caller was verified + what was approved
seclab request <verified-and-approved> approve
seclab request <urgent-reset> deny
seclab request <permanent-admin> deny
```

---

## 7. Incident Response

### 7.1 Incident Response · Easy
```bash
seclab status; seclab logs
seclab disable <compromised-user>     # not delete: keep the record
```

### 7.2 Contain, Don't Destroy · Easy
```bash
seclab disable <second-user>          # stolen browser session; MFA doesn't help
```

### 7.3 What They Left Behind · Easy
```bash
seclab disable <attacker-created-account>
seclab rules <user>; seclab rule <user> delete <n>
seclab remove <user> admins           # temporary rights from the system move
```

### 7.4 Recovery · Easy
```bash
seclab enable <employee>
seclab unblock <home-ip>
seclab release <invoice-id>; seclab verdict <invoice-id> safe
```
Restore only what the evidence has cleared.

### 7.5 The Incident Report · Easy
```bash
seclab answer first <time-of-first-outside-LOGIN_OK>
seclab answer entry <how-they-got-in>
seclab answer accounts <user1>,<user2>
seclab answer loss <data-actually-copied>
```
Keep facts (backed by logs) separate from guesses.

### 7.6 The Night Alert · Medium (capstone)
**Ticket:** Full incident: outside sign-in, an unapproved admin account (`audit-helper`), and a forwarding rule.
```bash
seclab status; seclab logs --ip <attacker-ip>
seclab block <attacker-ip>
seclab disable audit-helper           # or remove it, per the ticket
seclab rules <user>; seclab rule <user> delete <n>
seclab reset <affected-user>
seclab enable <home-worker>; seclab unblock <home-worker-ip>
seclab release <invoice-id>; seclab verdict <invoice-id> safe
```
All of it had to be done while keeping the backup service, sorting rules, password policy and the real IT account intact.

---

## Key takeaways

- **Contain → eradicate → recover → report**, without breaking legitimate work.
- A password reset ends sessions, but a stolen **session** also needs revoking, even when MFA is on.
- Persistence lives in **mail rules, new accounts and admin rights**. Remove all three.
- Least privilege, separation of duties and no shared accounts limit the blast radius.
- These map directly onto **Microsoft Entra ID / Defender / Exchange Online** tasks: Conditional Access and MFA, revoking sessions, inbox-rule investigation, Tenant Allow/Block lists, and access reviews.
