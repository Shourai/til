# Set docker log size

https://docs.docker.com/config/containers/logging/local/

## Usage

To use the `local` driver as the default logging driver, set the `log-driver` and `log-opt` keys to appropriate values in the `daemon.json` file, which is located in `/etc/docker/` on Linux hosts or `C:\ProgramData\docker\config\daemon.json` on Windows Server. For more about configuring Docker using `daemon.json`, see [daemon.json](https://docs.docker.com/reference/cli/dockerd/#daemon-configuration-file).

The following example sets the log driver to `local` and sets the `max-size` option.

```
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "10m"
  }
}
```

Restart Docker for the changes to take effect for newly created containers. Existing containers don't use the new logging configuration automatically.

You can set the logging driver for a specific container by using the `--log-driver` flag to `docker container create` or `docker run`:

```
$ docker run\
      --log-driver local --log-opt max-size=10m\
      alpine echo hello world

```

Note that `local` is a bash reserved keyword, so you may need to quote it in scripts.
