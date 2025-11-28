# Docker daemon.json useful options

The docker daemon resides at `/etc/docker/daemon.json`

```json
{
  "dns": ["8.8.8.8", "1.1.1.1"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```
