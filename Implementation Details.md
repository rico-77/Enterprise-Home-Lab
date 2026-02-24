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


**Ubuntu Server 22.04 (JumpBox)**

• Ubuntu Server will act as a JumpBox on our Lab environment, this means it will be set up as a server that will be secure and will provide secure access to the closed environment. Additionally, this server will provide FTP, DNS, and email access. 


Let's get to the setup and how it was done for this Lab. Because we are using a VM the primary installation was done over ISO that we attached to the VM and set up an on-premises infrastructure that we can fully control, provide minimum system requirements in memory and processing, and we can start, same as with other ISO. Once the installation part is completed + the server is connected to NATnetwork, we are on the way to connect this server to AD. All users will connect to the Server over SSH, making it a secure connection type.


Because we will provide FTP, DNS, and email access. For this infrastructure to work on our JumpServer, we will be downloading and installing Docker Engine that will allow us to run the containers on different machines on the Lab.






