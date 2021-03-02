---
layout: default
---
CLI/IAM permissions for IT-deployed AWS accounts
====

We're using Quimby. [Quimby info](https://github.azc.ext.hp.com/cwp/gcd-quimby)

Our process is not yet fully defined so this page will need to be updated.  It's important that we do as much with Quimby as we can and document the gaps otherwise. Zucolotto, Alan, and Rodrigo should be able to answer all questions.  If not, ping Evandro and he'll beat us up until we get good answers.

TL;DR create policies, create groups associated with one or many policies, associate users with one or many groups.
Users are being created with some custom devops code.  Reusing the PPS webops stuff, but taking out some dead items, like the link to the sign in page and a username/password.

In the medium/long term these accounts may need to be created by some system integrated with LDAP/IT.  Will make it so users get deleted when they leave HP.
