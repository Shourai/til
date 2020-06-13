# Run sudo commands over ssh

## using '-S' flag
The -S (stdin) option causes sudo to read the password from the standard input instead of the terminal device.
The password must be followed by a newline character.

The following commands pipes the passwords to the sudo command which takes the password from stdin.

```
ssh user@192.168.100.100 'echo <password> | sudo -S reboot now'
```
