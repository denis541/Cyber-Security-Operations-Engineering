# 🌐 Network Path Analysis: ICMP Diagnostics & Route Tracing

![Traceroute](https://img.shields.io/badge/Traceroute-Path%20Analysis-blue?logo=mapbox)
![ICMP Echo](https://img.shields.io/badge/ICMP-Echo%20Requests-green?logo=signal)
![Ping](https://img.shields.io/badge/Ping-Latency%20Testing-yellow?logo=wifi)![Network Diagnostics](https://img.shields.io/badge/Network-Diagnostics-critical?logo=activitypub)
![Packet Flow](https://img.shields.io/badge/Packet%20Flow-Analyzing-red?logo=arxiv)
![Connectivity Test](https://img.shields.io/badge/Connectivity-Test-blueviolet?logo=connectdevelop)

## 📖 Executive Summary
This project demonstrates the use of **ICMP-based diagnostic tools** to verify network reachability and analyze the geographical path of data packets. By utilizing `ping` and `traceroute`, I mapped the logical and physical "hops" between a local source and international destinations, simulating a real-world **Network Troubleshooting** scenario.

## 🚀 Technical Implementations

### 1. Connectivity Validation (ICMP Echo Request)
- **Task:** Verified end-to-end connectivity to remote mirrors (e.g., Akamai Edge).
- **Analyst Insight:** Confirmed 0% packet loss and analyzed **Round-Trip Time (RTT)**. Monitoring latency (measured in ms) is a primary method for detecting network congestion or potential **Man-in-the-Middle (MitM)** delays.

### 2. Multi-Hop Route Analysis
- **Task:** Executed `traceroute` via the Linux CLI to identify every Layer 3 device (router) in the transmission path.
- **Investigation:** Analyzed the path to Regional Internet Registry (RIR) websites (e.g., RIPE, AFRINIC).
- **Forensic Observation:** Identified the transition points between local ISPs and international backbones. This visibility is critical for **Network Forensic** investigations to determine where a packet may have been dropped or intercepted.

### 3. Geographical Mapping & Visual Tracing
- **Task:** Utilized web-based visual traceroute tools to map the physical location of responding hops.
- **Finding:** Observed the "meshed" nature of the internet, where packets often traverse multiple countries before reaching their final destination.

## 💻 Technical Reference
| Command | Professional Use Case |
| :--- | :--- |
| `ping -c 4 [Target]` | Rapid reachability testing and latency baseline. |
| `traceroute [Target]` | Identifying the specific router causing a bottleneck. |
| `> [filename].txt` | Exporting CLI output for incident reports and documentation. |

## 📊 Visual Evidence
![Traceroute CLI Output](./Screenshots/cli-traceroute-hops.png)
*Figure 1: Hop-by-hop analysis showing path latency and ISP handoffs.*

---
*Documentation developed for **CyberOps Associate** portfolio.*
