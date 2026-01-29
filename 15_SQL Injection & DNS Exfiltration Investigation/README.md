# Incident Response Simulation: SQL Injection and DNS Exfiltration
![Status](https://img.shields.io/badge/Project-Simulation-blue)
![Security](https://img.shields.io/badge/Focus-Incident%20Response-red)
![Logs](https://img.shields.io/badge/Logs-Zeek%20%26%20Suricata-orange)
![Platform](https://img.shields.io/badge/Platform-Security%20Onion-green)
![Documentation](https://img.shields.io/badge/Docs-Available-success)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## Overview
This repository documents a simulated end-to-end incident response case study involving a web server breach.  
The scenario provided a **Security Onion** environment pre-loaded with **Zeek** and **Suricata** logs from a June 2020 exploit window.

### Objectives
- **Identify the attack vector**: Determine the source and method of initial compromise via HTTP traffic analysis.  
- **Determine impact**: Investigate potential data exfiltration methods and scope of compromise.  
- **Propose remediation**: Develop actionable intelligence and mitigation strategies.  

---

## Tools Utilized
- **Security Onion**: Primary analysis platform (Linux-based distribution for intrusion detection).  
- **Kibana/Elasticsearch**: Log aggregation and visualization for event correlation.  
- **Zeek (Bro)**: Network Security Monitor (NSM) used for generating comprehensive network logs (HTTP, DNS).  
- **capME!**: Web interface for viewing full PCAP transcripts.  

---

## Case Study: Investigation & Findings

### Part 1: Initial Compromise via SQL Injection
Analysis began with reviewing HTTP traffic within Kibana, focusing on the `uri` fields for anomalies suggestive of web application attacks.  
The investigation successfully isolated a specific threat actor IP and identified the malicious payload.

#### Key Findings
- **Source IP**: `[Insert Source IP found in lab]`  
- **Vulnerability**: Classic SQL Injection (SQLi) used to bypass authentication and execute unauthorized database queries.  
- **Payload Example**:
```bash
username='+union+select+ccid,ccnumber,ccv,expiration,null+from+credit_cards+--+&password=
```

This indicates an attempt to dump the entire credit card table.  

📎 See `assets/kibana_http_filter.png` and `assets/capme_transcript.png` for visual evidence of logs and PCAP transcript.

---

### Part 2: Data Exfiltration via DNS Tunneling
Subsequent analysis shifted to DNS logs after noticing abnormally long query strings, a common indicator of covert data transfer (tunneling).  
The data was encoded and sent in subdomains of lookups.

#### Key Findings
- **Technique**: Data Exfiltration over DNS.  
- **Data Exfiltrated**: PII and credit card information encoded within DNS A records.  
Example:
```bash
ccnumber.signature.data.maliciousdomain.com
```
See `assets/dns_log_counts.png` for metrics on unusual DNS query length spikes.

---

## Detections & Remediation (Analyst Recommendations)

### Detection Rules (IOCs and Signatures)
Custom **Suricata/Snort rules** have been drafted to detect these specific behaviors going forward.  
Refer to `documentation/detection_rules.md` for specific detection logic.

### Remediation Report
A formal report detailing immediate containment, eradication, and future hardening measures.  
Refer to `documentation/remediation_report.md` for:
- SQLi prevention using **parameterized queries**  
- Implementation of **DNS query length monitoring**  

---
