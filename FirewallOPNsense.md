Infrastruture Background:
After having configured my network on Proxmox with a Openldap server with a Apacheldap GUI, I decided 
it was neccessary to tighten up my security, as manually typing ip addresses in my host.allow config 
on the proxmox server, was repetitive and time consuming especially as my internet ip addresses change 
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

I assigned the interfaces on OPNsense:

vtnet1 to vmbr1 via tap103i1

vtnet0 to vmbr0 via tap103i0

I changed all the vms accept OPNsense to virtual switch vmbr1

<img width="1306" height="436" alt="image" src="https://github.com/user-attachments/assets/d8a13608-1222-4d0c-8653-76cc78d041b4" />

<img width="829" height="181" alt="2026-07-15_15-05-21" src="https://github.com/user-attachments/assets/c7ffbec5-6edd-4766-8076-a8cf7e66583a" />

I decided to tes tthe connectivity of my bridge from the clients to the Opnsense, i temporarily added a ip address to my Openldap server, and then i pinged it from OPNsense.

<img width="606" height="250" alt="image" src="https://github.com/user-attachments/assets/e1f65784-2c20-4909-bf43-0dfb0a4c6cdb" />


<img width="759" height="309" alt="2026-07-16_14-09-13" src="https://github.com/user-attachments/assets/f097ebf5-e331-44b4-9306-ba5d28a1a6c6" />

Later on I had added more to my infrastructure including a DC Controller with Windows Server 2025 and I also added DNS services to the server.

I was not able to successfully configure my Infrastructure so that my only OPnsense would be reachable via the public ip 144.217.X.X as it would always be reachable via Bridge vmbr0, regardless
of how I configured my OPnsense, and I tried to shift the ip address to OPnsense but I was unsuccessful.

I decided instead to buy an additonal IP address from OVH 15.235.X.X this i assigned to Opnsense under vmbr0.

I then edited the interfaces config on my proxmox node:


vmbr0:
    manual
    VLAN-aware
    bridge-vids 2-4094
    physical interfaces eno1/eno2



removing the public ip 144.217.X.X


After I assinged my Opnsense WAN to 15.235.X.X

So that my architecture would look as follows:



Thereafter once Opnsense was available on the public ip in the web browser I decided that I needed to tighten the security, so i began to do further ocnfigurations using wireguard.





