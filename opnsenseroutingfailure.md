A common issue is that after restarting certain services or the entire Proxmox Server the OPNsense webgui is no longer reachable.

In order to resolve the the static route has to be routed via iptables:

iptables -t nat -A PREROUTING -d 144.217.X.X -p tcp --dport 8443 -j DNAT --to-destination 192.168.1.10
0:443

root@nXXXX:~# iptables -t nat -A POSTROUTING -p tcp -d 192.168.XX.XX --dport 443 -j MASQUERADE

root@nsXXXX:~# iptables -A FORWARD -p tcp -d 192.168.XX.XX --dport 443 -j ACCEPT

along with the firewall: 

pfctl -d

restart webGUI:

configctl webgui restart renew

Diagnostic:
fetch -o - https://144.217.X:X:8443


sockstat -4 -l | grep lighttpd

check if port is listening on lighttpd service
