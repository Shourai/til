# Create compressed encrypted archives with tar and gpg

https://linuxconfig.org/how-to-create-compressed-encrypted-archives-with-tar-and-gpg


## Create an Encrypted Archive

```
$ tar -cvzf - folder | gpg -c > folder.tar.gz.gpg
```
## Decrypt, decompress and extract

```
gpg -d folder.tar.gz.gpg | tar -xvzf -
```
