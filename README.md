# iam-user-cleanup

Introduction: This will allow you to automate the management and removal of orphaned IAM users, access keys, and roles within your organization using Lambda and EventBridge. Integrated IAM Access Analyzer and SNS for reporting, with logs stored in CloudWatch for auditing.

Installations: 
  - Have an AWS Account and select a region with support for AWS Lambda.
  - Here is a link that lists AWS Lambda endpoints and quotas: https://docs.aws.amazon.com/general/latest/gr/lambda-service.html
  - Install AWS Command Line Interface (AWS CLI).


Steps:

Deploy the CloudFormation Stack

1. Download the "cloudformation-iam-user-cleanup.yaml" CloudFormation template
2. Open the CloudFormation console
3. For Create Stack, choose With new resources (standard).
  - For Prerequisite - Prepare template, choose Template is ready.
  - For Specify Template, choose Upload a template file.
  - Choose Choose file, then select the cloudformation-iam-user-cleanup template you downloaded.
  - Choose Next
4. In the Specify Stack Details menu:
  - For Stack name, enter iam-cleanup.
  - For AnalyzerType, IAM Access Analyzer produces findings on the external availability of resources within your AWS account. If this account is the root account of an organization or the delegated administrator select ORGANIZATION, otherwise select ACCOUNT or NONE to skip this check.
  - For MinAgeKeysToDelete, leave the default of 0.
  - For MinAgeKeysToDisable, leave the default of 0.
  - For MinAgeKeysToReport, leave the default of 30.
  - For MinAgeRolesToDelete, leave the default of 0.
  - For MinAgeRolesToDisable, leave the default of 0.
  - For MinAgeRolesToReport, leave the default of 30.
  - For MinAgeUnusedUsersToDelete, leave the default of 0.
  - For MinAgeUnusedUsersToDisable, leave the default of 0.
  - For MinAgeUnusedUsersToReport, leave the default of 30.
  - For NotificationEmail, enter the email address to send the IAM report to.
  - Choose Next
5. For Configure Stack Options, choose Next.
6. Under Capabilities and transforms, for Transforms might require access capabilities, select the checkbox next to each:
  - I acknowledge that AWS CloudFormation might create IAM resources..
  - I acknowledge that AWS CloudFormation might create IAM resources with custom names..
  - I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND..
7. Choose Submit, wait a few minute until the stack shows CREATE COMPLETE
  - Select Resources.
  - Open the UserCleanupLambda link in a new tab. You will invoke this to test the Lambda function in the next step.
  - By now you have deployed a CloudFormation stack to create the base resources needed for this lab

Testing the Lambda Function

1. By now you should have received an email from no-reply@sns.amazonaws.com  to the email you entered as a parameter when you deployed the CloudFormation stack in the last step. You need to Confirm subscription to the SNS topic before continuing with the steps below.
2. Using the open Lambda function tab from the previous step that you deployed the CloudFormation stack.
3. In the navigation page, under Test, enter the Event name as Test-IAM-Cleanup.
4. Select the Test button to begin executing the Lambda function.
  - It will take a few minutes depending on how many IAM Users and IAM Roles you have in the account.
5. If your test runs successfully you should receive an email from: AWS Notifications no-reply@sns.amazonaws.com  with the subject line of: IAM user cleanup from <account_ID>
  - The body of the email will have a status report from the findings. E.g. IAM Users and AWS Access Keys which require a cleanup.




