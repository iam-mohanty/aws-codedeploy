# AWS-CodeDeploy

### Step 1 - Login to your AWS console and search for codedeploy.

### Step 2 - Click Create application.

```sh
my-app-application
```

Compute platform   --->  EC2/on-premises

### Step 3 - Create CodeDeploy IAM role

Create Role   --->  AWS Service = codedeploy

Role Name
```sh
code-deploy-role
```

Attach Policy to role

```sh
AmazonEC2FullAccess
AmazonEC2RoleforAWSCodeDeploy
AmazonEC2RoleforAWSCodeDeployLimited
AmazonS3FullAccess
AWSCodeDeployFullAccess
```

### Step 4 - The application is created. Now click on Create Deployment group.

Deployment group name

```sh
my-app-deployment-group
```

