# Application Threat Model: Sneaker E-Commerce Platform (PASTA Framework)

## Project Description
This project demonstrates the execution of a structured threat modeling engagement for a mobile e-commerce application using the **Process for Attack Simulation and Threat Analysis (PASTA)** framework. PASTA is a risk-centric, threat-modeling methodology designed to align security mitigations directly with business objectives and compliance requirements. By decomposing the application's technological scope, mapping data flows, and modeling potential attack paths (Attack Trees), I identified critical software vulnerabilities and established automated technical controls to secure transactional databases and protect sensitive user data.

---

## PASTA Threat Modeling Worksheet

| PASTA Stage | Focus Area | Sneaker Platform Assessment & Findings |
| :--- | :--- | :--- |
| **Stage I** | **Define Business and Security Objectives** | **Business Objectives:**<br>1. Seamlessly connect sellers and buyers through a mobile marketplace featuring direct messaging and transactional rating systems.<br>2. Facilitate smooth, rapid, and secure financial transactions using multiple payment channels.<br>3. Protect user data privacy and financial records to prevent legal liabilities, transaction fraud, and damage to brand reputation. |
| **Stage II** | **Define Technical Scope** | **Technological Stack:** API Gateways, PKI (AES/RSA), SHA-256 Hashing, and SQL Database Systems.<br><br>**Prioritization Rationale:** The **SQL Database and APIs** must be prioritized first. Because the mobile app relies on public APIs to execute transaction requests and queries a SQL database to fetch listings and process checkouts, these components form the primary entry point for external traffic. Exploiting these interfaces (e.g., via SQL injection) would immediately expose sensitive credit card records and hashed user databases, representing the highest risk to business operations. |
| **Stage III** | **Decompose the Application** | **Data Flow Analysis:**<br>Based on the Application Data Flow Diagram, the user initiates a search query on the mobile client. This traffic transits the API Layer to invoke the **Product Search Process**, which queries inventory tables directly inside the backend **Database** before returning results to the client interface. This continuous read/write pipeline represents a key trust boundary where untrusted input must be rigorously sanitized before executing system-level operations. |
| **Stage IV** | **Threat Analysis** | **Primary Threat Vectors:**<br>1. **SQL Injection (SQLi):** External threat actors targeting open input fields to hijack database queries and gain unauthorized backend access.<br>2. **Session Hijacking:** Opportunistic or targeted interception of user authentication tokens to impersonate buyers or sellers on the platform. |
| **Stage V** | **Vulnerability Analysis** | **Identified Security Gaps:**<br>1. **Lack of Prepared/Parameterized Statements:** Codebase vulnerability where user-supplied inputs are concatenated directly into raw SQL query strings.<br>2. **Weak Credential/Session Controls:** Lack of strict rate-limiting or secure cookie flags on authentication endpoints, exposing accounts to credential-stuffing and session-hijacking attacks. |
| **Stage VI** | **Attack Modeling** | **Threat Vector Mapping (Attack Tree):**<br>The ultimate objective of the threat actor is to compromise **User Data**. The attack tree maps two distinct operational paths to achieve this state:<br>- **Path A:** Compromise database records via *SQL Injection* by exploiting a *lack of prepared statements*.<br>- **Path B:** Compromise active sessions via *Session Hijacking* by exploiting *weak login credentials* or missing token rotation policies. |
| **Stage VII** | **Risk Analysis & Impact** | **Mandatory Security Controls:**<br>1. **Enforce Parameterized SQL Queries:** Restrict the codebase to prepared statements to eliminate SQL injection vulnerabilities.<br>2. **Multi-Factor Authentication (MFA):** Mandate MFA across all accounts to protect credentials from brute-force exploits.<br>3. **Transport Layer Security (TLS/HTTPS):** Encrypt all API traffic in transit to prevent session hijacking and packet-sniffing.<br>4. **API Gateway Rate Limiting:** Apply strict query-rate thresholds on checkout and login API endpoints to mitigate automated brute-force attacks. |

---

## Technical Analysis & Remediation Roadmap

### 1. Hardening the Data Flow Path
According to the Stage III Data Flow Diagram, the `Product Search Process` interacts directly with the backend `Database`. To prevent SQL Injection:
* Developers must use Object-Relational Mappers (ORMs) or parameterize all manual database queries.
* Raw inputs must be sanitized using strict whitelist validation rules (e.g., allowing only alphanumeric characters in search fields).

### 2. Protecting Session Integrity
To disrupt the `Session Hijacking` branch of the Attack Tree:
* Implement cryptographically strong, random session IDs.
* Configure authentication cookies with `Secure`, `HttpOnly`, and `SameSite=Strict` flags.
* Enforce automated session expiration (timeouts) and token rotation upon privilege state changes (e.g., at login or during checkout).
