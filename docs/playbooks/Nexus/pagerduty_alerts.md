---
layout: default
---
# PagerDuty Alerts

| Severity | Name |
| ------------- |-------------|
| Critical | [Health Check Alert](#health-check-alarm) |
| Warning | [Nexus Snap Shot Failure](#nexus-snapshot-failure) |
| Warning | [Nexus Pods in Terminating](#nexus-pods-in-terminating) |
| Warning | [Nexus Pods in Error](#nexus-pods-in-error) |
| Warning | [Nexus Disk Usage](#nexus-disk-usage) |

---

# Health check alarm

## Synopsis

Nexus service DNS is not responding.

## Impacts

All services that connects to Nexus will return error and operations will not be performed.

* Pipeline
* Manual Nexus downloads and uploads
* HPID services

## Remediation

A notification message has to be send to all HPID and GCD teams about the Nexus service outage.
Check if the Nexus DNS (https://nexus.id.hpcwp.com) is pointing up to the Nexus external endpoint:

`$ kubectl get services -n nexus -o wide`

`NAME            CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)         AGE       SELECTOR
nexus-service   100.68.229.238   aa0cb7f8b70ba11e78daf02ab577a0c1-364767359.us-west-2.elb.amazonaws.com   443:31231/TCP   22d       app=nexus`

If it is not, the Nexus DNS need to be fixed, otherwise the service should be down and a Root Cause Analysis (RCA) should be done before making Nexus running again.

## Check pod status

 - `kubectl get pods --namespace nexus`
   - If STATUS is not running or zero nodes in service continue
 - `kubectl -n nexus describe pods nexus`
   - Check logs for messages
     - [FailedMount](errors_failedmount.md)


---
# Nexus Pods in Terminating

## Synopsis

The POD where Nexus is running is in terminating state.

## Impacts

All services that connects to Nexus will return error and operations will not be performed.

* Pipeline
* Manual Nexus downloads and uploads
* HPID services


## Remediation

If this is due to a programmed maintenance, nothing should be done unless performing the maintenance itself and then start the service again. Otherwise, a notification message has to be send to all HPID and GCD teams about the Nexus service outage. In this case, a Root Cause Analysis (RCA) should be done before starting the service again.

---
# Nexus Snapshot Failure

## Synopsis

The Rule for creating Nexus shap shot is failing

## Impacts

No Service or operations are impacted.
But its important to fix it to make sure the snapshots are available for restores.


## Remediation

* Check is the Nexus Volume attached to nodes.infra-us-west-2a.id.hpcwp.com is terminated. 
  Chances are the volume might have been replace.
* If the volume ID is not same as the volumeID in the **Nexus_EBS_Snapshot** Cloudwatch Rule [https://us-west-2.console.aws.amazon.com/cloudwatch/home?region=us-west-2#rules:name=Nexus_EBS_Snapshot] update the volumeID to the right one

Steps to update the volume ID.
* Get the volume ID for nexus snapshot 
* Go to hp-id-prod AWS account - Cloudwatch service. Look for service **Nexus_EBS_Snapshot** Cloudwatch Rule [https://us-west-2.console.aws.amazon.com/cloudwatch/home?region=us-west-2#rules:name=Nexus_EBS_Snapshot]

* Edit Rule **Nexus_EBS_Snapshot** and update the volumeID in the **Targets** section 
* Update rule 
* Verify if the snapshot are getting created. 

---
# Nexus Pods in Error

## Synopsis

The POD where Nexus is running is in error state.

## Impacts

All services that connects to Nexus will return error and operations will not be performed.

* Pipeline
* Manual Nexus downloads and uploads
* HPID services

## Remediation

A notification message has to be send to all HPID and GCD teams about the Nexus service failure. In this case, a Root Cause Analysis (RCA) should be done before starting the service again.

---

# Nexus Disk Usage

## Synopsis

Nexus service is low in disk space: less than 20% of disk space available.

## Impacts

There is no impact for any service. This is a warning message.

## Remediation

Run clean up routines that free up disk space before reaching out all disk space usage.
