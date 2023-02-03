# PGP and OpenSSL

## Extract .crt and .key files from .pfx file

Export private key:

```sh
openssl pkcs12 -in yourfile.pfx -nocerts -out yourfile_with_password.key
```

Remove password:

```sh
openssl rsa -in yourfile_with_password.key -out yourfile.key
```

Export certificate:

```sh
openssl pkcs12 -in yourfile.pfx -clcerts -nokeys -out yourfile.crt
```

Source: [IBM](https://www.ibm.com/docs/en/arl/9.7?topic=certification-extracting-certificate-keys-from-pfx-file)
