# Create a simple fileserver Caddy file with directory index

In this situation we want to show the main 'site' on port 80.
We also want to have two directory indices, on at `site.tld/log/` and the other at `site.tld/config/`.

The folder for the main site with `index.html` is stored under `/usr/share/caddy/main`.
The log and config folder is stored under `/usr/share/caddy/log` and `/usr/share/caddy/config`  with root being `/usr/share/caddy`

```
:80 {
 root * /usr/share/caddy/docs
 file_server browse

 redir /log /log/ 301
 route /log/* {
   root * /usr/share/caddy
 }

 redir /config /config/ 301
 route /config/* {
   root * /usr/share/caddy
 }
}
```
