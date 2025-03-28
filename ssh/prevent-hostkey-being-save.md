# Prevent hostkey from being saved

Add a section to ~/.ssh/config

```shell
host 192.168.*.*
    StrictHostkeyChecking no
    UserKnownHostsFile /dev/null
```
