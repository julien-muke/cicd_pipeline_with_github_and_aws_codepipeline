# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     CI/CD Pipeline form GitHub to AWS EC2


## <a name="introduction">🤖 Introduction</a>

In this demo, we are going learn how to upload code from our local computer to a remote repository in GitHub. Once the code is in GitHub it automatically triggers a code pipeline to take that code and deploy it on our AWS EC2 using CodeDeploy.


## <a name="design">📐 Project Architecture</a>

![CI_CD](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/cd6ed4f7-83f2-47c4-aa0a-af543e1493bc)


## <a name="steps">☑️ Steps</a>

* Setup IAM Roles
* Setup EC2 as Deployment Server
* Integrate GitHub as a source/repository
* Setup application & Deployment Group
* Setup CodeDeploy Pipeline
* Deploy Application on EC2 once the code is in GitHub it automatically triggers a code pipeline to take that code and deploy it on EC2 using Code deploy.


## ➡️ Step 1 - Setup IAM Roles

A. First, create an IAM Role for EC2

To create a role:

1. Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.
2. In the navigation pane of the console, choose Roles and then choose Create role.

![Screenshot 2024-06-12 at 14 01 20](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/3857d445-8cec-4b19-9d7b-f416989c8667)

3. Choose AWS service as Trusted entity type
4. Choose EC2 as Use case

![Create-role-IAM-Global(7)](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/fb258cba-4a77-4f4a-a4b2-4a86fd77c674)

5. Choose `AmazonEC2RoleforAWSCodeDeploy` policies to attach to your new role

![Create-role-IAM-Global(8)](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/161503e6-d0c3-4f74-8bc0-a97872fad9e7)

6. Enter Role name `EC2CodeDeployRole`
7. You will see a Trust policy already attached to you role
8. You will see the Permissions policy already attached to you role

![Create-role-IAM-Global(9)](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/c808ff54-ff2f-416e-acfd-72052cc55c26)


B. Second, create an IAM Role for CodeDeploy

Let's follow the same steps as our first IAM Role, with a CodeDeploy use case:

1. In the navigation pane of the console, choose Roles and then choose Create role.
2. Choose AWS service as Trusted entity type
3. Choose `CodeDeploy` as Use case
4. Choose `AWSCodeDeployRole` policies to attach to your new role
5. Enter Role name `CodeDeployRole`
6. Click "Next" and save changes


## ➡️ Step 2 - Setup EC2 as Deployment Server

To launch an instance:

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. In the navigation bar at the top of the screen, you can use the selected Region, or optionally select a Region that is closer to you.
3. From the EC2 console dashboard, in the Launch instance pane, choose `Launch instance`.

![Launch instance](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/c0667b1a-c639-4c99-86b9-b013531f5d3a)


4. Under Name and tags, for Name, enter a descriptive name for your instance, i will name it `MyInstance`
5. Under Application and OS Images (Amazon Machine Image), Choose Quick Start, and then choose the operating system (OS) for your instance. For your first Linux instance, we recommend that you choose Amazon Linux.
6. From Amazon Machine Image (AMI), select an AMI that is marked Free Tier eligible.

![Screenshot 2024-06-15 at 09 30 48](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/b1ec0133-b693-44bd-92d9-81e2f71d151c)


7. Under Instance type, for Instance type, choose t2.micro, which is eligible for the Free Tier. In Regions where t2.micro is not available, t3.micro is eligible for the Free Tier.
8. Under Key pair, for this demo choose Proceed without a key pair (Not recommended)

![Launch instance2](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/38fa9920-af55-4dbe-b825-6182669b16d3)


9. Under Network settings, select "Create security group" then choose "Allow SSH traffic from" and "Allow HTTP traffic from the internet"

![Launch instance3](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/3f50379b-0ecc-48ff-aad9-fee9e72b4440)


10. Under Advanced details, select `EC2CodeDeployRole` that we created as IAM instance profile

![Launch instance4](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/79fa95b3-eaeb-45fb-bec7-7e450b3a9b26)


11. Allow tags in metadata by choosing "Enable"
12. For User data, let's Upload a file with the user data, copy and paste file below:

```bash
#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wet https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto
sudo yum install -y python-pip
sudo pip install awscli
```

![Launch instance5](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/37625a17-e3fc-4e17-ae66-d83c13fece58)

13. Click "Launch instance"


