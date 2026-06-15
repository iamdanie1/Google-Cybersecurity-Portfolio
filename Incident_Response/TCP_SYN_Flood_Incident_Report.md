# Cybersecurity Incident Report: TCP SYN Flood Attack

## 1. Executive Summary
During routine operations, the corporate sales website experienced an unexpected outage, resulting in connection timeout errors for internal employees and external customers. Monitoring systems flagged abnormal web server behavior. Upon investigation using (`Wireshark`) to analyze the captured network traffic, it was determined that the server was the victim of a Denial of Service (DoS) attack—specifically a TCP SYN flood—which exhausted server resources and caused the service interruption.

---

## 2. Incident Identification and PCAP Analysis
### Identification of the Attack Vector

Based on the captured Wireshark TCP/HTTP logs, the root cause of the connection timeout error is a **TCP SYN Flood Attack**. 

* **The Baseline:** Initial log entries (e.g., lines 47-51) show normal traffic, where a legitimate client (`198.51.100.23`) successfully completes a 3-way handshake with the web server (`192.0.2.1`) and receives an `HTTP 200 OK` response.
* **The Malicious Traffic:** The packet sniffer revealed an abnormally high volume of incoming TCP `[SYN]` requests originating from a single, unfamiliar IP address: **`203.0.113.0`**.
* **The Smoking Gun:** The attacker continuously sent `[SYN]` packets (dominating the log from line 66 onwards). The web server (`192.0.2.1`) replied with `[SYN, ACK]` packets, but the attacker's IP never returned the final `[ACK]`, leaving the connections half-open.

---

## 3. Attack Mechanics and Network Impact
### How the Attack Malfunctions the Website

To understand how this attack successfully took the web server offline, it is necessary to understand the standard TCP Three-Way Handshake (SYN > SYN-ACK > ACK).

**The Exploitation:**
The malicious actor (`203.0.113.0`) intentionally abandoned the TCP handshakes. The web server dutifully reserved memory for each incoming request, waiting for final acknowledgments that never arrived. 

**The Impact (Server Exhaustion):**
The server was forced to keep thousands of "half-open" connections in its memory queue, quickly exhausting its available resources. The logs explicitly show the moment the server failed:
* At line 63, a legitimate user (`198.51.100.5`) attempted to connect.
* By line 77, the overwhelmed web server (`192.0.2.1`) was forced to respond to this legitimate user with an **`HTTP/1.1 504 Gateway Time-out`**. 
* Subsequently, the server began sending `[RST, ACK]` (Reset) packets to other legitimate incoming connections, effectively dropping all normal business traffic.

---

## 4. Immediate Response and Future Remediation

### Immediate Actions Taken
To restore business operations, the affected server was temporarily taken offline to clear the backlog of half-open connections and recover its memory allocation. Concurrently, a temporary block was applied at the firewall level to drop all traffic originating from the malicious IP address (`203.0.113.0`).

### Strategic Remediation (Next Steps)
While blocking the specific IP address temporarily resolved the immediate outage, an attacker can easily spoof a different IP address to bypass this block. To harden the network against future SYN floods, I recommend discussing the implementation of the following controls with the infrastructure team:

1. **Implement SYN Cookies:** Configure the server/firewall to use SYN cookies, allowing the server to avoid allocating memory for a connection until the final ACK is received.
2. **Reduce SYN Timeout Thresholds:** Lower the amount of time the server waits for an ACK before dropping the half-open connection, freeing up resources faster.
3. **Deploy a WAF/DDoS Mitigation Service:** Consider routing web traffic through a cloud-based Web Application Firewall (WAF) or mitigation service that can automatically absorb and filter malicious volumetric traffic before it reaches the internal network.
