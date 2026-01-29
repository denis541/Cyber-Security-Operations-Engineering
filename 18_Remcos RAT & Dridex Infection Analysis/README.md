# Incident Investigation: Remcos RAT & Dridex Distribution
![Case](https://img.shields.io/badge/Case-Multi--Stage%20Infection-blue)
![Threats](https://img.shields.io/badge/Threats-Remcos%20RAT%20%7C%20Dridex-red)
![Detection](https://img.shields.io/badge/Detection-Suricata%20%7C%20Sguil-yellow)
![Tools](https://img.shields.io/badge/Tools-Wireshark%20%7C%20Zeek%20%7C%20Elastic-green)
![IOC](https://img.shields.io/badge/IOCs-DNS%20%7C%20C2%20%7C%20SSL-orange)
![Status](https://img.shields.io/badge/Status-Resolved-success)

**Date:** 2019-03-19  
**Analyst:** Denis O. Onduso

**Status:** Resolved / Documented  

---

## 1. Executive Summary
On **2019-03-19**, network security monitoring (NSM) systems triggered multiple high-severity alerts indicating a **multi-stage infection** on a Windows endpoint.  
The attack chain involved:
- An initial **DNS update**  
- Retrieval of two **malicious executables**  
- Subsequent **Command & Control (C2) check-ins** via the **Remcos Remote Access Trojan (RAT)** and **Dridex malware families**  

---

## 2. Intrusion Timeline (2019-03-19)

| Time (UTC) | Event Type        | Description                                                                 |
|------------|------------------|-----------------------------------------------------------------------------|
| 19:15:30   | DNS Activity      | Dynamic DNS update initiated by the internal Windows host.                  |
| 19:16:05   | Initial Payload   | HTTP GET request for `test1.exe` from external IP.                          |
| 19:17:42   | C2 Check-in       | Encrypted communication detected on non-standard ports (**Remcos RAT**).    |
| 19:22:11   | Secondary Infection | Detection of **Dridex-associated SSL certificates** via source IP `31.22.4.176`. |

---

## 3. Technical Analysis & Tool Methodology

### A. Signature Triage (Suricata / Sguil)
- **Detection Source**: Suricata signatures via Sguil console.  
- **Alert IDs**: ET TROJAN ABUSE.CH SSL Blacklist, Remcos RAT Checkin.  
- **Methodology**: Pivoted from signature alerts to raw packet data to validate detection and eliminate false positives.  

---

### B. Traffic Forensics (Wireshark / Zeek)
- **PCAP Analysis**: Exported traffic into Wireshark for stream reassembly.  
- **Payload Extraction**: Manual extraction revealed executable with **MZ (hex 4D 5A)** file signature.  
- **Protocol Analysis**: Zeek metadata correlated source/destination IPs and verified C2 session duration.  

---

### C. Malware Hash Verification
- **File Integrity**: Verified using SHA256 hashing.  
- **Sample 1 Hash**:
  2a9b0ed40f1f0bc0c13ff35d304689e9cadd633781cbcad1c2d2b92ced3f1c85
- **Intelligence Correlation**: Cisco Talos Intelligence confirmed the sample as a malicious downloader associated with Trojan activity.  

---

### D. SIEM Correlation (Kibana / Elastic Stack)
- **Log Aggregation**: Combined NIDS and HIDS logs to confirm endpoint execution of downloaded malware.  
- **Data Visualization**: Filtered logs by IP `31.22.4.176` to isolate communications tied to Dridex certificate detection.  

---

## 4. Indicators of Compromise (IOCs)
- **Hostnames**: [Redacted Hostname from Lab]  
- **Malicious IPs**: `31.22.4.176` (C2 Server)  
- **Files**: `test1.exe` (SHA256: `2a9b0ed4...`)  
- **Threat Actors**: Remcos RAT, Dridex  

---

## 5. Analyst Recommendations
- **Isolation**: Disconnect affected Windows host from the network for re-imaging.  
- **Blacklisting**: Block IP `31.22.4.176` and associated malicious SSL hashes at the firewall level.  
- **Credential Reset**: Force password reset for the user involved, as **Remcos RAT** is capable of credential harvesting.  

---
