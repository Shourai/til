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
