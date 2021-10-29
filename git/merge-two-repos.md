# Merge two git repositories and keep history

https://blog.devgenius.io/how-to-merge-two-repositories-on-git-b0ed5e3b4448

Step 1: Clone one of the repositories (say repo1).

```
git clone <https/ssh-link-for-repo1>
```

Step 2: Create another remote in this clone which points to repo2-our second repository.

```
git remote add <your-custom-remote-name> <https/ssh-for-repo2>
```

Now you will have the following remotes

```
origin <repo-1-link> (fetch)
origin <repo-1-link> (push)
your-custom-remote-name <repo-2-link> (fetch)
your-custom-remote-name <repo-2-link> (push)
```

Run : `git remote -v` to check the remotes you have

Step 3: Fetch content from repo2 into this remote

```
git fetch <your-custom-remote-name>
```

Step 4: From remote to a local branch

Note that the content of second repo is in the remote still, which we fetched but can’t yet access. So we copy it onto a new local branch.

```
git checkout -b <new-branch-name> <your-custom-remote-name>/master
```

Check for files if you have to, see if you’ve correctly pulled in the right data.

Step 5: Merge with master.

Notice that the last command had you checkout of the master branch and into a new-branch. So, first checkout from that to master with `git checkout master`

then run: `git merge <new-branch-name>`

Now it may return this error

Fatal: refusing to merge unrelated histories

Since we are trying to merge two separate repositories, git checks for co-relation between merge and commits histories and blocks the merge process.

You can surpass this with the following flag with the last command

```
git merge <new-branch-name> --allow-unrelated-histories
```
