# Mount an external harddrive as local user.

An example of mounting `sdb1` as user with id 1000:

```
sudo mount /dev/sdb1 /mnt/extHDD -o uid=1000
```

To check the uid use
```
id
```

