# Removing Sensitive Data from Git History

https://medium.com/@eduardosilva_94960/git-tips-removing-sensitive-data-from-git-history-153f9ce5511c

## The Process: How to Remove Sensitive Data

Here's a step-by-step guide on how to scrub sensitive data from your Git history:

1.  Identify the Sensitive Data: Before taking any action, identify the exact files or commits containing sensitive data.
2.  Backup Your Data: As a precaution, create a backup of your repository. This ensures that even if something goes wrong during the process, your work is safe.

## Remove the data

Git's filter-branch command lets you rewrite the repository history. The following command will remove a specific file containing sensitive data:

git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch path/to/sensitive-file' --prune-empty --tag-name-filter cat -- --all

Let's breaking down:

- git filter-branch: Rewrite the commit history by applying various filters. In the context of removing sensitive data, it's employed to remove files that contain such data from the commit history.
- --- force: This flag is used to forcibly overwrite the existing branch references if necessary.
- --- index-filter 'git rm --- cached --- ignore-unmatch path/to/sensitive-file': This part of the command is the actual filter. It's executed for each commit and removes the specified sensitive file from the index (staging area) using `git rm --cached`. The `--ignore-unmatch` flag ensures that if the file doesn't exist in a particular commit, the command won't fail.
- --- prune-empty: This flag ensures that empty commits (commits that only remove sensitive data and don't change anything else) are removed from the history.
- --- tag-name-filter cat: This filter ensures that tag names are rewritten correctly as part of the history rewriting process.
- --- --- all: This signals the end of options and specifies that all branches and tags should be processed.

Expire Refs and Garbage Collection: Run the following commands to remove any cached references to the old commits:

git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin\
git reflog expire --expire=now --all\
git gc --prune=now

- git for-each-ref: Iterate over all references in your repository, including branches and tags. It's used in combination with other commands to help remove references to the old, rewritten commits.
- --- format="delete %(refname)": This flag specifies the format in which the output should be displayed. Here, it's set to generate a command that deletes the references.
- refs/original: This refers to the original references that were stored before the `git filter-branch` operation.
- git update-ref: Update references (like branches and tags) based on the input provided, which in this case comes from the output of `git for-each-ref`.
- --- stdin: This flag tells git update-ref to read the references to be updated from standard input.
- git reflog expire: Expire command is used to expire (clean up) the reflog, essentially removing references to old commits that are no longer reachable after the filtering process.
- --- expire=now: This flag specifies that the reflog entries should be expired immediately.
- git gc: Optimize and clean up the repository by removing unnecessary objects, including old commits that are no longer reachable due to the history rewriting process.
- --- prune=now: This flag tells Git to perform a more aggressive pruning immediately, removing any unreachable objects.

Force Push: Since you've rewritten history, a force push is necessary to update the remote repository:

git push origin --force --all

Remember that these commands are powerful and can rewrite history in a significant way. Always create a backup of your repository before attempting these operations, as mistakes can lead to data loss. Additionally, using force pushes to update remote branches can have implications for collaborators, so it's important to communicate and coordinate with your team when performing these actions.
