# Ignore ssh config file

You can pass the -F option to the ssh command that specifies an alternative
per-user configuration file instead of the default ~/.ssh/config file.
Unfortunately there is no ignore option. You need to get a little creative and
pass the /dev/null as a config file which tells ssh to ignore the default ssh
config file named ~/.ssh/config. For example:

```
ssh -F /dev/null user@server-name
```

OR some people use none as file:
```
ssh -F none user@server-name
```

The global /etc/ssh/ssh_config will be ignored if a configuration file is given on the command line.
The default for the per-user configuration file is ~/.ssh/config, and it will be ignored too.

https://www.cyberciti.biz/faq/tell-ssh-to-exclude-ignore-config-file/
