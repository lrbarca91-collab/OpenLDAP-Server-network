I have configured a new Server: Windows Server 2025 with the private ip address 192.168.40.1, on a new VLAN0.40 on my OPNSense LAN on the vmbr1 bridge in my Proxmox Server.

I have now assigned the Ip address and setup DNS on the Windows Server:

<img width="1059" height="790" alt="Bildschirmfoto vom 2026-07-23 18-42-53" src="https://github.com/user-attachments/assets/dd1d0327-9249-4227-ac92-a8228a3795ea" />

I then configured my server in Server manager to a domain controller with a domain name corp.local
and i set the domain functional level and forest functional level to windows server 2016 is the best
maximum functional level as it allows me to run windows server 2016, 2019, 2022, 2025 in the domains while my fores tis 2016.

<img width="600" height="827" alt="Bildschirmfoto vom 2026-07-23 19-47-02" src="https://github.com/user-attachments/assets/90c26214-b42c-498b-af11-3c13f68d7f66" />

<img width="600" height="359" alt="Bildschirmfoto vom 2026-07-23 19-47-26" src="https://github.com/user-attachments/assets/d482ebcb-01c1-4736-ab3d-ae353c4c3bbf" />
