# Internal Security Audit: Botium Toys

## Objective
The goal of this internal audit is to assess the current security posture of Botium Toys, identify critical vulnerabilities in asset management, and evaluate compliance with PCI DSS and GDPR regulations.

## Scenario
Botium Toys is a small U.S. business experiencing rapid growth in its online market. To support this global expansion, the IT Manager requested an internal security audit to assess the current security posture, identify potential risks to critical assets, and evaluate compliance with U.S. and international regulations, specifically PCI DSS and the E.U. GDPR.

---

## 1. Controls Assessment Checklist

| Control Name | Status |
| :--- | :--- |
| Least Privilege | **No** |
| Disaster recovery plans | **No** |
| Password policies | **No** |
| Separation of duties | **No** |
| Firewall | **Yes** |
| Intrusion detection system (IDS) | **No** |
| Backups | **No** |
| Antivirus software | **Yes** |
| Manual monitoring, maintenance, and intervention for legacy systems | **No** |
| Encryption | **No** |
| Password management system | **No** |
| Locks (offices, storefront, warehouse) | **Yes** |
| Closed-circuit television (CCTV) surveillance | **Yes** |
| Fire detection/prevention (fire alarm, sprinkler system, etc.) | **Yes** |

---

## 2. Compliance Checklist

### Payment Card Industry Data Security Standard (PCI DSS)
| Compliance Best Practice | Status |
| :--- | :--- |
| Only authorized users have access to customers’ credit card information. | **No** |
| Credit card information is stored, accepted, processed, and transmitted internally, in a secure environment. | **No** |
| Implement data encryption procedures to better secure credit card transaction touchpoints and data. | **No** |
| Adopt secure password management policies. | **No** |

### General Data Protection Regulation (GDPR)
| Compliance Best Practice | Status |
| :--- | :--- |
| E.U. customers’ data is kept private/secured. | **No** |
| There is a plan in place to notify E.U. customers within 72 hours if their data is compromised/there is a breach. | **Yes** |
| Ensure data is properly classified and inventoried. | **No** |
| Enforce privacy policies, procedures, and processes to properly document and maintain data. | **Yes** |

### System and Organizations Controls (SOC type 1, SOC type 2)
| Compliance Best Practice | Status |
| :--- | :--- |
| User access policies are established. | **No** |
| Sensitive data (PII/SPII) is confidential/private. | **No** |
| Data integrity ensures the data is consistent, complete, accurate, and has been validated. | **Yes** |
| Data is available to individuals authorized to access it. | **Yes** |

---

## 3. Executive Recommendations
Based on the internal audit, Botium Toys is currently carrying significant operational and compliance risks, particularly regarding PCI DSS and GDPR. To harden the infrastructure and reduce liability, I recommend prioritizing the following actions:

1. **Identity and Access Management (IAM):** Immediately implement a centralized password management system enforcing strict complexity rules. Roll out Role-Based Access Control (RBAC) to establish Least Privilege and Separation of Duties, ensuring that only authorized personnel can access PII and cardholder data.
2. **Data Protection:** Deploy encryption protocols for all data at rest and in transit, specifically targeting the internal database where customer credit card information is stored. 
3. **Business Continuity & Network Security:** Develop and test a formal Disaster Recovery Plan, including offline backups of critical data. Additionally, install an Intrusion Detection System (IDS) to monitor internal network traffic and establish a regular, documented maintenance schedule for all legacy systems.
