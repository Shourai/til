* Setup gnome keyring

** On Arch
https://wiki.archlinux.org/title/GNOME/Keyring

- Install `gnome-keyring` and `seahorse`.

- Reboot, a default keyring called `Login` should be created.
- Change the password for this keyring, make sure the password of the default keyring is the same as the one used to login.
if unable to change the password check if `journalctl --this-boot --no-pager | grep -i WARNING` shows that 'org.gnome.keyring.SystemPrompter' failed.
If that is the case try `source /etc/X11/xinit/xinitrc.d/50-systemd-user.sh` and try again.
- Complete the PAM and xinitrc step

```
/etc/pam.d/login
auth       optional     pam_gnome_keyring.so
session    optional     pam_gnome_keyring.so auto_start
```

```
~/.xinitrc

eval $(gnome-keyring-daemon --start)
export SSH_AUTH_SOCK
```

source:
https://unix.stackexchange.com/questions/265503/how-do-i-fix-no-such-secret-collection-at-path-for-gnome-keyring-and-arch-l
