# How to login using root user into docker container

In order to exec using the root user inside the Docker container, we’ll use the –-user option:


```
docker exec -it --user root <container> bash
```

https://www.baeldung.com/ops/root-user-password-docker-container#2-using-the-root-user
