# How to Mount Windows Share on Linux using CIFS

https://linuxize.com/post/how-to-mount-cifs-windows-share-on-linux/

On Linux and UNIX operating systems, a Windows share can be mounted on a particular mount point in the local directory tree using the `cifs` option of the [`mount`](https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/) command.

The Common Internet File System (CIFS) is a network file-sharing protocol. CIFS is a form of SMB.

In this tutorial, we will explain how to manually and automatically mount Windows shares on Linux systems.

Installing CIFS Utilities Packages
----------------------------------

To mount a Windows share on a Linux system, first you need to install the CIFS utilities package.

-   Installing CIFS utilities on Ubuntu and Debian:

    ```
    sudo apt update
    ```

Installing CIFS utilities on CentOS and Fedora:

```
sudo dnf install cifs-utils
```

The package name may differ between Linux distributions.

Mounting a CIFS Windows Share
-----------------------------

Mounting a remote Windows share is similar to mounting regular file systems.

First, [create a directory](https://linuxize.com/post/how-to-create-directories-in-linux-with-the-mkdir-command/) to serve as the mount point for the remote Windows share:

```
sudo mkdir /mnt/win_share
```

Run the following command as root or user with [sudo](https://linuxize.com/post/sudo-command-in-linux/) privileges to mount the share:

```
sudo mount -t cifs -o username=<win_share_user> //WIN_SHARE_IP/<share_name> /mnt/win_share
```

You will be prompted to enter the password:

```
Password:

```

On success, no output is produced.

To verify that the remote Windows share is successfully mounted, use either the `mount` or [`df -h`](https://linuxize.com/post/how-to-check-disk-space-in-linux-using-the-df-command/) command.

Once the share is mounted, the mount point becomes the root directory of the mounted file system. You can work with the remote files as if they were local files.

The password can also be provided on the command line:

```
sudo mount -t cifs -o username=<win_share_user>,password=<win_share_password> //WIN_SHARE_IP/<share_name> /mnt/win_share
```

If the user is in windows workgroup or domain you can set it as follows:

```
sudo mount -t cifs -o username=<win_share_user>,domain=<win_domain> //WIN_SHARE_IP/<share_name> /mnt/win_share
```

For better security it is recommended to use a credentials file, which contains the share username, password and domain.

The credentials file has the following format:

/etc/win-credentials

```
username=user
password=password
domain=domain

```

The file must not be readable by users. To set the correct [permissions](https://linuxize.com/post/chmod-command-in-linux/) and [ownership](https://linuxize.com/post/linux-chown-command/) , run:

```
sudo chown root: /etc/win-credentials
```

To use the credentials file, define it as follows:

```
sudo mount -t cifs -o credentials=/etc/win-credentials //WIN_SHARE_IP/<share_name> /mnt/win_share
```

By default of the mounted share is owned by root, and the permissions are set to 777.

Use the `dir_mode` option to set the directory permission and `file_mode` to set the file permission:

```
sudo mount -t cifs -o credentials=/etc/win-credentials,dir_mode=0755,file_mode=0755 //WIN_SHARE_IP/<share_name> /mnt/win_share
```

The default user and group ownership can be changed with the `uid` and `gid` options:

```
sudo mount -t cifs -o credentials=/etc/win-credentials,uid=1000,gid=1000,dir_mode=0755,file_mode=0755 //WIN_SHARE_IP/<share_name> /mnt/win_share
```

To set additional [options](http://man7.org/linux/man-pages/man8/mount.8.html#FILESYSTEM-INDEPENDENT_MOUNT_OPTIONS) , add them as a comma-separated list after the `-o` option. To get a list of all mount options type `man mount` in your terminal.

Auto Mounting
-------------

When the share is manually mounted with the `mount` command, it does not persist after a reboot.

The `/etc/fstab` file contains a list of entries that define where how and what filesystem will be mounted on system startup.

To automatically mount a Windows share when your Linux system starts up, define the mount in the `/etc/fstab` file. The line must include the hostname or the IP address of the Windows PC, the share name, and the mount point on the local machine.

Open the `/etc/fstab` file with your [text editor](https://linuxize.com/post/how-to-use-nano-text-editor/) :

```
sudo nano /etc/fstab
```

Add the following line to the file:

/etc/fstab

```
# <file system>             <dir>          <type> <options>                                                   <dump>  <pass>
//WIN_SHARE_IP/share_name  /mnt/win_share  cifs  credentials=/etc/win-credentials,file_mode=0755,dir_mode=0755 0       0

```

Run the following command to mount the share:

```
sudo mount /mnt/win_share
```

The `mount` command, will read the content of the `/etc/fstab` and mount the share.

Next time you reboot the system, the Windows share will be mounted automatically.

Unmounting Windows Share
------------------------

The `umount` command detaches (unmounts) the mounted file system from the directory tree.

To detach a mounted Windows share, use the `umount` command followed by either the directory where it has been mounted or remote share:

```
sudo umount /mnt/win_share
```

If the CIFS mount has an entry in the `fstab` file, remove it.

The `umount` command will fail to detach the share when it is in use. To find out which processes are accessing the windows share, use the `fuser` command:

```
fuser -m MOUNT_POINT
```

Once you find the processes, you can stop them with the [`kill`](https://linuxize.com/post/how-to-kill-a-process-in-linux/) command and unmount the share.

If you still have problems unmounting the share, use the `-l` (`--lazy`) option, which allows you to unmount a busy file system as soon as it is not busy anymore.

```
sudo umount -l MOUNT_POINT
```
