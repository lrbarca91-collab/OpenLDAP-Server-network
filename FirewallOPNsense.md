Infrastruture Background:
After having configured my network on Proxmox with a Openldap server with a Apacheldap GUI, I decided 
it was neccessary to tighten up my security, as manually typing ip addresses in my host.allow config 
on the proxmox server, was reptitive and time consuming especially as my internet ip addresses change 
often, and this also creates security vulnerabilities.

I have so far a Proxmox Server with a Openldap server, Apacheldap and vm's one for HR and Accounting.

<img width="368" height="260" alt="2026-07-15_13-39-15" src="https://github.com/user-attachments/assets/57b82d58-b9d4-4f04-9a26-3d5de509c2aa" />

Objectives:

I want to configure my infrastructure so that whenever there is an external request whether via http/https or ssh it will first be verified by my OPNsense firewall and then either allowed or rejected according to the firewalls settings.

My Opnsense will have the static IP address 192.168.1.100/24

It will be also a dhcp Server and give the HR and Accounting vm's addresses dynamically.
Along with creating vlans for the Accountings and HR departments.
It will allow ssh from port 22 from my ip address or user.

Journey:

I have installed OPNsense in my proxmox server with the static ip address 192.168.1.100/24

<img width="920" height="706" alt="2026-07-14_22-36-31" src="https://github.com/user-attachments/assets/59bcc1d5-00fd-4f64-984b-1a21ea000948" />

I have analysed my current infra settings via brctl show and then I drew a diagramm:



<img width="941" height="592" alt="image" src="https://github.com/user-attachments/assets/182fd25f-0aae-4373-8acd-c86b94306577" />


vmbr0 is the default switch that comes with the Proxmox server. I need to shift the Accounting and HR vms to vmbr1 along with the Openldap server, then have only opnsense connected to vmbr0 and vmbr0 to the internet.

For some reason despite being able to log into all clients with lorenzo89 user created in my openldap server, my openldap server has no interfaces and connections.





