---
layout: default
---
AWS Linux server domain join
====


----------


From the [HP Public Cloud wiki](https://rndwiki.corp.hpicorp.net/confluence/display/CLOUD/AWS+Linux+Server+Domain+Join "Public Cloud Wiki"):

> Linux servers provisioned in AWS should be joined to the CORP Active Directory.  This is accomplished using the Powerbroker Open Source AD agent, and is achieved by running a script which prepares the system, installs the agent, joins the Active Directory domain, and configures the PAM authentication system to use AD for authentication.

## Account attributes

The following tables list the required information for adding Active Directory (AD) to provisioned instances.

### Available AD groups

This table of generalized AD groups is provided for information only:

| Account Name | Account Number | Admin_AD_Group | Developer_AD_Group | Operator_AD_Group |
| --- | --- | --- | --- | --- |
|HPID-DEV         | 871386769552  |  AWS-871386769552-ADMIN | AWS-871386769552-DEVELOPER |  AWS-871386769552-OPERATOR |
|HPID-CD-DEV | 120744138334 | AWS-120744138334-ADMIN | AWS-120744138334-DEVELOPER  | AWS-120744138334-OPERATOR |
|HPID-PROD-PRO | 872569379934 | AWS-872569379934-ADMIN | AWS-872569379934-DEVELOPER  | AWS-872569379934-OPERATOR  |


### Preferred AD groups

CWP has our own AD groups per team that should be used in lieu of the more general groups:

| Team | Admin (sudo) | Operator | Developer |
| --- | --- | --- | --- |
| RTE | rteadmin | rteoperator | rtedeveloper |
| Vault | vaultadmin | vaultoperator | vaultdeveloper |
| Ping | pingadmin | pingoperator | pingdeveloper |
| SRE | sreadmin | sreoperator | sredeveloper |
| SI | siadmin | sioperator | sideveloper |

### Roles

| Name |
| --- |
| Application |
| Database | 
| Web |

Servers with a Web role will automatically have Apache installed

## Workstation setup

Being able to SSH to HP IT instances when working off-site requires additional settings on your workstation. These apply settings apply if you are on the VPN with the corporate proxy. Under VPN connections a route must be added for 172.

| Address | Netmask | Gateway |
| --- | --- | --- |
| 172.0.0.0 | 255.0.0.0 | 0.0.0.0 |

## Ubuntu setup (non-HP image)

The following steps add HP active directory to a deployed EC2 instance. The AMI should not have been provided by HP IT.

```
wget https://github.com/BeyondTrust/pbis-open/releases/download/8.5.4/pbis-open-8.5.4.334.linux.x86_64.deb.sh
touch /etc/sysconfig/network-scripts/ifcfg-eth0
bash pbis-open-8.5.4.334.linux.x86_64.deb.sh

reboot now

export NEW_SERVERNAME="lx-`curl -s http://169.254.169.254/latest/meta-data/local-ipv4 | sed -e 's/\.//g'`"
/opt/pbis/bin/domainjoin-cli setname $NEW_SERVERNAME
/opt/pbis/bin/domainjoin-cli join --userDomainPrefix auth --ou "OU=<Role>,OU=Servers,OU=<Application>,OU=<Account Number>,OU=AWS,OU=Cloud,DC=CORP,DC=HPICLOUD,DC=NET" corp.hpicloud.net \$automation@CORP.HPICLOUD.NET [password]

reboot now

/opt/pbis/bin/domainjoin-cli configure --enable ssh
/opt/likewise/bin/lwconfig AssumeDefaultDomain true
/opt/likewise/bin/lwconfig LoginShellTemplate /bin/bash
/opt/likewise/bin/lwconfig HomeDirTemplate "%H/%D__%U"

/opt/likewise/bin/lwconfig RequireMembershipOf "AUTH\\<OPERATOR_AD_GROUP>" "AUTH\\<DEVELOPER_AD_GROUP>" "AUTH\\<ADMIN_AD_GROUP>" "CORP\\Domain Admins" "AUTH\\Domain Admins"
```

Also, add the following line to `/etc/sudoers` using `visosudo`:

```
%<ADMIN_AD_GROUP> ALL=(ALL)   ALL
```

### Current limitations

| Issue | Date | Notes |
| --- | --- | --- |
| Server name registered with HP contains the IPv4 internal address | 9/15/2017 | This is a limitation that will cause collisions in the future as IP addresses are re-used. We can update the command to generate our own name. |

## Centos setup (HP image)

1. Launch image using `CENTOS_7_AD_IMAGE_2.2`.
2. SSH using `centos@[hostname]`` and the key specified during setup.
3. Run the installation command:
    
    Normal command with reboot
    ```
    curl -s http://internal-core-hp-linux-repo.corp.hpicloud.net/utils/userdata.sh | /bin/sh /dev/stdin <AWS Account Name> <role> AUTH\\<operator_ad_group> AUTH\\<developer_ad_group> AUTH\\<admin_ad_group>
    ```
    
    No reboot command
    ```
    curl -s http://internal-core-hp-linux-repo.corp.hpicloud.net/utils/userdata.sh | grep -v reboot | /bin/sh /dev/stdin <AWS Account Name> <role> AUTH\\<operator_ad_group> AUTH\\<developer_ad_group> AUTH\\<admin_ad_group>
    ```

### Current limitations

| Issue | Date | Notes |
| --- | --- | --- |

## Manual commands ##

The following commands can be used to see the status of Active Directory and other useful items.

| Command | Example | Notes |
| --- | --- | --- |
| `/opt/likewise/bin/lwconfig --detail RequireMembershipOf` | N/A | List users, groups, or accounts added to the EC2 instance |
| `/opt/likewise/bin/lwconfig RequireMembershipOf` | `/opt/likewise/bin/lwconfig RequireMembershipOf "AUTH\\quantumadtest3" "AUTH\\quantumadtest"` | Manually add a group or user to the EC2 instance<br />**Note**: When adding an item, specify the whole list or it will be overwritten with just the new item. |
| `/opt/likewise/bin/lw-get-status` | N/A | Displays information regarding the machine's join status |
