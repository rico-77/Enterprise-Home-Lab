# Enterprise-Home-Lab

The lab was fully built, configured, and validated independently, with
additional customization, documentation, and analysis added to demonstrate
practical understanding of enterprise security architecture, attack paths,
and defensive controls.


**Network Topology**
<img width="1566" height="1042" alt="image" src="https://github.com/user-attachments/assets/5019e4e3-80be-4b94-80cc-127fc9357bef" />

**Oracle VirtualBox Manager used as primary VM.**

🌐 VirtualBox: NAT Network
 - Name: project-x-nat (NatNetwork)
 - IP Address Range: 10.0.0.0/24
 - Usable Range: 10.0.0.1 – 10.0.0.254
 - DHCP Dynamic Scope: 10.0.0.100 – 10.0.0.200

     ( Safe behind NAT )



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




⬇️ Instalation of VM's

Download the ISO files directly from the provider. Depending on when you take this program, there could be changes to the UI. The fundamentals will stay the same.
You can download the ISO Files Using the Links:
💡 For the Microsoft ISOs, you will have to enter in contact details, you can add dummy information.

Windows Server 2025: https://info.microsoft.com/ww-landing-evaluate-windows-server-2025.html?lcid=en-us&culture=en-us&country=us

Windows 11 Enterprise: https://info.microsoft.com/ww-landing-windows-11-enterprise.html

Ubuntu Desktop 22.04: https://releases.ubuntu.com/jammy/

Ubuntu Server 22.04: https://releases.ubuntu.com/jammy/

Security Onion: https://securityonionsolutions.com/software

Kali Linux 2024.4: https://www.kali.org/get-kali/#kali-installer-images



