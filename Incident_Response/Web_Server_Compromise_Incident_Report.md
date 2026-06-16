# Cybersecurity Incident Report: Web Server Compromise & Malware Distribution

## 1. Executive Summary
The IT Helpdesk received multiple reports from customers stating that their computers were experiencing severe performance degradation after downloading a prompted "browser update" from the company website (`yummyrecipesforme.com`). Upon investigation, the cybersecurity team confirmed that the web server had been compromised. A malicious actor gained unauthorized administrative access, altered the website's source code, and embedded a malicious payload designed to redirect end-user traffic to an unauthorized domain.

---

## 2. Incident Timeline and PCAP Analysis
### Identification of Network Protocols and Attack Flow

A sandbox environment was established to safely interact with the compromised website, and `tcpdump` was utilized to capture the resulting network traffic. The packet capture explicitly outlines the malware's execution chain:

* **Initial Access (14:18:32 - 14:18:36):** The log shows the sandbox machine initiating a **DNS** request for the legitimate website, followed by a standard **HTTP** `GET` request over Port 80. This traffic represents the user accessing the compromised page and unwittingly downloading the malicious executable (disguised as a browser update) at the Application Layer.
* **Malware Execution & Redirection (14:20:32):** Following the execution of the downloaded file, the logs show an unprompted **DNS** query originating from the host machine for a known malicious domain: `greatrecipesforme.com`.
* **C2 / Traffic Hijacking (14:25:29):** The host machine then initiates a new **HTTP** connection to the malicious IP (`192.0.2.17`). This confirms the payload successfully hijacked the browser's routing, redirecting user traffic to the threat actor's infrastructure.

---

## 3. Root Cause Analysis
### The Vulnerability: Inadequate Access Controls

Concurrent to the traffic analysis, the website owner reported being locked out of the administrative panel. An investigation into the server access logs revealed the root cause of the compromise: a successful **brute-force attack** against the administrative portal. 

The threat actor was able to compromise the web server because:
1. **Default Credentials:** The administrative account was still utilizing the manufacturer's default password, severely reducing the time required to guess the credentials.
2. **Lack of Rate Limiting:** The server lacked basic OS and application hardening configurations, allowing the attacker to make unlimited automated login attempts without triggering an account lockout.

---

## 4. Strategic Remediation and OS Hardening

To securely restore the web server and prevent future brute-force attacks, the server must be wiped, restored from a clean backup, and hardened using the following Identity and Access Management (IAM) controls:

1. **Enforce Multi-Factor Authentication (MFA/2FA):** * **Implementation:** Require all administrative accounts to authenticate using both a password and a time-based one-time password (TOTP) generated on a separate device. 
   * **Why it works:** Even if an attacker successfully brute-forces or steals a password, they cannot access the system without physical possession of the secondary authentication token.
2. **Implement Account Lockout Policies:**
   * **Implementation:** Configure the server to automatically lock an account for 30 minutes after three consecutive failed login attempts.
   * **Why it works:** This mathematically defeats brute-force attacks by preventing the attacker's automated scripts from rapidly guessing thousands of password combinations.
3. **Disable Default Credentials & Enforce Complexity:**
   * **Implementation:** Mandate that all default vendor passwords are changed immediately upon installation, enforcing a minimum complexity of 14 characters.
