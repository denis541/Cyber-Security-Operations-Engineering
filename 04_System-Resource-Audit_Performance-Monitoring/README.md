# System Resource Auditing & Event Log Analysis

![Windows](https://img.shields.io/badge/Platform-Windows-0078D4?style=flat&logo=windows&logoColor=white)
![Performance Monitor](https://img.shields.io/badge/Tool-Performance_Monitor-5391FE?style=flat&logoColor=white)
![Event Viewer](https://img.shields.io/badge/Tool-Event_Viewer-0078D4?style=flat&logo=windows&logoColor=white)
![Domain](https://img.shields.io/badge/Domain-Host_Monitoring-4CAF50?style=flat&logoColor=white)

**Analyst:** Denis O. Onduso  
**Focus:** Service lifecycle monitoring, Event ID correlation, performance baseline establishment

---

## Objective

Audit system resource consumption and service state changes on a live Windows endpoint using native monitoring tools — establishing the baseline visibility and log correlation methodology used during SOC investigations to detect unauthorized modifications and resource-exhaustion attacks.

---

## Environment

- Windows host
- Tools: Event Viewer, Performance Monitor (perfmon), Services.msc, Data Collector Sets

---

## Analysis

### Service Lifecycle Monitoring — Routing and Remote Access

The Routing and Remote Access (RRAS) service was started and stopped manually while monitoring the effect on the system's network interfaces:

- On service start: a virtual network adapter was dynamically provisioned and became visible in the network interface list
- On service stop: the adapter was removed and the interface disappeared from the stack

This behavior matters in an incident context — unauthorized remote access tools (VPNs, tunneling software, RAT components) frequently register as services and provision virtual adapters. Knowing what legitimate adapter provisioning looks like is the baseline against which a suspicious interface registration stands out.

### Event Log Correlation — Service State Changes

Event Viewer was used to audit the `System` log for entries corresponding to the manual service toggles above. The following Event IDs were correlated:

| Event ID | Source | Description |
|----------|--------|-------------|
| 7036 | Service Control Manager | Service entered running or stopped state |
| 7040 | Service Control Manager | Service start type changed |
| 7045 | Service Control Manager | New service installed on the system |

Each manual start and stop produced a corresponding `7036` entry with a precise timestamp, confirming that Windows event logs provide a reliable paper trail for service state changes. In a forensic context, gaps or out-of-sequence entries in this trail are as significant as the entries themselves.

### Performance Baseline — Data Collector Set

A custom Data Collector Set was configured in Performance Monitor to capture `Available MBytes` of RAM at 4-second intervals over a sustained collection window. This produced a baseline memory profile for the endpoint under normal operating conditions.

Baseline findings:
- Memory availability remained stable within an expected range during idle and light-use conditions
- No unexpected spikes or sustained drops were observed during the collection window

In a SOC environment, deviations from this baseline — sustained memory exhaustion with no corresponding user activity, or rapid consumption spikes from a single process — are detection signals for memory-scraping malware, cryptojacking, or buffer overflow exploitation. The value of the baseline is that it makes the anomaly measurable rather than subjective.

---

## Key Findings

Service state changes on Windows produce consistent, attributable Event Log entries that can be used to reconstruct administrative or attacker activity with timestamp precision. The RRAS service test confirmed that virtual adapter provisioning is directly tied to service lifecycle events — a useful indicator for detecting unauthorized remote access tooling. The performance baseline established a reference for memory behavior that can be used to calibrate future anomaly detection thresholds.

---

## Tools Reference

| Tool | Investigative Use |
|------|-------------------|
| Event Viewer | Audit service state changes, logon events, and system modifications via Event ID correlation |
| Performance Monitor | Capture resource metrics over time; establish baselines for anomaly detection |
| Data Collector Sets | Schedule and store custom metric collections for trend analysis |
| Services.msc | Enumerate running services; identify unauthorized or unexpected service registrations |

---

## MITRE ATT&CK Relevance

| Technique | ID | Relevance |
|-----------|----|-----------|
| System Service Discovery | T1007 | Enumerating services to identify unauthorized or unexpected entries |
| Create or Modify System Process: Windows Service | T1543.003 | Event ID 7045 detects new service installation — common persistence method |
| Resource Hijacking | T1496 | Performance baseline enables detection of cryptojacking via anomalous CPU/memory consumption |
| Indicator Removal: Clear Windows Event Logs | T1070.001 | Understanding expected log entries makes gaps or deletions detectable |
