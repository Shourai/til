# Add DNS server to Docker

Through docker's configuration file /etc/docker/daemon.json (create it if it doesn't exist):

```
$ cat /etc/docker/daemon.json
{
  "dns": [
    "172.17.0.1",
        "8.8.8.8",
        "8.8.4.4"
  ]
}
```

`172.17.0.1`, i.e. the host's IP address from within docker,

https://stackoverflow.com/a/50001940
