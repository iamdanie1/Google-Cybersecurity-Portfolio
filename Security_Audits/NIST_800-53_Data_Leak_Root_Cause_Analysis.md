# Incident Post-Mortem & Root Cause Analysis: NIST SP 800-53 Control Alignment

## Project Description
In response to an unauthorized data exposure incident where internal product plans and customer analytics data leaks occurred on public social media, I executed a technical Root Cause Analysis (RCA). This audit evaluates the systemic breakdown of internal Identity and Access Management (IAM) practices and maps the operational failures directly to the NIST SP 800-53 security control framework. By identifying systemic policy gaps regarding loose data-sharing inheritance, I established a suite of remediation blueprints designed to enforce technical Least Privilege controls and deploy automated Data Loss Prevention (DLP) parameters across the corporate environment.

---

## Incident Root Cause Investigation

### 1. Issue Analysis
The data leak was driven by a compounding failure of administrative oversight and loose access inheritance boundaries:
* **Privilege Creep / Excessive Access:** A manager granted broad folder-level read permissions to a customer success representative for a temporary briefing but failed to implement an automated access teardown or revocation protocol post-meeting.
* **Lack of Logical Segmentation:** High-value internal intellectual property (unannounced product plans and customer analytics data stores) was co-mingled inside the same parent directory folder structure as public-facing marketing materials.
* **Reliance on Administrative Controls:** The organization relied on verbal warnings ("wait for approval before sharing") rather than hard technical constraints, allowing a single human error (an accidental link-sharing misclick during an external sales call) to result in catastrophic data exfiltration.

### 2. Framework Review: NIST SP 800-53 (AC-6)
The NIST SP 800-53 control enhancement family **AC-6 (Least Privilege)** explicitly dictates that organizations must allocate the absolute minimum set of privileges required for users to execute necessary business functions. AC-6 operates as a foundational mechanism for data privacy and system hardening; by compressing the operational footprint of an individual user account, the system naturally limits the blast radius of credential theft, lateral threat movement, and accidental data exposure stemming from human error.

---

## Strategic Remediation Framework

To address the vulnerabilities identified during this investigation, the organization must transition from vulnerable administrative directives to strict technical enforcement pathways.

### Recommended Control Enhancements (NIST SP 800-53 Allocation)

* **Recommendation 1: Enforce AC-6 (1) - Explicit Role-Based Access Control (RBAC) & Folder Separation**
  * *Implementation:* Systematically decouple internal corporate project data structures from external-facing promotional repositories. Marketing personnel and customer success staff must only be provisioned access to isolated, consumer-facing assets. Furthermore, any out-of-role folder sharing permissions must be governed by time-bound, self-expiring access tokens (TTL - Time-to-Live) to prevent the accumulation of residual access rights.

* **Recommendation 2: Enforce AC-6 (9) - Technical Data Loss Prevention (DLP) & Sharing Restrictions**
  * *Implementation:* Deploy automated tenant-level Data Loss Prevention (DLP) classification policies across the cloud collaboration suites. Files tagged as `Internal-Only` or containing sensitive data strings (such as customer analytics or source code parameters) must be hard-blocked from being shared via anonymous or external URL strings. 

---

## Remediation Justification
Implementing these technical controls drastically reduces the organization's exposure profile by removing human error from the security equation. Even if a customer success representative accidentally copies an incorrect folder link during a live external call, the tenant-level **DLP sharing restrictions** will intercept the transmission and block external resolution of the URL asset. 

Concurrently, by restructuring the data architecture into segregated **RBAC directories**, the representative would lack the underlying authorization tokens required to interact with internal business designs in the first place. Transitioning from verbal policy reliance to cryptographic and technical system barriers permanently mitigates the risk of accidental data exposure, ensuring absolute alignment with enterprise information privacy expectations.
