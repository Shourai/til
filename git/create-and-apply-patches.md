# Creating and applying patches

```
git format-patch X
```
with `X` the commit SHA from where to start creating patches.

Create a patch from commit X to commit Y:
```
git format-patch X..Y
```

### Print to stdout
To check the patches you could print it to stout first with the `--stdout` flag.

### Create git patch files in a directory
```
git format-patch X -o <directory>
```

## Applying patches

Apply all patches at once without committing:
```
git apply patches/*
```

Apply as individual commits
```
git am patches/*
```


See also
https://devconnected.com/how-to-create-and-apply-git-patch-files/

https://www.ivankristianto.com/create-patch-files-from-multiple-commits-in-git/
