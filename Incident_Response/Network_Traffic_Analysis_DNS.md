# Cybersecurity Incident Report: Network Traffic Analysis

## 1. Executive Summary & Incident Scenario
Client customers reported an inability to access the corporate website (`www.yummyrecipesforme.com`), encountering a "destination port unreachable" error upon page load. As the responding analyst, I utilized the network protocol analyzer tool `tcpdump` to capture data packets in transit and investigate the root cause of the connectivity failure.

---

## 2. Packet Capture (PCAP) Analysis
### Summary of Findings in the DNS and ICMP Traffic Log

During the investigation, `tcpdump` captured the outgoing query and the subsequent server response. An analysis of the UDP and ICMP protocols reveals the following:

* **The UDP Protocol Request:** The log shows the host machine (`192.51.100.15`) sending a standard DNS query via UDP to the DNS server (`203.0.113.2`). The presence of the `A?` flag indicates a request for an A record to resolve the domain `yummyrecipesforme.com`.
* **The ICMP Protocol Response:** The network analysis shows that the query failed. The DNS server immediately returned an ICMP echo reply containing a specific error message: `"udp port 53 unreachable"`.
* **Port Identification:** The port noted in the error message (Port 53) is the standard port reserved for DNS (Domain Name System) protocol communications.
* **The Core Issue:** The ICMP error explicitly indicates that the DNS server is not listening on Port 53. Because DNS resolution is failing, the browser cannot obtain the destination IP address required to establish an HTTPS connection to the web server, resulting in a site-wide outage for the users.

---

## 3. Investigation Timeline and Incident Cause

* **Time Incident Occurred:** The incident was first captured in the logs today at 1:24 p.m. (13:24:32).
* **Incident Discovery:** The IT team became aware of the incident following multiple reports from client customers who were unable to access the website.
* **Current Status:** The cybersecurity team is actively investigating the network flow to restore access.
* **Investigative Actions Taken:** We conducted packet sniffing tests using `tcpdump` to observe the traffic behavior between the host browser and the DNS server, confirming the traffic drops at the DNS level, not the web server level.

### Suspected Root Cause
Due to the ICMP error response, it is highly likely the DNS server is down or rejecting traffic. The two most probable root causes are:
1. **Denial of Service (DoS) Attack:** An attacker may have successfully flooded the DNS server, causing the service to crash and making it unable to respond to legitimate network traffic.
2. **Network Misconfiguration:** A recent, undocumented change to the firewall configurations may be actively blocking UDP Port 53 traffic. 

### Next Steps for Remediation:
1. Contact the network administration team to identify whether the DNS server is actively down or if traffic to Port 53 is being blocked by the firewall.
2. If the server was subjected to a DoS attack, implement rate-limiting and IP filtering to mitigate the flood before restarting the DNS daemon.
