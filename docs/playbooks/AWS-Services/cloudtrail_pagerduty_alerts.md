---
layout: default
---
# PagerDuty Alerts

| Severity  | Name |
| --------- |------|
| Critical  | [Cloudtrail log delivery failure](#cloudtrail-log-delivery-failure) |


---

# Cloudtrail log delivery failure

## Synopsis

Cloudtrail Log delivery to S3 bucket is failing. 

## Impacts

No external service or Partner impacted. We however loose data from the cloudtrail events recorded in the S3 bucket. 


## Remediation

* Check if Amazon service for cloudtrail or s3 are in degraded state.
* Login to the amazon account for the alert, look for the account name in the description of the alert.
* Go to [Cloudtrail Service](https://us-west-2.console.aws.amazon.com/cloudtrail/home?region=us-west-2#/dashboard)
* Click on Trails on the left hand side of the panel.
* Click on **Cloudtrail-[AccountNumber]**  in the **Trails** Page
* Look for the **Storage** section of the Configuration page
* Make sure there is an *S3 bucket* with Name cloudtrail-[aws-account-number]

If its not available: 

* Create a S3 bucket with Name cloudtrail-[aws-account-number].
* Send an email to the team about the event
* Open a ticket with Amazon to find out how the bucket got deleted.
