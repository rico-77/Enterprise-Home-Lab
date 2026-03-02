# ⚔️ Security Exercises

## 🔓 Configure Vulnerable Environment

## 🕵️ Cyber Attack Simulation

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


![win clin](https://github.com/user-attachments/assets/c3ac95d8-eb27-44f2-9586-383d8a22df84)

---

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

---

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

---

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

---

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

## 🕵️ Cyber Attack Simulation


