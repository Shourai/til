# How to merge multiple git repositories and keep commit history

### Goals:

1. Merge several separate repos into one.
2. Preserve the complete Git commit history.

```
# Before: two repos
repo_a
    └── dir_a
    └── dir_b
    └── dir_c
repo_b
    └── dir_x
    └── dir_y
    └── dir_z
```

```
# After: new repo X containing both a and b
repo_X
    └── repo_a
        └── dir_a
        └── dir_b
        └── dir_c
    └── repo_b
        └── dir_x
        └── dir_y
        └── dir_z
```

### Overview

This is an overview of the basic steps.

1. Re-arrange each repo so they can merge without conflicts
2. Add each repo as a remote source for the repo you want to merge into
3. Merge the repos with --allow-unrelated-histories

### Prepare each repo for merging

1. make sure you don't have overlapping directories and/or files
   if you do, try to rename them or put them in directory with the repo name.
2. Commit and push the changes of the repo

### Connect the repos

```
# Add a remote for and fetch the old repo while on repo_X
git remote add -f repo_a <repo_a URL>

# Merge the files from old_a/master into new/master while in repo_X
git merge old_a/master
```

https://gfscott.com/blog/merge-git-repos-and-keep-commit-history/
https://hacks.mozilla.org/2022/08/merging-two-github-repositories-without-losing-commit-history/
