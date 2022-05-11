# Sharing an SSH Agent between a host machine and a Docker container


### SSH agent
https://medium.com/@vanuan/ssh-and-docker-compose-7bce10b67765

You can see its location on your system:

```
echo $SSH_AUTH_SOCK
# /run/user/1000/keyring-AvTfL3/ssh
```

SSH client communicates with SSH agent through this file so that you’d enter passphrase only once. Once it’s unencrypted, SSH agent will store it in memory and send to SSH client on request. Can we use that in Docker? Sure, just mount that special file and specify a corresponding environment variable:

```
environment:
  SSH_AUTH_SOCK: $SSH_AUTH_SOCK
  ...
volumes:
  - $SSH_AUTH_SOCK:$SSH_AUTH_SOCK

```
We don’t even need to copy keys in this case. To confirm that keys are available we can use ssh-add utility:

```
if [ -z "$SSH_AUTH_SOCK" ]; then
  echo "No ssh agent detected"
else
  echo $SSH_AUTH_SOCK
  ssh-add -l
fi
```


### Sharing the SSH Agent
https://www.jamesridgway.co.uk/sharing-an-ssh-agent-between-a-host-machine-and-a-docker-container/

To shared the SSH agent between your host machine and your docker container all you need to do is set an environment variable and a volume mount in your docker setup.

Here is an example of what this will look like with a fictitious docker-compose.yml:

```
version: '3'

services:
  app:
    container_name: yourcontainer
    environment:
      - SSH_AUTH_SOCK=/ssh-agent
    image: yourapp
    volumes:
      - ${SSH_AUTH_SOCK}:/ssh-agent
```
In the container, we're setting the environment variable SSH_AUTH_SOCK to the path /ssh-agent. We're then mounting the host's SSH agent socket path onto /ssh-agent within the container.

With this in place, you will now be able to run any SSH command from within your docker container as if you were running the command from the host machine.
