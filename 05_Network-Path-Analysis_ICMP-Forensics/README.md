# Network Path Analysis: ICMP Diagnostics & Route Tracing

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679C2?style=flat&logo=wireshark&logoColor=white)
![Linux](https://img.shields.io/badge/Platform-Linux_CLI-FCC624?style=flat&logo=linux&logoColor=black)
![Domain](https://img.shields.io/badge/Domain-Network_Forensics-4CAF50?style=flat&logoColor=white)

**Analyst:** Denis O. Onduso  
**Focus:** ICMP-based reachability testing, hop-by-hop route mapping, ISP handoff identification, latency baseline establishment

---

## Objective

Use ICMP diagnostic tools to map the full Layer 3 path between a local host and international destinations, establish latency baselines, and identify ISP handoff points — building the network path visibility used during traffic anomaly investigations and MitM detection.

---

## Environment

- Linux CLI
- Tools: `ping`, `traceroute`, web-based visual traceroute for geographic hop mapping
- Targets: Regional Internet Registry sites (RIPE, AFRINIC) and Akamai edge nodes

---

## Analysis

### Connectivity Validation — ICMP Echo Requests

`ping -c 4` was run against multiple remote targets to establish baseline reachability and RTT:

- 0% packet loss confirmed on all tested targets under normal conditions
- RTT values provided a latency baseline against which future deviations can be measured
- Consistent RTT across multiple runs ruled out transient congestion on the local segment

In an investigative context, anomalous RTT increases to a known destination — particularly if sudden and sustained — can indicate path changes, traffic redirection, or an interception point introducing processing delay. The baseline established here makes that kind of deviation detectable.

### Route Tracing — Hop-by-Hop Path Reconstruction

`traceroute` was executed against RIPE and AFRINIC to map the full path from the local host to international destinations:

- First 2–3 hops: local ISP infrastructure (RFC 1918 or ISP-assigned addresses)
- Mid-path: transit provider handoffs — points where local ISP traffic exits to international backbone carriers
- Final hops: destination-side infrastructure at the target RIR

Several intermediate hops returned `* * *` (no ICMP TTL-exceeded response), which is common at carrier boundaries where routers are configured to drop ICMP. This is expected behavior and does not indicate a problem — but in a forensic context, a sudden increase in non-responding hops on a path that previously resolved fully warrants investigation.

### Geographic Mapping

Visual traceroute tools were used to map the physical location of responding hops. Key observations:

- Packets from the local host to European RIR destinations transited through multiple countries before reaching the target
- The path was not geographically linear — routing decisions reflected BGP policy and peering agreements, not physical proximity
- No unexpected geographic detours were identified that would suggest traffic manipulation

Understanding normal routing geography matters in forensic investigations involving data exfiltration or traffic interception — an unexpected country-level detour in an otherwise stable path is an anomaly worth flagging.

---

## Key Findings

Baseline RTT and path were established to multiple international destinations. All paths showed expected ISP-to-backbone handoff behavior. Non-responding hops at carrier boundaries were consistent with standard ICMP filtering policy. No path anomalies, unexpected geographic detours, or packet loss events were observed. This baseline documents normal routing behavior for this endpoint and network segment.

---

## Commands Reference

| Command | Investigative Use |
|---------|-------------------|
| `ping -c 4 <target>` | Reachability testing and RTT baseline establishment |
| `traceroute <target>` | Hop-by-hop path mapping; identifying bottlenecks or path changes |
| `traceroute -I <target>` | ICMP-mode traceroute (useful when UDP probes are filtered) |
| `traceroute -n <target>` | Suppress DNS resolution for faster output and cleaner IP logging |
| `> output.txt` | Redirect CLI output to file for documentation and reporting |

---

## Tools

- **ping** — ICMP echo request/reply; latency and packet loss measurement
- **traceroute** — TTL-based hop enumeration for path reconstruction
- **Visual traceroute** — geographic mapping of responding hops

---

## MITRE ATT&CK Relevance

| Technique | ID | Relevance |
|-----------|----|-----------|
| Network Service Discovery | T1046 | Understanding path topology supports detection of scanning activity |
| Remote System Discovery | T1018 | Hop enumeration reveals intermediate infrastructure |
| Adversary-in-the-Middle | T1557 | RTT baseline enables detection of interception-induced latency anomalies |
| Traffic Signaling | T1205 | Unusual ICMP behavior (unexpected responses, modified TTL) can indicate covert channel use |

![Traceroute CLI Output](./Screenshots/cli-traceroute-hops.png)
*Hop-by-hop path output showing latency per hop and ISP handoff points*
