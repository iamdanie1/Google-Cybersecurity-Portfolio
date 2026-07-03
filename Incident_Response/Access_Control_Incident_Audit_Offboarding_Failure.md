# Access Control Audit & Incident Investigation: Offboarding Architecture Breakdown

## Project Description
In this portfolio project, I acted as a lead security analyst to perform an incident triage and access control accounting audit following a suspicious, unauthorized financial payroll transaction. By cross-referencing system event logs with corporate active directory databases, I traced an active compromise vector back to an unrevoked, highly privileged legacy account belonging to an external contractor. This project highlights the critical importance of Identity and Access Management (IAM), lifecycle offboarding governance, and technical authentication controls required to reduce an organization's internal and external attack surface.

---

## Forensic Incident Accounting

### 1. Artifact Triage: System Event Log
The security team intercepted a critical log entry from the internal personnel platform signaling an unauthorized system modification:

```
Event Type:     Information
Event Source:   AdsmEmployeeService
Event ID:       1227
Date/Time:      10/03/2023 at 8:29:57 AM
User Account:   Legal\Administrator
Workstation:    Up2-NoGud
Source IP:      152.207.255.255
Description:    Payroll event added. FAUX_BANK
```
### 2. Personnel Database Cross-Reference
To discover the identity behind the account, I audited the active employee directory database, resulting in a positive match:

* **Identity Profile**: Robert Taylor Jr.
* **Corporate Role**: Legal Attorney / External Contractor
* **Assigned Host IP**: 152.207.255.255
* **Account Authorization Level**: Admin
* **Lifecycle Timestamps**: Lifecycle Start: 09/04/2019 | Lifecycle End: 12/27/2019
* **Last Logged Access**: 10/03/2023 at 8:29:57 AM (Executed via contractor endpoint)

# Vulnerability & Root Cause Analysis
| Audit Sector | Identified Security Gap & Root Cause Analysis | Operational Risk Level |
| :---: | :--- | :---: |
| **Authentication Controls** | **Complete Offboarding Protocol Failure:** The contractor's operational agreement terminated on 12/27/2019. However, the account remained active within the identity provider for nearly four years, allowing an unauthorized user to log in in 2023. | **Critical** |
| **Authorization Boundaries** | **Violation of Least Privilege (Privilege Creep):** An external legal contractor was provisioned global `Admin` rights. There was zero logical segregation blocking a legal advisor from modifying transactional financial payroll tables. | **Critical** |
| **Managerial / Monitoring** | **Absence of Shared Drive & Account Governance:** Every employee across the enterprise manages resources via a unified, shared cloud drive with uniform administrative capabilities. There are no technical alerts flagging active connections from dead accounts. | **High** |

# Technical Remediation Playbook
To safeguard enterprise assets and guarantee that legacy or external threat vectors cannot execute remote modifications, I recommend implementing the following control matrix:

## 1. Short-Term Technical Mitigations (Immediate Enforcement)
* **Automated Account Expiration Lifecycles**: Configure the Identity Provider (IdP) to enforce hard expiration limits on all external contractor profiles. Temporary accounts must automatically lock or self-terminate after 30 days unless explicitly renewed by administrative override.
* **Multi-Factor Authentication (MFA) Mandate**: Deploy tenant-wide Multi-Factor Authentication (MFA) across all corporate endpoints and administrative panes. This ensures that even if an offboarded user retains valid passwords or access links, they cannot complete authentication without passing a secondary biometric or cryptographic challenge.

## 2. Long-Term Architectural Mitigations (System Hardening)
* **Role-Based Access Control (RBAC) Architecture**: Deconstruct the shared cloud drive system. Segment permissions into explicit departmental roles (e.g., Legal cannot view or write to Payroll resources). Administative privileges (Admin) must be removed from general staff and limited strictly to dedicated system security personnel.
* **Enforce Strict Account Provisioning and Review Policies**: Develop strict managerial checklists requiring HR and IT infrastructure teams to execute synchronized account tear-downs the exact day a worker or contractor's end date is reached.
 
# Summary & Security Takeaway
This incident highlights the structural dangers of relying on loose, unmonitored access control boundaries. A clear breakdown in offboarding governance allowed a contractor whose agreement ended in 2019 to authenticate to a high-value cloud environment in 2023 and alter critical payroll destinations. By implementing automated lifecycle expiration states, restricting cross-departmental administrative rights through strict Role-Based Access Control (RBAC), and enforcing Multi-Factor Authentication (MFA), the organization completely eliminates the viability of stale credentials being leveraged to disrupt financial or operational data integrity.
