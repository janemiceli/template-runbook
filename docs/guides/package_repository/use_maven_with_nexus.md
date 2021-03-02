---
layout: default
---
# Configure Maven Locally to Integrate with HP ID Nexus

## Development Environment

In order to build your artifacts we recommend using the [GCD dev-env](https://github.azc.ext.hp.com/cwp/gcd-devenv).
With this configured and running, you can easily execute Maven commands using the alias `mvn`.

If you already have a development environment, make sure that you have Maven installed.

## Subscribe to a LDAP group

Nexus is configured with LDAP. To get permission to download artifacts from Nexus, you must be member of a specific LDAP group. For information, see the [Nexus service page](./index.md).

After you have joined this group, you can test your credentials using a browser at this URL: [https://nexus.hpcwp.com](https://nexus.hpcwp.com) (sign in at the top-right corner).

## Configure Maven

Reference: [Apache Maven encryption guide](http://maven.apache.org/guides/mini/guide-encryption.html)

First, we need to generate an arbitrary Maven Master key to be used locally. This password is chosen by you at creation time and is not the password used to authenticate against Nexus; it will be used to encrypt the real password in a later step. Therefore, all required passwords will be cryptographed based on this input.
Run the following command:

```

$ mvn --encrypt-master-password <enter-your-master-password>
```

Then, create a file called `settings-security.xml` under your `.m2` folder (i.e. `~/.m2/settings-security.xml`), and type the generated key between the master element.

For example:

```xml

<settingsSecurity>
    <master>{zETseM6/slgHe91C9XtZlFst7Bo4iuT/0jJfCuD834Q=}</master>
</settingsSecurity>
```

Nexus has its configuration in the `~/.m2/settings.xml` file.  Your `<servers/>` section should be:

```xml

<servers>
  <server>
    <id>hpid-central</id>
    <username>john.doe@hp.com</username>
    <password>{============================================}</password>
  </server>
</servers>
```

You need to generate your own Maven-encrypted password for your HP NT credentials to access the `hpid-central` Nexus Maven repository, instead of the fake password above for the fake user `john.doe@hp.com`. First create the password:

```

$ mvn --encrypt-password <enter-your-hp-nt-password>
```

> Note: This command is different and has different output from the previous encryption command. The password input to both commands is the same.

With the above setup, the `<repositories>` section in your top-level `pom.xml` in your repos should be:

```xml

<repositories>
  <repository>
    <id>hpid-central</id>
    <url>https://nexus.hpcwp.com/repository/mvn-cwp-central</url>
    <layout>default</layout>
    <releases>
      <enabled>true</enabled>
    </releases>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
</repositories>
```

With this configuration, your Maven should be able to pull artifacts from Nexus using your LDAP credentials:

```

$ mvn -U clean install
```
