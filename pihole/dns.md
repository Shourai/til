# Fix DNS issues of docker containers on the same host

https://github.com/geerlingguy/internet-monitoring/issues/12

Worth a try... note that /etc/resolv.conf is managed by resolvconf, so it's rewritten on reboot.
So I edited the /etc/resolvconf.conf file and uncommented the line name_servers=127.0.0.1, and rebooted.

Hey, look at that! It's working now!

So... it turns out if you're running other Docker containers on the same Pi where you're running pi-hole (or another DNS server), you should edit resolvconf.conf and uncomment that line.
Nice that documentation there is actually helpful :)
