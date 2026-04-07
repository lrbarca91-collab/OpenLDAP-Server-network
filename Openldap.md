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

<img width="516" height="279" alt="2026-04-07_23-51-38" src="https://github.com/user-attachments/assets/fcc676ce-ab6a-4878-b38e-d0ef3f017516" />


I then configured my network, all vm's have linux ubuntu:

internal ip's:

Gateway: 192.168.1.1/24

VM 100 OpenLDAP Server: 192.168.1.0/24

VM 101 Accounting01: 192.168.1.11/24

Vm 102 HR01: 192.168.1.12/24

I configured Openldap on my clients VM101 and VM102:

sudo apt install -y libnss-ldap libpam-ldap ldap-utils nscd


<img width="1310" height="757" alt="2026-03-29_18-55-53" src="https://github.com/user-attachments/assets/62a6db4e-da8c-40b6-a6f4-9cf8f968db78" />

I then created the structure for my users and groups by creating a structure.ldif

<img width="618" height="288" alt="image" src="https://github.com/user-attachments/assets/80c50470-2a5e-463f-8d8e-8fcfb7afd145" />

Thereafter i created a users.ldif with the user lorenzo with a hash generated password:

<img width="690" height="344" alt="image" src="https://github.com/user-attachments/assets/1bb31cf4-121f-4e3e-8671-6434fe4d0ac5" />


Afterwards I configured the sshd config under /etc/ssh/sshd_config to allow me to be able to ssh via powershell into my proxmox and the internal network 
without needing to use the proxmox console.

Thereafter I configured the nssswitch.conf so that the vm's know to check the openldap for users:

<img width="999" height="622" alt="2026-04-07_23-57-59" src="https://github.com/user-attachments/assets/54ec736c-7607-4c1e-9d9a-bc50227b95e8" />

During configuration I wished to test if my user with "getent passwd lorenzo"

This did not return any output so I had to check if my client has connectivity to my openldap server:

# Test anonymous search (should work without password)
ldapsearch -x -H ldap://192.168.1.10 -b "dc=mylab,dc=local" -LLL

# Test authenticated search as admin
ldapsearch -x -H ldap://192.168.1.10 -b "dc=mylab,dc=local" -D "cn=admin,dc=mylab,dc=local" -W -LLL

# Search specifically for the user
ldapsearch -x -H ldap://192.168.1.10 -b "ou=users,dc=mylab,dc=local" "(&(objectClass=posixAccount)(uid=lorenzo))"
<img width="1698" height="519" alt="2026-04-08_00-20-17" src="https://github.com/user-attachments/assets/b4532ade-b1c8-4cde-a00e-0b0d2716b8ff" />

My client passed all the connectivity tests from the above commands.

Due to deprecation of the libnss-ldap and libpam-ldap, I had to purge these and then install nslcd and then reocnfigure the nsswitch.cong:

passwd, group, shadow to files nslcd

<img width="1020" height="569" alt="2026-04-08_00-33-49" src="https://github.com/user-attachments/assets/fd608af7-45f3-4f46-8a00-87033f3eea36" />



