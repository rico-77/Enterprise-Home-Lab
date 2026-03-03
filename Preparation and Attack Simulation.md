# ⚔️ Security Exercises

## 🔓 Configure Vulnerable Environment

## 🕵️ Cyber Attack Simulation

 <br>
 
---

## 🔓 Configure Vulnerable Environment

## Overview

With the infrastructure fully deployed (Domain Controller, JumpBox, Security Server, workstations, and monitoring stack), this phase focuses on intentionally introducing controlled weaknesses.

The objective is not misconfiguration for its own sake —  
it is to create realistic attack paths and validate detection coverage across:

- Wazuh (host-based monitoring)
- Security Onion (network-based monitoring)

Each exposed service is immediately paired with detection integration and alert validation.

This transforms the lab into a structured attack-and-detect simulation environment.

---

 <br>
 
# Service Exposure & Detection Integration

## 1️⃣ Open SSH on `project-x-corp-svr`

**Action:**
- Enabled SSH service
- Allowed internal network access
- Used weak credentials for testing

**Attack Surface Created:**
- Brute-force potential
- Credential harvesting
- Remote command execution

🚨 **Detection Integration**
- Wazuh monitors authentication attempts
- Security Onion monitors SSH traffic patterns
- Failed login attempts logged and indexed

---

 <br>
 
## 2️⃣ Open SSH on `project-x-linux-client`

**Action:**
- Enabled SSH server
- Allowed lateral movement from internal hosts

**Attack Surface Created:**
- Internal pivot point
- Privilege escalation testing

🚨 **Detection Integration**
- Authentication log monitoring enabled
- Alert thresholds configured for repeated failed logins

🚨 **Detection Alert Created**
- Custom rule for excessive SSH authentication failures
- Alert severity tuned for brute-force simulation

