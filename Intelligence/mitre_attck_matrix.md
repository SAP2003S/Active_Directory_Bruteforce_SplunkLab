# 🗺️ MITRE ATT&CK Framework Mapping

This document provides a technical mapping between simulated adversary techniques, the defensive sensors deployed across the Active Directory environment, and the resulting forensic telemetry.

---

## 📊 Overview Matrix

| Tactic | Technique ID | Technique Name | Tool / Execution | Sensor / Log Source | Captured Event Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Credential Access** | `T1110.001` | Password Guessing: Brute-Force | THC-Hydra (`passwords.txt`) | `WinEventLog:Security` | **Event ID 4625** (Audit Failure) |
| **Initial Access** | `T1078.002` | Valid Accounts: Domain Accounts | FreeRDP (`xfreerdp`) | `WinEventLog:Security` | **Event ID 4624** (Audit Success) |
| **Privilege Escalation**| `T1078.001` | Valid Accounts: Local Accounts | Administrative Promotion | `WinEventLog:Security` | **Event ID 4672** (Special Privileges) |
| **Impact / Persistence**| `T1531` | Account Removal | Atomic Red Team (`T1136`) | `WinEventLog:Security` | **Event ID 4726** (Account Deleted) |
| **Execution** | `T1059.001` | Command & Scripting: PowerShell | `Invoke-AtomicTest` | `Microsoft-Windows-Sysmon` | **Event ID 1** (Process Creation) |

---

## 🔍 Detailed Technique Breakdown & Telemetry Analysis

### 1. Credential Access: Password Guessing (`T1110.001`)
* **Adversary Action:** THC-Hydra automated dictionary brute-force against TCP Port 3389 (RDP) on `TARGET-PC` (`192.168.10.100`).
* **Sensor:** Windows Security Audit Log (`WinEventLog:Security`).
* **Event ID:** `4625` (An account failed to log on).
* **Forensic Metadata:**
  * **Target Account:** `spandey`
  * **Workstation Name:** `kali`
  * **Source Network Address:** `192.168.10.250`
  * **Logon Type:** `3` (Network Logon) / `10` (RemoteInteractive)
  * **Authentication Package:** `NTLM` / `MICROSOFT_AUTHENTICATION_PACKAGE_V1_0`
  * **Failure Reason:** `Unknown user name or bad password`

---

### 2. Initial Access: Valid Accounts - Domain Accounts (`T1078.002`)
* **Adversary Action:** FreeRDP (`xfreerdp`) interactive remote desktop logon using the cracked domain credential (`spandey : Password@123`).
* **Sensor:** Windows Security Audit Log (`WinEventLog:Security`).
* **Event ID:** `4624` (An account was successfully logged on).
* **Forensic Metadata:**
  * **Target Account:** `spandey`
  * **Target Domain:** `SOCLAB`
  * **Workstation Name:** `kali`
  * **Source Network Address:** `192.168.10.250`
  * **Logon Type:** `3` / `10` (RemoteInteractive / RDP)
  * **Process Name:** `-` (Windows Logon Process)

---

### 3. Privilege Escalation: Valid Accounts - Local Accounts (`T1078.001`)
* **Adversary Action:** Privilege assignment and elevated interactive authentication verified on `TARGET-PC`.
* **Sensor:** Windows Security Audit Log (`WinEventLog:Security`).
* **Event ID:** `4672` (Special privileges assigned to new logon).
* **Forensic Metadata:**
  * **Subject Account:** `TARGET-PC\Administrator`
  * **Privilege List:** `SeSecurityPrivilege`, `SeBackupPrivilege`, `SeRestorePrivilege`, `SeTakeOwnershipPrivilege`, `SeDebugPrivilege`

---

### 4. Impact / Persistence: Account Removal & Lifecycle (`T1531` / `T1136.001`)
* **Adversary Action:** Execution of Atomic Red Team post-exploitation cleanups and user management tests.
* **Sensor:** Windows Security Audit Log (`WinEventLog:Security`).
* **Event ID:** `4726` (A user account was deleted).
* **Forensic Metadata:**
  * **Target Account Name:** `NewLocalUser`
  * **Subject Account Name:** `Administrator`
  * **Subject Domain:** `TARGET-PC`
  * **Task Category:** `User Account Management`

---

### 5. Execution: Command and Scripting Interpreter - PowerShell (`T1059.001`)
* **Adversary Action:** Automated execution of PowerShell scripts with execution policy bypass flags (`-ExecutionPolicy Bypass -NoProfile`).
* **Sensor:** Sysmon Operational Log (`Microsoft-Windows-Sysmon/Operational`).
* **Event ID:** `1` (Process Creation).
* **Forensic Metadata:**
  * **Image:** `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
  * **Parent Image:** `C:\Windows\explorer.exe` or `C:\Windows\System32\cmd.exe`
  * **Command Line:** Includes execution bypass arguments and script block invocations.
