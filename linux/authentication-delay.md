# Disable authentication delay

add `nodelay` to the the line containing `pam_unix.so` and `pam_faillock.so` in

```
/etc/pam.d/system-auth
```

e.g.

```
auth       required                    pam_faillock.so      preauth nodelay


auth       [success=1 default=bad]     pam_unix.so          try_first_pass nullok nodelay
auth       [default=die]               pam_faillock.so      authfail nodelay
```

see also:

```
man pam_unix
```
