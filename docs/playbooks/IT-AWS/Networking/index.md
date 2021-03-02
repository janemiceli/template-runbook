---
layout: default
---
# Network Setup for IT deploy AWS accounts

----------


IT has set up the networking in the AWS accounts.  There are three txt files here documenting the current setup.  This was generated following this [spike](../../../spikes/AWSSubnet.md).  

In general the subnets are unique across all of our accounts.  The VPC is as well from the point of view of the HP network.

The architecture of the networking might be useful as well. TODO: Get from PPT after CHMY is complete.

### Security Groups

----------

When adding SSH rules to a SG, anywhere (0.0.0.0/0) is NOT allowed and will automatically be removed from HP IT. The below table contains acceptable IP ranges for port 22.

| Range        | 
| ------------- |
| 15.0.0.0/9 |
| 172.16.0.0/12 |
| 16.192.0.0/10 |
| 72.3.186.100/32  |
| 134.213.183.100/32  |
| 146.20.30.100/32  |
| 161.47.6.100/32  |
| 134.213.182.100/32  |
| 120.136.39.100/32  |
| 119.9.163.100/32  |
