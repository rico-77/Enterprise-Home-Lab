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

**Security Server: Ubuntu Server 22.04 (Wazuh)**


• Ubuntu Server will act as a Security Server. We opted for this option because we will need a dedicated server for security, and incorporating all in just one JupBox would be a problem for our Lab and VM, a dedicated server with a SIEM is a more stable option.


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start, same as with other ISO. Once the installation part is completed + the server is connected to NATnetwork, we are on the way to connect this server to AD using winbind.
Space and storage can be found on README.



• Wazuh SIEM - Refers to a system that combines log management, threat detection, and incident response to help organizations monitor and secure their IT environments. Wazuh acts as a SIEM solution by collecting and analyzing security data from multiple sources, detecting threats in real time, and facilitating efficient incident response.

Wazuh relies on an agent-based ecosystem. Software agents are deployed to workstations, servers, containers, and virtual machines, which send data to Wazuh’s server for processing, aggregation, and visualization of security-relevant information.

We will use Wazuh as our central hub for security logging, analysis, defense, and remediation while we conduct cyber-attack and defend exercises.

Wazuh provides a solid foundation for gathering relevant data while applying remediations. We will be able to actively view and visualize what happens when attackers are able to achieve initial access, lateral movement, elevation of privileges, persistence, and exfiltration.

As part of this project, we will be configuring Wazuh’s SIEM, XDR, and File Integrity Monitoring (FIM) modules. The Vulnerability Detection module already has a default configuration applied

• Installation steps: Sign into the Wazuh server VM (Security Server) as sec-user → escalate with sudo → install curl if needed (sudo apt install curl) → download and start the Wazuh installation script with curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i → allow the installer to complete (this installs the Wazuh Indexer and Server) → note the generated admin credentials and passwords from the output or from the wazuh-passwords.txt file → open a browser to https://localhost → accept the security warning → log in with the Wazuh credentials → once logged into the dashboard, deploy Wazuh agents on endpoints by installing the appropriate agent packages (Windows MSI for Windows clients, and DEB package for Linux client) and register each agent with the Wazuh manager using the manager’s IP and authorization key → start the agents on endpoints (NET START WAZUH on Windows, systemctl enable --now wazuh-agent on Linux) → in the Wazuh dashboard, create agent groups for Linux and Windows → assign each agent to the correct group → edit the agent.conf for each group to onboard custom logs (e.g., Windows event logs, Linux syslog/auth/audit logs) and save.

![wazuh A1](https://github.com/user-attachments/assets/1a570d7c-13f9-41f7-9949-ce476bbdfaee)




