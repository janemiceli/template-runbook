---
layout: default
---

# Qualys


Qualys is a vunrability scanner that HP IT utilizes. This is a requirement for all EC2 instances deployed into AWS. The ActivationId and CustomerId are protected and can be found in the Quantum password manager.

## Install Qualys

### Ubuntu setup (non-HP image)

1. Sudo.
2. Download installation file [here](https://s3.amazonaws.com/gcd-rte-downloads/qualys-cloud-agent.x86_64.deb).
3. Install:

    ```
    dpkg -i qualys-cloud-agent.x86_64.deb
    ```  

4. Activate the agent on the node:

   ```
   /usr/local/qualys/cloud-agent/bin/qualys-cloud-agent.sh ActivationId=XXXXXXXX CustomerId=XXXXXXXX
   ```  

5. Verify:

   ```
   service qualys-cloud-agent  status
   ```  

   The output should include `Active (running)`.

### CentOS setup (HP image)

CentOS is configured when the HP Active Directory script is run. See the [CentOS setup (HP image) section](../../LinuxJoinHPDomain/index.md#centos-setup-hp-image) under AWS Linux server domain join.

# Set up Qualys emails with new IP subnet ranges for scans

Qualys provides automated scanning for vulnerabilities across network infrastructure.

IT has set up the networking in the AWS accounts. To set up Qualys scans for additional IP subnet ranges, open a ticket with HP Public cloud [here](https://itsm-support.corp.hp.com/sm-ess/ess.do). Provide:

* Subnet ranges
* Expected frequency of report (currently: weekly with a manual report delivered immediately after scanning is set up)
* Email addresses (including distribution lists) to receive the reports (currently: HPID-Security@external.groups.hp.com)

Please add the subnet ranges below with a note as to whether or not we've emailed about them.
This work is related to [HPID-6208](https://jira.cso-hp.com/browse/HPID-6208): Set up IT infrastructure for Qualys scanning.

List of HP ID Accounts, regions, subnet ranges:

| Account            |  Region              | Subnet range                      | IT contact status |
|:------------------|:----------------------|:----------------------------------|:------------------|
|hp-id-dev          | us-west-2           | 172.20.168.0/22                 | Reports set up    |
|hp-id-dev          | us-east-1           | 172.22.168.0/22                 | Reports set up    |
|hpid-cd-dev        | us-west-2           | 172.20.172.0/22                 | Reports set up    |
|hpid-cd-dev        | us-east-1           | 172.22.172.0/22                 | Reports set up    |
|hpid-prod-pro      | us-west-2           | 172.20.176.0/22                 | Reports set up    |
|hpid-prod-pro      | us-east-1           | 172.22.176.0/22                 | Reports set up    |

These can be found via VPC by listing the IPv4 CIDR in each region for each account. Here is the link (once you are logged in) to [us-west-2](https://us-west-2.console.aws.amazon.com/vpc/home?region=us-west-2#vpcs:).

The distribution list is [HPID-Security@external.groups.hp.com](mailto:HPID-Security@external.groups.hp.com) and can be edited [here](https://directoryworks.hpicorp.net/protected/groups/view/normal/?dn=cn%3DHPID-Security%2Cou%3DGroups%2Co%3Dhp.com).

[This is the email I sent last which can be used as a template](https://github.azc.ext.hp.com/cwp/gcd-quantum/blob/master/docs/playbooks/IT-AWS/Networking/Qualys/HPIT%20Enabled%20AWS%20Account%20for%20HPID%20subnets.msg).

Send new additions [to: fares@hp.com and chmy@hp.com, cc: quantum@groups.hp.com](mailto:fares@hp.com;chmy@hp.com&cc=quantum@groups.hp.com).
