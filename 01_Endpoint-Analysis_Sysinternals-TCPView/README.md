# 🖥️ Endpoint Analysis: Identifying Running Processes & Network Connections

### 📖 Project Overview
This implementation focuses on host-based security monitoring. By utilizing the **Microsoft Sysinternals Suite**, I analyzed active processes and their associated network endpoints (TCP/UDP) to identify baseline system behavior and user-initiated activity.

## 🚀 Execution Steps

### 1. Environment Setup
- Deployed the **Windows Sysinternals Suite**, a collection of advanced system utilities for troubleshooting and security analysis.
- Initialized **TCPView (Tcpview.exe)** to gain real-time visibility into the network activity of all running processes.

### 2. System Process Exploration
- Analyzed core Windows processes to establish a security baseline.
- **Deep Dive: `lsass.exe` (Local Security Authority Subsystem Service)**
  - **Role:** Responsible for enforcing security policy on the system (user logins, password changes).
  - **Location:** `C:\Windows\System32`
  - *Security Insight: Verifying the file path of lsass.exe is a critical step in detecting malware that mimics system process names.*

### 3. User-Activity Monitoring & Dynamic Analysis
- **Test:** Opened a web browser (Microsoft Edge) while monitoring the TCPView interface.
- **Observation:** Immediately identified multiple new TCP connections originating from the browser process to various remote IP addresses on ports 80 (HTTP) and 443 (HTTPS).
- **Termination:** Observed the transition from `ESTABLISHED` to `TIME_WAIT` and eventual disappearance of the process threads upon closing the application.

## 🔍 Key Findings
| Process | Type | Observed Behavior |
| :--- | :--- | :--- |
| `lsass.exe` | System | Persistent, local security authority, no external traffic. |
| `msedge.exe` | User | Dynamic, spawned multiple remote connections via TCP. |
| `svchost.exe` | System | Hosting various Windows services; frequent local port activity. |

## 🛠️ Tools Used
- **[Windows Sysinternals](https://learn.microsoft.com):** Specifically **TCPView** for endpoint mapping.
- **Windows Command Line:** For directory navigation and tool execution.

---
*Documentation developed for **CyberOps Associate** portfolio.*
