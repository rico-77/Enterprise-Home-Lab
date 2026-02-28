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


**Corporate Server: Ubuntu Server (JumpBox)** + **MailHog**

• Ubuntu Server will act as a JumpBox on our Lab environment, this means it will be set up as a server that will be secure and will provide secure access to the closed environment. Additionally, this server will provide FTP, DNS, and email access. 


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start, same as with other ISO. Once the installation part is completed + the server is connected to NATnetwork, we are on the way to connect this server to AD. All users will connect to the Server over SSH, making it a secure connection type. We will use this machine as a mailing server by installing MailHog inside a Docker container. MailHog will run on ports 1025 (SMTP) and 8025 (web interface). We will also install a Python polling script that will work with MailHog to simulate a phishing attack scenario.
Space and storage can be found on README.


Because we will provide FTP, DNS, and email access. For this infrastructure to work on our JumpServer, we will be downloading and installing Docker Engine, which will allow us to run the containers on the Server, in our case we will be using it for a fake SMTP, more about that when we talk about MailHog.

• MailHog: MailHog is a lightweight email testing tool that acts as a fake SMTP server. It captures all outgoing emails sent by applications without delivering them to real inboxes. You can inspect emails via a web interface (Web UI accessible on port 8025), the SMTP service (default port 1025), or through its API. We will be using MailHog to simulate a business email server, which will be used as part of our phishing exercise in the Cyber Attack module.

This will be the first use of this server, and it will be made possible by using Docker. MailHog provides an official Docker image ready to use from Docker Hub. In our case, we will configure MailHog SMTP on the server and set up an Email Poller Script on our Linux client. All of this will be used in our lab when executing an attack scenario (phishing attack).

• Installation steps: install Linux Ubuntu → reboot → set Administrator password → log in → configure static IP → join the machine to Active Directory with sudo net ad join -U Administrator using the AD Administrator password → verify join and log in as CORP+Administrator → install Docker Engine on the Ubuntu host (follow official Docker install commands for Ubuntu) → test Docker with docker pull hello-world and docker run hello-world → create a directory /home/mailhog → inside it create a docker-compose.yml with the MailHog service (image mailhog/mailhog mapping ports 1025:1025 and 8025:8025) → save file → run sudo docker compose up -d to start MailHog → open a browser pointed at http://localhost:8025 to view the MailHog UI → create a test Python email script (test_message.py) with standard smtplib to send a message via localhost:1025 → make it executable and run it → confirm the email appears in the MailHog UI → optionally create an email poller script under the Linux client if needed for later exercises.

• Issues encountered: No active problems encountered, but DNS, DHCP and AD issues can arise from time to time when in active use.



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




