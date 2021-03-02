---
layout: default
---
Console permissions for IT-deployed AWS accounts
====

# Reoccurring steps
* Invite the user to an account with propel

Go [here](https://propel-pro.houston.hp.com/propel/CMP_HPInc).  Search for a task called 'AWS Infrastructure Enabled Account Security Management' which might be [here](https://propel-pro.houston.hp.com/propel/CMP_HPInc/Form/Intermediate?subCategoryId=4e3fd419-b522-4e48-9045-6d2ff7e881e3&subCategoryName=AWS%2BInfrastructure%2BEnabled%2BAccount%2BSecurity%2BManagement&comingFrom=My%20Quick%20Links).

Click request.  Then follow the prompts, choosing the correct options. (Leaving this out for now as it seems pretty straight forward.)

Keep in mind that the security champion email always gets reset back to Chris, so if you don't want that work, make sure you know who should be the security champion. It's likely OK if that value is set to Chris.

In general, normal users should be added to the Development Users Group. Filtering is only available by last name.  At the end, press request.  Wait a few minutes (progress monitoring is available in Propel), and then the users should now have access when they go to the STS site in the [user guide](../../../guides/cloud_resources/UserPermissions.md).

# Onetime (completed) Steps - All accounts currently set up the same

* Create a policy for developers
* Create a policy for admins
* Associate Admin/Developer roles with new policies

TL;DR Admins can only do IAM, developers can do pretty much anything except IAM.

### Developer policy

`hpid-developer`

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1488297360970",
            "Action": [
                "iam:List*",
                "iam:AddRole",
                "iam:Create*",
                "iam:DeleteServerCertificate",
                "iam:UploadServerCertificate",
                "iam:GetServerCertificate",
                "iam:UpdateServerCertificate"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "NotAction": [
                "iam:*",
                "organizations:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Admin Policy

`hpid-admin`

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1488297360970",
            "Action": [
                "aws-marketplace:*",
                "ec2:*",
                "iam:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Admin/Developer Roles

`ADMIN`
`DEVELOPER`

Should have two roles attached, Developer and hpid-developer  and Admin/hpid-admin.
