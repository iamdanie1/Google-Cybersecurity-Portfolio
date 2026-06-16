# Incident Report Analysis: NIST Cybersecurity Framework (CSF 2.0)

## Executive Summary
The organization experienced a severe network disruption lasting approximately two hours. The cybersecurity incident response team determined the outage was caused by a volumetric Denial of Service (DoS) attack, specifically an **ICMP (Ping) Flood**. A malicious actor exploited an unconfigured edge firewall to overwhelm the internal network with ICMP echo requests, rendering all internal network services inaccessible. The incident management team successfully mitigated the attack by implementing emergency block rules on incoming ICMP traffic and isolating non-critical systems to restore core business functionality.

---

## NIST CSF 2.0 Incident Mapping

### 1. Govern
The root cause of this incident was a governance and policy failure (an unconfigured firewall being deployed to production). To establish foundational oversight moving forward:
* **Policy & Lifecycle Management:** Implement a strict Configuration Management Policy requiring secondary approval and security audits before any edge device (e.g., firewall, router) goes live.
* **Roles & Responsibilities:** Clearly define and document the Incident Response (IR) hierarchy for volumetric attacks, designating specific personnel for ISP escalation and internal stakeholder communication.
* **Risk Strategy:** Update the organizational risk register to formally include DoS/DDoS threats, ensuring management allocates appropriate budget for proactive mitigation tools.

### 2. Identify
* **Threat Vector:** Volumetric Denial of Service (DoS) via ICMP Flood.
* **Affected Assets:** The entire internal corporate network, including all inbound and outbound network services.
* **Vulnerability:** An unconfigured edge firewall that lacked rate-limiting or filtering for ICMP traffic protocols.

### 3. Protect
To safeguard internal assets and mitigate the risk of recurrent volumetric attacks, the network security team implemented the following proactive controls:
* **Access Control (Firewall):** Configured and deployed new firewall rules to strictly rate-limit incoming ICMP packets.
* **Spoofing Prevention:** Implemented Source IP Address Verification on the edge firewall to drop packets with spoofed or malformed origin addresses.
* **Intrusion Prevention System (IPS):** Deployed an IPS appliance to actively filter and drop ICMP traffic exhibiting known malicious or suspicious characteristics.

### 4. Detect
To ensure rapid identification of future anomalies before they result in a full network outage, the following visibility enhancements were integrated:
* **Continuous Monitoring:** Deployed network monitoring software specifically baselined to detect abnormal traffic volume patterns (spikes).
* **Intrusion Detection System (IDS):** Configured IDS alerts to notify the Security Operations Center (SOC) immediately upon detecting traffic signatures indicative of a DoS/DDoS attempt.

### 5. Respond
In the event of a future detected DoS incident, the cybersecurity team will execute the following standardized response protocol:
* **Containment:** Immediately isolate heavily targeted segments to prevent lateral network congestion. 
* **Mitigation:** Dynamically route or drop the malicious traffic at the firewall/ISP level.
* **Communication:** Escalate the incident to upper management (as defined in the Govern phase) and notify external ISPs or DDoS mitigation partners.
* **Analysis:** Preserve network logs and PCAP files for post-incident root cause analysis and threat actor attribution.

### 6. Recover
To restore operations systematically following an attack, the organization will follow a phased recovery approach:
* **Phased Restoration:** Ensure all external ICMP flood traffic has ceased or been successfully blackholed. 
* **Prioritization:** Restore critical business services (e.g., core databases, customer-facing web servers) to a functioning state first.
* **Normalization:** Once stability is verified, systematically bring all non-critical network systems and secondary services back online.
