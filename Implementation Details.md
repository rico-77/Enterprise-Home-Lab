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


**Windows Server 2025 (AD)**

• Windows Server, and primarily Active Directory, are the core of this entire lab setup. It handles all the Authentication, Authorization, and Management. As in many systems, AD is a prime target for attackers due to its role, the same will be in our environment. Due to our plan for  this Lab, our AD settings will be variable for get go, that means we will be using open ports and weak credentials.


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start. First task is assining of static IP Address, basically we are getting the AD online, and that will be fist step in creating our internal network, to do that we will promote the AD to a Domain Controler. After completing all work on AD regarding network and domein we switch to the wider network settings and get to DNS settings and DHCP settings, both are done over the Server Manager. This Domain Controller we created will handle DNS requests for all connected devices, including workstations and other networked systems, and DHCP component will allow the Domain Controller to assign IP addresses to its connected workstations, servers, and devices.
Space and storage can be found on README.


• Installation steps: Mount the Windows Server 2022/2025 ISO → boot the VM → choose Windows Server (Desktop Experience) → accept license → create new partition on Disk 0 → install Windows → reboot → set Administrator password → log in → configure static IP (IP: 10.0.0.5 | Subnet: 255.255.255.0 | Gateway: 10.0.0.1 | DNS: 10.0.0.5) → open Server Manager → Add Roles and Features → install Active Directory Domain Services, DNS Server, DHCP Server, File Services, and IIS → after installation click “Promote this server to a domain controller” → choose “Add a new forest” → set domain name (corp.project-x-dc.com) → set DSRM password → complete configuration and reboot → open DNS Manager and add forwarder (8.8.8.8) → open DHCP Manager → create new IPv4 scope (range 10.0.0.100–10.0.0.200, subnet 255.255.255.0, router 10.0.0.1)

All of this will allow us at the end is to add Users to our AD, in our case, those will be Linux and Windows clients on our network.

• Issues encountered: no big ones that could not be resolved with little troubleshooting, but after the final setup of the Lab and on the steps when adding Clients to AD, I often had an issue with DNS. After hours spent trying with reconnecting the Clients and other advice found on the web, the best solution was restarting the DNS and Client side, oh the misery :D.  


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




