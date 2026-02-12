# Enterprise-Home-Lab

The lab was fully built, configured, and validated independently, with
additional customization, documentation, and analysis added to demonstrate
practical understanding of enterprise security architecture, attack paths,
and defensive controls.

**Network Topology**
<img width="1566" height="1042" alt="image" src="https://github.com/user-attachments/assets/5019e4e3-80be-4b94-80cc-127fc9357bef" />

**Oracle VirtualBox Manager used as primary VM.**

VirtualBox: NAT Network
 - Name: project-x-nat (NatNetwork)
 - IP Address Range: 10.0.0.0/24
 - Usable Range: 10.0.0.1 – 10.0.0.254
 - DHCP Dynamic Scope: 10.0.0.100 – 10.0.0.200
        ( Safe behind NAT )
