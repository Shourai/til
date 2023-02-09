# Disable authentication delay

add `nodelay` to the the line containing `pam_unix.so` in
```
/etc/pam.d/system-auth
```

also add `nodelay` to
```
/etc/security/faillock.conf
```

see also:
```
man pam_unix
man pam_faillock
```
