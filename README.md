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
* Setup CodeDeply Pipeline
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

