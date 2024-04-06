# Allow commands to be run without password (visudo - NOPASSWD)

<https://www.cyberciti.biz/faq/linux-unix-running-sudo-command-without-a-password/>

According to man `sudoers`:
> When multiple entries match for a user, they are applied in order.
> Where there are multiple matches, the last match is used (which is not necessarily the most specific match).

To allow certain commands to be run without password this following can be put in either the `sudoers` file
or in a separate file in `/etc/sudoers.d/00_<username>`

```
<username> ALL=NOPASSWD: /usr/bin/pacman, /usr/bin/reboot, ...
```

## askubuntu post

<https://askubuntu.com/questions/159007/how-do-i-run-specific-sudo-commands-without-a-password>

Use the `NOPASSWD` directive
----------------------------

You can use the `NOPASSWD` directive in your [`/etc/sudoers` file](https://manpages.ubuntu.com/manpages/precise/en/man5/sudoers.5.html).

If your user is called `user` and your host is called `host` you could add these lines to `/etc/sudoers`:

```
user host = (root) NOPASSWD: /sbin/shutdown
user host = (root) NOPASSWD: /sbin/reboot

```

This will allow the user `user` to run the desired commands on `host` without entering a password. All other [`sudo`](https://manpages.ubuntu.com/manpages/precise/en/man8/sudo.8.html)ed commands will still require a password.

The commands specified in the `sudoers` file *must* be fully qualified (i.e. using the absolute path to the command to run) as described in the [`sudoers` man page](https://manpages.ubuntu.com/manpages/precise/en/man5/sudoers.5.html). Providing a relative path is considered a syntax error.

If the command ends with a trailing `/` character and points to a directory, the user will be able to run any command in that directory (but not in any sub-directories therein). In the following example, the user `user` can run any command in the directory `/home/someuser/bin/`:

```
user host = (root) NOPASSWD: /home/someuser/bin/

```

**Note:** Always use the command [`visudo`](https://manpages.ubuntu.com/manpages/precise/en/man8/visudo.8.html) to edit the `sudoers` file to make sure you do not lock yourself out of the system -- just in case you accidentally write something incorrect to the `sudoers` file. `visudo` will save your modified file to a temporary location and will *only* overwrite the real `sudoers` file if the modified file can be parsed without errors.

Using `/etc/sudoers.d` instead of modifying `/etc/sudoers`
----------------------------------------------------------

As an alternative to editing the `/etc/sudoers` file, you could add the two lines to a new file in `/etc/sudoers.d` e.g. `/etc/sudoers.d/shutdown`. This is an elegant way of separating different changes to the `sudo` rights and also leaves the original `sudoers` file untouched for easier upgrades.

**Note:** Again, you should use the command [`visudo`](https://manpages.ubuntu.com/manpages/precise/en/man8/visudo.8.html) to edit the file to make sure you do not lock yourself out of the system:

```
sudo visudo -f /etc/sudoers.d/shutdown

```

This also automatically ensures that the owner and permissions of the new file is set correctly.

If `sudoers` is messed up
-------------------------

If you did not use `visudo` to edit your files and then accidentally messed up `/etc/sudoers` or messed up a file in `/etc/sudoers.d` then you will be locked out of `sudo`.

The solution could be to fix the files using [`pkexec`](https://manpages.ubuntu.com/manpages/precise/en/man1/pkexec.1.html) which is an alternative to `sudo`.

To fix `/etc/sudoers`:

```
pkexec visudo

```

To fix `/etc/sudoers.d/shutdown`:

```
pkexec visudo -f /etc/sudoers.d/shutdown

```

If the ownership and/or permissions are incorrect for any `sudoers` file, the file will be ignored by `sudo` so you might also find yourself locked out in this situation. Again, you can use `pkexec` to fix this.

The correct permissions should be like this:

```
$ ls -l /etc/sudoers.d/shutdown
-r--r----- 1 root root 86 Jul 16 15:37 /etc/sudoers.d/shutdown

```

Use `pkexec` like this to fix [ownership and permissions](https://help.ubuntu.com/community/FilePermissions):

```
pkexec chown root:root /etc/sudoers.d/shutdown
pkexec chmod 0440 /etc/sudoers.d/shutdown

```
