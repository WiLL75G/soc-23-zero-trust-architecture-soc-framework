# Day 22 – SOC Tier 1 Incident Report: Zero Trust Architecture & SOC Framework

---

## Incident Summary

- **Project Type:** Zero Trust Architecture Design & SOC Policy Framework
- **Severity:** Strategic Enterprise Security Architecture
- **Scope:** Identity, Devices, Network, Applications, Data
- **Tools Referenced:** Azure AD, CrowdStrike, Zscaler, Microsoft Purview, Splunk
- **Status:** Complete ZTA Framework and SOC Policy Delivered

---

## Executive Summary

A complete Zero Trust Architecture framework was designed and documented for Nexus Corp's Security Operations Center. The framework covers the three core ZTA pillars Verify Explicitly, Use Least Privilege, and Assume Breach across five security domains: Identity, Devices, Network, Applications, and Data. Five enterprise grade SOC policies were produced with detection rules, MITRE ATT&CK mappings, and automated response procedures. This framework replaces the traditional perimeter-based security model with an identity first, assume-breach posture.

---

## Affected System

- **Organisation:** Nexus Corp SOC
- **Scope:** All users, devices, networks, applications, and data
- **Current Model:** Traditional perimeter based security
- **Target Model:** Zero Trust Architecture
- **Migration Priority:** Identity → Devices → Network → Applications → Data

---

## Investigation Methodology

---

### 1. Zero Trust Core Principles Documented

- Documented the 3 core ZTA pillars Verify Explicitly, Least Privilege, Assume Breach
- Mapped each pillar to SOC operations and detection strategy
- Compared Zero Trust vs traditional perimeter security model
- Identified the 5 ZTA components and their tooling requirements

#### SOC Observations:

- Zero Trust creates more log data which means more detection opportunities for the SOC
- The "Assume Breach" pillar is where SOC operations live threat hunting and behavioural analytics
- Identity has replaced the network perimeter every authentication event is a security event

---

### 2. Identity Verification Policy

- MFA mandatory for all accounts including service accounts
- Hardware security keys required for privileged accounts
- Risk-based conditional access blocks anomalous logins automatically
- All authentication events logged to SIEM for SOC visibility

#### SOC Detection Rules Built:
- Login from new country or impossible travel
- MFA bypass attempt detected
- Service account login outside maintenance window
- Failed MFA attempts exceeding threshold

#### SOC Observations:

- MFA is the single most effective control against credential-based attacks
- Service account logins outside maintenance windows are a confirmed red flag
- Risk-based conditional access automates Tier 1 decisions at machine speed

---

### 3. Device Compliance Policy

- All devices enrolled in MDM before network access is granted
- EDR agent mandatory on all endpoints
- Device health verified at every login not just enrolment
- Non-compliant devices blocked at the access layer automatically

#### SOC Detection Rules Built:
- Unmanaged device attempting network access
- EDR agent disabled or uninstalled
- Device accessing resources from unusual location

#### SOC Observations:

- An EDR agent that gets disabled is an immediate high severity alert
- Device compliance checks at login catch compromised endpoints before they cause damage
- BYOD environments require stricter device posture policies

---

### 4. Network Segmentation Policy

- Flat network replaced with 5 micro segmented zones
- No implicit trust between zones all cross-zone traffic requires policy
- SOC Zone isolated from all other zones monitoring infrastructure protected
- Privileged Access Zone requires jump host + MFA + hardware token

#### Network Zones Designed:
- Zone 1: Public DMZ
- Zone 2: Corporate Network
- Zone 3: Privileged Access Zone
- Zone 4: Data Zone
- Zone 5: SOC Zone (isolated)

#### SOC Observations:

- Micro-segmentation is the most effective control against lateral movement
- Isolating the SOC Zone means attackers who breach the corporate network cannot reach monitoring tools
- Cross-zone traffic alerts are high-fidelity legitimate cross-zone traffic should be rare and documented

