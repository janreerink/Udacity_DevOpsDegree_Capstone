# Udacity_DevOpsDegree_Capstone
Final project for the Udacity DevOps Nanodegree. 

## Infrastructure setup
Deploy an EKS cluster (based on AWS quickstart template available [here](https://aws.amazon.com/de/quickstart/architecture/amazon-eks/)):

Follow this guide to install eksctl:
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

### Install Jenkins on Bastion Host
The Quickstart Template provisions one EC2 instance as bastion host to the EKS cluster. Install Jenkins on this instance:
- Update OS, install JDK, get the repo for latest Jenkins version, install and verify service is running.
- `sudo yum update`
- `sudo yum upgrade`
- `sudo amazon-linux-extras install java-openjdk11`
- `sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo`
- `sudo rpm --import http://pkg.jenkins.io/redhat/jenkins.io.key`
- `sudo yum install jenkins`
- `sudo service jenkins start`

### Configure Jenkins
- Navigate to port 8080 on EC2 instance
- Use `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` to retrieve install pwd, create new admin
- Install suggested plugins, then manually add Blue Ocean Aggregator
- Connect git to jenkins (using github access token) to build pipeline. Change pipeline to check repo for changes every 2 minutes.
- In Jenkins credentials menu add dockerhub credentials for pipeline; also add to pipeline configuration

## Install hadolint
- `wget https://github.com/hadolint/hadolint/releases/download/v1.18.0/hadolint-Linux-x86_64`
- `mv hadolint-Linux-x86_64 hadolint`
- `chmod +x hadolint`
- `sudo cp hadolint-Linux-x86-64 /usr/local/bin/`
- `export PATH=$PATH:/usr/local/bin/hadolint-Linux-x86-64`


## Install docker CE on bastion host
- `sudo amazon-linux-extras install docker`

Alternatively set up new bastion host based on ubuntu (set VPC, publicsubnet1, IAM role and security group)
- Install aws authenticator
- Copy and export kubectl config
- export KUBECONFIG=./test.conf   
- Install docker



## Use jenkins pipeline to deploy
Jenkinsfile contains a pipeline that:
### Linting stage
Performs a linting step of the Dockerfile and the python script.
- todo: screenshot failed linting
- todo: screenshot succesful linting
### Docker stage
- build an image from the dockerfile
- pushes the image to the public docker repo
### Deploy stage
- Runs the image created in the step above
- Uses loadbalancer.yaml to create service




