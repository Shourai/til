# GPG agent forwarding over SSH

### How to configure GPG Agent Forwarding

To find out the name of the local socket:

```
$ gpgconf --list-dirs agent-extra-socket
/run/user/1000/gnupg/S.gpg-agent.extra
```

To find out the name of the remote socket, which differs from the local socket:
```
$ gpgconf --list-dirs agent-socket
/run/user/1000/gnupg/S.gpg-agent
```

Edit the SSH configuration in `~/.ssh/config` on the local machine to forward the socket to the remote machine:

```
Host remote
  RemoteForward <remote socket> <local socket>
```

It is important to note that to work properly GnuPG on the remote system still needs your public keys.
So you have to make sure they are available on the remote system even if your secret keys are not. 
To copy the public key to remote we can use:

```
scp .gnupg/pubring.kbx remote:~/.gnupg/
```

Finally add the following to `/etc/ssh/sshd_config` on the remote server to remove existing socket files when you disconnect.

```
StreamLocalBindUnlink yes
```


### Troubleshooting

Personally I had to add the following to my local `gpg-agent.conf`:
```
extra-socket /run/user/1000/gnupg/S.gpg-agent.extra
```

### References
https://wiki.gnupg.org/AgentForwarding
https://github.com/drduh/YubiKey-Guide#remote-machines-gpg-agent-forwarding
