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


All of this will allow us at the end is to add Users to our AD, in our case, those will be Linux and Windows clients on our network.

• Issues encountered: no big ones that could not be resolved with little troubleshooting, but after the final setup of the Lab and on the steps when adding Clients to AD, I often had an issue with DNS. After hours spent trying with reconnecting the Clients and other advice found on the web, the best solution was restarting the DNS and Client side, oh the misery :D.  



 <br>


**Corporate Server: Ubuntu Server (JumpBox)** + **MailHog**

• Ubuntu Server will act as a JumpBox on our Lab environment, this means it will be set up as a server that will be secure and will provide secure access to the closed environment. Additionally, this server will provide FTP, DNS, and email access. 


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start, same as with other ISO. Once the installation part is completed + the server is connected to NATnetwork, we are on the way to connect this server to AD. All users will connect to the Server over SSH, making it a secure connection type.


Because we will provide FTP, DNS, and email access. For this infrastructure to work on our JumpServer, we will be downloading and installing Docker Engine, which will allow us to run the containers on the Server, in our case we will be using it for a fake SMTP, more about that when we talk about MailHog.


• Issues encountered: No active problems encountered, but DNS, DHCP and AD issues can arise from time to time when in active use.


• MailHog: MailHog is a lightweight email testing tool that acts as a fake SMTP server. It captures all outgoing emails sent by applications without delivering them to real inboxes. You can inspect emails via a web interface (Web UI accessible on port 8025), the SMTP service (default port 1025), or through its API. We will be using MailHog to simulate a business email server, which will be used as part of our phishing exercise in the Cyber Attack module.

This will be the first use of this server, and it will be made possible by using Docker. MailHog provides an official Docker image ready to use from Docker Hub. In our case, we will configure MailHog SMTP on the server and set up an Email Poller Script on our Linux client. All of this will be used in our lab when executing an attack scenario (phishing attack).

![hog](https://github.com/user-attachments/assets/f67197c5-63e8-4275-9626-c9c3eb07aa08)


 <br>

**Security Server: Ubuntu Server 22.04 (Wazuh)**


• Ubuntu Server will act as a Security Server. We opted for this option because we will need a dedicated server for security, and incorporating all in just one JupBox would be a problem for our Lab and VM, a dedicated server with a SIEM is a more stable option.


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start, same as with other ISO. Once the installation part is completed + the server is connected to NATnetwork, we are on the way to connect this server to AD using winbind.


• Wazuh 




