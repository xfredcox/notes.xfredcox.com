# Introduction to DevOps on AWS

Notes from this [whitepaper](http://d0.awsstatic.com/whitepapers/AWS_DevOps.pdf).

## Introduction
 
> ...

> Historically many organizations have been vertically structured with poor integration among development, infrastructure, security and support teams. Frequently the groups report into different organizational structures with different corporate goals and philosophies.

> ...

> Fundamentally developers like to build software and change things quickly, whereas IToperations focus on stability and reliability. 

> ...

> Today, these old divisions are breaking down, with the IT and developer roles merging and following a series of systematic principles:

> " _Infrastructure as code_

> " _Continuous deployment_

> " _Automation_

> " _Monitoring_

> " _Security_

> ...

## Infrastructure As Code

The principle that the rigor applied to applicaiton code (syntax, version control, etc.) should be applied to scripts responsible for creating, deploying and maintaining infrastructure.

Examples: 

  - AWS CloudFormation: templating infrastructure stacks
  - Instance Amazon Machine Image (AMI): preserving production ready states

## Continuous Deployment

> ...

> Its primary goal is to enable the automated deployment of production-ready application code.

> ...

Examples: 

  - AWS CodeDeploy: coordinates deployment group updates from an S3 stored app revision (Code + AppSpec).
  - AWS CodePipeline: ?
  - AWS CodeCommit: amazon's github
  - AWS Elastic Beanstalk & AWS OpsWorks: rolling deployments
  - Amazon Route 53: blue-green deployment

## Automation

> Another core philosophy and practice of DevOps is automation. Automation focuses on the setup, configuration, deployment, and support of infrastructure and the applications that run on it. By using automation, you can set up environments more rapidly in a standardized and repeatable manner. The removal of manual processes is a key to a successful DevOps strategy.

## Monitoring

Examples:

  - Amazon CloudWatch: tracks latency and CPU metrics and can be hooked to auto-scaling
  - AWS CloudTrail

> All AWS interactions are handled through AWS API calls that are monitored and logged by AWS CloudTrail. All generated log files are stored in an Amazon S3 bucket that you define. Log files are encrypted using Amazon S3 server-side encryption (SSE). All API calls are logged whether they come directly from a user or on behalf of a user by an AWS service.

## Security

 AWS Identity and Access Management service (IAM):

> With IAM, you can centrally manage users and security credentials such as passwords, access keys, and permissions policies that control which AWS services and resources users can access. You can also use IAM to create roles that are used widely within a DevOps strategy. With an IAM role you can define a set of permissions to access the resources that a user or service needs. But instead of attaching the permissions to a specific user or group, you attach them to a named role. Resources can be associated with roles and services can then be programmatically defined to assume a role.

----

[Source](http://d0.awsstatic.com/whitepapers/AWS_DevOps.pdf)