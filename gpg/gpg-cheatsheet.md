# A Cheatsheet for common gpg commands

## Encryption and decryption commands
To encrypt and sign a plaintext file with the recipient's public key
```
gpg --encrypt --sign --recipient <recipient> <file>
```

To sign a plaintext file with your secret key and have the output readable to people without running GPG first
```
gpg --clearsign <file>
```

To decrypt an encrypted file, or to check the signature integrity of a signed file
```
gpg [-o <output file>] <file>
```
# Key management commands
To generate your own unique public/secret key pair:
```
gpg --full-generate-key
```

To add a public or secret key file's contents to your public or secret key ring:
```
gpg --import keyfile
```
To extract (copy) a key from your public or secret key ring:
```
gpg --armor --output <keyfile> --export <userid>
```
or
```
gpg --armor --output keyfile --export-secret-key
```
To view the contents of your public key ring:
```
gpg --list-keys
```
To view the "fingerprint" of a public key, to help verify it over the telephone with its owner:
```
gpg --fingerprint userid
```
To view the contents and check the certifying signatures of your public key ring:
```
gpg --check-sigs
```
To edit a key:
```
gpg --edit-key userid
```
To remove a key or just a userid from your public key ring:
```
gpg --delete-key userid
```
To permanently revoke your own key, issuing a key compromise certificate:
```
gpg --gen-revoke userid
```
To disable or re-enable a public key on your own public key ring:
```
gpg --batch --edit-key userid disable
```
or
```
gpg --batch -edit-key userid enable
```
