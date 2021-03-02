---
layout: default
---

# AWS Permissions User Guide

The Gadget team is responsible for maintaining the user permissions and service accounts in AWS. In general, after we know you need permissions, we try to follow a pattern of giving you the permissions of the team you're on.  If you change teams and need more (or fewer) permissions please let us know (via email), and we'll do our best to help.

If your team changes the technologies it is using (for example, adding a new one), let us know so that we can give you the correct permissions and/or solve the problem you're having another way.

Send us an [email message](mailto: hpid-infrastructure-operations@external.groups.hp.com) with the work you need done, and we'll do our best to help.

## After Being Granted Permissions

### Console

[This IT guide](https://rndwiki.corp.hpicorp.net/confluence/display/CLOUD/ADFS+MFA+UserGuide+for+AWS) has quite a bit more information.

For console access go here and sign in: [HP STS](https://sts.corp.hpicloud.net/adfs/ls/idpinitiatedsignon.aspx)  

The first time, follow these steps:
1. Download the Microsoft MFA app (called Microsoft authenticator on my mobile). [Google store link for the app](https://play.google.com/store/apps/details?id=com.azure.authenticator&hl=en)
2. In the MFA app, choose to add the account as a work app.
3. After signing in, you’ll see a QR code on the web app that you can scan with the authenticator to link the two.
4. Create a new PIN on the web page. This PIN becomes a password for the MFA app; it didn’t exist before this step.
5. Accept a sign-in request with your new PIN.


Managing your MFA is available from here: [MFA](https://mfaconfig.hpcloud.hp.com/MultiFactorAuth/)

#### Troubleshooting
If things go wrong, go to the MPA web site. You may have to wait for the timeout.  If you have a phone number, you can use that to get to this web site.  Then you can add your phone again, change your PIN, and so on.

### IAM

We are not planning on provisioning separate users for CLI connections for users.  You can use CLI tools by calling a tool the Gadget team has developed.  The simplest setup is to use it in the `gcd-devenv`, but if you cannot use that dev environment for any reason, feel free to call the container or the Python tool directly. The Gadget team cannot provide much support for using the Python tool directly. The package requirements are in `requirements.txt`, so feel free to use that to bootstrap.

1. Download the [gcd-devenv](https://github.azc.ext.hp.com/cwp/gcd-devenv) and set up. Or, use the [login tool](https://github.azc.ext.hp.com/cwp/gcd-aws-it-login) directly.
2. After calling the aws-it-login tool, use the AWS CLI (call with `--profile saml`).

For help with any issues, feel free to ask in the GCD forum HipChat: [https://hpi-cso-jira-hipchat.hipchat.com/chat/room/3669983](https://hpi-cso-jira-hipchat.hipchat.com/chat/room/3669983)
