# Update existing images with docker-compose

- Pull latest images:

```
docker-compose pull
```

- Then restart containers:

```
docker-compose up -d --remove-orphans
```

- Optionally, remove obsolete images:

```
docker image prune
```

from: https://stackoverflow.com/questions/49316462/how-to-update-existing-images-with-docker-compose
