# Incident Response: Remcos RAT & Dridex Multi-Stage Infection

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679C2?style=flat&logo=wireshark&logoColor=white)
![Elastic](https://img.shields.io/badge/Tool-Elastic_Stack-005571?style=flat&logo=elastic&logoColor=white)
![Domain](https://img.shields.io/badge/Domain-Incident_Response-E01B1B?style=flat&logoColor=white)

**Analyst:** Denis O. Onduso  
**Date:** 2019-03-19  
**Status:** Resolved — full infection chain documented, IOCs verified against threat intelligence, remediation recommendations issued

---

## Scenario

On 2019-03-19, NSM systems triggered multiple high-severity alerts on a Windows endpoint. Investigation confirmed a multi-stage infection: a dynamic DNS update preceded the download of two malicious executables, followed by C2 check-ins from both Remcos RAT and the Dridex banking trojan. This report documents the full investigation methodology from alert triage to containment recommendation.

---

## Environment

- **Detection platform:** Security Onion — Suricata, Sguil
- **Traffic analysis:** Wireshark, Zeek
- **SIEM:** Kibana / Elastic Stack
- **Threat intelligence:** Cisco Talos, ABUSE.CH SSL Blacklist

---

## Intrusion Timeline

| Time (UTC) | Event | Detail |
|------------|-------|--------|
| 19:15:30 | DNS Activity | Dynamic DNS update initiated by the internal Windows host — likely resolving a freshly rotated C2 domain |
| 19:16:05 | Payload Download | HTTP GET request for `test1.exe` from external IP — MZ header confirmed executable |
| 19:17:42 | Remcos C2 Check-in | Encrypted traffic on non-standard port — Suricata ET signature `Remcos RAT Checkin` fired |
| 19:22:11 | Dridex Detection | SSL certificate associated with `31.22.4.176` matched ABUSE.CH SSL blacklist — Dridex banking trojan confirmed |

---

## Investigation

### Phase 1: Alert Triage — Suricata / Sguil

Two distinct Suricata signatures fired within the same session window:

| Alert | Severity | Significance |
|-------|----------|-------------|
| `ET TROJAN Remcos RAT Checkin` | High | Confirms active C2 communication from the endpoint to attacker infrastructure |
| `ET TROJAN ABUSE.CH SSL Blacklist` | High | SSL certificate fingerprint matched a known-malicious certificate associated with Dridex |

Both alerts were pivoted from signature to raw packet data in Wireshark to validate detection and confirm they were not false positives before proceeding with the investigation.

---

### Phase 2: Traffic Forensics — Wireshark / Zeek

**Payload extraction:**

The HTTP GET response for `test1.exe` was exported from the PCAP via Wireshark's HTTP object export. The extracted file was validated:

```
File header: MZ (hex 4D 5A) — Windows PE executable confirmed
SHA256: 2a9b0ed40f1f0bc0c13ff35d304689e9cadd633781cbcad1c2d2b92ced3f1c85
```

**Threat intelligence correlation:**

The SHA256 hash was submitted to Cisco Talos Intelligence, which confirmed the sample as a malicious downloader associated with Trojan activity — consistent with the Remcos RAT family's typical staged delivery mechanism.

**C2 session analysis:**

Zeek metadata correlated source/destination IPs across the session window and measured C2 session duration. The Remcos check-in used an encrypted channel on a non-standard port — a deliberate evasion technique to avoid inspection by port-based firewall rules that only monitor 80/443.

**Dridex SSL certificate:**

Traffic to `31.22.4.176` contained an SSL certificate whose fingerprint was matched against the ABUSE.CH SSL Blacklist. Dridex commonly uses SSL to encrypt C2 communications while reusing certificate infrastructure that has been catalogued in threat intelligence feeds — making certificate-based detection effective even against encrypted channels.

---

### Phase 3: SIEM Correlation — Kibana / Elastic Stack

NIDS (Suricata) and HIDS logs were combined in Kibana to confirm endpoint execution of the downloaded payload:

- Filtering by `31.22.4.176` isolated all communications tied to the Dridex certificate detection
- Log correlation confirmed the downloaded `test1.exe` was executed on the endpoint — not merely downloaded
- Timeline alignment between DNS activity (19:15:30) and first C2 check-in (19:17:42) established a 2-minute 12-second window from initial resolution to active compromise

---

## IOC Summary

| Type | Indicator | Context |
|------|-----------|---------|
| IP | `31.22.4.176` | Dridex C2 server — SSL certificate on ABUSE.CH blacklist |
| File | `test1.exe` | Initial payload — Remcos RAT downloader |
| Hash (SHA256) | `2a9b0ed40f1f0bc0c13ff35d304689e9cadd633781cbcad1c2d2b92ced3f1c85` | Confirmed malicious by Cisco Talos |
| SSL certificate | ABUSE.CH blacklisted fingerprint | Associated with Dridex banking trojan C2 |
| Protocol | Non-standard port (encrypted) | Remcos RAT C2 channel — evasion of port-based inspection |

---

## Analyst Recommendations

**Host isolation:** Disconnect the affected Windows endpoint from the network immediately for re-imaging. Remcos RAT provides the attacker with persistent remote access — the host should be treated as fully compromised until rebuilt.

**Firewall blocking:** Block IP `31.22.4.176` and the associated malicious SSL certificate hashes at the perimeter firewall and any inline SSL inspection appliances.

**Credential reset:** Force a password reset for the affected user account. Remcos RAT is capable of keylogging and credential harvesting — any credentials entered on the host after 19:17:42 should be considered compromised.

**Threat hunting:** Search for the SHA256 hash (`2a9b0ed4...`) and the C2 IP across all other endpoints in the environment to determine whether the infection is isolated or part of a broader campaign.

---

## MITRE ATT&CK Mapping

| Technique | ID | Phase | Detail |
|-----------|----|-------|--------|
| Dynamic Resolution | T1568 | C2 | Dynamic DNS update at 19:15:30 — likely resolving a freshly rotated C2 domain |
| Ingress Tool Transfer | T1105 | C2 | HTTP GET download of `test1.exe` — PE32 executable confirmed |
| Remote Access Software | T1219 | C2 | Remcos RAT providing persistent remote access to attacker |
| Encrypted Channel | T1573 | Defense Evasion | Non-standard port with encrypted Remcos C2 traffic |
| Financial Theft | T1657 | Impact | Dridex banking trojan targeting financial credentials |
| Input Capture: Keylogging | T1056.001 | Credential Access | Remcos RAT capability — all post-compromise keystrokes at risk |
