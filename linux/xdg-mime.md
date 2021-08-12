# Set defaults for extensions

### browser
Check what program is being used as default
```
xdg-mime query default text/html
```

Check which alternatives are available:
```
ls /usr/share/applications/
```

Change browser default using:
```
xdg-settings set default-web-browser firefox.desktop
```
