---
layout: default
---
# AWS IAM Service Accounts

If a particular Policy is only used once (for one specific user), attach an Inline Policy to the user. If the Policy will apply to multiple Users, create a Customer Managed Policy. Note: this convention has not been consistently used in the past, so don't expect existing users/policies to be correct.

* Inline Policy
  * Create user in IAM, but do not attach any Policies during the creation
     * Create an API key and email it to the ticket requester
  * Edit the user and on the Permissions tab, click + Add inline policy
    * Paste in the Policy JSON specified in the ticket
  * Paste the user ARN (example: arn:aws:iam::871386769552:user/health-application-user) into the ticket
* Customer Managed Policy
  * Create the Policy in IAM if it does not exist already
  * Create the user in IAM and attach the Policy 
    * Create an API key and email it to the ticket requester
  * Paste both the User and Policy ARN (example: arn:aws:iam::871386769552:user/health-application-user, arn:aws:iam::120744138334:policy/HealthApplicationUserPolicy) into the ticket

## Registration with HP IT

It is essential that all new IAM objects (Users, Roles, Groups, Policies, etc) be registered with HP IT or they will be auto-deleted after 2 days. To register,

* Go to https://awsiammgmt.hpcloud.hp.com/ and go to Manage IAM Database
* Select HPID for the APPLICATION (or create a new application and enter HPID into the APP NAME field)
* In APP OWNER, enter EPRID:XXXXXX,NAME:HPID,CONTACT:FIRST.LAST@HP.COM (using your email address)
  * Enter exactly 6 'X's for the EPRID
* In APPLICATION TEAM, enter CWP-SRE@EXTERNAL.GROUPS.HP.COM
* **Do not remove** any existing ARNs from the ADDITIONAL IAM ARNS field. Instead, **append** your new entries (comma separated, whitespace ignored) as noted in the tickets earlier and click Submit. The submission is successful when your new entries are changed to UPPERCASE.

