# 🌐 Telemetry Pipeline & Network Flow

> **Environment Baseline:** Fully isolated host-only virtual network segment (`192.168.10.0/24`) designed for controlled adversary simulation and high-fidelity SOC log ingestion.

---

## 🗺️ Visual Architecture Diagram

```text
+---------------------------------------------------------------------------------------------------+
|                                Host-Only Virtual Subnet: 192.168.10.0/24                          |
|                                                                                                   |
|  +---------------------------+   +---------------------------+   +-----------------------------+  |
|  |     ADDC01 (DC Host)      |   |   TARGET-PC (Endpoint)    |   |     SPLUNK-SIEM (Indexer)   |  |
|  |       192.168.10.7        |   |      192.168.10.100       |   |        192.168.10.10        |  |
|  |  Windows Server 2022 AD   |   |   Windows 10 Enterprise   |   |    Splunk Enterprise v9.x   |  |
|  |   Domain: SOCLAB.LOCAL    |   |    Domain-Joined (SOCLAB) |   |  Web UI: http://...:8000    |  |
|  |  Splunk UF (TCP :9997)    |   |  Splunk UF + Sysmon + ART |   |  Receiver Port: TCP :9997   |  |
|  +-------------+-------------+   +-------------+-------------+   +--------------+--------------+  |
|                |                               |                                ^                 |
|                +---------------+---------------+--------------------------------+                 |
|                                |                                                                  |
|                  +-------------+-------------+                                                    |
|                  |     KALI (Adversary)      |                                                    |
|                  |      192.168.10.250       |                                                    |
|                  | Hydra, FreeRDP, Wordlists |                                                    |
|                  +---------------------------+                                                    |
+---------------------------------------------------------------------------------------------------+