## ➡️ Step 3 - Setup CodeDeply Pipeline

To create an application for an in-place deployment using the CodeDeploy console:

1. Sign in to the AWS Management Console and open the CodeDeploy console at https://console.aws.amazon.com/codedeploy.
2. In the navigation pane, expand Deploy, and then choose Getting started.
3. Choose Create application

![codedeploy1](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/30e4cab2-b710-4676-b2fe-6aa305da47d9)

4. In Application name, enter the name of your application `MyApp`
5. From Compute Platform, choose EC2/On-premises
6. Choose Create application

![codedeploy2](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/b0bcc73a-5586-433b-a143-f84bc18c3a66)

7. On your application page, from the Deployment groups tab, choose Create deployment group

![deploymentgroup](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/b75e9bfe-c92a-44b6-b408-951b34dd5534)

8. In Deployment group name, enter a name that describes the deployment group `MyApp-deployment-group`
9. In Service role, choose a service role that grants CodeDeploy access to your target instance, the one that we created
10. In Deployment type, choose In-place.
11. In Environment configuration, select Amazon EC2 instances
12. In the Key and Value fields, enter Name as key and MyInstance as value
13. In Deployment settings, choose a deployment configuration to control the rate your application is deployed to instances, such as all at once.
14. Uncheck Enable load balancing
15. Choose Create deployment group

![Create-deployment-group-CodeDeploy-us-east-1](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/5351cbfc-ba6f-46c3-b3e0-4d1f6660c118)


## ➡️ Step 4 - Create a pipeline in CodePipeline

1. Sign in to the AWS Management Console and open the CodePipeline console at http://console.aws.amazon.com/codesuite/codepipeline/home.
2. On the Welcome page, choose Create pipeline. 

![CodePipeline1](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/1f947856-4b02-4636-86b3-d91714463322)

NOTE: To create a pipeline in the console, you must provide the source file location and information about the providers you will use for your actions.

I have uploaded in this repository, all the requided files for this demo, where we have `index.html` (source file) `appspec.yml` (where we give
the instruction to the deployment) and `scripts` ( where we install dependencies)

3. In Pipeline name, enter the name for your pipeline 
4. In Pipeline type, choose V2 type pipelines
5. In Service role, Choose New service role to allow CodePipeline to create a new service role in IAM.
6. Choose Next

![Create-new-pipeline-CodePipeline-us-east-1 (2)](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/84fbcd48-37d2-4235-9e43-ab81714c2892)

7. Next, lets' Add the source stage, if it's your first time to add a source stage with a third-party provider such as GitHub, there are few connections you need to make first.

NOTE: To add a source action for your GitHub or GitHub Enterprise Cloud repository in CodePipeline, Use the CodePipeline console Create pipeline wizard or Edit action page to choose the GitHub (Version 2) provider option. See Create a connection to GitHub Enterprise Server (console) to add the action. The console helps you create a connections resource.

##  	:octocat: :octocat: Create a connection to GitHub (console)

Before you begin:
   <br>* You must have created an account with GitHub.
   <br>* You must have already created a GitHub code repository.

1. Choose to create a pipeline. Follow the steps in Create a Pipeline to complete the first screen and choose Next. On the Source page, under Source Provider, choose GitHub (Version 2).
2. Under Connection, if you have not already created a connection to your provider, choose Connect to GitHub. Proceed to Step 2: Create a Connection to GitHub.

![Screenshot 2024-06-15 at 12 48 08](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/2070e82c-3d11-4c98-a16b-c14668741437)

3. After you choose to create the connection, the Connect to GitHub page appears.

![GitHub1](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/8f257dbe-7942-4e4f-bbee-4768ca0e9a91)

4. Under GitHub Apps, choose an app installation or choose Install a new app to create one.

![GitHub2](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/4a88cf73-39cc-4a75-84b9-e836a065d640)

5. On the Install AWS Connector for GitHub page, choose the account where you want to install the app.
6. In Repository name, choose the name of your third-party repository my is `aws_codedeploy_using_github` where all my files are saved.
7. Choose Save.

![github3](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/adb0540d-81a8-43c4-98ff-a392fef72488)









![Create-new-pipeline-CodePipeline-us-east-1 (3)](https://github.com/julien-muke/aws_codedeploy_using_github/assets/110755734/e9719b06-cd2d-4576-8aa0-a3f094b37df8)


