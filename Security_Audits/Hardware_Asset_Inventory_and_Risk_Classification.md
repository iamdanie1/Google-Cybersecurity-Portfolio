# Asset Management & Risk Classification Audit: Internal Network Baseline

## Project Description
In accordance with standard hardware asset management frameworks (NIST SP 800-40 / ISO 27001), I conducted an internal network asset discovery and sensitivity classification audit. The objective of this project was to identify all active IP-bearing endpoints connected to the local area network (LAN), map operational characteristics (ownership, network persistence, and physical location), and assign structured security classifications. By auditing the network perimeter down to individual endpoints—including Internet of Things (IoT) appliances and transient guest nodes—I established an authoritative asset inventory to calculate exposure vectors, enforce network segmentation strategies, and reduce the organization's overall attack surface.

---

## Data Sensitivity & Classification Matrix
Assets within the inventory are classified according to a structured four-tier data classification standard designed to preserve the confidentiality, integrity, and availability (CIA Triad) of system data:

| Sensitivity Level | Access Designation | Operational Impact Threshold |
| :--- | :--- | :--- |
| **Confidential** | Limited to explicit, specific authorized administrative accounts. | High; complete compromise of network perimeter or core configuration controls. |
| **Restricted** | Restricted to explicit users on a strict "Need-to-Know" baseline. | Medium-to-High; unauthorized disclosure of proprietary or personally identifiable information (PII). |
| **Internal-Only** | Restricted to authenticated users physically present on-premises. | Medium; exposure of internal infrastructure configurations or operational telemetry. |
| **Public** | Accessible by anyone; zero authentication layer required. | Low; non-impactful exposure of publicly non-sensitive data points. |

---

## Hardware Asset Inventory

```
+----+-----------------------+----------------+-------------+---------------------+-----------------------------------------------------------------------------------------+----------------+
| ID | Asset Description     | Network Access | Owner       | Location            | Technical Notes & Risk Vector Profiles                                                  | Sensitivity    |
+----+-----------------------+----------------+-------------+---------------------+-----------------------------------------------------------------------------------------+----------------+
| 01 | Network Router        | Continuous     | ISP         | On-Premises (Core)  | Perimeter gateway. Broad/Current channels active. High exposure to remote brute-force.  | Confidential   |
| 02 | Personal Laptop       | Continuous     | Homeowner   | On-Premises (Office)| Primary work machine. High data processing volatility; runs local dev/testing suites.   | Restricted     |
| 03 | Primary Smartphone 1  | Continuous     | Homeowner   | On/Off-Premises     | Active biometric authentication node; holds enterprise MFA tokens and session keys.     | Restricted     |
| 04 | Primary Smartphone 2  | Continuous     | Mother      | On/Off-Premises     | Authenticates to internal network; handles sensitive personal and financial data.       | Restricted     |
| 05 | Living Room Smart TV  | Continuous     | Homeowner   | On-Premises (Zone 1)| Embedded IoT OS. Unpatched firmware vulnerabilities present a risk for network pivoting.| Internal-Only  |
| 06 | Bedroom Smart TV      | Continuous     | Homeowner   | On-Premises (Zone 2)| Secondary IoT endpoint. High risk of legacy protocol vulnerabilities; unmonitored logs. | Internal-Only  |
| 07 | Transient Smart Device| Transient      | Sister      | Guest (Zone 3)      | Untrusted device configuration. Could introduce malware via lateral network movement.   | Internal-Only  |
| 08 | Transient Smart Device| Transient      | Brother-in-L| Guest (Zone 3)      | External device running unknown configuration patches; risk of cross-contamination.     | Internal-Only  |
+----+-----------------------+----------------+-------------+---------------------+-----------------------------------------------------------------------------------------+----------------+
```

## Forensic Threat Analysis: Legacy Hardware Asset Review
During the asset discovery phase, a legacy storage asset was inventoried and subjected to threat evaluation:
* Asset Profile: External Hard Disk Drive (Mechanical HDD).
* Physical Location: On-Premises (Secure Storage).
* Operational Status: Non-Functional / Physical Layer Failure (Suspected mechanical actuator/needle misalignment). Data can occasionally be detected at the system layer via read commands, but I/O operations (file transfers/writes) fail consistently.

### Risk & Security Assessment:
 1. Availability Deficit: The asset represents a critical Availability failure. Although the physical medium remains in the custody of the owner, the data layer is inaccessible due to physical degradation.
 2. Confidentiality Threat (Data at Rest): Because the drive platters contain intact, unencrypted data blocks, this device remains a high security risk. If stolen or disposed of improperly, a sophisticated threat actor could perform advanced hardware recovery techniques (such as platter swapping in a cleanroom environment) to extract raw bits directly from the disk.
 3. Remediation Mandate: The drive has been moved to an offline, air-gapped safe. It is flagged for professional physical destruction (degaussing or mechanical shredding) to guarantee data sanitization. It must never be attached to an active network asset.

## Technical Auditing Summary & Hardening Recommendations
By compiling and analyzing this asset inventory, I mapped the complete technical footprint of the local network environment to identify architectural weaknesses. The audit revealed a significant risk vector common to modern remote workspaces: IoT and Guest Contamination. Multiple unmanaged devices (Smart TVs and guest smartphones) share the same logical subnet as the high-value Corporate Desktop and Personal Laptop containing sensitive information.  

### Implemented Remediation Playbook:
* Network Segmentation: I initiated a logical separation on the primary Network Router. All high-risk IoT devices (Smart TVs) and transient guest devices were moved onto an isolated Guest Network Zone.
* Access Control Enforcement: AP Isolation (Access Point Isolation) was enabled within the Guest Zone. This setting blocks Layer 2 peer-to-peer visibility, preventing an attacker from executing internal reconnaissance or scanning corporate assets if a smart home component is compromised.
* Least Privilege Realignment: Corporate nodes running multi-factor authentication (MFA) parameters have been prioritized for static IP allocation and strict endpoint hardening (OS updates, firewalls active), isolating core data repositories from peripheral noise.
