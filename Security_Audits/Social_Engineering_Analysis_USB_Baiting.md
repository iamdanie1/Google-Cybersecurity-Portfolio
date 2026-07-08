# Threat Analysis Report: Investigating USB Baiting & Social Engineering Vectors

## Project Description
This project evaluates a physical-to-digital security anomaly involving a suspected USB baiting attack. Operating within an internal threat assessment framework, I executed a secure digital forensics triage of an untrusted physical storage medium found within the corporate parameter. By leveraging isolated virtualization software (sandbox containment), I inspected the drive's file structure to evaluate the risk of Personally Identifiable Information (PII) leaks and calculated the technical and operational risks associated with hardware-based social engineering vectors.

---

## Secure Forensic Triage & Inspection Scope
When an unfamiliar USB flash drive with the organization's logo was discovered in the physical parking lot area, immediate endpoint containment protocols were enforced. To neutralize the risk of active malware execution, weaponized firmware payloads, or Human Interface Device (HID) emulation attacks, the storage medium was explicitly restricted from interacting with production network assets. 

The device was attached exclusively to an isolated workstation executing localized virtualization software. This sandboxed virtual machine environment lacked logical network routes or shared host directory privileges, successfully isolating the baseline operating system from potential cross-contamination.

---

## Artifact Inventory & Data Audit

An inspection of the root directories on the discovered device revealed a critical compromise of both Personally Identifiable Information (PII) and corporate operational intelligence:

```
Jorge's USB (Root Directory)
├── 📁 Family photos
├── 📁 Our dog pics 🐶
├── 📄 New hire letter.gdoc
├── 📄 Vacation ideas.gdoc
├── 📄 Shift schedules.gsheet
├── 📄 Employee budget.gsheet
├── 📄 Wedding list.gslides
└── 📄 JB_Resume.gdoc
```

### Data Comingling Assessment
The filesystem structure indicates a critical operational security (OPSEC) failure: the absolute co-mingling of highly sensitive business infrastructure assets alongside personal user data. The owner of the drive was positively identified via the metadata as Jorge Bailey, the Human Resources Employment Manager. Storing unencrypted personal data (family photos, wedding lists) on the same external medium as corporate operational data (budgets, shift schedules, hiring letters) violates information privacy baselines and exponentially increases the organization's attack surface if the device is lost or stolen.

---

## Attacker Mindset & Threat Modeling
Assuming an adversary purposefully staged this device as a targeted USB Baiting / Social Engineering campaign, the contents provide an attacker with high-value tactical options to execute follow-on exploits against the employee and the hospital network:

### 1. Advanced Spear-Phishing & Social Engineering (OSINT Enrichment)
The personal files (`Our dog pics`, `Wedding list`, `Vacation ideas`) grant an adversary rich open-source intelligence (OSINT). An attacker can weaponize these highly specific personal details to construct flawless, high-credibility spear-phishing campaigns targeting Jorge or his immediate relatives via external vectors, completely bypassing standard behavioral filters.

### 2. Corporate Access & Infrastructure Exploitation
The corporate files present an existential risk to the organization's internal perimeter:
* `Shift schedules.gsheet` & `Employee budget.gsheet`: Exposes corporate staffing rosters, operating times, organizational hierarchies, and internal financial distributions. Attackers can map out operational blind spots or target specific high-value targets (e.g., financial employees) for credential harvesting.
* `New hire letter.gdoc`: Provides an official corporate text template. Attackers can duplicate the exact branding, tone, and formatting to distribute weaponized macro documents or malicious attachments to incoming employees, passing internal validation checks.
---

## Risk Analysis & Defense Controls
A physical USB asset found in a public zone constitutes an active threat vector regardless of whether it currently hosts malicious code blocks. Had an untrained employee found this device and plugged it directly into a production terminal out of curiosity, a threat actor could have achieved automated initial access or total network destruction.

### Potential Payload Vectors:
* **Malicious Software Deployment**: Immediate background execution of hidden Ransomware packages, Keyloggers, or Remote Access Trojans (RATs) designed to compromise network data confidentiality and integrity.
* **HID Emulation (e.g., BadUSB / Rubber Ducky)**: The hardware behaves as a spoofed keyboard, executing automated terminal commands at lightning speeds to open reverse shells, inject backdoors, or change registry parameters the millisecond it is plugged in, completely bypassing traditional antivirus detection.

### Comprehensive Mitigation Matrix:

| Control Category | Implemented Defensive Defense Protocol |
| :--- | :--- |
| **Technical Controls** | **Endpoint Device Hardening:** Enforce strict Group Policy Objects (GPO) to permanently disable USB mass storage autoplay/autorun capabilities across all corporate endpoints. Implement endpoint software controls or Endpoint Detection and Response (EDR) rules to block unauthorized USB Vendor IDs (VID) and Product IDs (PID). |
| **Operational Controls** | **Incident Response Triage:** Mandate that all discovered storage media be submitted immediately to security personnel for sandboxed virtualization testing. Establish clear policies strictly separating corporate data assets from personal hardware storage media. |
| **Managerial Controls** | **Security Awareness Training:** Execute corporate-wide training campaigns covering physical social engineering vectors, explicitly teaching staff about the risks of USB Baiting, baiting traps, and proper asset handling behaviors. |

---

## Technical Auditing Summary
This assessment highlights why physical endpoint hygiene is just as critical as logical firewall perimeters. By leveraging safe virtualization workflows, the security team was able to audit the exposure footprint of a lost asset without triggering potential malicious processes or firmware execution codes. Enforcing strict technical USB blocklists via GPOs, restricting administrative privilege inheritance, and scaling up workforce awareness surrounding baiting traps remains the most effective defense strategy to mitigate hardware-delivered network intrusion vectors.


