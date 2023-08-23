# Increase login attempts

Usually you get 3 login attempts before having to wait 10 seconds.
Every login you get wrong after that increases the time you have to wait.
To increase the amount of login attempts before that happens, edit `/etc/security/faillock.conf`

```
# Deny access if the number of consecutive authentication failures
# for this user during the recent interval exceeds n tries.
# The default is 3.
deny = 10
```

see also:
```
man pam_faillock
```
