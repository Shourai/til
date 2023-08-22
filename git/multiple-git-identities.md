# Setup multiple git identities & git user informations
https://gist.github.com/bgauduch/06a8c4ec2fec8fef6354afe94358c89e

> /!\ Be very carrefull in your setup : **any misconfiguration make all the git config to fail silently !**
Go trought this guide step by step and it should be fine :wink:

## Setup multiple git ssh identities for git
* Generate your SSH keys as per your git provider documentation.
* Add each public SSH keys to your git providers acounts.
* In your `~/.ssh/config`, set each ssh key for each repository as in this exemple:

  ``` bash
	Host github.com
		HostName github.com
		User git
		IdentityFile ~/.ssh/github_private_key
		IdentitiesOnly=yes
	Host gitlab.com
		Hostname gitlab.com
		User git
		IdentityFile ~/.ssh/gitlab_private_key
		IdentitiesOnly=yes
  ```

## Setup dynamic git user email & name depending on folder

> /!\ Require git 2.13+ for conditional include support.

The idea here is to use a different git user name & email depending on the folder you are in.

* In your `~/.gitconfig`, **remove** the `[user]` block and add the following (adapt this exemple to your needs) :

  ``` bash
	[includeIf "gitdir:~/code/personal/"]
		path = .gitconfig-personal
	[includeIf "gitdir:~/code/professional/"]
		path = .gitconfig-professional
  ```
* In your `~/.gitconfig-personal`, add your personnal user informations:

  ``` bash
	[user]
		email = user.personal@users.noreply.github.com     # note we use the noreply github mail
		name = personal_username
  ```
* In your `~/.gitconfig-professional`, add your professional user informations:

  ``` bash
	[user]
		email = user.professional@dns.com
		name = professional_username
  ```

## Test your setup
* Now each repository will use the custom user info setup depending on the top-level folder.
* Check your settings are taken into account, for instance in `~/code/personal/` :

  ``` bash
  $ cd ~/code/personal/
  $ git config --get user.email    	# should return user.personal@users.noreply.github.com as per the exemple
  $ git config --get user.name     	# should return personal_username as per the exemple
  $ git config --get user.signingkey	# should return the GPG key ID as configured for the user
  ```
* Do the same for each folder you have setup.
* You can also display and check the global git config: `git config --list --global`
  * Or just the local config for the repository folder you are in: `git config --list`
* Done !


## Update & tip

For your information you must now specify the .git folder in the gitdir such as :

```
[includeIf "gitdir:~/code/personal/repo1/.git"]
```

Another useful tip, you can use globbing on parent directory to detect new repos without editing your git-config file :

```
[includeIf "gitdir:~/code/personal/**/.git"]
```
