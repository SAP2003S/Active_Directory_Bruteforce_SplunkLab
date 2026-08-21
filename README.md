# ⚡ Active-Directory-Incident-Detection-Engineering-Lab

### *Adversary Credential Access • Windows Security Auditing • Splunk SIEM Telemetry Pipeline*

<p align="left">
  <a href="https://attack.mitre.org/matrices/enterprise/"><img src="https://img.shields.io/badge/FRAMEWORK-MITRE%20ATT%26CK%20v14-E35205?style=for-the-badge" alt="MITRE ATT&CK"/></a>
  <a href="https://www.splunk.com/"><img src="https://img.shields.io/badge/SIEM-SPLUNK%20ENTERPRISE-000000?style=for-the-badge&logo=splunk&logoColor=white" alt="Splunk"/></a>
  <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon"><img src="https://img.shields.io/badge/SENSOR-WINDOWS%20SECURITY%20%2B%20SYSMON-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="Sensors"/></a>
  <a href="https://github.com/redcanaryco/invoke-atomicredteam"><img src="https://img.shields.io/badge/SIMULATION-INVOKE--ATOMIC%20RED%20TEAM-FF0044?style=for-the-badge" alt="Simulation"/></a>
</p>

An end-to-end blue team investigation lab capturing an Active Directory RDP dictionary attack and adversary emulation. This repository contains raw Windows Security telemetry, Sysmon process analytics, custom Splunk correlation searches (SPL), and a forensic incident response writeup.

[Threat Scope](intelligence/adversary_profile.md) • [ATT&CK Matrix](intelligence/mitre_attck_matrix.md) • [Telemetry Flow](telemetry-pipeline/topology_and_flow.md) • [Sensor Configs](telemetry-pipeline/sensor_configurations.md) • [RCA Report](incident-response/root_cause_analysis.md) • [Remediation Playbook](incident-response/remediation_playbook.md) • [Detection Rules](detections/)

---

## 📑 Interactive Table of Contents

* [🎯 Executive Summary](#-executive-summary)
* [🌐 Lab Topology & Telemetry Ingestion](#-lab-topology--telemetry-ingestion)
* [🗺️ Interactive MITRE ATT&CK Matrix](#️-interactive-mitre-attck-matrix)
* [⚔️ Attack Lifecycle & Visual Evidence](#️-attack-lifecycle--visual-evidence)
* [🔍 Production Splunk Detection Repository](#-production-splunk-detection-repository)
* [📄 Incident Root Cause Analysis (RCA)](#-incident-root-cause-analysis-rca)
* [📁 Repository File Structure](#-repository-file-structure)

---

## 🎯 Executive Summary

The objective of this lab is to execute an adversary simulation against an Active Directory domain, establish bidirectional security telemetry pipelines, and develop high-fidelity detection analytics within Splunk Enterprise.

```text
[ Adversary Host: Kali ] 
       │ (Hydra RDP Dictionary Attack / TCP 3389)
       ▼
[ Domain Workstation: TARGET-PC ] ───► [ Domain Controller: ADDC01 ]
  • Sysmon (Event ID 1)                  • Security Log (Event 4625 / 4624)
  • Atomic Red Team (T1136/T1059)        • User Management (Event 4726)
       │                                        │
       └──────────────────┬─────────────────────┘
                          │ (Splunk Universal Forwarder / TCP :9997)
                          ▼
           [ SIEM Server: SPLUNK-SIEM ]
             • Index: endpoint
             • Automated Correlation Searches
             • TTP Gap Analysis
