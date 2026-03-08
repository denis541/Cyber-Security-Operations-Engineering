# Packet Analysis: ICMP Encapsulation & Layer 2/3 Header Inspection with Wireshark

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679C2?style=flat&logo=wireshark&logoColor=white)
![Mininet](https://img.shields.io/badge/Tool-Mininet_SDN-FCC624?style=flat&logo=linux&logoColor=black)
![Domain](https://img.shields.io/badge/Domain-Packet_Analysis-4CAF50?style=flat&logoColor=white)

**Analyst:** Denis O. Onduso  
**Focus:** Deep packet inspection, Layer 2/3 header validation, encapsulation behavior across network segments

---

## Objective

Capture and dissect ICMP traffic in a controlled Mininet SDN environment to validate Layer 2 and Layer 3 header behavior across local and routed segments — building the packet-level inspection methodology applied during network forensics investigations.

---

## Environment

- Mininet SDN topology: 4 hosts, 1 switch, 1 virtual router (`R1`)
- Local segment: `H1` (10.0.0.11), `H2` (10.0.0.12), `H3` (10.0.0.13)
- Remote segment: `H4` (172.16.0.40) reachable via `R1`
- Wireshark running on the capture interface during all ICMP exchanges

---

## Analysis

### Local Segment Traffic — Same Broadcast Domain

ICMP Echo Request captured between `H1` (10.0.0.11) and `H2` (10.0.0.12):

| Field | Value | Observation |
|-------|-------|-------------|
| Source IP | 10.0.0.11 | H1 |
| Destination IP | 10.0.0.12 | H2 — direct delivery, no routing needed |
| Source MAC | H1 MAC | Originating NIC |
| Destination MAC | H2 MAC | Target NIC — frame delivered directly |

Within the same broadcast domain, Layer 3 addressing and Layer 2 addressing align — the destination MAC is the target host itself. No gateway involvement. This is the expected baseline for intra-segment traffic.

### Routed Traffic — Cross-Segment Encapsulation

ICMP Echo Request captured from `H1` (10.0.0.11) to `H4` (172.16.0.40) crossing through `R1`:

| Field | Value | Observation |
|-------|-------|-------------|
| Source IP | 10.0.0.11 | H1 — unchanged end-to-end |
| Destination IP | 172.16.0.40 | H4 — unchanged end-to-end |
| Source MAC | H1 MAC | Originating NIC |
| Destination MAC | R1-eth1 MAC | Gateway — not H4 |

This is the critical finding: the destination IP remains `172.16.0.40` (the final host) throughout the transmission, but the destination MAC resolves to the gateway `R1-eth1`. The frame is physically addressed to the router, which strips the Layer 2 header, makes a routing decision, and re-encapsulates with a new Ethernet header for the next hop.

This encapsulation/de-encapsulation cycle is fundamental to how routed networks operate — and understanding it at the packet level is what makes MAC/IP spoofing, ARP poisoning, and MitM attacks readable in a capture.

### Wireshark Filter Methodology

| Filter | Purpose |
|--------|---------|
| `icmp` | Isolate ICMP echo requests and replies |
| `ip.dst == 172.16.0.40` | Filter to destination host across the routed segment |
| `eth.dst == <MAC>` | Confirm which MAC the frame is physically addressed to at each hop |
| `arp` | Observe ARP resolution preceding initial ICMP exchange |

Capturing ARP alongside ICMP confirmed that `H1` first resolved `R1-eth1`'s MAC via ARP before sending the cross-segment ICMP — the expected behavior when the destination is outside the local subnet.

---

## Key Findings

The packet captures confirmed textbook encapsulation behavior across both segments. Local traffic showed direct MAC-to-MAC delivery with no gateway involvement. Routed traffic showed the destination IP remaining constant while the destination MAC changed at each hop to reflect the next Layer 2 boundary. ARP resolution preceded cross-segment ICMP as expected. No anomalies in header construction were observed — this baseline confirms what clean routed traffic looks like at the packet level.

---

## Commands Reference

| Command | Purpose |
|---------|---------|
| `sudo cyberops_topo.py` | Launch the Mininet virtual topology |
| `ip address` | Display NIC-specific MAC and IP assignments per host |
| `ping -c 4 <target>` | Generate controlled ICMP traffic for capture |
| `arp -n` | Display ARP table to verify MAC resolution per host |

---

## Tools

- **Wireshark** — packet capture, protocol dissection, stream filtering
- **Mininet** — SDN topology simulation for controlled traffic generation
- **Linux CLI** — host command execution within the virtual topology

---

## MITRE ATT&CK Relevance

| Technique | ID | Relevance |
|-----------|----|-----------|
| Network Sniffing | T1040 | Wireshark methodology directly mirrors attacker packet capture tradecraft |
| Adversary-in-the-Middle | T1557 | Understanding gateway MAC substitution makes ARP poisoning detectable in captures |
| ARP Cache Poisoning | T1557.002 | Baseline ARP behavior enables detection of spoofed MAC-to-IP mappings |
| ICMP Tunneling | T1095 | Protocol-level ICMP inspection is the foundation for detecting covert ICMP channels |

![Wireshark Packet Dissection](./Screenshots/02-icmp-packet-dissection.png)
*Wireshark three-pane view showing Layer 2 Ethernet header and Layer 3 IP header for a cross-segment ICMP exchange — destination MAC resolves to gateway R1-eth1, not the final host*
