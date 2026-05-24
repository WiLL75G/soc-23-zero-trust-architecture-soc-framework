# Zero Trust SOC Policy Framework
## Nexus Corp Security Operations Center

---

## Purpose

This policy defines how Nexus Corp's Security Operations Center
implements Zero Trust Architecture principles across all systems,
users, and networks. Every access request is treated as potentially
hostile until verified.

---

## Policy 1 — Identity Verification Policy

### Rule
All users must authenticate with MFA before accessing any
corporate resource — no exceptions.

### Requirements
```
✅ MFA mandatory for all accounts — including service accounts
✅ Privileged accounts require hardware security keys (FIDO2)
✅ Session tokens expire after 8 hours — re-authentication required
✅ Risk-based conditional access — block or challenge anomalous logins
✅ All authentication events logged to SIEM
```

### SOC Detection Rules
```
ALERT: Login from new country or impossible travel
ALERT: MFA bypass attempt detected
ALERT: Service account login outside maintenance window
ALERT: Failed MFA attempts > 5 in 10 minutes
ALERT: Privileged account login without hardware token
```

### MITRE ATT&CK Coverage
```
T1078  — Valid Accounts
T1110  — Brute Force
T1556  — Modify Authentication Process
```

---

## Policy 2 — Device Compliance Policy

### Rule
Only compliant, managed devices may access corporate resources.
Unknown devices are blocked at the access layer.

### Requirements
```
✅ All devices enrolled in MDM (Microsoft Intune / Jamf)
✅ EDR agent installed and active on all endpoints
✅ OS and security patches applied within 72 hours of release
✅ Disk encryption enabled (BitLocker / FileVault)
✅ Device health check performed at every login
```

### SOC Detection Rules
```
ALERT: Unmanaged device attempting network access
ALERT: EDR agent disabled or uninstalled
ALERT: Device out of compliance attempting privileged access
ALERT: New device registered to existing user account
ALERT: Device accessing resources from unusual location
```

### MITRE ATT&CK Coverage
```
T1200  — Hardware Additions
T1554  — Compromise Host Software Binary
T1562  — Impair Defenses
```

---

## Policy 3 — Network Segmentation Policy

### Rule
The flat network is replaced by micro-segmented zones.
No implicit trust between network segments.

### Network Zones
```
Zone 1 — Public DMZ
         Internet-facing services only
         No direct access to internal zones

Zone 2 — Corporate Network
         Standard employee workstations
         Access to business applications only

Zone 3 — Privileged Access Zone
         IT administrators and security team only
         Requires jump host + MFA + hardware token

Zone 4 — Data Zone
         Databases and sensitive data stores
         No direct user access — API gateway only

Zone 5 — SOC Zone
         Security monitoring infrastructure
         Isolated from all other zones
```

### SOC Detection Rules
```
ALERT: Traffic between zones without authorised policy
ALERT: Direct database access bypassing API gateway
ALERT: Lateral movement detected between workstations
ALERT: Outbound connection to unauthorised external IP
ALERT: ICMP sweep or port scan detected internally
```

### MITRE ATT&CK Coverage
```
T1021  — Remote Services
T1046  — Network Service Discovery
T1135  — Network Share Discovery
T1570  — Lateral Tool Transfer
```

---

## Policy 4 — Least Privilege Access Policy

### Rule
Every user and service account has the minimum permissions
required to perform their role — and nothing more.

### Requirements
```
✅ Role-based access control (RBAC) for all systems
✅ Just-in-time (JIT) access for privileged operations
✅ Access reviews conducted quarterly
✅ Service accounts have no interactive login capability
✅ Temporary elevated access auto-expires after 4 hours
✅ All privilege escalations require approval + logging
```

### Access Tiers
```
Tier 0 — Domain Admin / Cloud Admin
         Requires: Hardware token + Manager approval + Audit log
         Session limit: 2 hours
         Auto-revoke: Yes

Tier 1 — Server Admin / Database Admin
         Requires: MFA + Manager approval
         Session limit: 4 hours
         Auto-revoke: Yes

Tier 2 — Standard User
         Requires: MFA
         Session limit: 8 hours
         Auto-revoke: No

Tier 3 — Read Only / Guest
         Requires: MFA
         Session limit: 4 hours
         Auto-revoke: Yes
```

### SOC Detection Rules
```
ALERT: User accessing resources outside their role
ALERT: Privilege escalation without approval ticket
ALERT: Elevated session exceeding time limit
ALERT: Service account used for interactive login
ALERT: Bulk data access by standard user account
```

### MITRE ATT&CK Coverage
```
T1078  — Valid Accounts
T1548  — Abuse Elevation Control Mechanism
T1134  — Access Token Manipulation
```

---

## Policy 5 — Data Protection Policy

### Rule
All data is classified and protected according to its
sensitivity level. Data cannot leave its classification
boundary without explicit authorisation.

### Data Classification Levels
```
PUBLIC      — Information approved for public release
              No special controls required

INTERNAL    — Standard business information
              Encryption in transit required

CONFIDENTIAL — Sensitive business data
               Encryption at rest and in transit
               Access logging mandatory
               DLP policies active

SECRET      — Highly sensitive data (PII, credentials, IP)
              End-to-end encryption mandatory
              IRM protection active
              Access requires Tier 1+ approval
```

### SOC Detection Rules
```
ALERT: CONFIDENTIAL data accessed from unmanaged device
ALERT: SECRET data download exceeding 50MB
ALERT: DLP policy violation — data exfiltration attempt
ALERT: Classified data sent to external email address
ALERT: Bulk export of customer PII records
```

### MITRE ATT&CK Coverage
```
T1530  — Data from Cloud Storage
T1567  — Exfiltration Over Web Service
T1048  — Exfiltration Over Alternative Protocol
T1005  — Data from Local System
```

---

## SOC Response Procedures for Zero Trust Violations

### Severity 1 — Critical (Automatic Response)
```
Trigger: MFA bypass + privileged access
         Impossible travel + data export
         Lateral movement + ransomware behaviour

Response:
1. Automatically isolate the affected account
2. Automatically isolate the affected device
3. Page on-call SOC Tier 2 immediately
4. Preserve all evidence automatically
5. Initiate IR playbook
```

### Severity 2 — High (SOC Triage Required)
```
Trigger: Unknown device access attempt
         Policy violation by privileged account
         Anomalous data access pattern

Response:
1. Alert fires in SIEM — SOC Tier 1 triages
2. Investigate within 15 minutes
3. Contain if confirmed — isolate account/device
4. Escalate to Tier 2 with full triage notes
```

### Severity 3 — Medium (Investigation Required)
```
Trigger: Standard user accessing new resource
         Login from new location (not impossible)
         Failed compliance check on device

Response:
1. Alert fires in SIEM — SOC Tier 1 reviews
2. Investigate within 1 hour
3. Contact user to verify if legitimate
4. Document findings in ticketing system
```
