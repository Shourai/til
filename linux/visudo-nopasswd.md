# Allow commands to be run without password (visudo - NOPASSWD)

According to man `sudoers`:
> When multiple entries match for a user, they are applied in order.
> Where there are multiple matches, the last match is used (which is not necessarily the most specific match).

To allow certain commands to be run without password this following can be put in either the `sudoers` file
or in a separate file in `/etc/sudoers.d/00_<username>`

```
<username> ALL=(ALL:ALL) NOPASSWD: /usr/bin/pacman, /usr/bin/reboot, ...
```
