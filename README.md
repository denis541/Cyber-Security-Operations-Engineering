# Security Operations & Cybersecurity Analysis Portfolio

![SOC Analyst](https://img.shields.io/badge/Role-SOC_Analyst-0052CC)
![Incident Response](https://img.shields.io/badge/Specialization-Incident_Response-FF6B6B)  
![Threat Hunting](https://img.shields.io/badge/Focus-Threat_Hunting-00B894)
![Forensics](https://img.shields.io/badge/Expertise-Digital_Forensics-6C5CE7)     
        
**Author:** Denis O. Onduso        

**Security Domain:** Security Operations Center (SOC) | Digital Forensics | Detection Engineering    
  
**Status:** Operational Analysis Portfolio | Professional Readiness Demonstration


## Executive summary.


This repository documents comprehensive security operations analysis capabilities developed through systematic, hands-on investigation of simulated production security incidents. The work demonstrates proficiency across the full spectrum of SOC analyst responsibilities, from initial alert triage through to comprehensive incident documentation and escalation.

## Scope of Analysis.

The repository reflects structured security operations workflows aligned with industry-standard incident analysis and response practices.

| Phase | Activities | Tools & Techniques |
|------|-----------|-------------------|
| **Evidence Collection** | Endpoint artifact acquisition, network packet capture, centralized log aggregation | Sysinternals Suite, Wireshark, PowerShell |
| **Analysis & Correlation** | Timeline reconstruction, cross-source validation, indicator extraction and enrichment | Timeline analysis methodologies, event correlation techniques, cryptographic hash validation |
| **Impact Assessment** | Scope determination, severity classification, affected asset identification | Business impact analysis, risk assessment frameworks |
| **Documentation & Reporting** | Incident reporting, executive summary preparation, evidence preservation | Standardized reporting formats, chain-of-custody practices |

---
## Technical Analysis Capabilities.

### Endpoint Security Analysis.

![Endpoint](https://img.shields.io/badge/Endpoint-Sysinternals-0078D4?style=for-the-badge)
![Automation](https://img.shields.io/badge/Automation-PowerShell-5391FE?style=for-the-badge)

- **Process Execution Analysis**  
  Identification and tracking of malicious process creation, including parent-child relationships and full execution chain reconstruction.

- **Memory Forensics**  
  Examination of volatile memory artifacts to identify hidden processes, injected code, and in-memory execution techniques.

- **File System Artifact Analysis**  
  Analysis of Prefetch files and ShimCache entries to validate program execution and establish execution timelines.

- **Registry Forensics**  
  Identification of persistence mechanisms and malicious configuration changes through registry analysis.

- **PowerShell Activity Auditing**  
  Analysis of script block logging, command-line arguments, and execution context to identify abuse of PowerShell for post-compromise activity.

### Network Security Analysis.

![Protocol Analysis](https://img.shields.io/badge/Protocol-Analysis-4ECDC4?style=for-the-badge)
![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679C2?style=for-the-badge)

- **Protocol Dissection**  
  Deep packet inspection across the TCP/IP stack to identify abnormal protocol behavior and misuse.

- **Anomaly Detection**  
  Identification of beaconing patterns, command-and-control communications, and data exfiltration activity.

- **Payload Extraction**  
  Recovery of malicious payloads from network streams to support secondary analysis and malware classification.

- **Traffic Reconstruction**  
  Session reassembly and application-layer inspection to reconstruct attacker activity and data flow.

- **Encryption Analysis**  
  Inspection of SSL/TLS usage where visibility allows, including cipher suite evaluation and encryption implementation review.

---

### Detection & Log Engineering

![SIEM Integration](https://img.shields.io/badge/Platform-SIEM%20Integration-F39C12?style=for-the-badge)
![Log Normalization](https://img.shields.io/badge/Process-Log%20Normalization-8E44AD?style=for-the-badge)

- **Log Normalization**  
  Standardization of heterogeneous log sources to enable consistent analysis and reliable correlation.

- **Event Correlation**  
  Development of multi-source correlation logic to identify complex attack patterns across environments.

- **Timeline Analysis**  
  Chronological reconstruction of security events to establish attack progression and dwell time.

- **Alert Tuning**  
  Iterative refinement of detection logic to reduce false positives and improve signal quality.

---

### Cryptographic Security Validation

![Integrity Validation](https://img.shields.io/badge/Crypto-Integrity%20Validation-3498DB?style=for-the-badge)
![PKI Validation](https://img.shields.io/badge/Trust-PKI%20Validation-2ECC71?style=for-the-badge)

- **Hash Validation**  
  Verification of file integrity using cryptographic hashing to detect tampering or unauthorized modification.

- **Certificate Analysis**  
  Validation of PKI trust chains through certificate parsing and issuer verification.

- **Encryption Assessment**  
  Evaluation of encryption implementations and cipher strength to identify weak or misconfigured cryptographic controls.
## Incident Investigation Case Studies

The following case studies demonstrate end-to-end incident investigation capabilities, from initial detection through impact assessment and documentation. Each scenario reflects analysis patterns commonly encountered in enterprise SOC and incident response operations.

---

### Case Study 1: Endpoint Compromise Analysis

**Scenario:**  
Investigation of a Remcos Remote Access Trojan (RAT) infection impacting multiple enterprise endpoints.

**Key Findings:**
- Identified malicious persistence mechanisms implemented through Windows Registry `Run` keys.
- Mapped recurring network beaconing activity to external command-and-control (C2) infrastructure.
- Recovered staged malware payloads from temporary directories, confirming multi-stage execution.
- Documented attempted lateral movement activity leveraging SMB-based techniques.

---

### Case Study 2: Data Exfiltration Investigation

**Scenario:**  
Analysis of unauthorized FTP-based transfer of sensitive data from internal systems.

**Key Findings:**
- Correlated user authentication events with outbound FTP transfer activity.
- Identified data staging behavior prior to exfiltration.
- Quantified the volume and sensitivity of data exfiltrated from the environment.
- Established attacker dwell time through timeline reconstruction and log correlation.

---

### Case Study 3: Web Application Compromise

**Scenario:**  
Investigation of a SQL injection attack resulting in unauthorized database access and credential exposure.

**Key Findings:**
- Traced injection payloads through web server and application logs.
- Identified anomalous database queries indicative of exploitation activity.
- Mapped attacker reconnaissance and enumeration behavior.
- Documented credential access and subsequent privilege escalation attempts.
## Technical Environment & Tooling

### Primary Analysis Platforms

![Windows Analysis](https://img.shields.io/badge/Windows-Security%20Analysis-0078D4?style=for-the-badge)
![Network Analysis](https://img.shields.io/badge/Network-Traffic%20Analysis-4ECDC4?style=for-the-badge)
![Forensics](https://img.shields.io/badge/Digital-Forensics-8E44AD?style=for-the-badge)

- **Windows Security Analysis**  
  Sysinternals Suite, Windows Event Logs, and PowerShell for process visibility, execution auditing, and endpoint investigation.

- **Network Traffic Analysis**  
  Wireshark, tcpdump, and NetworkMiner for packet capture analysis, session reconstruction, and artifact extraction.

- **Log Management & Parsing**  
  Custom parsing scripts and regular expression development for log normalization and structured analysis.

- **Digital Forensics Tooling**  
  FTK Imager, Autopsy, and the Volatility Framework for disk and memory artifact acquisition and analysis.

---

### Analysis Environment Specifications

![Isolated Environment](https://img.shields.io/badge/Environment-Isolated%20Analysis-F39C12?style=for-the-badge)
![Evidence Handling](https://img.shields.io/badge/Process-Evidence%20Handling-2ECC71?style=for-the-badge)

- Isolated, virtualized network environments designed for safe malware and intrusion analysis.
- Dedicated forensic workstations with write-blocking capabilities to preserve evidence integrity.
- Standardized evidence collection, handling, and preservation procedures.
- Reproducible analysis environments to ensure consistent and verifiable results.

---

## Professional Development Alignment

### Industry Framework Integration

![MITRE ATT&CK](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red?style=for-the-badge)
![NIST](https://img.shields.io/badge/Framework-NIST%20CSF-blue?style=for-the-badge)
![ISO 27001](https://img.shields.io/badge/Standard-ISO%2027001-black?style=for-the-badge)

- **MITRE ATT&CK® Framework**  
  Technique mapping and procedure documentation to classify adversary behavior.

- **NIST Cybersecurity Framework**  
  Alignment with Identify, Protect, Detect, Respond, and Recover functional domains.

- **ISO/IEC 27001**  
  Application of information security incident management principles.

- **Cyber Kill Chain Model**  
  Attack phase identification to support detection, disruption, and response strategies.

---

### Certification Competency Mapping

![Incident Response](https://img.shields.io/badge/Domain-Incident%20Response-purple?style=for-the-badge)
![Forensics](https://img.shields.io/badge/Domain-Digital%20Forensics-8E44AD?style=for-the-badge)

The analytical capabilities demonstrated align with the knowledge domains covered by the following industry certifications:

- **GIAC Certified Incident Handler (GCIH)**  
  Incident detection, containment, and response procedures.

- **CompTIA Cybersecurity Analyst (CySA+)**  
  Behavioral analytics, threat detection, and data-driven analysis.

- **EC-Council Certified Incident Handler (ECIH)**  
  Structured incident response lifecycle and investigation techniques.

- **GIAC Certified Forensic Analyst (GCFA)**  
  Advanced digital forensics and artifact analysis methodologies.
  

# Contact Information.
### Name: Denis O. Onduso

### Scope: Security Operations & Cybersecurity Analyst

### Email:  Denis.Onduso@outlook.com


