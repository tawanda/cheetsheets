# Decode an AWS Encrypted Authorization/Error message

Some actions that involve IAM permissions may return a Client.UnauthorizedOperation response (an HTTP 403 response).

eg

```
You are not authorized to perform this operation. 
Encoded authorization failure message: 
4Ai9qvVQYCx3kKoQaOtnKpzKCvrDlzzn_8R3vhbWP-T....
```

to decode run (the raw encoded mesage without the <>)

`aws sts decode-authorization-message --encoded-message <encoded message from error> --profile my-aruthorized-profile`

The error message is actually encoded JSON inside "", by default the embdeded quotes (") are escaped as \",
to facilitate reading the error extract the message portion and use a text editor to replace \" with "

REF: https://support.turbot.com/hc/en-us/articles/360042567891-Decode-an-Encrypted-Authorization-Error-message


# Add swap space to Amazon Linux

https://aws.amazon.com/premiumsupport/knowledge-center/ec2-memory-swap-file/


# Setup SSM

Follow this guide (use google translate) Ref [https://musma.github.io/2019/11/29/about-aws-ssm.html](https://musma.github.io/2019/11/29/about-aws-ssm.html)
e.g create a role ec2-systemmanager with the policy needed then:
Assign ec2-systemsmanager role to EC2

From AWS Systems Manager → Fleet Manage → Settings → Enable Auto update SSM Agent.

Create profile with proper region setting: aws configure --profile us_west_1

(need to create multiple profile if you want to use multiple region)

```
Host i-xxxxxx i-yyyy
	ProxyCommand sh -c "/home/tawanda/.virtualenvs/env/bin/aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p' --profile ap-northeast-2"
	User ec2user
	identityfile /home/tawanda/.ssh/mykey.pem
```

# Set up codedeploy

## **Steps Overview**

REF https://medium.com/codemonday/github-actions-for-ci-cd-with-ec2-codedeploy-and-s3-e93e75bf1ce0

1. Create IAM Role for EC2 instance & CodeDeploy
2. Launch EC2 Instance for our application
3. Create S3 bucket for our application revision
4. Setup our project for CodeDeploy, PM2 and Github Workflows
5. Setup CodeDeploy
6. Create IAM account
7. Push code to Github, setup secrets key and test

## install the agent 

Install the agent via user data or manually

the code for the agent is gotten at your regions url

https://aws-codedeploy-{{REGION_NAME}}.s3.{{REGION_NAME}}.amazonaws.com/latest/install

so for ireland its eu-west-1 so

https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install

to install go

first get dependencies
```
sudo yum -y update
sudo yum install -y ruby
sudo yum install -y wget
```

then install

```
cd /home/ec2-user

wget https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install
 
chmod +x ./install
 
sudo ./install auto
```


 To check that the service is running, run the following command:

``
sudo service codedeploy-agent status
```













