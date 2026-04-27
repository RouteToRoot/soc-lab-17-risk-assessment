# SOC Lab 17 — GRC: Risk Assessment

## Executive Summary

This lab performs a formal risk assessment on the SSH brute force threat identified during Lab 13. The Kali Linux VM is defined as the asset under review. The threat, vulnerability, likelihood, impact, and risk rating are formally documented using a standardized risk assessment methodology. Mitigation controls are proposed and mapped to NIST SP 800-53. This lab demonstrates the ability to translate technical SOC findings into structured risk documentation used in enterprise GRC programs.

---

## Incident Ticket (ServiceNow Simulation)

| Field | Details |
|---|---|
| **Incident ID** | INC-0017 |
| **Date/Time Detected** | 2026-04-26 |
| **Detected By** | Eric Ellison — SOC Analyst |
| **Severity** | Medium |
| **Category** | Governance, Risk & Compliance |
| **Subcategory** | Risk Assessment |
| **Short Description** | Formal risk assessment — SSH brute force threat on Kali Linux VM |
| **Detailed Description** | A formal risk assessment was conducted on the Kali Linux VM following the SSH brute force simulation in Lab 13. The assessment evaluates the threat of SSH brute force attacks, the vulnerability of weak authentication and missing SIEM log ingestion, and assigns a risk rating based on likelihood and impact scoring. Mitigation controls are proposed and mapped to NIST SP 800-53. |
| **IOCs** | Source: 127.0.0.1 — 10 consecutive SSH authentication failures — invalid_user |
| **Impact Assessment** | Medium risk — detection gap confirmed. No production impact in lab environment. Real-world equivalent would represent high-impact exposure. |
| **Response Actions Taken** | Asset identified. Threat and vulnerability documented. Risk scored. Mitigations proposed. |
| **Recommended Actions** | Remediate SSH log ingestion gap. Enforce strong authentication. Implement account lockout policy. |
| **Status** | Closed — Risk Documented |

---

## Lab Objectives

- Define the asset under assessment
- Identify the threat and vulnerability
- Score likelihood and impact
- Calculate risk rating
- Propose mitigations mapped to NIST SP 800-53

---

## Environment Overview

| Component | Details |
|---|---|
| Host OS | Windows |
| VM 1 | Kali Linux (asset under assessment) |
| Virtualization | VMware Workstation |
| SIEM | Elastic Cloud Serverless (GCP Iowa) |
| Incident Source | Lab 13 — SSH Brute Force Simulation |

---

## Asset Inventory

| Asset | Type | Owner | Criticality |
|---|---|---|---|
| Kali Linux VM | Virtual Machine | Eric Ellison — SOC Analyst | Medium |
| Elastic Agent | Security Tool | Eric Ellison — SOC Analyst | High |
| SSH Service (sshd) | Network Service | System | High |
| systemd journal logs | Log Source | System | High |

---

## Threat Identification

| Field | Details |
|---|---|
| **Threat** | SSH Brute Force Attack |
| **Threat Actor** | External attacker / insider threat |
| **Attack Vector** | Network — SSH port (TCP 22) |
| **Technique** | Repeated failed authentication attempts using invalid credentials |
| **Evidence** | 10 consecutive SSH failures logged in journalctl (Lab 13) |
| **MITRE ATT&CK** | T1110 — Brute Force |

---

## Vulnerability Identification

| Vulnerability | Description | Status |
|---|---|---|
| SSH auth logs not ingested into SIEM | Failed login attempts not visible in Elastic — detection gap confirmed in Labs 13 and 14 | Open |
| No account lockout policy | Unlimited authentication attempts permitted without lockout | Open |
| No automated alerting for brute force | Custom detection rule created in Lab 12 did not fire due to log ingestion gap | Open |
| Weak authentication controls | No MFA or certificate-based authentication enforced on SSH | Open |

---

## Risk Scoring

### Scoring Methodology
- **Likelihood:** 1 (Low) — 2 (Medium) — 3 (High)
- **Impact:** 1 (Low) — 2 (Medium) — 3 (High)
- **Risk Rating = Likelihood × Impact**
- **Risk Level:** 1–2 (Low) | 3–4 (Medium) | 6–9 (High)

### Risk Register

| Risk ID | Threat | Vulnerability | Likelihood | Impact | Risk Score | Risk Level | Mitigation |
|---|---|---|---|---|---|---|---|
| R-001 | SSH Brute Force | No account lockout | 3 | 3 | 9 | High | Implement account lockout policy |
| R-002 | SSH Brute Force | Auth logs not in SIEM | 3 | 2 | 6 | High | Fix Elastic log ingestion |
| R-003 | SSH Brute Force | No automated alerting | 2 | 2 | 4 | Medium | Remediate log gap — alerts will fire |
| R-004 | SSH Brute Force | No MFA on SSH | 2 | 3 | 6 | High | Enforce key-based or MFA authentication |

---

## Mitigation Controls

| Risk ID | Mitigation | NIST SP 800-53 Control | Priority |
|---|---|---|---|
| R-001 | Implement SSH account lockout after 5 failed attempts | AC-7 — Unsuccessful Login Attempts | High |
| R-002 | Ingest SSH auth logs into Elastic SIEM data stream | AU-2 — Event Logging, AU-12 — Audit Record Generation | High |
| R-003 | Verify detection rule fires after log ingestion remediation | SI-4 — System Monitoring | Medium |
| R-004 | Enforce SSH key-based authentication, disable password auth | IA-5 — Authenticator Management | High |

---

## Residual Risk

If all mitigations are implemented:

| Risk ID | Residual Likelihood | Residual Impact | Residual Score | Residual Level |
|---|---|---|---|---|
| R-001 | 1 | 3 | 3 | Medium |
| R-002 | 1 | 1 | 1 | Low |
| R-003 | 1 | 1 | 1 | Low |
| R-004 | 1 | 2 | 2 | Low |

---

## Conclusions

- Four risks identified — two rated High, one Medium, one High
- The most critical risk is R-001: no account lockout policy allows unlimited brute force attempts
- R-002 and R-004 are also High — the detection gap and weak authentication together represent significant exposure
- All four risks have defined mitigations mapped to NIST SP 800-53
- Residual risk drops to Medium or Low across all entries after mitigation

---

## Next Steps

- Lab 18: Access Control Review — analyze users, permissions, and authentication mapped to NIST access control requirements
- Remediate R-002 (SSH log ingestion) as highest-priority technical action
