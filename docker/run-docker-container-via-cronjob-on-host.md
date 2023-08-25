# Run a docker container using cronjob on the host
Sometimes you want to run a docker container on the host at specific times.
E.g. star the container, do whatever it has to do and remove it.
This can be accomplished using cron.

The following can be added to crontab

```
00 10 * * * docker run --rm -v ~/nodescripts:/tmp -w /tmp node /bin/bash -c "node index.js"
```

This will run a docker container with the folder nodescripts mounted to /tmp and calls bash to run `node index.js`

An important note is to not have the `-it` flag otherwise you get "the input device is not a TTY" errors.
