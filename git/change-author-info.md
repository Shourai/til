# Changing author info

Before running this script, you'll need:

- The old email address that appears in the author/committer fields that you want to change
- The correct name and email address that you would like such commits to be attributed to

1. Open Terminal.

2. Create a fresh, bare clone of your repository:

```
git clone --bare https://github.com/user/repo.git
cd repo.git
```
3. Copy and paste the script, replacing the following variables based on the information you gathered:

- `OLD_EMAIL`
- `CORRECT_NAME`
- `CORRECT_EMAIL`

```
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_COMMITTER_NAME="$CORRECT_NAME"
export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_AUTHOR_NAME="$CORRECT_NAME"
export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

4. Press Enter to run the script.

5. Review the new Git history for errors.

6. Push the corrected history to GitHub:

```
 git push --force --tags origin 'refs/heads/*'
```

7. Clean up the temporary clone:

```
cd ..
rm -rf repo.git
```

https://help.github.com/en/github/using-git/changing-author-info \
https://stackoverflow.com/questions/750172/how-to-change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-gi/4903673
