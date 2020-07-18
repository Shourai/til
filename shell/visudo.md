# Run sudo commands without passwords

## For specific commands
Open up visudo and add

```
username ALL=(ALL) NOPASSWD:/sbin/reboot, /sbin/shutdown
```
Here we added reboot and shutdown to the list of commands where you don't need a password to run.
You do still need to type `sudo reboot`.

## For all commands

```
username ALL=(ALL) NOPASSWD:ALL
```

## Open visudo with specific editor

```
sudo EDITOR=vim visudo
```

## Note
The visudo command will overwrite rules if it applies to the same user/group.
For example if you have a line that allows users in the group `wheel` to run sudo commands
and you have an user in that group where you want to run commands with sudo without passwords.
Add the user after the group.

```
%group ALL=(ALL) ALL
username ALL=(ALL) NOPASSWD: /sbin/reboot
```
Here the user in `group` can run all sudo commands _with_ password and run `reboot` without password.
