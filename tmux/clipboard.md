# Clipboard conditional to OS

For linux

```
bind-key -T copy-mode-vi y send-keys -X copy-pipe 'wl-copy'
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe "wl-copy"
```

Check if the OS is MacOS

```
if-shell 'uname | grep -q Darwin' \
'bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe "pbcopy"'

if-shell 'uname | grep -q Darwin' \
'bind-key -T copy-mode-vi y send-keys -X copy-pipe "pbcopy"'
```

<https://superuser.com/questions/539595/tmux-configuration-conditional-to-os>
