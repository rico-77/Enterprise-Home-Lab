🏗️ **Enterprise-Home-Lab Setup Overview**

 <br>
 

🖥️ **Virtual Machines**

•💻 Platform: VirtualBox

 <br>
 

🖧 **Servers**

•🏢 AD Server: Windows Server 2025

•🏢 Corporate Server: Ubuntu Server 22.04

•🛡️ Security Server: Ubuntu Server 22.04

 <br>
 

🖥️ **Workstations**

•💻 Windows 11 Workstation

•🐧 Ubuntu Desktop 22.04

•🛡️ Security Onion

 <br>
 

🛠️ **Tools**

•📧 MailHog Email Server

•📊 Wazuh SIEM Setup

 <br>
 

⚔️ **Security Exercises**

•🔓 Configure Vulnerable Environment

•🕵️ Cyber Attack Simulation

----

🖥️ **Virtual Machines**

•💻 Platform: VirtualBox

The best option is to download from the original page (https://www.virtualbox.org/) 

![vox](https://github.com/user-attachments/assets/6e421919-5d08-47d2-9990-af893e1d74e9)



 <br>
 

There are different Network settings available for VirtualBox,for this project, we will be using the NAT Network setting, this will allow us to have the VMs communicate with one another, while also having access to the Internet.

 <br>
 
Navigate to File ➔ Tools ➔ Network Manager.
 
<img width="458" height="221" alt="image" src="https://github.com/user-attachments/assets/de4c0697-5801-4e41-8eef-89d73243f445" />

 <br>
 

Select NAT Networks ➔ “Create”.

<img width="318" height="111" alt="image" src="https://github.com/user-attachments/assets/3179ca86-4652-4cfc-b98b-6e2294ef8d72" />


At the bottom, name the NatNetwork “project-x-network” and choose an IPv4 prefix (10.0.0.0/24)

