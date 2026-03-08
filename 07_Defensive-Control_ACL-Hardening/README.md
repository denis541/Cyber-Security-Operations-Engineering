# Defensive Control: Incident Containment via Network ACLs

![Cisco](https://img.shields.io/badge/Platform-Cisco_IOS-1BA0D7?style=flat&logo=cisco&logoColor=white)
![Packet Tracer](https://img.shields.io/badge/Tool-Packet_Tracer-1BA0D7?style=flat&logo=cisco&logoColor=white)
![Domain](https://img.shields.io/badge/Domain-Defensive_Controls-4CAF50?style=flat&logoColor=white)

**Analyst:** Denis O. Onduso  
**Focus:** ACL-based network segmentation, lateral movement containment, policy-to-configuration translation

---

## Objective

Implement standard IPv4 ACLs on a Cisco IOS router to enforce a containment boundary around a compromised subnet — simulating the network-level response during the containment phase of an active incident.

---

## Environment

- Cisco Packet Tracer simulation
- Router `R1` running Cisco IOS CLI
- Network topology:
  - Internal subnet under containment: `192.168.10.0/24`
  - Remote resources to be blocked: DNS Server, Remote PC4
  - Authorized traffic beyond the contained segment: permitted

---

## Scenario

During an incident investigation, unauthorized outbound communication was identified originating from the `192.168.10.0/24` subnet. The containment objective was to block that subnet from reaching remote resources — DNS and external hosts — while preserving internal communication for local services still in use.

This mirrors a real SOC containment action: rather than taking a host fully offline (which can destroy volatile evidence), a targeted ACL restricts its reach while the investigation continues.

---

## Configuration

Standard numbered ACL 11 applied outbound on `R1`'s external-facing interface:

```bash
R1# show access-lists
Standard IP access list 11
    10 deny   192.168.10.0 0.0.0.255
    20 permit any
```

```bash
R1# show running-config | include interface|access-group
interface GigabitEthernet0/1
 ip access-group 11 out
```

**Rule logic:**
- Statement 10 blocks all traffic sourced from `192.168.10.0/24` from exiting the router outbound — preventing DNS resolution and remote host access
- Statement 20 permits all other traffic, ensuring no unintended disruption to other network segments
- Applied outbound (`out`) on the external interface so the filter acts at the egress point before traffic leaves the router

---

## Validation

Post-configuration testing confirmed the ACL was functioning as intended:

| Test | Source | Destination | Result |
|------|--------|-------------|--------|
| ICMP ping | 192.168.10.x | DNS Server | Blocked ✓ |
| ICMP ping | 192.168.10.x | Remote PC4 | Blocked ✓ |
| ICMP ping | 192.168.20.x | DNS Server | Permitted ✓ |
| ICMP ping | 192.168.10.x | Local gateway | Permitted ✓ |

The contained subnet lost external reachability while retaining local segment communication — confirming surgical containment without a full network blackout.

---

## Key Findings

The ACL achieved targeted containment of the `192.168.10.0/24` subnet without disrupting other segments. The outbound application point on the external interface was the correct placement — applying the ACL inbound on the internal interface would have also blocked inter-VLAN traffic, which was not the intent. Statement ordering mattered: the explicit deny before the `permit any` ensured the containment rule was evaluated first.

This configuration demonstrates the translation of a security policy requirement into a functional router-level control — the core skill required during the containment phase of an incident response engagement.

---

## Commands Reference

| Command | Purpose |
|---------|---------|
| `access-list 11 deny 192.168.10.0 0.0.0.255` | Create deny statement for target subnet |
| `access-list 11 permit any` | Permit all other traffic after the deny |
| `ip access-group 11 out` | Apply ACL outbound on the interface |
| `show access-lists` | Verify ACL statements and hit counters |
| `show running-config \| include access` | Confirm ACL is bound to the correct interface |

---

## Tools

- **Cisco Packet Tracer** — network simulation and topology provisioning
- **Cisco IOS CLI** — ACL configuration and validation

---

## MITRE ATT&CK Relevance

| Technique | ID | Relevance |
|-----------|----|-----------|
| Lateral Movement | T1210 | ACL containment directly limits attacker ability to move between segments |
| Exfiltration Over C2 Channel | T1041 | Blocking outbound access from compromised subnet prevents data exfiltration |
| DNS Resolution | T1071.004 | Blocking DNS from the contained subnet disrupts C2 domain resolution |
| Network Segmentation Bypass | T1599 | Correct ACL placement and direction prevents policy bypass via routing |
