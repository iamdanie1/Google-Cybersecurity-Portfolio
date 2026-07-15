# Incident Handler's Journal: Phishing-Driven Ransomware Attack Triage

## Project Description
This repository serves as my professional Incident Handler's Journal, documenting hands-on cybersecurity incident response operations, threat investigations, and post-incident remediation procedures. This first entry details the systematic triage, scoping, and operational impact analysis of an active ransomware deployment targeting a U.S. primary-care healthcare clinic. 

---

## Journal Entry #1: Active Ransomware Triage & Scoping

| Incident Log Parameter | Record Details |
| :--- | :--- |
| **Date of Entry** | 2026-07-15 |
| **Journal Entry #** | 01 |
| **Incident Classification** | Ransomware / Phishing Campaign (Initial Access) |
| **Operational Status** | Active Containment & Eradication Stage |
| **Tool(s) Used** | Isolated Virtual Sandbox (Analysis), Email Gateway Logs, Endpoint Protection Console (EDR) |

---

## The 5 W's of the Incident

### 1. WHO caused the incident?
An organized, financially motivated cybercriminal threat group known for systematically targeting healthcare and transportation sector infrastructures. The initial threat vector was triggered when multiple internal healthcare clinic employees interacted with a malicious phishing email delivery path.

### 2. WHAT happened?
A ransomware binary payload was executed on internal endpoints after employees downloaded a malicious email attachment. This instantly initiated cryptographic locking of local and shared files (including electronic medical records - EMR). Business operations have been completely paralyzed due to data unavailability, and a physical/digital ransom note has been displayed demanding cryptocurrency in exchange for the decryption key.

### 3. WHEN did the incident occur?
The attack payload was executed on Tuesday morning, at approximately **09:00 AM EST**.

### 4. WHERE did the incident happen?
The compromise occurred within the internal local area network (LAN) of a small U.S. primary-care healthcare clinic, moving laterally from individual employee email endpoints to local file-sharing drives.

### 5. WHY did the incident happen?
The root cause was the absence of proactive technical perimeter and endpoint controls. Specifically, the organization lacked a secure Email Security Gateway (SEG) to quarantine malicious attachments, and local endpoints did not have active Endpoint Detection and Response (EDR) or robust application-whitelisting policies to block untrusted file execution.

---

## Forensic Analysis & Strategic Remediation Notes

### Operational Vulnerability Gaps Identified:
1. **Critical Off-Site Backup Verification:** It is currently unknown if the clinic's data backups are stored offline or segmented from the main network. If backups were connected to the main server, they are likely encrypted as well.
2. **Defensive Endpoint Blind Spots:** Standard antivirus software failed to intercept the malicious download, highlighting the immediate need to transition to dynamic behavioral analysis tools (EDR).

### Immediate Tactical Directives (Playbook):
* **Network Isolation:** Instruct all employees to immediately disconnect their workstations from the physical network (unplug ethernet cables/disable Wi-Fi) to prevent further lateral movement of the ransomware. **Do not power down the machines**, as this can destroy volatile memory (RAM) critical for forensic investigation.
* **Isolate the Email Server:** Purge the malicious phishing email from all active employee inboxes to prevent other staff members from executing the attachment.
* **External Notification:** Engage external incident response professionals, legal counsel, and appropriate regulatory agencies (such as HHS/OCR due to HIPAA regulations in healthcare).
