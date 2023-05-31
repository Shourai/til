# Bash git prompt

Adding git status to Bash.

```
source /etc/bash_completion.d/git-prompt
PS1='\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]$(GIT_PS1_SHOWUNTRACKEDFILES=1 GIT_PS1_SHOWDIRTYSTATE=1 __git_ps1)\$ '
```

see also: https://code.mendhak.com/simple-bash-prompt-for-developers-ps1-git/
