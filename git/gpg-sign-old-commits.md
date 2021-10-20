# Use GPG to sign old commits

To rebase everything in the development branch and sign them.
```
git rebase --exec 'git commit --amend --no-edit -n -S' -i development
```

sign all commits:
```
git rebase --exec 'git commit --amend --no-edit -n -S' -i --root
```

https://superuser.com/questions/397149/can-you-gpg-sign-old-commits
