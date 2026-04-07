I researched the steps needed to create my OpenLDAP server on a Proxmox Server from OVH.

I used a Ubuntu 24.03.3 iso for the OpenLDAP server

wI applied the following setting in my OpenLDAP installation:

sudo apt update -y

As the configuration did not start automatically:

sudo dpkg-reconfigure slapd

Omit OpenLDAP server configuration? No
DNS domain namemylab.local
Organization name
MyLab
Remove the database when slapd is purged?No
Move old database?Yes

Checked services:
sudo systemctl status slapd
sudo systemctl enable slapd
<img width="879" height="558" alt="image" src="https://github.com/user-attachments/assets/90a4ce5b-ef3e-4ed2-9bf9-aea0648faf3e" />

ldif file created for the structure of my groups and users
structure.ldif:

<img width="1101" height="264" alt="image" src="https://github.com/user-attachments/assets/db284ed7-2b8c-499d-8e78-e915615cccb1" />


<img width="429" height="230" alt="2026-03-29_18-56-43" src="https://github.com/user-attachments/assets/07948e88-3abf-43b9-b53e-654c74de90e2" />




<img width="1310" height="757" alt="2026-03-29_18-55-53" src="https://github.com/user-attachments/assets/91e9429f-2bce-4888-b688-246f0305663d" />




<img width="764" height="624" alt="2026-03-28_19-27-53" src="https://github.com/user-attachments/assets/9a16f5ce-c4c5-4798-849f-91af540ddc2c" />



<img width="951" height="404" alt="2026-03-28_20-08-27" src="https://github.com/user-attachments/assets/ef8d9668-e2c6-45d5-958e-0206ce15b82c" />
