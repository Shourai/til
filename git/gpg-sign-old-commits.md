# GPG sign old commits

To rebase everything in the development branch and sign them.
```
git rebase --exec 'git commit --amend --no-edit -n -S' -i development
```
