# AWS-CodeDeploy

### Step 1 - Login to your AWS console and search for codedeploy.

### Step 2 - Click Create application.

```sh
my-app-application
```

Compute platform   --->  EC2/on-premises

### Step 3 - The application is created. Now click on Create Deployment group.

Deployment group name

```sh
my-app-deployment-group
```










Step 3 - So we need to create a user and IAM users permissions

go to iam ---> crt role  ---> aws service = codedeploy 

role name = code-deploy-role   ---> crt 

attach policy to role = ec2 full access , s3 full , code deploy full ,  
