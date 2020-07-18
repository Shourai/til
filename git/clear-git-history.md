# Clear Git History

Create a temporary branch and checkout:

```
$ git checkout --orphan temp_branch
```

|Option |	Description |
|-------|-------------|
|--orphan  |	Create a branch in a git init-like state |

Add all files to the temporary branch and commit the changes:

```
$ git add -A
$ git commit -am "The first commit"
```

Delete the master branch:

```
$ git branch -D master
```

Rename the temporary branch to master:

```
$ git branch -m master
```

Forcefully update the remote repository:

```
$ git push -f origin master
```

see also https://www.shellhacks.com/git-remove-all-commits-clear-git-history-local-remote/
