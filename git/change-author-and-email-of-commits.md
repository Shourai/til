# Change the author name and email for multiple commits

NOTE: This answer changes SHA1s, so take care when using it on a branch that has already been pushed.
If you only want to fix the spelling of a name or update an old email, Git lets you do this without rewriting history using .mailmap. See my other answer.

## Using Rebase
First, if you haven't already done so, you will likely want to fix your name in git-config:

```
git config --global user.name "New Author Name"
git config --global user.email "<email@address.example>"
This is optional, but it will also make sure to reset the committer name, too, assuming that's what you need.
```

To rewrite metadata for a range of commits using a rebase, do

```
git rebase -r <some commit before all of your bad commits> \
    --exec 'git commit --amend --no-edit --reset-author'
```

`--exec` will run the git commit step after each commit is rewritten (as if you ran `git commit && git rebase --continue` repeatedly).

If you also want to change your first commit (also called the 'root' commit), you will have to add `--root` to the rebase call.

This will change both the committer and the author to your `user.name/user.email` configuration.
If you did not want to change that config, you can use `--author "New Author Name <email@address.example>"` instead of `--reset-author`.
Note that doing so will not update the committer -- just the author.

## Single Commit
If you just want to change the most recent commit, a rebase is not necessary. Just amend the commit:

```
git commit --amend --no-edit --reset-author
```

source: https://stackoverflow.com/questions/750172/how-do-i-change-the-author-and-committer-name-email-for-multiple-commits
