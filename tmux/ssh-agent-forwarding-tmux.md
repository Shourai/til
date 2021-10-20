# SSH agent forwarding with tmux
** With X11 ** support

`~/.ssh/rc`

```
#!/bin/bash

# Fix SSH auth socket location so agent forwarding works with screen.
if test "$SSH_AUTH_SOCK" ; then
    ln -sf $SSH_AUTH_SOCK ~/.ssh/ssh_auth_sock
fi

# Taken from the sshd(8) manpage.
if read proto cookie && [ -n "$DISPLAY" ]; then
        if [ `echo $DISPLAY | cut -c1-10` = 'localhost:' ]; then
                # X11UseLocalhost=yes
                echo add unix:`echo $DISPLAY |
                    cut -c11-` $proto $cookie
        else
                # X11UseLocalhost=no
                echo add $DISPLAY $proto $cookie
        fi | xauth -q -
fi
```

`~/.tmux.conf`

```
set -g update-environment "DISPLAY SSH_ASKPASS SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHORITY"
set-environment -g 'SSH_AUTH_SOCK' ~/.ssh/ssh_auth_sock
```


https://gist.github.com/martijnvermaat/8070533
https://werat.dev/blog/happy-ssh-agent-forwarding/
https://stackoverflow.com/questions/21378569/how-to-auto-update-ssh-agent-environment-variables-when-attaching-to-existing-tm


** Without X11 support **
Using the following config xauth will not be sourced resulting in x11 forwarding not working.

`~/.ssh/rc`
```
#!/bin/bash
if [ -S "$SSH_AUTH_SOCK" ]; then
    ln -sf $SSH_AUTH_SOCK ~/.ssh/ssh_auth_sock
fi
```
Add to `~/.tmux.conf`

```
set -g update-environment "DISPLAY SSH_ASKPASS SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHORITY"
set-environment -g 'SSH_AUTH_SOCK' ~/.ssh/ssh_auth_sock
```

from: https://superuser.com/questions/237822/how-can-i-get-ssh-agent-working-over-ssh-and-in-tmux-on-os-x
