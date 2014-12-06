wercker-aws-code-deploy
=======================

This wercker step permits to deploy applications with [AWS Code Deploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html
) service.

Please read the [AWS Code Deploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) documentation and [API](http://docs.aws.amazon.com/cli/latest/reference/deploy/index.html) before using this step.

Note: Before using this step you have to install [AWS Cli](https://github.com/EdgecaseInc/wercker-step-install-aws-cli).

## Versions

| Release date | Step version | 
| -------------| -------------| 
| 2014-12-04   | 0.0.7        | 


## Configuration

The following configuration is required to configure this step :

#### AWS Code Deploy - Application 

* `application-name` (required) Name of the application to deploy
* `application-revision` (required) Revision of the application to deploy

#### AWS Code Deploy - Deployment Config

* `deployment-config-name` (optional) [Deployment config name](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-config.html). By default : CodeDeployDefault.OneAtATime
* `minimum-healthy-hosts` (optional) The minimum number of healthy instances during deployment. By default : type=FLEET_PERCENT,value=75

#### AWS Code Deploy - Deployment Group

* `deployment-group-name` (required) [Deployment group name](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html)
* `service-role-arn` (required for creation) Service role arn giving permissions to use [Code Deploy](http://docs.aws.amazon.com/codedeploy/latest/userguide/how-to-create-deployment-group.html)
* `ec2-tag-filters` (required for creation) EC2 tags to filter on when [creating a deployment group](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html)
* `auto-scaling-groups` (optional) Auto Scaling groups when [creating a deployment group](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) 

#### AWS Code Deploy - S3 Configuration

* `s3-bucket` (required) S3 Bucket where applications are pushed 
* `s3-region` (required) S3 Bucket region

## Example

The following example deploy an `hello` application on the deployment group `development` after pushed the application on the `apps.mycompany.com` S3 bucket :

```
deploy:
  steps:
  # Install aws cli
  - edgecaseadmin/install-aws-cli:
     key: AKRAIRVTDYKVCGUDJ3FJ3
     secret: 7ERFCYIVkZGPH9ujUJsmSsB9qxXWLYPmcsa4Os1Z5
     region: us-east-1 
  # AWS Code Deploy
  - nhuray/aws-code-deploy:
     application-name: hello
     application-revision: 1.1.0
     deployment-group-name: development
     s3-bucket: apps.mycompany.com
     s3-region: us-east-1
     service-role-arn: arn:aws:iam::89862646$091:role/CodeDeploy
     ec2-tag-filters: Key=app,Value=hello,Type=KEY_AND_VALUE Key=environment,Value=development,Type=KEY_AND_VALUE
```
