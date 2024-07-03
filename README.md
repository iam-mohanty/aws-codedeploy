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

### Step 4 - Search for EC2 and launch an instance.

Go to EC2 & Create ubuntu-22 instance

Instance Name

```sh
my-app-ec2
```

### Step 5 - Now click on Create Deployment group.

Deployment group name

```sh
my-app-deployment-group
```

Deployment type = In-place

Environment configuration =  Amazon EC2 Instances

Install AWS CodeDeploy Agent  =  Never

Load balancer  =  Disable

Click Create Deployment Group

### Step 6 - Setting up the agent for CodeDeploy.

Now Connect your EC2 instance and run the following code

```sh
vim install.sh
```

Paste the below code

```sh
#!/bin/bash
# This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04. 
sudo apt-get update
sudo apt-get install ruby-full ruby-webrick wget -y
cd /tmp
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb
mkdir codedeploy-agent_1.3.2-1902_ubuntu22
dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22
sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control
dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/
sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb
systemctl list-units --type=service | grep codedeploy
sudo service codedeploy-agent status

```

Run this CodeDeploy agent on ec2 instance.

```sh
bash install.sh
```

### Step 7 - Now We need to Push our appspec.yml and scripts File into CodeCommit Repository

Connect again Linux-Instance Terminal.

```sh
sudo su -
mkdir aws
cd aws/
yum install git -y
```
git clone < CodeCommit Repo Url >

```sh
cd my-app/
```

#### vi appspec.yml

```sh
version: 0.0
os: linux
files:
  - source: /
    destination: var/www/html
hooks:
  AfterInstall:
    - location: scripts/install_nginx.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/install_nginx.sh
      timeout: 300
      runas: root

```

Create :-  mkdir scripts  ----->   cd scripts/

#### vi install_nginx.sh

```sh
#!/bin/bash

sudo apt-get update
sudo apt-get install -y nginx
```

#### vi start_nginx.sh

```sh
#!/bin/bash

sudo service nginx start
```


cd ..

#### Now We need to Push files into CodeCommit Repository

```sh
git status
git add .
git commit -m "my appspec.yml and scripts File"
git push origin master
```
