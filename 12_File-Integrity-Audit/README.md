# File Integrity Verification Utility

![PowerShell](https://img.shields.io/badge/PowerShell-5.1-blue?logo=powershell&style=for-the-badge)
![Security Restricted](https://img.shields.io/badge/Security-Restricted-red?style=for-the-badge)
![Algorithm SHA-256](https://img.shields.io/badge/Algorithm-SHA--256-green?style=for-the-badge)
![OS Windows](https://img.shields.io/badge/OS-Windows-blue?style=for-the-badge&logo=windows)
![Audit Manual Trigger](https://img.shields.io/badge/Audit-Manual_Trigger-orange?style=for-the-badge)

## Project Overview.
This utility provides a standardized method for cryptographic validation of file integrity using PowerShell's `Get-FileHash` cmdlet. It is designed to mitigate the risk of data corruption or unauthorized file substitution by comparing binary-level SHA-256 signatures.

# Security Context: Execution Policies.
Windows systems implement an Execution Policy as a primary defense mechanism against the automated execution of malicious code.  

**Default State:** Most systems are set to Restricted, which prevents all scripts from running, regardless of their source.  
**The Constraint:** This policy often blocks legitimate administrative tools and locally authored scripts, resulting in a PSSecurityException.  
**Operational Workaround:** Temporary Bypass.  

To execute this utility without compromising the long-term security posture of the host system, we employ a Process-Level Bypass.  
Instead of permanently altering the system registry via Set-ExecutionPolicy, we utilize a scoped execution flag. This allows the script to run within a single, isolated session. Once the PowerShell process terminates, the system's original security restrictions remain fully intact.

## Command for Execution
To run the validator under a temporary bypass, execute the following from the terminal:

```powershell
PowerShell.exe -ExecutionPolicy Bypass -File ".\Hashcompaire.ps1"
```
## Functional Logic
The utility follows a three-stage analytical workflow:

Input Sanitization: Captures and cleans string literal paths provided by the user.

Cryptographic Generation: Calculates the SHA-256 hash (Industry Standard) for the specified source and target binaries.

Binary Comparison: Performs a boolean match of the hash strings to confirm if the files are bit-for-bit identical.
## Technical Requirements
Environment: PowerShell 5.1 or PowerShell Core (7.x+).
Permissions: Read-access to the target files is mandatory.
Algorithm: ``SHA-256 (Default).`` but can be changed to MD5 or any other.
