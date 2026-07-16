Step 1: Assign IPs via Option 2
You need to make sure both interfaces have their correct IP addresses.

Type 2 and press Enter (Set interface IP address).

Select the number for WAN (vtnet0).

When asked, configure it as follows:

Configure IPv4 via DHCP? Type n (No).

Enter the new IPv4 address: 192.168.1.100

Enter the new IPv4 subnet bit count: 24

Enter the IPv4 gateway address: 192.168.1.1 (This is Proxmox's IP on vmbr0).

Configure IPv6 via DHCP6/6to4/etc? Press Enter to skip/select none.

Do you want to enable the DHCP server on WAN? Type n (No).

Now, repeat the process by choosing Option 2 again, but select LAN (vtnet1):

Enter the new IPv4 address: 192.168.2.1

Enter the new IPv4 subnet bit count: 24

Enter the IPv4 gateway address: Press Enter (LAN does not get an upstream gateway).

Do you want to enable the DHCP server on LAN? Type y (Yes).

Enter the start address: 192.168.2.10

Enter the end address: 192.168.2.200

Step 2: Disable the Packet Filter (Crucial!)
Because OPNsense blocks all administrative access on the WAN interface by default, you will be locked out of the GUI from the Proxmox side until 
you temporarily disable the firewall.

In the main menu, type 8 and press Enter to open the Shell.

Run the following command:

Bash
pfctl -d

You will see the console output: pf disabled. ---

Step 3: Run a quick test from the CLI

While still in the shell, verify that OPNsense can talk to the Proxmox gateway:

Bash
ping -c 3 192.168.1.1
If the ping succeeds, your virtual switch routing is healthy!

Type exit to return to the main menu.

Step 4: Access the GUI & Complete the Setup
Now, go to your web browser and navigate to:

https://YOUR_PROXMOX_PUBLIC_IP:8443

Log in with:

Username: root

Password: opnsense

Once you are in, the setup wizard will launch. Follow the steps to set up your hostname, DNS, and most importantly, remember to uncheck
"Block private networks" on the WAN configuration page so you don't lose access when you re-enable the firewall!
