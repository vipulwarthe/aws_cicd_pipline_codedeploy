# Vipul Warthe AWS CICD Pipeline Code Deployment to AWS EC2 Instance

## go to the IAM and create 2 IAM Role first:

    1) Create Role - AWS services - common use case (click on EC2) - Next - select policy (AmazonEc2RoleForAWSCodeDeploy) - Next - Role Name (EC2CodeDeployRole) - create Role.
    2) Create Role - AWS services - Use Cases for other AWS services - (select Code Deploy) - Next -Next - Role Name (CodeDeployRole) - create Role.
    3) Create one Amazon linux2 (5.10) ec2 instance(Name-CICD-Deployment)/t2.micro/sg-ssh/http/all-traffic-anywhere/8gb
    4) go to the Advance details in ec2 instance and select IAM instance profile (EC2CodeDeploy) and paste below userdata

<b>User Data for Dependencies installations for AMAZON Linux 2:-</b>

#!/bin/bash<br />
sudo yum -y update<br />
sudo yum -y install ruby<br />
sudo yum -y install wget<br />
cd /home/ec2-user<br />
wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install<br />
sudo chmod +x ./install<br />
sudo ./install auto<br />
sudo yum install -y python-pip<br />
sudo pip install awscli<br />


## Create CodePipeline: 
    Now go to the CodePipeline - click on "Deploy" - click on "Application" - create Application - Application name (VW-CICD) - Compute Platform (EC2/On-premises) - create application.
    Now click on Deployment Groups - create deployment group - enter name (VW-CICD-DG) - select service role (CodeDeployRole) - select deployment type (in-place) - Environment configuration (Amazon EC2 instance) - other 
    setting default - Deployment settings (CodeDeployDefaultAllAtOnce) - untick loadbalancer - create deployment group. 

    Now go to the Pipeline - click on Pipeline - create Pipeline - Pipeline Name (VW-CICD-Pipeline) - Service Role (New service role) - Advance setting - Artifact store - default location select - Encryption Key - default 
    AWS Managed key - Next - source provider - Github version 2 - Next  - source - source provider (Github version2) - connection (VW-CICD-GIT) - connect to github - repository name - brance name - main - output artifact 
    format - codepipeline default - Build -Skip - Deploy - AWS CodeDeploy - region - application name -deployment group - next - create pipeline.

    - open the public IP address in browser you will see the html page. 
    - try to make some changes in html file you will see the pipeline will trigger automatically.
    - Demo is over.
