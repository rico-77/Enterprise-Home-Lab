![BBBBB](https://github.com/user-attachments/assets/59c1709a-caae-48e6-b2a4-1097a4cbe5a3)


🖥️ **Virtual Machines**

💻 Platform: VirtualBox


• At the start of this project, I selected Oracle VirtualBox as the virtualization platform. I chose it based on previous experience from previous lab environments, which allowed me to efficiently configure, network, and manage multiple interconnected virtual machines. What else to say OVB is user friendly there are tons of available guides online and the UI is very simple to use. 

---

🖧 **Servers**

•🏢 AD Server: Windows Server 2025 (AD)

•🏢 Corporate Server: Ubuntu Server 22.04 (JumpBox)

•🛡️ Security Server: Ubuntu Server 22.04 (Wazuh)

 <br>




# 🏢 Windows Server 2025 – Active Directory


Windows Server 2025 running Active Directory is the core of this lab environment.  
Active Directory handles authentication, authorization, and identity management.  

Because of its central role in enterprise environments, AD is a common target for attackers.  
In this lab, some early-stage configurations intentionally use open ports and weak credentials to simulate a realistic attack surface for security testing.

This Domain Controller forms the foundation of the internal corporate network.

---

## Lab Architecture Decision

The server is deployed as a **Virtual Machine using an ISO image**, allowing a fully controlled on-premises setup.  

Benefits of this approach:

- Full control over CPU, RAM, and storage  
- Network segmentation using NAT Network  
- Safe snapshot, reset, and rebuild capabilities  
- Realistic enterprise-style environment  

The Domain Controller was the first system brought online, as all other lab services depend on it.

---

## Network Configuration

Assigning a **static IP address** was the first step:

- IP Address: `10.0.0.5`  
- Subnet Mask: `255.255.255.0`  
- Default Gateway: `10.0.0.1`  
- DNS Server: `10.0.0.5`

After network setup, the server was promoted to **Domain Controller** and a new forest was created:
corp.project-x-dc.com


---

## Roles Installed

Using Server Manager, the following roles were installed:

- Active Directory Domain Services (AD DS)  
- DNS Server  
- DHCP Server  
- File and Storage Services  
- IIS (for future lab modules)  

### DNS
The Domain Controller handles internal DNS for all connected clients.  
Forwarder configured: `8.8.8.8` for external resolution.

### DHCP
Automatically assigns IP addresses to connected clients.

- Scope Range: `10.0.0.100 – 10.0.0.200`  
- Subnet: `255.255.255.0`  
- Router: `10.0.0.1`  

---

## Installation Summary

Mount Windows Server ISO → Install Windows Server (Desktop Experience) → Configure static IP → Install AD DS, DNS, DHCP roles → Promote to Domain Controller → Create new forest → Configure DNS forwarder → Create DHCP scope → Reboot → Verify services → Add users → Join clients to domain.

---

## Integration With Lab Environment

Once Active Directory was operational:

- Windows clients were domain-joined  
- Linux clients were integrated  
- Centralized authentication became active  
- Internal enterprise structure established  

This setup enables all lab services to operate under a managed corporate network.

---

## Challenges & Lessons Learned

No major blockers occurred, but DNS issues were frequent when joining clients to the domain.  

Sometimes, client machines failed to authenticate despite correct configuration.  
After troubleshooting (network resets, reviewing settings, online research), the issue was resolved by restarting DNS services and affected clients.

> Lesson learned: In AD environments, if authentication fails unexpectedly, always verify DNS first.

This reinforced the critical dependency between Active Directory and DNS.



![all dc](https://github.com/user-attachments/assets/63dae2c4-ed1d-4f17-b54d-9c6d831040a6)



 <br>


# 🏢 Corporate Server – Ubuntu JumpBox + MailHog

## Overview

The Ubuntu Server acts as a **JumpBox** in our lab environment, providing:

- Secure SSH access for all users  
- FTP services  
- DNS services  
- Email simulation services (MailHog)

This server serves as the gateway to the lab, enabling secure management of the closed environment while supporting other lab infrastructure.

---

## Lab Architecture Decision

The server is deployed as a **Virtual Machine using an Ubuntu ISO**, giving full control over:

- CPU, memory, and storage allocation  
- Network connectivity (NAT network)  
- Snapshotting, testing, and rebuilding safely  

All users connect via **SSH**, ensuring encrypted remote access.  
This server also hosts **MailHog** in a Docker container for email testing.

---

## Docker & MailHog

**Docker Engine** is installed on the Ubuntu host to enable containerized services.  
We use it to run **MailHog**, a lightweight email testing tool that:

- Acts as a fake SMTP server (port 1025)  
- Provides a Web UI to inspect emails (port 8025)  
- Can be interacted with via API  

MailHog simulates a business email server, which is used in the **Cyber Attack module** to conduct controlled phishing exercises.

The first use of this server demonstrates how Docker simplifies deployment:

1. Pull official MailHog Docker image  
2. Configure SMTP and Web UI ports  
3. Set up a Python email polling script on a Linux client to monitor messages  

This allows the lab to simulate email-based attacks safely.

---

## Installation Steps (Technical Flow)

Install Ubuntu → reboot → set Administrator password → log in → configure static IP → join Active Directory (`sudo net ad join -U Administrator`) → verify join and log in as `CORP+Administrator` → install Docker Engine → test Docker (`docker pull hello-world` and `docker run hello-world`) → create `/home/mailhog` directory → create `docker-compose.yml` for MailHog (image `mailhog/mailhog`, ports `1025:1025` and `8025:8025`) → run `sudo docker compose up -d` → open browser at `http://localhost:8025` → create Python test email script (`test_message.py`) → send test email via `localhost:1025` → confirm receipt in MailHog UI → optionally deploy email poller script on client.

---

## Integration With Lab Environment

Once the JumpBox is operational:

- All clients can securely connect via SSH  
- MailHog captures all outgoing lab emails for analysis  
- The Python polling script allows simulation of phishing attacks  
- FTP, DNS, and other services are available for testing

This server becomes the central point for controlled lab experiments involving networking and email security.

---

## Challenges & Lessons Learned

No major blockers were encountered during setup, but as expected:

- DNS, DHCP, and AD issues occasionally arose when joining or managing clients  
- Troubleshooting reinforced the importance of proper IP and network configuration  

> Personal insight: Docker greatly simplified MailHog deployment — what would normally take multiple configuration steps on a full mail server was completed in minutes, highlighting the power of containerized services in lab environments.


![hog](https://github.com/user-attachments/assets/f67197c5-63e8-4275-9626-c9c3eb07aa08)


 <br>

# 🛡️ Security Server – Ubuntu 22.04 + Wazuh SIEM

## Overview

The Security Server is a dedicated **Ubuntu Server 22.04** instance running **Wazuh SIEM**.

Instead of combining security tooling with the JumpBox, I intentionally deployed a separate server. This decision improves:

- Stability
- Performance isolation
- Realistic enterprise architecture
- Scalability for logging and monitoring

In real-world environments, security monitoring systems are rarely hosted on general-purpose access servers. Separating roles reflects better operational practice.

---

## Architecture Decision

The server was deployed as a **Virtual Machine using an Ubuntu ISO**, allowing full control over:

- CPU and RAM allocation (SIEM tools are resource-intensive)
- Storage management for log retention
- NAT network segmentation
- Snapshot-based recovery

After installation, the server was:

- Connected to the internal NAT network
- Joined to Active Directory using `winbind`
- Prepared for centralized log ingestion

This setup allows domain-based authentication while maintaining security role separation.

---

## Why Wazuh?

Wazuh is an open-source **SIEM and XDR platform** that provides:

- Log collection and analysis
- Threat detection
- File Integrity Monitoring (FIM)
- Vulnerability detection
- Incident response capabilities

Wazuh operates using an **agent-based architecture**:

- Agents are deployed on endpoints (Windows, Linux, etc.)
- Agents send logs and telemetry to the Wazuh Server
- The server processes, correlates, and visualizes events

In this lab, Wazuh acts as the central security monitoring hub during offensive and defensive exercises.

It provides real-time visibility into:

- Initial access attempts
- Lateral movement
- Privilege escalation
- Persistence mechanisms
- Data exfiltration attempts

This transforms the lab from just an infrastructure build into a monitored enterprise simulation.

---

## Modules Used in This Lab

- SIEM (Log aggregation and correlation)
- XDR (Extended Detection & Response)
- File Integrity Monitoring (FIM)
- Vulnerability Detection (default configuration)

Allow installation to complete (Wazuh Server + Indexer) → save generated credentials (wazuh-passwords.txt) → access dashboard at https://localhost → accept certificate warning → log in → deploy Wazuh agents on endpoints (Windows MSI / Linux DEB) → register agents with manager IP and key → start agents (NET START WAZUH on Windows / systemctl enable --now wazuh-agent on Linux) → create agent groups (Windows / Linux) → assign agents → configure agent.conf to collect relevant logs (Windows Event Logs, Linux syslog/auth/audit).

Integration With Lab Environment

Once operational:

All endpoints report security telemetry to the Security Server

Attack simulations are logged and visualized in real time

Alerts are generated during malicious activity

File changes are monitored via FIM

This setup enables controlled attack-and-defend exercises with full visibility into system behavior.

Challenges & Lessons Learned

Deploying Wazuh reinforced an important architectural principle:

SIEM solutions are resource-intensive and should be isolated from operational servers.

Log ingestion and indexing require sufficient memory and storage planning.
Separating the Security Server prevented performance bottlenecks and kept the lab stable.

Additionally, proper agent grouping and log tuning significantly reduced alert noise and improved detection quality.

## Installation Steps (Technical Flow)

Install Ubuntu 22.04 → configure networking → join Active Directory via `winbind` → log in as `sec-user` → escalate with `sudo` → install `curl` if needed → download Wazuh installer:

```bash
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh
sudo bash ./wazuh-install.sh -a -i
![wazuh A1](https://github.com/user-attachments/assets/1a570d7c-13f9-41f7-9949-ce476bbdfaee)
```
---
Allow installation to complete (Wazuh Server + Indexer) → save generated credentials (`wazuh-passwords.txt`) → access dashboard at `https://localhost` → accept certificate warning → log in → deploy Wazuh agents on endpoints (Windows MSI / Linux DEB) → register agents with manager IP and authorization key → start agents (`NET START WAZUH` on Windows / `systemctl enable --now wazuh-agent` on Linux) → create agent groups (Windows / Linux) → assign agents → configure `agent.conf` to collect relevant logs (Windows Event Logs, Linux syslog/auth/audit).

---

## Integration With Lab Environment

Once operational:

- All endpoints report security telemetry to the Security Server
- Attack simulations are logged and visualized in real time
- Alerts are generated during malicious activity
- File changes are monitored via File Integrity Monitoring (FIM)

This setup enables controlled attack-and-defend exercises with full visibility into system behavior.

---

## Challenges & Lessons Learned

Deploying Wazuh reinforced an important architectural principle:

> SIEM solutions are resource-intensive and should be isolated from operational servers.

Log ingestion and indexing require sufficient memory and storage planning.  
Separating the Security Server prevented performance bottlenecks and kept the lab stable.

Additionally, proper agent grouping and log tuning significantly reduced alert noise and improved detection quality.




