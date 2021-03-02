---
layout: default
---
# Scripted upload

  * Upload the certificate into AWS IAM and ACM using the [script](https://github.azc.ext.hp.com/cwp-set/devops/blob/master/hpid/cert/upload.sh). Please contact [quantum](https://pages.github.azc.ext.hp.com/cwp/gcd-quantum/team/) if you need access to this script.
  * Upload the certificate into Vault using the [script](https://github.azc.ext.hp.com/cwp-set/devops/blob/master/hpid/cert/certs_vault_load.py). Please contact [quantum](https://pages.github.azc.ext.hp.com/cwp/gcd-quantum/team/) if you need access to this script.

# Upload certificate into ACM (Amazon Certificate Manager) for Ping/Accenture Manuallu

Existing certificates can be found in the [CWP-SET](https://hp.sharepoint.com/teams/cwp-set/) Sharepoint site under Documents, certs.zip. If you must [request a new certificate](create_public_or_private_ssl_certificate.md), **please add it to certs.zip**.

  - NOTE: The password for the certs.zip can be found in the same CWP-SET Sharepoint under KeePass, SWSops.kdbx, in the HPID folder.

Certificates are uploaded to the hpi-id-dev and hpid-cd-dev AWS accounts in both the us-east-1 and us-west-2 regions, so this procedure must be performed 4 times.

1. Log in to the AWS web console and open the Certificate Manager service.
1. Click Import Certificate
1. In the certificate body, paste the contents of the .cer file
1. In the certificate private key, paste the contents of the .key file
1. In the certificate chain, paste the contents of the all_intermediate_certs.pub file
1. Click Review and Import, then Import
