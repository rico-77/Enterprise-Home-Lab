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
