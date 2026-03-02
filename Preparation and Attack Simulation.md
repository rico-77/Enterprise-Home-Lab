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

This simulation represents a structured attack chain, not random exploitation.

---

# Attack Scenario Flow

The simulated attack follows a realistic progression:

1. Initial Access (Phishing)
2. Credential Abuse
3. Lateral Movement
4. Privilege Escalation
5. Sensitive File Access
6. Data Exfiltration

Each phase is monitored by:

- Wazuh (endpoint visibility)
- Security Onion (network visibility)

---

# 1️⃣ Initial Access – Phishing Simulation

**Method:**
- Email sent via MailHog SMTP
- Delivered from `project-x-corp-svr`
- Received on `project-x-linux-client`

**Objective:**
- Simulate user interaction
- Trigger credential exposure or execution attempt

### Detection Validation
- SMTP traffic captured by Security Onion
- Mail service logs indexed in Wazuh
- Email activity visible in SIEM dashboard

---

# 2️⃣ Credential Abuse

**Method:**
- Attempted authentication using weak credentials
- SSH and WinRM tested where applicable

**Objective:**
- Validate brute-force feasibility
- Confirm authentication logging

### Detection Validation
- Failed login attempts logged by Wazuh
- SSH authentication anomalies flagged
- Windows Event Logs indexed
- Alert thresholds triggered

---

# 3️⃣ Lateral Movement

**Method:**
- Remote access via:
  - SSH (Linux systems)
  - WinRM (Windows client)
  - RDP (Domain Controller)

**Objective:**
- Pivot between internal systems
- Escalate access privileges

### Detection Validation
- Remote session events recorded
- PowerShell logging captured (WinRM)
- RDP login events indexed
- Security Onion identifies remote session traffic

---

# 4️⃣ Privilege Escalation

**Method:**
- Abuse of over-permissioned accounts
- Attempt access to high-value systems

**Objective:**
- Validate access control weaknesses
- Confirm monitoring of elevated sessions

### Detection Validation
- Group membership activity logged
- Privileged login events flagged
- Alert severity increased for domain-level access

---

# 5️⃣ Sensitive File Access

**Target:**
- “Sensitive File” located on `project-x-dc`

**Method:**
- Accessed from compromised account
- Modified or copied for staging

**Objective:**
- Simulate data targeting
- Trigger File Integrity Monitoring (FIM)

### Detection Validation
- Wazuh FIM alert generated
- File access and modification recorded
- Correlation between user session + file activity visible in SIEM

---

# 6️⃣ Data Exfiltration

**Method:**
- Transfer of staged file to `project-x-attacker`
- Simulated outbound data movement

**Objective:**
- Validate outbound monitoring
- Detect abnormal traffic patterns

### Detection Validation
- Security Onion captures outbound session
- Wazuh correlates file access with network activity
- Alert generated for suspicious data transfer

---

# Correlation & Monitoring Outcome

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
