# Test if DHCP is working
To check if a DHCP server exists and is working in your network we can use `nmap`.

nmap does this easily:

```
sudo nmap --script broadcast-dhcp-discover -e eth0
```

will show:

```
Starting Nmap 6.40 ( http://nmap.org ) at 2016-08-16 09:25 UTC
Pre-scan script results:
| broadcast-dhcp-discover: 
|   IP Offered: 192.168.0.67
|   DHCP Message Type: DHCPOFFER
|   Server Identifier: 192.168.0.1
|   IP Address Lease Time: 0 days, 0:05:00
|   Subnet Mask: 255.255.255.0
|   Router: 192.168.0.1
|   Domain Name Server: 8.8.8.8
|   Domain Name: maas
|   Broadcast Address: 192.168.0.255
|_  NTP Servers: 91.189.91.157, 91.189.89.199, 91.189.94.4, 91.189.89.198
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.27 seconds
```

Note: there is a similar script for dhcpv6

```
sudo nmap --script broadcast-dhcp6-discover -e eth0
```

In some distros like alpine you have to install nmap scripts
`apk add nmap-scripts`

from: https://superuser.com/questions/750359/check-if-a-dhcp-server-existing-in-my-network-using-bash/1114078#1114078