![ssh](https://github.com/user-attachments/assets/d7e5ac80-9ffb-4c30-9c55-4d4e3ed84d54)

---

 <br>
 
## 3️⃣ Configure SMTP Email Connection  
`project-x-corp-svr` → `project-x-linux-client`

**Action:**
- Configured MailHog SMTP (port 1025)
- Enabled email delivery simulation between hosts

**Attack Surface Created:**
- Phishing simulation capability
- Email-based payload delivery testing

🚨 **Detection Integration**
- Wazuh monitors mail service logs
- Security Onion inspects SMTP traffic
- Email transmission events indexed for correlation
![smtp](https://github.com/user-attachments/assets/01e4f692-493a-41c8-9c25-51eb23ea7d88)

---

 <br>
 
## 4️⃣ Enable WinRM on `project-x-win-client`

**Action:**
- Enabled Windows Remote Management
- Allowed remote administrative access

**Attack Surface Created:**
- Remote command execution
- Credential abuse simulation
- Lateral movement via PowerShell

🚨 **Detection Integration**
- Windows Event Logs forwarded to Wazuh
- WinRM activity monitored
- PowerShell logging enabled

🚨 **Detection Alert Created**
- Custom alert for suspicious WinRM sessions
- Elevated severity for non-administrative account usage

![winrm](https://github.com/user-attachments/assets/356f4767-2f2c-4605-b4f1-e5df44062cdf)



---

 <br>
 
## 5️⃣ Enable RDP on `project-x-dc`

**Action:**
- Enabled Remote Desktop Protocol
- Allowed internal access for testing

**Attack Surface Created:**
- Brute-force testing
- Privileged remote login attempts
- Persistence validation

🚨 **Detection Integration**
- RDP login events collected via Wazuh
- Security Onion monitors RDP traffic signatures

![rdp](https://github.com/user-attachments/assets/a829446d-84cb-4e5b-8247-b6b880740fbb)


---

 <br>
 
## 6️⃣ Setup “Sensitive File” on `project-x-dc`

**Action:**
- Created high-value file on Domain Controller
- Enabled File Integrity Monitoring (FIM)

**Attack Surface Created:**
- Target for privilege escalation
- Simulated data theft objective

🚨 **Detection Integration**
- Wazuh FIM enabled on directory
- File access, modification, and deletion logged

🚨 **Detection Alert Created**
- High-severity alert for file access or modification
- Indexed for correlation with user session activity
- 
![ssensitiv](https://github.com/user-attachments/assets/74516f60-07db-4231-94a2-41d5bc3a3b09)

---

 <br>
 
## 7️⃣ Exfiltration to `project-x-attacker`

**Action:**
- Simulated data transfer from internal host to attacker machine

**Attack Surface Created:**
- Data exfiltration testing
- Network anomaly detection
- Lateral movement validation

🚨 **Detection Integration**
- Security Onion monitors outbound traffic patterns
- Wazuh correlates file access + outbound connection
- Alerts generated for unusual transfer behavior

![sett](https://github.com/user-attachments/assets/61daa936-72b1-43b5-b1a9-6df07b0b2ff0)

---

 <br>
 
# Security Validation Outcome

After completing this phase:

- Multiple controlled attack paths exist
- Remote access services are exposed internally
- Weak authentication paths are available
- High-value assets are defined
- Detection tools actively monitor each attack vector

This environment now supports:

- Brute-force attacks
- Phishing simulations
- Lateral movement
- Privilege escalation
- Data exfiltration
- Detection tuning and alert validation

---

# Architectural Intent

Every vulnerability introduced was:

- Documented
- Contained within an isolated NAT network
- Paired with monitoring integration
- Used to validate detection capability

This phase demonstrates not only offensive understanding,  
but defensive validation and alert engineering.

The lab is now prepared for full cyber attack simulation.

---

 <br>
 


# 🕵️ Cyber Attack Simulation

## Overview

With the vulnerable environment configured and detection tooling in place, this phase executes a controlled cyber attack simulation.

The objective is to validate:

- Attack path feasibility
- Detection visibility (host + network)
- Alert accuracy
- Correlation between telemetry sources
- Defensive monitoring effectiveness

All activity is performed inside an isolated NAT-based lab network.

This simulation represents a structured attack chain, not random exploitation

This simulation follows the documented attack path:

1️⃣ Reconnaissance  
2️⃣ Initial Access  
3️⃣ Lateral Movement  
4️⃣ Privilege Escalation  
5️⃣ Data Exfiltration  
5️⃣ Persistence  
❌ Defense Evasion  

---

# 1️⃣ Reconnaissance

## Objective
Identify reachable services, exposed ports, and potential entry points inside the internal network.

## Activity
- Internal host discovery
- Port scanning of:
  - `project-x-linux-client`
  - `project-x-win-client`
  - `project-x-dc`
  - `project-x-corp-svr`
- Service enumeration (SSH, SMTP, WinRM, RDP)

## Detection Integration

- Security Onion detects network scanning behavior
- Suspicious port sweep activity logged
- Traffic anomalies indexed for review
- Wazuh correlates unusual connection attempts

---

# 2️⃣ Initial Access

## Objective
Gain foothold through weak authentication or phishing simulation.

## Activity
- Phishing email delivered via MailHog (SMTP 1025)
- Authentication attempts using weak credentials
- SSH access attempts on Linux systems
- WinRM authentication testing on Windows client

## Detection Integration

- Failed login attempts logged by Wazuh
- Windows Event Logs forwarded and indexed
- SMTP traffic visible in Security Onion
- Custom alert for excessive authentication failures triggered

---

# 3️⃣ Lateral Movement

## Objective
Pivot from compromised system to additional internal hosts.

## Activity
- SSH pivot from Linux client
- WinRM session to Windows workstation
- RDP access to `project-x-dc`

## Detection Integration

- Remote session events recorded
- PowerShell activity logged (WinRM)
- RDP login events indexed
- Network session metadata captured by Security Onion
- Alert triggered for suspicious remote access patterns

---

# 4️⃣ Privilege Escalation

## Objective
Escalate privileges to obtain elevated or domain-level access.

## Activity
- Abuse of over-permissioned accounts
- Access to higher-privileged groups
- Validation of domain-level authentication

## Detection Integration

- Privileged logon events logged
- Group membership activity indexed
- Elevated account activity flagged
- Severity-based alerting for administrative sessions

---

# 5️⃣ Data Exfiltration

## Objective
Access and transfer sensitive data outside the compromised system.

## Activity
- Access “Sensitive File” on `project-x-dc`
- Stage file for transfer
- Simulated outbound transfer to `project-x-attacker`

## Detection Integration

- File Integrity Monitoring (FIM) alert generated
- File access and modification logged
- Outbound traffic captured by Security Onion
- Correlation between file access + network transfer

---

# 5️⃣ Persistence

## Objective
Maintain access after initial compromise.

## Activity
- Establish recurring access mechanism
- Maintain valid credentials or remote access capability
- Validate continued remote connectivity

## Detection Integration

- New service or scheduled task monitoring
- Authentication pattern monitoring
- Alert for abnormal recurring logins
- Persistence artifacts logged in Wazuh

---

# ❌ Defense Evasion

## Objective
Attempt to reduce visibility or bypass detection controls.

## Activity
- Attempt log clearing (where applicable in lab)
- Modification of monitoring-related configurations
- Testing detection blind spots

## Detection Integration

- Log tampering attempts flagged
- Service modification alerts triggered
- Integrity monitoring detects configuration changes
- Correlation engine highlights suspicious suppression behavior

---


# Simulation Outcome

During the full attack chain:

- Endpoint telemetry was collected
- Network traffic was analyzed
- Alerts were generated at each critical stage
- File Integrity Monitoring detected sensitive file interaction
- Authentication logs confirmed brute-force attempts
- Remote access events were indexed and visualized

The lab successfully demonstrated full visibility across:

- Initial compromise
- Internal movement
- Privilege escalation
- Data access
- Exfiltration

---

# Defensive Value

This simulation validates:

- Detection rule effectiveness
- Logging configuration accuracy
- Alert severity tuning
- Cross-platform telemetry integration
- Network + host correlation capability

It demonstrates the ability to:

- Build a vulnerable environment intentionally
- Execute a structured attack chain
- Monitor activity in real time
- Validate defensive coverage

---

# Key Takeaway

Security is not just about building infrastructure —  
it is about validating visibility under adversarial conditions.

This lab demonstrates both offensive understanding and defensive monitoring integration within a controlled enterprise simulation.
