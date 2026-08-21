# ⚙️ Sensor Configurations & Pipeline Setup

## 1. Splunk Universal Forwarder Configuration (`inputs.conf`)
Located on `ADDC01` and `TARGET-PC`:

```ini
[default]
host = TARGET-PC

[WinEventLog://Security]
disabled = 0
index = endpoint
renderXml = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = endpoint
renderXml = false
