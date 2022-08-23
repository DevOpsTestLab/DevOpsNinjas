# AWS DevOps Lab

## Please find the list of steps that are required to complete the Lab

## Tasks

### Task 1: Create the CI/CD pipeline on using AWS Codepipeline/Codebuild and CodeCommit

Use the following repo to deploy your infrastructure on AWS.

Repo: https://github.com/DevOpsTestLab/infra

This repo has the terraform templates required to deploy the following resources:

1. [AWS Codepipeline](https://github.com/DevOpsTestLab/infra/blob/main/main.tf#L7) - your CICD orchestrator
2. [AWS CodeCommit](https://github.com/DevOpsTestLab/infra/blob/main/main.tf#L42) - Your source code repo
3. [AWS ECR](https://github.com/DevOpsTestLab/infra/blob/main/main.tf#L49) - Your container registry

#### Instructions:
- Use terraform from your local to deploy these resources on AWS
- Use the AWS CLI to configure your user/roles/policy needed to deploy these resources
- Store the statefile for this on a S3 bucket if possible. However its fine if you store it locally
- Review the [var files](https://github.com/DevOpsTestLab/infra/blob/main/terraform.tfvars) to ensure that all the right variables are available for the templates to execute successfully
- Review the modules for each of the resources under the mdoules folder - https://github.com/DevOpsTestLab/infra/tree/main/modules

### Task 2: Setup your repository on CodeCommit

Once you successfully complete the above step, you will have a codecommit repository created. Go ahead and download and upload the following application codebase into your new CodeCommit Repo

Application Codebase - https://github.com/DevOpsTestLab/sample-aws-lambda

This is a simple hello-world application built on python. Which gets packaged as a Docker Container and Deploy to AWS Lambda.

#### Instructions:
- Review the application code here - https://github.com/DevOpsTestLab/sample-aws-lambda/blob/main/lambda/aws-lambda-url.py (This just prints hello)
- Review the dockerfile here - https://github.com/DevOpsTestLab/sample-aws-lambda/blob/main/lambda/Dockerfile
- This application is also deployed to AWS using Terraform. Here is the terraform template, https://github.com/DevOpsTestLab/sample-aws-lambda/blob/main/main.tf

### Task 3: Setup your CI/CD pipeline - Build

Once you commit your code into AWS CodeCommit. The pipeline created from Task 1 will be kicked off automatically. Make sure the build stage is successfully.

The build stage is driven by this [codebuild step](https://github.com/DevOpsTestLab/infra/blob/main/modules/codepipeline/main.tf#L13) and this [buildspec file](https://github.com/DevOpsTestLab/infra/blob/main/modules/codepipeline/templates/buildspec_build.yml)

*Update the buildspec file if you need to and re-apply the terraform templates to update your infrastructure*


### Task 4: Setup your CI/CD pipeline - Sonarqube

The next step is the sonarqube quality scan step. Its driven by the following [buildspec file](https://github.com/DevOpsTestLab/infra/blob/main/modules/codepipeline/templates/buildspec_scan.yml)

We will be using sonarcloud which is the SaaS offering for Sonarqube analysis - https://sonarcloud.io/

Have the following SSM Parameters created manually or though terraform from step 1 that is needed by your sonarqube analysis step. 

https://github.com/DevOpsTestLab/infra/blob/main/modules/codepipeline/templates/buildspec_scan.yml#L4

Use the corresponding values for your 
 - token - https://sonarcloud.io/account/security
 - organization - https://sonarcloud.io/account/organizations
 - sonarendpoint will be - https://sonarcloud.io/

Some documentation for your reference - https://docs.sonarcloud.io/advanced-setup/analysis-parameters/

*Update the buildspec file if you need to and re-apply the terraform templates to update your infrastructure*

### Task 5: Setup your CI/CD pipeline - Deploy

The next step is the sonarqube quality scan step. Its driven by the following [buildspec file](https://github.com/DevOpsTestLab/infra/blob/main/modules/codepipeline/templates/buildspec_deploy.yml)

*Update the buildspec file if you need to and re-apply the terraform templates to update your infrastructure*

Once the Lambda fuction is deploy. Run a test with some test event. The results should just print "hello world"


**NOTE:**
1. Keep in mind that you may run into issues due to any errors deliberately injected into the terraform templates/buildspec files. 
2. Dont expect things to work out of the box. Make the necessary assumptions when you get blocked. 
3. Remember that you may be questioned on your troubleshooting/problem solving capabilities as well