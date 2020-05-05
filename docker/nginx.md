# Setting up a nginx directory listing with docker compose

docker-compose.yaml

```yaml
version: '3'
services:
  nginx:
    image: nginx:latest
    container_name: production_nginx
    environment:
      - TZ=Europe/Amsterdam
    volumes:
      - ./conf/nginx.conf:/etc/nginx/nginx.conf
      - ../log:/log
      - ../config:/config
    ports:
      - 80:80
      - 443:44
```

nginx.conf
```sh
events {}

http {
	include /etc/nginx/mime.types;

  server {
    server_name your.server.url;

    location / {
    root   /usr/share/nginx/html;
    index  index.html;
    }

    location /log {
    alias /log;
	rewrite ^/logs/?$ http://address/log;
	autoindex on;
 	autoindex_localtime on;
    charset UTF-8;
    }

    location /config {
    alias /config;
	rewrite ^/configs/?$ http://address/config;
	autoindex on;
 	autoindex_localtime on;
    charset UTF-8;
    }
  }
}
```
