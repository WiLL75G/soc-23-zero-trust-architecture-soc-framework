# Zero Trust Architecture — Core Principles

## Overview

Zero Trust is a security model based on one principle:

> **"Never trust, always verify."**

Traditional security assumed everything inside the network perimeter
was safe. Zero Trust assumes the perimeter has already been breached.
Every user, device, and connection must prove itself — every time.

---

## The 3 Core Pillars of Zero Trust

### Pillar 1 — Verify Explicitly

```
WHAT IT MEANS:
Always authenticate and authorise based on all available
data points — identity, location, device, service, workload,
data classification, and anomalies.

NEVER assume trust based on:
- Network location (being on the internal network)
- IP address
- Previous authentication
- User role alone

SOC IMPLICATION:
Every access request generates a log entry.
Every log entry is a detection opportunity.
Zero Trust creates more visibility — not less.
```

---

### Pillar 2 — Use Least Privilege Access

```
WHAT IT MEANS:
Limit user access to only what they need —
and only for as long as they need it.

Principles:
- Just-in-time (JIT) access — grant access only when needed
- Just-enough-access (JEA) — grant only the minimum permissions
- Risk-based adaptive policies — increase friction for risky access
- Data protection — limit what data users can see and export

SOC IMPLICATION:
Privilege escalation attacks become much harder.
A compromised account with minimal permissions
does far less damage than one with admin rights.
```

---

### Pillar 3 — Assume Breach

```
WHAT IT MEANS:
Design as if attackers are already inside.
Minimise blast radius. Segment access. Encrypt everything.
Hunt proactively — don't wait for alerts.

Actions:
- Segment networks so attackers can't move laterally
- Encrypt all data in transit and at rest
- Use analytics to detect anomalies in behaviour
- Automate response to contain threats quickly

SOC IMPLICATION:
This pillar is where the SOC lives.
Threat hunting, behavioural analytics, and automated
containment are all "assume breach" in action.
```

---

## The 5 Zero Trust Components

### 1. Identity
```
The primary security perimeter in Zero Trust.
Every access request starts with identity verification.

Controls:
- Multi-factor authentication (MFA) — mandatory
- Privileged Identity Management (PIM)
- Conditional access policies
- Identity risk scoring

Tools: Azure AD, Okta, CrowdStrike Identity
```

---

### 2. Devices
```
Every device must be verified before granting access.
Unknown or non-compliant devices are blocked.

Controls:
- Device compliance policies
- Endpoint Detection and Response (EDR)
- Mobile Device Management (MDM)
- Certificate-based authentication

Tools: Intune, CrowdStrike, Jamf
```

---

### 3. Network
```
Micro-segmentation replaces the flat network perimeter.
Attackers who breach one segment cannot reach others.

Controls:
- Micro-segmentation
- Software Defined Networking (SDN)
- Zero Trust Network Access (ZTNA)
- Encrypted communications everywhere

Tools: Zscaler, Illumio, Cisco SD-Access
```

---

### 4. Applications
```
Applications must verify identity before granting access.
Shadow IT must be discovered and governed.

Controls:
- Application access policies
- Cloud Access Security Broker (CASB)
- API security
- App-level MFA

Tools: Microsoft Defender for Cloud Apps, Netskope
```

---

### 5. Data
```
Data is the ultimate target. Protect it directly.
Classification drives protection level.

Controls:
- Data classification (Public, Internal, Confidential, Secret)
- Data Loss Prevention (DLP)
- Encryption at rest and in transit
- Information Rights Management (IRM)

Tools: Microsoft Purview, Varonis, Digital Guardian
```

---

## Zero Trust vs Traditional Security

| Aspect | Traditional | Zero Trust |
|---|---|---|
| Trust model | Trust inside the network | Never trust anyone |
| Perimeter | Hard external boundary | Identity is the perimeter |
| Lateral movement | Easy — attackers move freely | Hard — micro-segmentation blocks it |
| Remote access | VPN | ZTNA |
| Detection | Perimeter monitoring | Behavioural analytics everywhere |
| Breach assumption | External threats | Assume already breached |
| Access control | Role-based | Least privilege + JIT |

---

## MITRE ATT&CK Techniques Zero Trust Mitigates

| Technique ID | Technique | ZTA Control |
|---|---|---|
| T1078 | Valid Accounts | MFA + Conditional Access |
| T1110 | Brute Force | Risk-based lockout policies |
| T1021 | Remote Services | ZTNA replaces VPN |
| T1550 | Use Alternate Auth Material | Token validation policies |
| T1534 | Internal Spearphishing | Email security + user training |
| T1557 | MITM | Encrypted communications |
| T1486 | Data Encrypted for Impact | Micro-segmentation limits blast radius |
