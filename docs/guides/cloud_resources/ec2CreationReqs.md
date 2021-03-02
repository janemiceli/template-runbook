---
layout: default
---
# EC2 Creation Requirements

Things you'll need to know while creating an EC2 in the IT account:

* HP's network is currently set up to allow access only to instances provisioned on the private IP.
* Connect (with SSH, etc.) to the private IP.
* Provision the instance in a private subnet.
* If you want pr provide public connections to your instance, make an elastic load balancer (ELB).
* Always include the HP-Core security group on your instance.
* The default security group from IT may be useful to you. It might grant you access to SSH.
* You can make your own security group and attach it to the instance. (More than one security group is allowed for an instance at a time.)
* If you want your instance to talk to AWS resources, set up some extra permissions with IAM roles. If you want your role to have a long and happy life, get in touch with the Gadget team.
* IT is very particular about security group rules. When they detect a rule attached to an instance that violates the below rule, they will alter the rule.  For example, if you provide access to 22 from anything that is not 15.0.0.0/9 they will remove that rule.
    These are the allowed subnets to be provided.

    ```[[DENY, TCP, 22, FROM, !15.0.0.0/9,!172.16.0.0/12,!16.192.0.0/10,!72.3.186.100/32,!134.213.183.100/32,!146.20.30.100/32,!161.47.6.100/32,!134.213.182.100/32,!120.136.39.100/32,!119.9.163.100/32, ENFORCE, DEFAULT]]```
