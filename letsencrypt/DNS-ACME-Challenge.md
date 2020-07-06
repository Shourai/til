# Letsencrypt DNS acme challenge on Namecheap

https://serverfault.com/questions/750902/how-to-use-lets-encrypt-dns-challenge-validation

```
sudo certbot certonly --manual --preferred-challenges dns
```

Certbot will then provide you instructions to manually update a TXT record for the domain in order to proceed with the validation.

```
Please deploy a DNS TXT record under the name
_acme-challenge.domain.com with the following value:

667drNmQL3vX6bu8YZlgy0wKNBlCny8yrjF1lSaUndc

Once this is deployed,
Press ENTER to continue
```

The TXT record should only needs the `_acme-challenge` part on namecheap for example.

To check the TXT record you can use the `dig` command

```
dig -t TXT _acme-challenge.domain.com
```

see also:
https://www.digitalocean.com/community/tutorials/how-to-acquire-a-let-s-encrypt-certificate-using-dns-validation-with-acme-dns-certbot-on-ubuntu-18-04
