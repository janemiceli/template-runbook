# FailedMount Error

---

## Synopsis
(As of now) This alert means the EBS volume that Nexus was using has attached to another instance causing the pod to not start. 

## Impacts
Pod and service are not running

## Remediation

 - Get volume name
   - `kubectl -n nexus describe pods nexus`
   - Look in the messages for 'Failed to attach volume '
     - You can use the pvc-* or the vol-* name
   - Sometimes a 'Failed to pull image' error may accompany this error and will be resolved once the volume attachment error is resolved
 - Check the AWS console for volume status
   - At this point it should say 'IN-USE'
   - If not in use another issue is occurring. Stop this playbook and contact SRE.
 - Delete deployment
   - `kubectl delete deployment -n nexus nexus`
 - Validate PV claim still exists
   - ` kubectl describe -n nexus PersistentVolumeClaim`
   - If the Volume doesnt match what was in the error logs stop running the playbook
 - Check the AWS console for volume status
   - At this point it should say 'AVAILABLE'
   - If not available, then force detach and wait 5 minutes (TBD appropriate wait time later)
 - Once the volume is available, start the deployment again
   - Ensure you have the latest gcd-nexus repository
   - Run
     - `kubectl -n nexus apply -f 3-nexus-deployment.yaml`
 - Validate the deployment is running
   - ` kubectl describe -n nexus deployment`
 - Validate service is running
   - `kubectl describe -n nexus service`
   - Note: After destroying and recreating the service, it takes some time for the ELB to come alive. You can monitor the K8s web console for the `nexus` namespace and `nexus-service` Service until an "External endpoints" is populated; and then wait for a few minutes after that for the link to actually start working.
 - Copy the LoadBalancer Ingres from the service and validate
   - IE: https://a5e2cd65e28fd11e7923d02be4442b81-1236855836.us-west-2.elb.amazonaws.com
 - Validate DNS
   - https://nexus.hpcwp.com
 - Verify alarm is gone
   - https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#alarm:alarmFilter=ANY;name=NexusHealthCheck-awsroute53-48efa84f-feee-47f3-b670-f295e42ea805-Low-HealthCheckStatus

