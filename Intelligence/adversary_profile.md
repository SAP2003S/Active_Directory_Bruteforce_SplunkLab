# 🎯 Adversary Profile & Threat Scope

## 👤 Simulated Threat Actor
* **Designation:** `APT-SIM-01`
* **Adversary Node Hostname:** `kali`
* **Static IP Address:** `192.168.10.250/24`
* **Target Domain:** `SOCLAB.LOCAL` (`192.168.10.0/24`)

---

## 🎯 Threat Campaign Objectives
* **Reconnaissance & Pre-Attack Staging:** Perform subnet routing validation and tailor credential dictionaries from large corpus datasets (`rockyou.txt`).
* **Initial Access Execution:** Target exposed management interfaces (RDP / TCP Port 3389) on domain-joined workstation `TARGET-PC` (`192.168.10.100`) via automated dictionary brute-forcing.
* **Interactive Foothold:** Establish remote desktop sessions using valid domain credentials (`spandey`).
* **Post-Compromise Adversary Emulation:** Execute PowerShell commands under policy bypass and simulate user account lifecycle modification using Atomic Red Team.

---

## 🛠️ Adversary Tooling & Capabilities

| Capability | Tool Name | Scope & Execution |
| :--- | :--- | :--- |
| **Dictionary Guessing** | THC-Hydra v9.7 | High-velocity automated authentication attempts against RDP. |
| **Interactive Foothold** | FreeRDP (`xfreerdp`) | Remote interactive desktop session establishing graphical access. |
| **Wordlist Engineering** | Bash (`head`, `gunzip`) | Generating targeted password arrays (`passwords.txt`) based on known patterns. |
| **Adversary Emulation** | Atomic Red Team | PowerShell-driven TTP simulation targeting local accounts and command interpreters. |
