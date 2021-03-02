---
layout: default
---
# AWS EC2 Container Repository (ECR) User Guide

Our team performs no setup for the ECR directly. It comes with AWS out of the box. If you are unable to access the ECR, the most likely reason is that we have not yet granted permissions for you to interact with the ECR.

To obtain access, send a message to [hpid-infrastructure-operations@external.groups.hp.com](mailto:hpid-infrastructure-operations@external.groups.hp.com) with some information about what you're trying to do and the error you get.  Helpful information includes:
* The AWS account and region you're trying to use
* Your AWS username
* The action that failed

We'll get back to you as soon as we can.

If you're having issues, you might find [Amazon's guide](http://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html) useful.

## A Very Short Guide to Using ECR

Export AWS Credentials as environment variables. (Stubbed here, set correctly for your environment.)

```

export AWS_ACCESS_KEY_ID=AK******
export AWS_SECRET_ACCESS_KEY=R6**************************
export AWS_DEFAULT_REGION=us-west-2
```

If the repository does not exist, create it, replacing `$repo-name` with your repository name.
In most cases it should be your org (for example, cwp) and the thing you're making.

```

aws ecr create-repository --repository-name $repo-name
```

The output of this command, or a known repository, can be used for the following commands.  I named my container in the next few commands "test-container".

Login (replace with correct region if needed):

```

aws ecr get-login --region us-west-2
```

After this, if you're building an image or running a container locally that pulls from one of the CWP ECRs, this may be enough.

Then build (if you need to):

```

docker build -t test-container .
```

If the previous command was successful, tag and push your built container.  The repository names are stable, they change just on the account number of the account.

```

docker tag test-container:latest 871386769552.dkr.ecr.us-west-2.amazonaws.com/test-container:latest
docker push 871386769552.dkr.ecr.us-west-2.amazonaws.com/test-container:latest
```

There are a few options for getting the account number, this is one of them and might be useful.

```

aws sts get-caller-identity --output text --query 'Account'
```

## Current Bit Storage

Our current Docker containers are being build and deployed in a few different places. You may be interested in the following Docker containers:

|Account  | Account Number   |  Regions| Full ECR | Active? |
|--- | ---| ---|---|---|
|hp-id-dev (IT account)  | 252775557553 |  us-west-2|252775557553.dkr.ecr.us-west-2.amazonaws.com|YES|
|hpid-cd-dev (IT account) | 120744138334 | us-west-2|120744138334.dkr.ecr.us-west-2.amazonaws.com|YES|
|hp-id-prod (IT account) | 872569379934 | us-west-2|872569379934.dkr.ecr.us-west-2.amazonaws.com|YES|
|hpid-dev (GCD account) | 871386769552  |  us-west-2|871386769552.dkr.ecr.us-west-2.amazonaws.com|NO|
|ccp-dev | 957591566260 | us-west-2, us-east-1, eu-central-1|957591566260.dkr.ecr.us-west-2.amazonaws.com|NO|

## Update Repository Permissions to be Accessible by Other Accounts

1. Select the repository to update.
2. On the **Permissions** tab, click **Add** to include a new permission.
3. On the permission form, provide:
    * A name for the permission "Sid"
    * The principal to access this repository
    * The permissions you are willing to give

        Usually only the "Push/Pull actions" and "Pull only actions" are necessary.

The permissions page is accessible at: [https://us-west-2.console.aws.amazon.com/ecs/home?region=us-west-2#/repositories/cwp:fluentd-elasticsearch-aws#permissions](https://us-west-2.console.aws.amazon.com/ecs/home?region=us-west-2#/repositories/cwp:fluentd-elasticsearch-aws#permissions)

Example of a policy JSON structure with those options:

```

"{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowTheOtherAccount",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::871386769552:root"
            },
            "Action": [
                "ecr:GetDownloadUrlForLayer",
                "ecr:PutImage",
                "ecr:CompleteLayerUpload",
                "ecr:BatchGetImage",
                "ecr:InitiateLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "ecr:UploadLayerPart"
            ]
        }
    ]
}"
```
