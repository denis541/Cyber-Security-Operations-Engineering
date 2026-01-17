# 📉 System Resource Auditing & Event Log Analysis

![Security](https://img.shields.io/badge/security-passed-brightgreen)
![Tool](https://img.shields.io/badge/tool-active-blue)

## 📖 Executive Summary
This project demonstrates **Host-Based Monitoring** and **Service Management** on a Windows endpoint. The objective was to audit system resource consumption and analyze **Event Logs** to verify service state changes—a critical skill for identifying unauthorized system modifications or resource-exhaustion attacks.

## 🚀 Technical Implementations

### 1. Service Lifecycle & Network Impact Analysis
- **Task:** Monitored the **Routing and Remote Access** service to observe its impact on network interface availability.
- **Analyst Insight:** By toggling service states, I identified how specific background processes dynamically provision virtual network adapters. This is vital for detecting **Unauthorized Remote Access** tools.

### 2. Forensic Event Log Correlation
- **Task:** Utilized **Event Viewer** to audit the `System` logs.
- **Investigation:** Correlated manual service starts/stops with specific **Event IDs**. 
- **Finding:** Verified the sequential log entries that provide a "Paper Trail" for administrative actions, ensuring accountability during a forensic audit.

### 3. Custom Performance Baseline (Data Collector Sets)
- **Task:** Engineered a manual **Data Collector Set** to monitor `Available MBytes` of memory at 4-second intervals.
- **Analyst Insight:** Established a **Performance Baseline**. Deviations from this baseline are utilized in a SOC environment to trigger alerts for **Buffer Overflow** attacks or **Cryptojacking** malware.

## 💻 Technical Reference
| Tool | Forensic Use Case |
| :--- | :--- |
| **Performance Monitor** | Identifying resource anomalies and "leaky" processes. |
| **Services.msc** | Disabling unnecessary services to reduce the **Attack Surface**. |
| **Event Viewer** | Investigating the "Who, What, and When" of system changes. |
