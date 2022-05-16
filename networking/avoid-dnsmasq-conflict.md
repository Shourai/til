# Avoid conflict between dnsmasq and systemd-resolved

https://jonamiki.com/2020/01/29/dnsmasq-failed-to-create-listening-socket-for-port-53-address-already-in-use/
https://medium.com/@niktrix/getting-rid-of-systemd-resolved-consuming-port-53-605f0234f32f

```
dnsmasq: failed to create listening socket for port 53: Address already in use
```

Ubuntu 19:10: systemd-resolved blocks port 53 and thereby preventing any service using port 53 (like dnsmasq) from starting

```
 Jan 29 03:31:58 ubuntupxe02 dnsmasq[2386]: dnsmasq: failed to create listening socket for port 53: Address already in use
 Jan 29 03:31:58 ubuntupxe02 dnsmasq[2386]: failed to create listening socket for port 53: Address already in use
 Jan 29 03:31:58 ubuntupxe02 dnsmasq[2386]: FAILED to start up
 Jan 29 03:31:58 ubuntupxe02 systemd[1]: dnsmasq.service: Control process exited, code=exited, status=2/INVALIDARGUMENT
 Jan 29 03:31:58 ubuntupxe02 systemd[1]: dnsmasq.service: Failed with result 'exit-code'.
 Jan 29 03:31:58 ubuntupxe02 systemd[1]: Failed to start dnsmasq - A lightweight DHCP and caching DNS server.
Verify that the port is used by systemd-resolve
```

```
jonas@ubuntupxe02:~$ sudo lsof -i -P -n | grep LIST
 systemd-r  784 systemd-resolve   13u  IPv4  19378      0t0  TCP 127.0.0.53:53 (LISTEN)
 sshd       859            root    3u  IPv4  23918      0t0  TCP *:22 (LISTEN)
 sshd       859            root    4u  IPv6  23920      0t0  TCP *:22 (LISTEN)
 apache2   1705            root    4u  IPv6  27900      0t0  TCP *:80 (LISTEN)
 apache2   1706        www-data    4u  IPv6  27900      0t0  TCP *:80 (LISTEN)
 apache2   1707        www-data    4u  IPv6  27900      0t0  TCP *:80 (LISTEN)
```
Stop the service

```
jonas@ubuntupxe02:~$ sudo systemctl stop systemd-resolved
```
Edit the systemd-resolved config file

```
 jonas@ubuntupxe02:~$ sudo vi /etc/systemd/resolved.conf
 jonas@ubuntupxe02:~$ cat !$ | grep DNS
 cat /etc/systemd/resolved.conf | grep DNS
 DNS=8.8.8.8
 FallbackDNS=
 MulticastDNS=no
 DNSSEC=no
 DNSOverTLS=no
 DNSStubListener=no
 jonas@ubuntupxe02:~$
```
Create symlink to /etc/resolv.conf

```
jonas@ubuntupxe02:~$ sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```
Start systemd-resolved service

```
jonas@ubuntupxe02:~$ sudo systemctl start systemd-resolved
```
Start dnsmasq

```
jonas@ubuntupxe02:~$ sudo systemctl start dnsmasq
jonas@ubuntupxe02:~$
jonas@ubuntupxe02:~$ sudo systemctl status dnsmasq
 ● dnsmasq.service - dnsmasq - A lightweight DHCP and caching DNS server
    Loaded: loaded (/lib/systemd/system/dnsmasq.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2020-01-29 03:56:12 UTC; 6s ago
   Process: 1312 ExecStartPre=/usr/sbin/dnsmasq --test (code=exited, status=0/SUCCESS)
   Process: 1314 ExecStart=/etc/init.d/dnsmasq systemd-exec (code=exited, status=0/SUCCESS)
```
Verify that dnsmasq is now the user of port 53

```
jonas@ubuntupxe02:~$ sudo lsof -i -P -n | grep LISTEN
 sshd     823     root    3u  IPv4  23016      0t0  TCP *:22 (LISTEN)
 sshd     823     root    4u  IPv6  23018      0t0  TCP *:22 (LISTEN)
 apache2  874     root    4u  IPv6  22454      0t0  TCP *:80 (LISTEN)
 apache2  875 www-data    4u  IPv6  22454      0t0  TCP *:80 (LISTEN)
 apache2  876 www-data    4u  IPv6  22454      0t0  TCP *:80 (LISTEN)
 dnsmasq 1331  dnsmasq    5u  IPv4  28097      0t0  TCP *:53 (LISTEN)
 dnsmasq 1331  dnsmasq    7u  IPv6  28099      0t0  TCP *:53 (LISTEN)
```
