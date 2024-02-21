# Creating a new Serverless Application

1. Check the version of SAM CLI you are using (minimum version 1.0.0 is required):
``` bash
$ sam --version
SAM CLI, version 1.108.0
```


2. Create a new SAM application locally by running the following command:

``` bash
$ sam init --name demo-sam-app --runtime python3.10 --app-template hello-world --architecture x86_64 --no-tracing --no-application-insights --no-structured-logging
```

3. Build the sam application
``` bash
$ cd demo-sam-app
$ sam build
```

4. Create the pipeline for the first stage.  
Answer the interactive questions.  
When prompted to choose a “user permissions provider”, make sure to select OpenID Connect (OIDC). In the next question, select GitHub Actions as the OIDC provider. These selections result in additional prompts for information that later result in a least privilege integration with GitHub Actions.

``` bash
$ sam pipeline bootstrap --stage dev
.....fill in the prompts...my answers are below
Below is the summary of the answers:
        1 - Account: 588459062833
        2 - Stage configuration name: dev
        3 - Region: us-east-1
        4 - OIDC identity provider URL: https://token.actions.githubusercontent.com
        5 - OIDC client ID: sts.amazonaws.com
        6 - GitHub organization: scottmillers
        7 - GitHub repository: sam-app
        8 - Deployment branch:  main
        9 - Pipeline execution role: [to be created]
        10 - CloudFormation execution role: [to be created]
        11 - Artifacts bucket: [to be created]
        12 - ECR image repository: [skipped]
Press enter to confirm the values above, or select an item to edit the value: 

This will create the following required resources for the 'dev' configuration: 
        - IAM OIDC Identity Provider
        - Pipeline execution role
        - CloudFormation execution role
        - Artifact bucket
Should we proceed with the creation? [y/N]: y
        Creating the required resources...
```

5. Create the deployment resources for the second stage
``` bash
$ sam pipeline bootstrap --stage prod
.....fill in the prompts...my answers are below
[4] Summary
Below is the summary of the answers:
        1 - Account: 588459062833
        2 - Stage configuration name: prod
        3 - Region: us-east-1
        4 - OIDC identity provider URL: https://token.actions.githubusercontent.com
        5 - OIDC client ID: sts.amazonaws.com
        6 - GitHub organization: scottmillers
        7 - GitHub repository: sam-app
        8 - Deployment branch:  main
        9 - Pipeline execution role: [to be created]
        10 - CloudFormation execution role: [to be created]
        11 - Artifacts bucket: [to be created]
        12 - ECR image repository: [skipped]
Press enter to confirm the values above, or select an item to edit the value: 

This will create the following required resources for the 'prod' configuration: 
        - IAM OIDC Identity Provider
        - Pipeline execution role
        - CloudFormation execution role
        - Artifact bucket
Should we proceed with the creation? [y/N]: y
        Creating the required resources...
        Successfully created!
The following resources were created in your account:
        - Pipeline execution role
        - CloudFormation execution role
        - Artifact bucket
        - IAM OIDC Identity Provider
View the definition in .aws-sam/pipeline/pipelineconfig.toml,
run sam pipeline bootstrap to generate another set of resources, or proceed to
sam pipeline init to create your pipeline configuration file.
```

6. Initialize the pipeline configuration file
``` bash
$ sam pipeline init
sam pipeline init

sam pipeline init generates a pipeline configuration file that your CI/CD system
can use to deploy serverless applications using AWS SAM.
We will guide you through the process to bootstrap resources for each stage,
then walk through the details necessary for creating the pipeline config file.

Please ensure you are in the root folder of your SAM application before you begin.

Select a pipeline template to get started:
        1 - AWS Quick Start Pipeline Templates
        2 - Custom Pipeline Template Location
Choice: 1
                                                                                                                                                                           
Cloning from https://github.com/aws/aws-sam-cli-pipeline-init-templates.git (process may take a moment)                                                                    
Select CI/CD system
        1 - Jenkins
        2 - GitLab CI/CD
        3 - GitHub Actions
        4 - Bitbucket Pipelines
        5 - AWS CodePipeline
Choice: 3
You are using the 2-stage pipeline template.
 _________    _________ 
|         |  |         |
| Stage 1 |->| Stage 2 |
|_________|  |_________|

Checking for existing stages...

2 stage(s) were detected, matching the template requirements. If these are incorrect, delete .aws-sam/pipeline/pipelineconfig.toml and rerun

This template configures a pipeline that deploys a serverless application to a testing and a production stage.

What is the git branch used for production deployments? [main]: 
What is the template file path? [template.yaml]: 
We use the stage configuration name to automatically retrieve the bootstrapped resources created when you ran `sam pipeline bootstrap`.

Here are the stage configuration names detected in .aws-sam/pipeline/pipelineconfig.toml:
        1 - dev
        2 - prod
Select an index or enter the stage 1's configuration name (as provided during the bootstrapping): 1
What is the sam application stack name for stage 1? [sam-app]: 
Stage 1 configured successfully, configuring stage 2.

Here are the stage configuration names detected in .aws-sam/pipeline/pipelineconfig.toml:
        1 - dev
        2 - prod
Select an index or enter the stage 2's configuration name (as provided during the bootstrapping): 2
What is the sam application stack name for stage 2? [sam-app]: 
Stage 2 configured successfully.

SUMMARY
We will generate a pipeline config file based on the following information:
        Select a user permissions provider.: OpenID Connect (OIDC)
        What is the git branch used for production deployments?: main
        What is the template file path?: template.yaml
        Select an index or enter the stage 1's configuration name (as provided during the bootstrapping): 1
        What is the sam application stack name for stage 1?: sam-app
        What is the pipeline execution role ARN for stage 1?: arn:aws:iam::588459062833:role/aws-sam-cli-managed-dev-pipel-PipelineExecutionRole-v32etBsCb7ZU
        What is the CloudFormation execution role ARN for stage 1?: arn:aws:iam::588459062833:role/aws-sam-cli-managed-dev-p-CloudFormationExecutionRo-bpPpPyGE4hsV
        What is the S3 bucket name for artifacts for stage 1?: aws-sam-cli-managed-dev-pipeline-r-artifactsbucket-jfrnrjuodaee
        What is the ECR repository URI for stage 1?: 
        What is the AWS region for stage 1?: us-east-1
        Select an index or enter the stage 2's configuration name (as provided during the bootstrapping): 2
        What is the sam application stack name for stage 2?: sam-app
        What is the pipeline execution role ARN for stage 2?: arn:aws:iam::588459062833:role/aws-sam-cli-managed-prod-pipe-PipelineExecutionRole-tSNFiEdc214Q
        What is the CloudFormation execution role ARN for stage 2?: arn:aws:iam::588459062833:role/aws-sam-cli-managed-prod--CloudFormationExecutionRo-alvqYPuiioNB
        What is the S3 bucket name for artifacts for stage 2?: aws-sam-cli-managed-prod-pipeline--artifactsbucket-hvawiipqu81w
        What is the ECR repository URI for stage 2?: 
        What is the AWS region for stage 2?: us-east-1
Successfully created the pipeline configuration file(s):
        - .github/workflows/pipeline.yaml
```

AWS SAM CLI has created a file named pipeline.yaml in your .github/workflows directory.  This is the GitHub Actions workflow definition. Inspect the pipeline.yaml file to see how the GitHub Actions workflow deploys within your AWS account:


7. Commit changes to your GitHub repository to trigger the pipeline
My pipeline failed since my GitHub files are in the demo-sam-app directory not the root directory. 
