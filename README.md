# Zero Trust Architecture and SOC Policy Framework

Designing the architecture, not responding to it. Three Zero Trust pillars across five domains, translated into detection rules a SOC can actually run.

## At a Glance

| Field | Detail |
| --- | --- |
| Work Type | Security architecture design and policy authoring |
| Framework | Zero Trust, verify explicitly, least privilege, assume breach |
| Domains | Identity, devices, network, applications, data |
| Delivered | 5 policies, detection rules per domain, ATT&CK mapping, 5 zone segmentation model |
| Organisation | Nexus Corp, fictional |

## What This Is

Zero Trust is not a product and it is not a purchase. It is the decision to stop treating "inside the network" as evidence of anything.

The old model trusted location. Get past the firewall and you were trusted, which meant one phished credential bought an attacker the entire estate. Zero Trust replaces that with the position that no request is trusted regardless of where it came from, and every access is verified on its own merits.

For a SOC that changes the work in one specific way: verification generates telemetry. Every access decision becomes a logged event, and every logged event is a detection opportunity that did not exist under implicit trust.

Scope stated plainly: this is a design and policy exercise for a fictional organisation, written on paper. It is not a deployed architecture. What it demonstrates is understanding the model well enough to translate it into rules.

## The Three Pillars

Verify explicitly. Authenticate and authorise on every request, using identity, device health, location, and behaviour. Not once at the door.

Least privilege. Just enough access, just in time, and it expires.

Assume breach. Design as though the attacker is already inside, because eventually they are. This is the pillar the SOC lives in. Threat hunting and behavioural analytics only make sense if you have already accepted that prevention failed somewhere.

The SOC reading of Zero Trust: the first two pillars are engineering's job. The third one is yours.

## Identity

Controls:

MFA mandatory on all accounts, service accounts included.

Hardware keys for privileged accounts.

Risk based conditional access blocking anomalous logins automatically.

All authentication events shipped to the SIEM.

Detection rules:

Login from a new country, or impossible travel.

MFA bypass attempt.

Service account login outside a maintenance window.

Failed MFA exceeding threshold.

The service account rule is the strongest of the four. A human logging in at an odd hour has an explanation. A service account is a schedule, and a schedule that deviates is not tired or travelling, it is being used by someone.

Identity is where the perimeter went. Every authentication event is now a security event, which is a much larger log volume and a much better detection surface.

## Devices

Controls:

MDM enrolment before network access.

EDR agent mandatory on every endpoint.

Device health verified at every login, not just at enrolment.

Non compliant devices blocked at the access layer.

Detection rules:

Unmanaged device attempting access.

EDR agent disabled or uninstalled.

Device accessing resources from an unusual location.

The EDR disabled rule is the one that matters. It is the only alert on the list where there is no benign version. Nobody disables their EDR by accident, and an attacker who does it has told you they are there and what they intend to do next.

Checking health at every login rather than at enrolment is the Zero Trust part. A device that was compliant in March is not evidence of anything in July.

## Network

Five zones replacing a flat network. No implicit trust between them, all cross zone traffic policed.

Zone 1, public DMZ.

Zone 2, corporate.

Zone 3, privileged access, requires jump host plus MFA plus hardware token.

Zone 4, data.

Zone 5, SOC, isolated from everything.

Micro segmentation is the control that answers ransomware. Ransomware's damage is not encryption, it is spread. Segmentation does not stop the first host from being encrypted, it stops the other four hundred.

Isolating the SOC zone is the detail worth defending in an interview. An attacker in the corporate network who can reach the SIEM can delete their own evidence, disable the rules watching them, and turn the investigation off from inside. The monitoring infrastructure has to be somewhere the compromise cannot reach.

Cross zone traffic alerts are high fidelity by design. Legitimate cross zone traffic should be rare and documented, which means anything undocumented is worth a look. That is the opposite of most SIEM rules, and it is why segmentation improves detection as well as containment.

## Data

Four tier classification: public, internal, confidential, secret.

DLP active on confidential and secret. Bulk export alerting. IRM on secret.

Detection rules:

Confidential data accessed from an unmanaged device.

Secret data download exceeding threshold.

DLP policy violation.

Classified data sent to an external address.

Classification is the foundation and it is the step organisations skip. You cannot write a DLP rule for "sensitive data" because DLP does not know what that means. Every data control downstream depends on the labels existing first, and labelling is unglamorous work that nobody wants to fund.

## Privileged Access

Tiered model, Tier 0 through Tier 3, with just in time elevation and automatic expiry.

Standing privilege is the problem. An admin account that is always an admin is always available to an attacker who takes it. JIT access means the privilege exists for the window it is needed and then does not, which shrinks the target from permanent to occasional.

Expiry is the part that has to be automatic. Access that requires someone to remember to revoke it is access that persists.

## MITRE ATT&CK Coverage

| Technique | ID | ZTA Control |
| --- | --- | --- |
| Valid accounts | T1078 | MFA and conditional access |
| Brute force | T1110 | Risk based lockout |
| Remote services | T1021 | ZTNA replacing VPN |
| Abuse elevation control mechanism | T1548 | JIT access with approval |
| Network service discovery | T1046 | Segmentation blocks the sweep |
| Lateral tool transfer | T1570 | Cross zone traffic policy |
| Data from cloud storage | T1530 | Classification and DLP |
| Exfiltration over web service | T1567 | DLP and outbound monitoring |
| Data encrypted for impact | T1486 | Segmentation limits blast radius |
| Impair defences | T1562 | EDR compliance monitoring |

Mapping note: these are the techniques the architecture is designed against, mapped control to technique. Nothing was observed. This is a design document.

## Findings

Perimeter security fails on one assumption: that inside means trusted. One credential defeats it.

Zero Trust removes implicit trust, and the side effect is that every access decision becomes a logged event the SOC can detect on.

Identity is the primary control surface. MFA and conditional access do more work than any other pair on this list.

Micro segmentation is the strongest available control against lateral movement and ransomware spread.

Classification is the prerequisite for every data control, and it is the one most often skipped.

Assume breach is what makes proactive hunting a policy rather than an initiative.

## The Honest Part

This is architecture on paper. None of it is deployed.

The tooling named, Azure AD, CrowdStrike, Zscaler, Purview, Splunk, is referenced as the category of product each control needs. I have hands on with Splunk. I have not deployed the others, and the design does not depend on having done so, but the distinction matters if anyone asks.

What this demonstrates is the ability to reason about controls at the architecture level and translate them into detection logic, which is a different skill from operating them.

## What This Demonstrates

Understanding Zero Trust as a trust model rather than a product category.

Translating each pillar into detection rules a SOC could actually run.

Designing segmentation with the monitoring infrastructure protected from the estate it monitors.

Knowing which control in each domain carries the most weight, and why.

Mapping architecture controls to ATT&CK techniques, control to technique rather than alert to technique.

Recognising that Zero Trust generates telemetry, and that telemetry is the SOC's return on it.

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

[![LinkedIn](https://img.shields.io/badge/LinkedIn-WilliamInCyber-blue?style=flat&logo=linkedin)](https://linkedin.com/in/WilliamInCyber)
[![X](https://img.shields.io/badge/X-@WilliamInCyber-black?style=flat&logo=x)](https://x.com/WilliamInCyber)
