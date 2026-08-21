
# 🛡️ Remediation & Defensive Hardening Playbook

## 🎯 Incident Reference: `INC-2026-0821-AD01`

---

## 📋 Action Checklist

- [ ] **Phase 1: Immediate Containment**
  - [ ] Terminate active RDP sessions for user `spandey`.
  - [ ] Temporarily disable or reset credentials for `SOCLAB\spandey`.
  - [ ] Block incoming connections from `192.168.10.250` at the host firewall level.

- [ ] **Phase 2: Active Directory GPO Hardening**
  - [ ] Open `gpmc.msc` on `ADDC01`.
  - [ ] Navigate to **Default Domain Policy** $\rightarrow$ **Computer Configuration** $\rightarrow$ **Policies** $\rightarrow$ **Windows Settings** $\rightarrow$ **Security Settings** $\rightarrow$ **Account Policies** $\rightarrow$ **Account Lockout Policy**.
  - [ ] Configure:
    * **Account lockout threshold:** `5 invalid logon attempts`
    * **Account lockout duration:** `15 minutes`
    * **Reset account lockout counter after:** `15 minutes`

- [ ] **Phase 3: Network Access Control (RDP Segmentation)**
  - [ ] Enforce Network Level Authentication (NLA) across all domain endpoints.
  - [ ] Block inbound TCP port `3389` across general user subnets.
  - [ ] Restrict RDP access strictly to dedicated Administrative Jump Hosts / Bastions protected by MFA.

- [ ] **Phase 4: SIEM Continuous Monitoring**
  - [ ] Enable scheduled alert searches for `EventCode=4625` threshold spikes (>5 attempts in 5 minutes).
  - [ ] Configure automated notifications for any `EventCode=4726` (Account Deletion) events on workstations.
