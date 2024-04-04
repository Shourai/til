# Modify LUKS iter-time on existing luks partition

<https://unix.stackexchange.com/questions/690138/how-to-modify-iter-time-on-a-existing-luks-partition>

You don't need to reencrypt the device if you want to change only `--iter-time`.
Reencryption is used when you want to change the way the data on the disk is encrypted (so different key, algorithm or, in case of the linked question, hash function).
Iteration time is a "property" of the key slot -- it tells how long should [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) take when derivating the key from your passphrase.
To change it you need to change only the key slot property with

```
cryptsetup luksChangeKey /dev/nvme0n1p2 --iter-time <time in ms>
```

It will ask for passphrase and change properties of key slot with that passphrase, it is possible to select key slot for the operation with `--key-slot` (if you have same passphrase for multiple key slots).
You'll need to repeat this if you want to change the iteration time for both key slots.

(This will also ask you to change your password, you can just reuse your old one, `luksConvertKey` which only changes the parameters doesn't work with LUKS 1.)

You can check the result with `cryptsetup luksDump /dev/nvme0n1p2` you should see the number PBKDF iterations decreased.
