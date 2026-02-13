# Enterprise-Home-Lab

Independently deployed a full enterprise homelab environment based on industry‑aligned architecture. Configured and connected multiple virtual machines, including Windows Domain Controller, Windows 11 workstation, Linux systems, and attacker hosts on a custom network. Conducted vulnerability setups and attack simulations to evaluate security gaps, followed by defensive verification and documentation. This project demonstrates practical competence in enterprise networking, Active Directory, attack vectors, and security controls through real‑world hands‑on implementation and analysis.


**Network Topology**
<img width="1566" height="1042" alt="image" src="https://github.com/user-attachments/assets/5019e4e3-80be-4b94-80cc-127fc9357bef" />

**Oracle VirtualBox Manager used as primary VM.**

🌐 VirtualBox: NAT Network
 - Name: project-x-nat (NatNetwork)
 - IP Address Range: 10.0.0.0/24
 - Usable Range: 10.0.0.1 – 10.0.0.254
 - DHCP Dynamic Scope: 10.0.0.100 – 10.0.0.200

     ( Safe behind NAT )

---

🖥️ VM Setup
Virtual Machines Deployed:
| VM Name               | Operating System        | Specs           | Storage (minimum) |
|-----------------------|-----------------------|----------------|-----------------|
| project-x-dc          | Windows Server 2025    | 2 CPU / 4096 MB | 50 GB           |
| project-x-win-client  | Windows 11 Enterprise  | 2 CPU / 4096 MB | 80 GB           |
| project-x-linux-client| Ubuntu 22.04 Desktop   | 1 CPU / 2048 MB | 80 GB           |
| project-x-sec-work    | Security Onion         | 1 CPU / 2048 MB | 55 GB           |
| project-x-sec-box     | Ubuntu 22.04 Desktop   | 2 CPU / 4096 MB | 80 GB           |
| project-x-corp-svr    | Ubuntu Server 22.04    | 1 CPU / 2048 MB | 25 GB           |
| project-x-attacker    | Kali Linux 2024.2      | 1 CPU / 2048 MB | 55 GB           |

 - Minimum 18G DDR5 to run all machines.


---

⬇️ **Instalation of VM's**

Download the ISO files directly from the provider.
You can download the ISO Files Using the Links:
💡 For the Microsoft ISOs, you will have to enter in contact details, you can add dummy information.

Windows Server 2025: https://info.microsoft.com/ww-landing-evaluate-windows-server-2025.html?lcid=en-us&culture=en-us&country=us

Windows 11 Enterprise: https://info.microsoft.com/ww-landing-windows-11-enterprise.html

Ubuntu Desktop 22.04: https://releases.ubuntu.com/jammy/

Ubuntu Server 22.04: https://releases.ubuntu.com/jammy/

Security Onion: https://securityonionsolutions.com/software

Kali Linux 2024.4: https://www.kali.org/get-kali/#kali-installer-images

---

🛠**Tools**

**Enterprise Tools + Defense**

🖥**Microsoft Active Director**y: A directory service used for managing and organizing network resources, users, and permissions in a Windows environment. ( Set on Windows Server /project-x-dc )

🛡 **Wazuh**: An open-source security monitoring platform that provides intrusion detection, log analysis, vulnerability detection, and compliance reporting. ( Set on Windows Client /project-x-win-client )

📧**MailHog**: MailHog is a lightweight email testing tool that acts as a fake SMTP server. It captures all outgoing emails sent by applications, without delivering them to real inboxes. You can inspect emails via a web interface or API. ( Set on Linux /project-x-corp-svr )

🐳 **Docker**: Docker is an open-source platform that lets you package, ship, and run applications inside containers.

🔗 **RDP**: (Remote Desktop Protocol) is a network protocol that lets you remotely access and control another computer over a network.

🔐 **OpenSSH**: OpenSSH is a free, open-source tool that lets you securely connect to and manage another computer over a network.

🗂 **Active Directory (AD)**: Active Directory (AD) is a directory service developed by Microsoft that manages users, computers, and permissions inside a Windows network.

🌐 **DNS / 📡 DHCP**: DNS Converts domain names → IP addresses / DHCP automatically assigns IP addresses and network settings to devices.

🖧 **NAT Network 🔄 NAT**: NAT is the default network mode, where VirtualBox acts like a middleman for internet access / NAT Network is similar to regular NAT but allows multiple VMs to see each other while still keeping them hidden from the rest of your network. 

🖥 **PowerShell**: A cross-platform task automation and configuration management framework, combining a command-line shell and scripting language for Windows


**Offense**

💀**Evil-WinRM**: A powerful Ruby-based Windows Remote Management (WinRM) client used by penetration testers to connect to and interact with Windows systems, often for post exploitation tasks such as command execution and data extraction. ( Kali Linux / project-x-attacker )

🔑 **Hydra**: A fast and flexible password-cracking tool designed to perform brute-force and dictionary-based attacks on various network protocols, including SSH, HTTP, FTP, and more. ( Kali Linux / project-x-attacker )

📜 **SecLists**: A comprehensive collection of penetration testing resources, including wordlists for usernames, passwords, web directories, and other payloads used in reconnaissance and exploitation phases. ( Kali Linux / project-x-attacker )

🌐 **NetExec**: A network exploitation tool that enables remote command execution on target machines through various protocols, assisting in lateral movement and privilege escalation scenarios. ( Kali Linux / project-x-attacker )

💻 **XFreeRDP3**: An open-source implementation of the Remote Desktop Protocol (RDP), enabling penetration testers to connect to and control Windows systems remotely for reconnaissance and post-exploitation purposes. ( Kali Linux / project-x-attacker )

<img width="935" height="1033" alt="image" src="https://github.com/user-attachments/assets/cdb6d019-8513-401c-bd60-ca3ab3fcaf34" />


--This lab was developed following the structure and guidance from a security lab course, with all configurations, customizations, and analyses completed independently.--
