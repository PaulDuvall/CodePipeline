# CodePipeline Advanced FAQ

[AWS CodePipeline](https://aws.amazon.com/codepipeline/) is a managed service that orchestrates workflow for continuous integration, continuous delivery, and continuous deployment. With CodePipeline, you define a series of stages composed of actions that perform tasks in a release process from a code commit all the way to production. It helps teams deliver changes to users whenever there’s a business need to do so.

If you use AWS CodePipeline for continuous delivery/deployment, one of the first pages you should visit is its FAQ at https://aws.amazon.com/codepipeline/faqs/. 

After you’ve gone through and used CodePipeline for your applications, you’ll probably want to learn more. If so, this blog post is a resource for you. In it, I go over the FAQ and, when appropriate, I provide more detailed examples along with more advanced topics with code and other examples. It’s intended as a more comprehensive view of the service.

1. How do I specify parallel actions in CodePipeline?

Use the same integration in the RunOrder for each action you want to run in parallel within a particular stage.

      - Name: Build
        Actions:
        - InputArtifacts:
          - Name: MyApp
          Name: cfn_nag
          ActionTypeId:
            Category: Test
            Owner: AWS
            Version: '1'
            Provider: CodeBuild
          OutputArtifacts: []
          Configuration:
            ProjectName:
              Ref: CodeBuildWebsite
          RunOrder: 1
        - InputArtifacts:
          - Name: MyApp
          Name: Build
          ActionTypeId:
            Category: Build
            Owner: AWS
            Version: '1'
            Provider: CodeBuild
          OutputArtifacts:
          - Name: MyAppBuild
          Configuration:
            ProjectName:
              Ref: CodeBuildWebsite
          RunOrder: 1

1. What are the currently supported action types?

The only valid owner string is "AWS", "ThirdParty", or "Custom". 

Here are examples of configuring action types in the AWS CloudFormation AWS::CodePipeline::Pipeline resource.

          Name: Source
          ActionTypeId:
            Category: Source
            Owner: ThirdParty
            Version: '1'
            Provider: GitHub

        - Name: Artifact
          ActionTypeId:
            Category: Build
            Owner: AWS
            Version: '1'
            Provider: CodeBuild


        - Name: Jenkins
          ActionTypeId:
            Category: Build
            Owner: Custom
            Version: '1'
            Provider: CodeBuild


1. What is the Artifact Store?

Artifact Store is an S3 bucket used to securely store CodePipeline artifacts. When using the CodePipeline console in a region for the first time, CodePipeline automatically generates a new S3 bucket for all CodePipeline artifacts. You can also create your own S3 bucket and make a reference to the bucket from CodePipeline.

Here’s an example of doing this in AWS CloudFormation: 

        PipelineBucket:
          Type: AWS::S3::Bucket
          DeletionPolicy: Delete

1. Where and how are CodePipeline artifacts stored?

Within the S3 bucket designated for the pipeline(s), there are S3 keys/folders. There’s a different key for each artifact name used across all pipelines.

Here’s an example of the S3 keys within an S3 bucket. The names of keys are a truncated version of the Artifact names specified in CodePipeline when developers were creating their pipelines.   

          Name: Source

1. How does CodePipeline encrypt artifacts in S3?

CodePipeline automatically encrypts artifacts using the default KMS key. You may also customize this by creating a custom KMS and assign it to the S3 artifacts.

          Name: Source

1. How do I disable and enable transitions?

You can disable/enable transitions using the AWS Console, API, CLI, or SDK.

Here’s an example of disabling a transition using the CLI:

          Name: Source


1. When and where does CodePipeline create an S3 bucket to store artifacts?
TBD

          Name: Source


1. How are the artifacts in CodePipeline structured in S3?
TBD

          Name: Source


1. Which Source providers are currently supported?
TBD

          Name: Source


1. Which Build providers are currently supported?
TBD

          Name: Source


1. Which Test providers are currently supported?
TBD

          Name: Source
1. Which Deploy providers are currently supported?
TBD

          Name: Source


1. What are the different mechanisms for triggering source events 
(hooks, cloudwatch events,  polling, etc.)

          Name: Source


1. How do I use S3 to look for changes to a non-supported Source Provider?
TBD

          Name: Source


1. How many revisions can go through a CodePipeline stage at a time?
Only one at a time. The result is that your feedback time might be slowed down if you have too many actions in a particular stage and revisions get queued up waiting for the stage to complete.

          Name: Source


1. How do I obtain history for an individual pipeline?
From the CodePipeline console, select a specific pipeline. From the pipeline, click on View pipeline history. From the AWS CLI, you  

          aws codepipeline list-pipeline-executions --pipeline-name MyFirstPipeline
          aws codepipeline get-pipeline-execution --pipeline-name MyFirstPipeline --pipeline-execution-id 7cf7f7cb-3137-539g-j458-d7eu3EXAMPLE

For more information, see View Pipeline Details and History in AWS CodePipeline.

1. How does CodePipeline compare to other continuous delivery providers?
TBD

          Name: Source

1. What’s the duration of the following provider types: CodeBuild, CodeDeploy, Lambda, CloudFormation, and Approval?

Approval action: 7 days
AWS CloudFormation deployment action: 3 days
AWS CodeBuild build action and test action: 8 hours
AWS CodeDeploy deployment action: 7 days
AWS Lambda invoke action: 1 hour

          Name: Source
          
1. How do a I create a pipeline in one AWS account and access it from other AWS accounts?
TBD

          Name: Source


1. How do I use CloudWatch Events to notifications?
TBD

          Name: Source


1. How do I create a customer managed KMS key with CodePipeline?
TBD

          Name: Source


1. How to I use CloudWatch Events to send CodePipeline notifications via Slack?
TBD

          Name: Source


1. What are the maximum number of actions in a stage?
TBD

          Name: Source


1. What are the maximum number of parallel actions in a stage?
10

1. What are the maximum number of sequential actions in a stage?
10

1. What are the pipeline naming constraints?
TBD

          Name: Source


1. What are maximum number of pipelines in an AWS account?
TBD

          Name: Source

1. How do I get CodePipeline statistics?
TBD

          Name: Source

1. How do I provide real-time dashboards for CodePipeline metrics?
TBD

          Name: Source


1. What 3rd party product integrations are available with AWS CodePipeline?
TBD

          Name: Source



1. How do I use CloudFormation Stack Sets and Change Sets with CodePipeline?
TBD

          Name: Source


1. How to I get a JSON object representing a pipeline using the AWS CLI?
By calling the get-pipeline command from the AWS CLI. This returns a JSON object describing all components of a specific pipeline including stages, actions, artifacts, artifact store, providers, configuration, etc. Below, you see an example of running this command. Below, MyFirstPipeline is the unique name of the pipeline for a particular region.    

aws codepipeline get-pipeline --name MyFirstPipeline --region us-east-1
1. How do I start a pipeline that’s not triggered by a source event?
TBD

1. How do I provide read-only access to a specific pipeline when provisioning the pipeline in CloudFormation?
TBD

1. What are the different types of deployments and deploy providers?
AWS CloudFormation
Amazon ECS
AWS CodeDeploy
AWS Elastic Beanstalk

1. How do I enable AWS Config for CodePipeline?
https://aws.amazon.com/about-aws/whats-new/2018/09/aws-config-adds-support-for-aws-codepipeline/ 

1. What AWS service integrations are available with AWS CodePipeline?
AWS CloudFormation
AWS CodeBuild
AWS CodeCommit
AWS CodeDeploy
AWS Config
AWS Device Farm
AWS Elastic Beanstalk
AWS Key Management Service
AWS Lambda
Amazon ECS
Amazon S3
AWS OpsWorks

          Name: Source

