# Convert multi-line ssh public key to one-line for .ssh/authorized_keys
https://serverfault.com/questions/947869/convert-multi-line-ssh-public-key-to-one-line-for-ssh-authorized-keys

Yes, you can use a text editor to create the matching line, or your can use this command line:
```
ssh-keygen -i -f tmp/Public-Key
```
Result:

```
ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEAucNIPbPoaEqyBAKtk3LTfM/hiZlWomTdQEf7zUI4LGz91aZYIZNpWGTAUZKuFLdIEsktxQTNwEJNWMe2QocqQWyPGA+xL08ZP7XkVEbVOyH0nQ3ZHptgmyH4y4+bbAWXAROL3078h2iwtsCO343VQKg1iSNvemnLafA59/RtkcCR8SxH+NEXcc8MwGOE9gLX2pph4bxrFz9R6yyw3oRGVLt4uU9BlD3+LXg1plUzc2KZXEt8Zr04I0Fd865zyiB8Q+2ZEPvHf7MMaW66FRe4BXCI7LMh/voXi0C8H4NDIu1GZr7dNxgbEO05ZnASMofpLDU6cq7LFVl0BQG8gt1hOw==
```

This works, too:

```
puttygen -O public-openssh tmp/Public-Key
```
