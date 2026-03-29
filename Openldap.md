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




