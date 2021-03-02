---
layout: default
---
# Nexus

In a nutshell Nexus is the leading open-source repository management tool. Built with Java, it supports technologies such as Docker, NuGet, NPM, Bower, PyPI and Ruby Gems.

We maintain one Nexus service for HP ID at the following location: [https://nexus.hpcwp.com](https://nexus.hpcwp.com)


| URL | Version | Repo | Program | Purpose | SLA |
|---|---|---|---|---|---|
| [https://nexus.hpcwp.com](https://nexus.hpcwp.com) | Alpha | [Github](https://github.azc.ext.hp.com/cwp/gcd-nexus) | HP ID | Development repository manager | [SLA](../slas/nexus.md) |

## Nexus Repositories

| Repository | Description | Naming Standard |
|---|---|---|
| CD | Contains project deliverables under qualification in the CD Platform | [technology]-[component]-[type] - (mvn-cd, gem-cd, etc) |
| Releases | Contains project releases that are meant to be deployed in production | [technology]-[component]-[type] - (mvn-releases, gem-releases, etc) |
| Central | Groups Partner’s Prod and third-party repositories and Community proxy repositories | [technology]-[component]-[type] - (mvn-central, gem-central, etc) |
| Proxies | The interface with external repositories maintained by the community | Follows the community repository name |

## Nexus Users and Roles

There are LDAP groups created with the correct access permission. The following groups are available for Nexus. If you want to get access to Nexus, just subscribe to the developer group (**cwp-nexus-dev** for CWP folks and **accenture-nexus-dev** for Accenture teams) on [DirectoryWorks](https://directoryworks.hpicorp.net). (Log in to DirectoryWorks using your LDAP credentials (hp.com email address / password).)

| Name | Owner | Description | Access To |
|---|---|---|---|
| [cwp-nexus-dev](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3Dcwp-nexus-dev%2Cou%3DGroups%2Co%3Dhp.com) | CWP Team | User account for use by developers to search and download artifacts. | HP ID and CWP Repos (read-only) |
| [accenture-nexus-dev](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3Daccenture-nexus-dev%2Cou%3DGroups%2Co%3Dhp.com) | Accenture | Group for Accenture engineers to access Nexus via LDAP. Restricted to HP ID artifacts. | HP ID repos (read-only) |
| [accenture-nexus-pipeline](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3Daccenture-nexus-pipeline%2Cou%3DGroups%2Co%3Dhp.com) | Pipeline | User account for use by the pipeline for the Accenture projects. | HP ID Repos (read/write) |
| [cwp-nexus-pipeline](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3Dcwp-nexus-pipeline%2Cou%3DGroups%2Co%3Dhp.com) | Pipeline | Group for Accenture pipeline. Restricted to HP ID artifacts. | HP ID and CWP Repos (read/write) |
| [gcd-nexus](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3Dgcd-nexus%2Cou%3DGroups%2Co%3Dhp.com) | GCD Team | User account for use by the GCD Team with the admin role. | Admin access |


## Specific Use guides:

* [npm](./use_npm_with_nexus.md)
* [maven](./use_maven_with_nexus.md)

## Specific Requirements

If you need specific artifacts or have a requirement on the Nexus service send a message to our [e-mail list](mailto:hpid-infrastructure-operations@external.groups.hp.com) explaining your requirement. We'll get back to you ASAP.
