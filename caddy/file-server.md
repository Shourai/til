# Create a simple fileserver Caddy file

In this situation we want to show the main 'site' on port 80.
We also want to have two directory indices, on at `site.tld/log/` and the other at `site.tld/config/`.

The folder for the main site with `index.html` is stored under `/usr/share/caddy/main`.
The log and config folder is stored under `/usr/share/caddy/log` and `/usr/share/caddy/config`  with root being `/usr/share/caddy`

```
:80 {
 root * /usr/share/caddy/docs
 file_server browse

 rewrite /log /log/
 route /log/* {
   root * /usr/share/caddy
 }

 rewrite /config /config/
 route /config/* {
   root * /usr/share/caddy
 }
}
```
