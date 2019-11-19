# Athena CloudTrail Partitioner

AWS Athena is a serverless query service that helps you query your unstructured S3 data without all the ETL.

This CloudFormation project helps you periodically add partitions to your Athena/Glue database for each day/month/year/region/account added to your CloudTrail log bucket.

## Prerequisite - Enable CloudTrail

CloudTrail is an audit log of every action to occur in your AWS Action. It should be on all the time.

You can now [enable CloudTrail at the AWS Organization level](https://docs.aws.amazon.com/organizations/latest/userguide/services-that-can-integrate-ct.html), which means that CloudTrail for each account will be centrally logged and automatically enabled for all new accounts.

Read about how to [create your organization CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/creating-trail-organization.html) here.

Make sure the zip file within the repo exists in a bucket within the same region you are going to deploy everything (namely, the lambda)

Also be sure to update the OrganizationID (this is not the account ID), and applies for org-level CloudTrails

Deploy requires access to the DynamoDB group that allows MFA to work for this service (called "AssumeRoleDyanamoFullAccess" - otherwise you will get Access Denied Errors)

## Installation

Install the Athena CloudTrail Partitioner through CloudFormation, either through the AWSCLI:

```
aws cloudformation deploy \
  --stack-name athena-cloudtrail-partitioner \
  --region ${AWS_DEFAULT_REGION} \
  --template-file cf/template.yml \
  --force-upload \
  --parameter-overrides \
    "OrganizationId=${ORGANIZATION_ID}" \
    "S3BucketName=${S3_BUCKET_NAME}" \
  --capabilities CAPABILITY_NAMED_IAM \
  --no-fail-on-empty-changeset \
  --profile ${ASSUME_ROLE_FOR_MFA_ACCESS}
```

or click this button to deploy throught the AWS Console:

[![Launch Stack](https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?stackName=cloudtrail-athena-partitioner&templateUrl=https%3A%2F%2Fcloudtrail-pj-002.s3.amazonaws.com%2Fathena-cloudtrail-partitioner.yml)
