====================================================================================================
INCIDENT ROOT CAUSE ANALYSIS (RCA) REPORT
====================================================================================================
INCIDENT ID       : INC-2026-0821-AD01
INCIDENT SEVERITY : CRITICAL (P1)
TARGET SYSTEM     : TARGET-PC (192.168.10.100) / Domain: SOCLAB.LOCAL
ADVERSARY IP      : 192.168.10.250 (Workstation: kali)
TARGET USER       : SOCLAB\spandey

1. INCIDENT DESCRIPTION:
   An adversary conducted an automated dictionary attack against TCP port 3389 (RDP) on TARGET-PC, 
   generating 20 consecutive Event ID 4625 logon failure audits before successfully discovering the 
   valid credentials (spandey:Password@123) and establishing an interactive FreeRDP session.

2. ROOT CAUSE SUMMARY:
   - Absence of an Account Lockout GPO on SOCLAB.LOCAL allowed unbounded password attempts.
   - User account was assigned a weak dictionary password present in standard wordlists.
   - TCP Port 3389 was exposed directly without network ACLs, jump hosts, or Multi-Factor Authentication.

3. REMEDIATION & PREVENTATIVE ACTIONS:
   - Deploy Fine-Grained Password Policy (FGPP): Lock accounts for 15 min after 5 failed attempts.
   - Enforce MFA on all RDP and interactive management access points.
   - Deploy automated Splunk alerting using the correlation queries provided above.
====================================================================================================
