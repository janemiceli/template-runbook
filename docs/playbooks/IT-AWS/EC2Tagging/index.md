---
layout: default
---
EC2 instance tagging
====

HP IT uses tags on AWS instances to:

- Detect anomalous network activity
- Notify owners of an issue
- Help determine the impact if an account is having a problem; tags can facilitate issue escalation for critical services

## Account attributes

Use these attributes to build the supported [Tags](#tags).

### Environment attributes

| Environment | Note |
| --- | --- |
| Pro | Production instances |
| Dev | Contains Dev/CD/QA/Staging instances|

### Criticality attributes

| Criticality | Note |
| --- | --- |
| Normal | One off deployments, testing and pipeline instances |
| Entity Essential | Long running stacks. Instances that support monitoring or pipeline related services |
| Mission Critical | All things in Prod |

### Name attributes

| Name | Note |
| --- | --- |
| Instance Name | The name should correspond to the instance name |

## Tags

| Key | Value |
| --- | --- |
| Category | Environment:[XXXX]+Criticality:[XXXX]+Role:[XXXX] |
| CostManagement | CostCenter:US98008810+MRU:E054+LocationCode:47005C0800000000+ExpirationDate:01/01/2020 |
| Name | [XXXXX] |
| Owner | EPRID:208525+Name:HPID+Contact:quantum@groups.hp.com |

## Add tags manually

1. Log in to the HP IT AWS account.
2. Navigate to EC2.
3. Select the EC2 instance.
4. Click the **Tags** tab.
5. Click **Add/Edit Tags**.
6. Add Category, CostManagement, Name, and Owner.
7. Save.

## Add tags with the AWS CLI

1. Launch `gcd-devenv`.
2. Log in to the HP IT AWS account.
3. Run the following command:

   ```aws ec2 create-tags --resources i-XXXXX --profile saml --tags Key=Category,Value="Environment:XXXX+Criticality:XXXX+Role:XXXX" Key=CostManagement,Value='CostCenter:US98008810+MRU:E054+LocationCode:47005C0800000000+ExpirationDate:01/01/2020' Key=Name,Value=XXXX Key=Owner,Value='EPRID:208525+Name:HPID+Contact:quantum@groups.hp.com'
   ```

    To specify multiple instances, list them after `--resources` with spaces between items.

## Automated tags

To be determined later when Quantum has deployments and pipelines.
