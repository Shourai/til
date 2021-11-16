# Using dig and host

## Systemd-revolved
To use dig and host with Systemd-revolved we have to specify the dns server address.

## Dig
dig @8.8.8.8 google.com

### reverse lookup
using the `-x` flag
```
dig @8.8.8.8 -x google.com
```

## Host
host google.com 8.8.8.8
