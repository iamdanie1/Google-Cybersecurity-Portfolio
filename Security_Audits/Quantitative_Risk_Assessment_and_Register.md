# Quantitative Risk Assessment & Risk Register: Commercial Banking Sector

## Project Description
In alignment with the NIST Cybersecurity Framework (CSF) Identify function, I executed a formal qualitative and quantitative risk assessment for a commercial banking infrastructure. Operating under strict regulatory requirements—including Federal Reserve capitalization and liquidity baselines—I analyzed threats against the bank's core assets (funds and customer repositories). By mapping vulnerabilities across the bank's physical and distributed operational perimeter, I calculated risk scores using the industry-standard matrix equation ($Likelihood \times Severity = Risk$). This risk register serves as a foundational audit document to prioritize capital allocation and defensive engineering controls.

---

## Operational Environment Security Context
The target institution operates in a coastal region characterized by low structural localized crime rates, which significantly minimizes external physical security vectors. However, the organization's logical threat surface is highly expanded and distributed, driven by a workforce of 100 on-premise corporate users and 20 permanent remote employees connecting via external networks. The database architecture stores high-value repositories for 2,000 individual retail accounts and 200 commercial corporate accounts. Marketing partnerships with community businesses and a professional sports franchise increase public visibility, making the bank an attractive target for opportunistic threat actors and targeted social engineering. Furthermore, strict compliance mandates require constant data availability and precise daily cash reserves to satisfy regulatory parameters.

---

## Risk Analysis Notes
Security events within this environment are driven primarily by human error, configuration drifting, and geographic environment threats rather than localized physical breaches. The presence of remote employees heavily expands the perimeter, creating entry points for credential harvesting and phishing vectors. Concurrently, software configuration gaps—such as weak cryptographic controls or misconfigured cloud backups—expose high-value financial databases to public indexing. Finally, structural dependencies on physical supply chains, coupled with coastal weather vulnerabilities (such as hurricanes), introduce clear threats to availability and compliance frameworks.

---

## Threat Matrix & Scoring Standard
Risk priority is calculated mathematically by evaluating both the estimated frequency of an exploit window and the overall operational/financial impact of the resulting incident.

### Likelihood Scale:
* **3 (Certain/High):** Highly frequent; expected to occur multiple times per annualized cycle.
* **2 (Likely/Moderate):** Occasional; documented history of occurrence within similar sectors.
* **1 (Rare/Low):** Unlikely; minimal historical precedent or mitigated by physical context.

### Severity Scale:
* **3 (Catastrophic/High):** Severe regulatory fines, complete data exfiltration, or permanent reputational damage.
* **2 (Major/Moderate):** Operational disruption, localized data exposure, or moderate financial impact.
* **1 (Minor/Low):** Negligible impact; easily absorbed by baseline operating procedures.

---

## Corporate Risk Register

| Asset | Identified Risk(s) | Vulnerability Description | Likelihood | Severity | Priority Score | Mitigation Urgency |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **Funds** | Business Email Compromise (BEC) | Remote access footprints allow threat actors to trick users into disclosing internal credentials or executing unauthorized transfers. | **3** | **2** | **6** | **High** |
| **Funds** | Compromised User Database | Inadequate or legacy encryption standards at rest allow attackers to read harvested customer authentication profiles. | **2** | **3** | **6** | **High** |
| **Funds** | Financial Records Leak | Database server or cloud backup repository configuration drift leaves sensitive files publicly accessible via the internet. | **2** | **3** | **6** | **High** |
| **Funds** | Physical Theft | Insufficient internal operational discipline results in the main corporate vault or safe being left unlatched. | **1** | **3** | **3** | **Medium** |
| **Funds** | Supply Chain Disruption | Regional coastal weather patterns (hurricanes) cause logistics delays, impacting daily liquidity delivery lines. | **1** | **2** | **2** | **Low** |

---

## Risk Assessment Findings & Remediation Roadmap

### 1. High-Priority Vector Triage (Scores 6)
The assessment distinctly isolates **Business Email Compromise**, **User Database Compromise**, and **Financial Records Leaks** as the most critical threat vectors threatening corporate funds. While a database leak or configuration issue may have a moderate-to-likely chance of occurrence, their catastrophic impact on regulatory compliance (Federal Reserve, data privacy laws) and customer churn pushes them to the top of the remediation matrix. 
* *Immediate Directive:* Implement strict identity controls (MFA enforcement across all 120 users), mandate quarterly Security Awareness training focused on phishing defense, and enforce automated configuration auditing for cloud storage infrastructure.

### 2. Lower-Priority Vector Triage (Scores 2–3)
Physical asset theft and supply chain delays score lower on the overall matrix due to the bank's operating context. The low localized crime rate mathematically suppresses the likelihood of targeted vault attacks, while historical weather data indicates that severe coastal disruptions are rare, multi-year events. 
* *Immediate Directive:* Enforce standard operational compliance checklists for vault operations and establish a secondary emergency cash logistics partner to maintain legal cash requirements during weather anomalies.
