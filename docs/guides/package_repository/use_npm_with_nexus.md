---
layout: default
---
# Configure npm Locally to Integrate with HP ID Nexus

## Development Environment

In order to build your artifacts we recommend using the [GCD dev-env](https://github.azc.ext.hp.com/cwp/gcd-devenv).
With this configured and running, you can easily execute npm commands using the alias `npm`.

If you already have a development environment, make sure that you have npm installed.

## Subscribe to an LDAP group

While Nexus is configured for LDAP authentication to non-NPM repositories, NPM repos are accessed with special credential. Using LDAP credentials for NPM access is not allowed as your password is basically sent in clear text -- in violation of HP CyberSecurity rules.

To obtain an NPM credential, you must request it from the SRE team by submitting an Access Request ticket on [Jira Service Desk](https://jira.cso-hp.com/projects/PE/) under the CWP Partner Engagement (PE) Project, SRE Component. For information, see the [Nexus service page](./index.md).

After you have received the credential, you can test it using a browser at this URL: [https://nexus.hpcwp.com](https://nexus.hpcwp.com) (sign in at the top-right corner).

## Configure npm

Reference: [Nexus npm](https://books.sonatype.com/nexus-book/reference/npm-deploying-packages.html)

First, we need to create or update your `.npmrc` file
This file should have the following fields:

```bash

email=<USERNAME>
always-auth=true
registry=https://nexus.hpcwp.com/repository/npm-hpid-central/
_auth=<SECRET>
```

Now, override the `<USERNAME>` and `<SECRET>` in the `.npmrc` file with the information provided by the SRE team.

With this configuration, your npm should be able to pull artifacts from Nexus using your LDAP credentials:

```bash

$ npm install
npm info it worked if it ends with ok
npm info using npm@3.10.10
npm info using node@v6.10.2
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/chai
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/istanbul
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/mocha
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/mocha-jenkins-reporter
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/prettify-xml
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/remark
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/remark-cli
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/sinon
npm info attempt registry request try #1 at 6:13:32 PM
npm http request GET https://nexus.hpcwp.com/repository/npm-hpid-central/standard
npm http 200 https://nexus.hpcwp.com/repository/npm-hpid-central/prettify-xml
npm info retry fetch attempt 1 at 6:13:33 PM
npm info attempt registry request try #1 at 6:13:33 PM
npm http fetch GET https://nexus.hpcwp.com/repository/npm-hpid-central/prettify-xml/-/prettify-xml-1.0.1.tgz
npm http 200 https://nexus.hpcwp.com/repository/npm-hpid-central/remark-cli
npm info retry fetch attempt 1 at 6:13:34 PM
```
