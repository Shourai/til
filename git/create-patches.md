# Creating patches

```
git format-patch -x
```
with `-x` the amount of commits from the current head.

## Print to stdout
To check the patches you could print it to stout first with the `--stdout` flag.

## Create git patch files in a directory
```
git format-patch -x -o <directory>
```

See also
https://devconnected.com/how-to-create-and-apply-git-patch-files/

https://www.ivankristianto.com/create-patch-files-from-multiple-commits-in-git/
