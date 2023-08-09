# Environmental Variables in cron

On debian you can define enviromental variables in cron as follows:
```
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin
TOKEN=123456abcdef

0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
```
