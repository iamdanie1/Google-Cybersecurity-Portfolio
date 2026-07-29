# SOC Incident Response: Phishing Alert Investigation & Playbook Escalation

## Project Overview
This project documents the end-to-end SOC Tier 1 triage and escalation process for a high-risk phishing alert (Ticket ID: **A-2703**) at a financial services organization. Following the organization's **Phishing Playbook (v1.0)**, I evaluated the email header metadata, analyzed body text anomalies, cross-referenced an executable attachment hash against global threat intelligence, and escalated the ticket to Tier 2 security personnel for endpoint containment.

---

## Alert Ticket Overview & Final Status

| Ticket Parameter | Initial State | Final State |
| :---: | :---: | :---: |
| **Ticket ID** | A-2703 | **A-2703** |
| **Alert Message** | SERVER-MAIL Phishing attempt possible download of malware | SERVER-MAIL Phishing attempt possible download of malware |
| **Alert Severity** | Medium | **Medium (Escalation Required)** |
| **Target Recipient** | `<hr@inergy.com>` (`176.157.125.93`) | `<hr@inergy.com>` (`176.157.125.93`) |
| **Ticket Status** | Open | **ESCALATED** |

---

## Incident Analysis & The 5 W's

### 1. WHO caused the incident?
An external threat actor impersonating a job applicant named "Clyde West" from a spoofed/suspicious domain (`Def Communications <76tguyhh6tgftrt7tg.su>` at IP `114.114.114.114`). The target was the internal Human Resources department (`<hr@inergy.com>`).

### 2. WHAT happened?
The threat actor sent a targeted spear-phishing email containing a password-protected attachment named `bfsvc.exe`. The email body instructed the user to enter the password `paradise10789` to view a "resume and cover letter." The attached executable matches a known malicious SHA-256 hash verified to deploy Trojan malware.

### 3. WHEN did the incident take place?
The phishing email was sent on **Wednesday, July 20, 2022 at 09:30:14 AM**.

### 4. WHERE did the incident occur?
The incident occurred at the perimeter mail server layer targeting the HR department inbox (`hr@inergy.com`) on host IP `176.157.125.93`.

### 5. WHY did it happen?
The threat actor used social engineering tactics (job application lure) combined with an encrypted/password-protected payload to bypass automated Secure Email Gateway (SEG) antivirus scanning and trick HR personnel into executing a malicious system binary.

---

## Phishing Playbook Execution (v1.0 Workflow)

```text
[Step 1: Receive Alert A-2703]
               │
               ▼
[Step 2: Evaluate Alert & Indicators] ──► Identified domain mismatch (.su), typos, & .exe attachment
               │
               ▼
[Step 3.0: Contains Links/Attachments?] ──► YES (Attachment: bfsvc.exe)
               │
               ▼
[Step 3.1: Is Attachment Malicious?] ──► YES (SHA-256 verified malicious on Threat Intel)
               │
               ▼
[Step 3.2: Update Ticket & Escalate] ──► Status updated to ESCALATED (Handover to Tier 2)
```
## Forensic Evidence Identified During Triage:
1. **Sender Domain Anomaly**: The sender address uses a suspicious Top-Level Domain (TLD) associated with high risk (`76tguyhh6tgftrt7tg.su` / Soviet Union TLD) which does not match the display name "Def Communications".
2. **Spelling & Grammar Errors**: The email subject contains a clear typo (`Re: Infrastructure Egnieer role`), and the body contains grammatical errors (`I am writing for to express my interest...`, `There is attached my resume`).
3. **Payload Obfuscation**: The sender password-protected the attachment (`paradise10789`) under the guise of "privacy." Threat actors frequently password-protect archives to prevent email security appliances from unzipping and scanning the contents in a sandbox.
4. **Dangerous File Extension**: The attachment is named `bfsvc.exe`. A legitimate resume would be a `.pdf` or `.docx`. `bfsvc.exe` is an executable file (and spoofed name of a Windows Boot File Servicing Utility) intended to run arbitrary code upon execution.
5. **Verified Hash Match**: The attachment corresponds to known malicious SHA-256 hash: `54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b`.
---
## TICKET COMMENTS / SOC HANDOVER NOTE:

* [STATUS]: ESCALATED TO TIER 2 SOC ANALYST
* [REASON]: Verified Phishing Campaign with Malicious Executable Attachment

Investigated phishing alert Ticket A-2703. Triage confirms this is a legitimate spear-phishing attempt targeting the HR department (hr@inergy.com).

Key Findings:
1. Sender address <76tguyhh6tgftrt7tg.su> shows clear domain spoofing, suspicious TLD (.su), and typos in subject/body text ("Egnieer").
2. Email contains a password-protected attachment disguised as a resume, designed to bypass automated mail gateway inspection.
3. Attachment filename is an executable (bfsvc.exe) with known malicious SHA-256 hash: 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b.

Actions Taken:
- Updated ticket status from OPEN to ESCALATED per Phishing Playbook Step 3.2.

Recommended Tier 2 / Incident Response Actions:
- Check mail server logs to purge/quarantine this message across all employee inboxes.
- Block sender IP (114.114.114.114) and domain (*.su) at the Secure Email Gateway.
- Verify whether user hr@inergy.com downloaded or executed bfsvc.exe on endpoint 176.157.125.93 for immediate EDR containment.
