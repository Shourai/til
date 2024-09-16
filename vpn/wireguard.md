# Wireguard

Specific use-case: VPN server
-----------------------------

![](https://wiki.archlinux.org/images/7/77/Merge-arrows-2.svg)**This article or section is a candidate for merging with [#Routing all traffic over WireGuard](https://wiki.archlinux.org/title/WireGuard#Routing_all_traffic_over_WireGuard).**

**Notes:** Same use case. (Discuss in [Talk:WireGuard](https://wiki.archlinux.org/title/Talk:WireGuard))

**Note:** Usage of the terms "server" and "client" were purposefully chosen in this section specifically to help new users/existing OpenVPN users become familiar with the construction of WireGuard's configuration files. WireGuard documentation simply refers to both of these concepts as "peers."

The purpose of this section is to set up a WireGuard "server" and generic "clients" to enable access to the server/network resources through an encrypted and secured tunnel like [OpenVPN](https://wiki.archlinux.org/title/OpenVPN "OpenVPN") and others. The "server" runs on Linux and the "clients" can run on any number of platforms (the WireGuard Project offers apps on both iOS and Android platforms in addition to Linux, Windows and MacOS). See the official project [install link](https://www.wireguard.com/install/) for more.

**Tip:** Instead of using [wireguard-tools](https://archlinux.org/packages/?name=wireguard-tools) for server/client configuration, one may also use [systemd-networkd](https://wiki.archlinux.org/title/WireGuard#systemd-networkd) native WireGuard support.

### Server

![](https://wiki.archlinux.org/images/7/77/Merge-arrows-2.svg)**This article or section is a candidate for merging with [#Site-to-point](https://wiki.archlinux.org/title/WireGuard#Site-to-point).**

**Notes:** Same use case. (Discuss in [Talk:WireGuard](https://wiki.archlinux.org/title/Talk:WireGuard))

On the peer that will act as the "server", first [enable IPv4 forwarding](https://wiki.archlinux.org/title/Internet_sharing#Enable_packet_forwarding "Internet sharing").

If the server has a public IP configured, be sure to:

- Allow UDP traffic on the specified port(s) on which WireGuard will be running (for example allowing traffic on `51820/UDP`).
- Setup the forwarding policy for the firewall if it is not included in the WireGuard configuration for the interface itself `/etc/wireguard/wg0.conf`. The example below should have the iptables rules and work as-is.

If the server is behind NAT, be sure to forward the specified port(s) on which WireGuard will be running (for example, `51820/UDP`) from the router to the WireGuard server.

### Key generation

Generate key pairs for the server and for each client as explained in [#Key generation](https://wiki.archlinux.org/title/WireGuard#Key_generation).

### Server configuration

Create the "server" configuration file:

/etc/wireguard/wg0.conf

```
[Interface]
Address = 10.200.200.1/24
ListenPort = 51820
PrivateKey = *SERVER_PRIVATE_KEY*

# substitute *eth0* in the following lines to match the Internet-facing interface
# the FORWARD rules will always be needed since traffic needs to be forwarded between the WireGuard
# interface and the other interfaces on the server.
# if the server is behind a router and receives traffic via NAT, specify static routing back to the
# 10.200.200.0/24 subnet, the NAT iptables rules are not needed but the FORWARD rules are needed.
# if the server is behind a router and receives traffic via NAT but one cannot specify static routing back to
# 10.200.200.0/24 subnet, both the NAT and FORWARD iptables rules are needed.
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
# foo
PublicKey = *PEER_FOO_PUBLIC_KEY*
PresharedKey = *PRE-SHARED_KEY*
AllowedIPs = 10.200.200.2/32

[Peer]
# bar
PublicKey = *PEER_BAR_PUBLIC_KEY*
PresharedKey = *PRE-SHARED_KEY*
AllowedIPs = 10.200.200.3/32
```

Additional peers ("clients") can be listed in the same format as needed. Each peer requires the `PublicKey` to be set. However, specifying `PresharedKey` is optional.

Notice that the `Address` has a netmask of `/24` and the clients on `AllowedIPs` `/32`. The clients only use their IP and the server only sends back their respective addresses.

The interface can be managed manually using [wg-quick(8)](https://man.archlinux.org/man/wg-quick.8) or using a [systemd](https://wiki.archlinux.org/title/Systemd "Systemd") service managed via [systemctl(1)](https://man.archlinux.org/man/systemctl.1).

The interface may be brought up using `wg-quick up wg0` respectively by [starting](https://wiki.archlinux.org/title/Start "Start") and potentially [enabling](https://wiki.archlinux.org/title/Enable "Enable") the interface via `wg-quick@*interface*.service`, e.g. `wg-quick@wg0.service`. To close the interface use `wg-quick down wg0` respectively [stop](https://wiki.archlinux.org/title/Stop "Stop") `wg-quick@*interface*.service`.

### Client configuration

Create the corresponding "client" configuration file(s):

foo.conf

```
[Interface]
Address = 10.200.200.2/32
PrivateKey = *PEER_FOO_PRIVATE_KEY*
DNS = 10.200.200.1

[Peer]
PublicKey = *SERVER_PUBLICKEY*
PresharedKey = *PRE-SHARED_KEY*
Endpoint = my.ddns.example.com:51820
AllowedIPs = 0.0.0.0/0, ::/0

bar.conf

[Interface]
Address = 10.200.200.3/32
PrivateKey = *PEER_BAR_PRIVATE_KEY*
DNS = 10.200.200.1

[Peer]
PublicKey = *SERVER_PUBLICKEY*
PresharedKey = *PRE-SHARED KEY*
Endpoint = my.ddns.example.com:51820
AllowedIPs = 0.0.0.0/0, ::/0
```

Using the catch-all `AllowedIPs = 0.0.0.0/0, ::/0` will forward all IPv4 (`0.0.0.0/0`) and IPv6 (`::/0`) traffic over the VPN.

**Note:** Users of [NetworkManager](https://wiki.archlinux.org/title/NetworkManager "NetworkManager"), may need to [enable](https://wiki.archlinux.org/title/Enable "Enable") the `NetworkManager-wait-online.service` and users of [systemd-networkd](https://wiki.archlinux.org/title/Systemd-networkd "Systemd-networkd") may need to [enable](https://wiki.archlinux.org/title/Enable "Enable") the `systemd-networkd-wait-online.service` to wait until devices are network-ready before attempting a WireGuard connection.
