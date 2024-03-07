# Set a static IP address with nmtui on Raspberry Pi OS 12 'Bookworm'

https://www.jeffgeerling.com/blog/2024/set-static-ip-address-nmtui-on-raspberry-pi-os-12-bookworm

So setting a static IP via the command line is a little different.

First, get the interface information---you can get a list of all interfaces with `nmcli device status`:

```
$ nmcli device status
DEVICE         TYPE      STATE                   CONNECTION
eth0           ethernet  connected               Wired connection 1
lo             loopback  connected (externally)  lo
wlan0          wifi      disconnected            --

```

In my case, I want to set an IP on `eth0`, the built-in Ethernet.

I can get all the current information about that port with `nmcli device show eth0`, and I can edit the connection using the terminal UI (`nmtui`):

```
$ sudo nmtui edit "Wired connection 1"

```

This brings up a connection editor, where you can highlight "IPv4 CONFIGURATION" and change it from `<Automatic>` to `<Manual>`, and then use `<Show>` to show all the settings.

Go through each setting adding in at least an IPv4 address, Gateway, and DNS Server, for example:

![nmtui edit wired eth0 Ethernet connection for static IP address](https://www.jeffgeerling.com/sites/default/files/images/nmtui-edit-wired-eth0-connection-raspberry-pi.jpg)

Then go down to the bottom and select 'OK'.

This saves the static IP configuration, but doesn't _apply_ it immediately. To apply the changes, you need to restart NetworkManager:

```
$ sudo systemctl restart NetworkManager

```

Then if you run `nmcli device show eth0`, you should see the new IP address (the old one might still be attached to the interface at the same time until you reboot):

```
pi@deskpi1:~ $ nmcli device show eth0
GENERAL.DEVICE:                         eth0
...
IP4.ADDRESS[1]:                         10.0.2.78/24
IP4.ADDRESS[2]:                         10.0.2.90/24

```

You can also configure a static IP entirely via `nmcli` without using the UI; see [this article on nmcli`](https://www.cyberciti.biz/faq/how-to-configure-a-static-ip-address-on-rhel-8/) from Cyberciti.biz.
