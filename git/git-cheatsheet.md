# Git

## Setup
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"

## Common commands
    git init                        create empty git repo or reinitialize existing repo
    git clone <repo url>            clone repository
    git status                      show working tree status
    git add <filename>              add file to the index
    git add -A                      stage all changes
    git add -A                      stage all changes
    git add .                       stage new files and modifications, without deletions
    git add -u                      stage modifications and deletions, without new files
    git add -i                      interactive mode
    git rm <filename>               remove filename from working tree and index
    git commit -m "msg"             add "msg" as commit message
    git commit --amend              modify recent commit
    git pull                        pull from remote repo
    git push                        push to remote repo
    git branch                      show local branches
    git branch -a                   show remote branches
    git branch -d 'branch_name'     delete branch
    git branch -m <newname>         rename current branch
    git checkout <branch>           switch branch or restore working tree
    git checkout -b <new branch>    create new branch and switch to it
    git log                         show commit log
    git reset                       reset current HEAD to the specified state

## References
https://github.com/git-tips/tips \
https://github.com/tiimgreen/github-cheat-sheet#readme
