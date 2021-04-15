Create `~/.ssh/rc`

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
