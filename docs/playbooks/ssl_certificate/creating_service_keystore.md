---
layout: default
---
## Creating your keystore from service certificate

If your service requires a keystore, you can follow the next steps to create one from the certificate.

- Create the chain file with the intermediate certificates from HP ( you can get [these certs from here](https://hp.sharepoint.com/teams/credentials/_layouts/15/start.aspx#/SitePages/HPI%20CA%20signed%20certificates.aspx) ):
```
cat cert.cer.txt  > chain.pem && cat HP_Inc_Private_SSL_CA.509.pem  >> chain.pem && cat HP_Inc_Private_Root_CA.base64.cer >> chain.pem
```

- Execute the openssl command to generate the pem file with the chain certificate:
```
openssl pkcs12 -inkey cert.key -in chain.pem -export -out cert.pkcs12
```

- Using the keytool command you generate the keystore:
```
keytool -importkeystore -srckeystore /nexus-data/keystores/node/cert.pkcs12 -srcstoretype PKCS12 -destkeystore /nexus-data/keystores/node/keystore.jks
```

These are the required steps to create the certificates, pem and keystores.

The final step is to update your service to use the keystore.
The Nexus service configuration you can find the [required steps here](https://books.sonatype.com/nexus-book/reference3/security.html#ssl-inbound).