---

### 5. Data Protection Policy

- Four-tier data classification system Public, Internal, Confidential, Secret
- DLP policies active on all Confidential and Secret data
- Bulk export alerts fire immediately for sensitive data
- IRM protection applied to Secret classification data

#### SOC Detection Rules Built:
- Confidential data accessed from unmanaged device
- Secret data download exceeding 50MB
- DLP policy violation data exfiltration attempt
- Classified data sent to external email address

#### SOC Observations:

- Data classification is the foundation of DLP you cannot protect what you haven't labelled
- Bulk export of PII records is one of the highest risk events in any SOC environment
- Data exfiltration over web services is the most common exfiltration technique — T1567

---

## MITRE ATT&CK Coverage

| Technique ID | Technique | ZTA Control |
|---|---|---|
| T1078 | Valid Accounts | MFA + Conditional Access |
| T1110 | Brute Force | Risk-based lockout policies |
| T1021 | Remote Services | ZTNA replaces VPN |
| T1548 | Abuse Elevation Control | JIT access + approval workflow |
| T1046 | Network Service Discovery | Micro segmentation blocks discovery |
| T1570 | Lateral Tool Transfer | Cross-zone traffic policies |
| T1530 | Data from Cloud Storage | Data classification + DLP |
| T1567 | Exfiltration Over Web Service | DLP + outbound traffic monitoring |
| T1486 | Data Encrypted for Impact | Micro-segmentation limits blast radius |
| T1562 | Impair Defenses | EDR compliance monitoring |

---

## SOC Analyst Findings

- Traditional perimeter security leaves attackers free to move laterally once inside
- Zero Trust eliminates implicit trust every access request is a detection opportunity
- Identity is the new perimeter MFA and conditional access are the most critical controls
- Micro-segmentation is the most effective control against ransomware lateral movement
- Data classification is the foundation of all data protection controls
- Assume Breach posture means the SOC hunts proactively not just reactively

---

## SOC Analyst Response

- Designed complete ZTA framework covering all 5 security domains
- Produced 5 enterprise SOC policies with detection rules and MITRE ATT&CK mappings
- Built tiered access model Tier 0 through Tier 3 with auto expiry controls
- Designed 5-zone network segmentation model with SOC Zone isolation
- Mapped automated response procedures for Severity 1, 2, and 3 ZTA violations
- Delivered complete policy framework ready for enterprise deployment

---

## Analyst Insight

Zero Trust is not a product it is a philosophy. Every SOC analyst needs to understand it because it fundamentally changes how alerts are interpreted. In a Zero Trust environment, a user accessing a resource they've never accessed before is not normal traffic to be ignored it is a signal to be investigated. The SOC's job in a Zero Trust world is not just to respond to breaches it is to enforce the verification that Zero Trust requires at every layer.

---

## Learning Outcome

- Understand the 3 core pillars of Zero Trust Architecture
- Design Zero Trust policies across all 5 security domains
- Build SOC detection rules aligned to Zero Trust violation patterns
- Map Zero Trust controls to MITRE ATT&CK techniques
- Understand how Zero Trust changes SOC detection and response strategy
- Design network micro segmentation with SOC Zone isolation
- Apply least privilege and JIT access principles to privileged account management

---

## Repository Structure

```
zero-trust-architecture-soc-framework/
├── README.md
├── docs/
│   └── zero_trust_principles.md
└── policies/
    └── zero_trust_soc_policy.md
```

---

## Conclusion

This project delivers a complete Zero Trust Architecture framework and SOC policy system for Nexus Corp. Five enterprise grade security policies were produced covering identity, devices, network, applications, and data each with detection rules, MITRE ATT&CK mappings, and automated response procedures. This framework demonstrates that a SOC analyst understands not just how to respond to threats, but how to design the architecture that makes threats harder to execute and easier to detect.
