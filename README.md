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