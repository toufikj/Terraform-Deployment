# Terraform EC2 Deployment

## Prerequisites
- Install Terraform: https://learn.hashicorp.com/tutorials/terraform/install-cli
- AWS CLI with proper credentials configured
- An existing SSH key pair in AWS

## Steps to Deploy

1. Clone the repository:
   ```bash
   git clone <repo-url>
   cd terraform


Terraform EC2 Deployment & GitHub Actions CI/CD Pipeline
This repository contains Terraform scripts to provision an EC2 instance in AWS and a GitHub Actions workflow to automate the deployment using Terraform. Additionally, it demonstrates how to push a Docker image to Amazon ECR using GitHub Actions.

Prerequisites
AWS Account: You need an AWS account with appropriate permissions to create EC2 instances, VPCs, and security groups.
Terraform: Install Terraform on your local machine. Terraform installation guide.
GitHub Account: You need a GitHub account to push changes and trigger GitHub Actions workflows.
Project Structure
bash
Copy code
├── .github
│   └── workflows
│       ├── deploy_ec2.yml       # GitHub Actions workflow to deploy EC2 using Terraform
│       └── docker_push.yml      # GitHub Actions workflow to push Docker image to ECR
├── terraform
│   ├── main.tf                  # Main Terraform configuration for EC2
│   ├── variables.tf             # Variables used in Terraform
│   └── outputs.tf               # Outputs from the Terraform execution
├── app
│   ├── app.py                   # Sample Python application
│   └── requirements.txt         # Python dependencies
├── Dockerfile                   # Dockerfile for the Python app
└── README.md                    # Documentation and instructions
Part 1: Running the Terraform Script Locally
1. AWS CLI Configuration
To interact with AWS services from your local machine, you need to configure the AWS CLI with your credentials.

Install the AWS CLI: Follow the official guide for your OS AWS CLI installation guide.

Configure AWS CLI: Run the following command and provide your AWS credentials.

bash
Copy code
aws configure
You'll be prompted for the following:

AWS Access Key ID: Your AWS IAM user access key ID.
AWS Secret Access Key: Your AWS IAM user secret access key.
Default region name: e.g., us-east-1.
Default output format: e.g., json.
2. Setting Up Terraform Variables
Ensure you have an existing EC2 Key Pair in AWS for SSH access. You can create a new key pair or use an existing one.

Create or edit the terraform/terraform.tfvars file with your key pair name and region:
bash
Copy code
touch terraform/terraform.tfvars
Example terraform.tfvars:

hcl
Copy code
key_name = "firstEC2"
region   = "us-east-1"
3. Running the Terraform Script
Initialize Terraform: Download necessary plugins/providers.
bash
Copy code
cd terraform
terraform init
Plan the infrastructure changes: Preview what Terraform will create.
bash
Copy code
terraform plan
Apply the changes: Deploy the EC2 instance in AWS.
bash
Copy code
terraform apply -auto-approve
Get the EC2 instance public IP: After successful deployment, the public IP will be printed in the output. You can use it to SSH into the instance.
bash
Copy code
ssh -i <path-to-your-key.pem> ec2-user@<instance-public-ip>
Part 2: Configuring GitHub Actions for CI/CD
1. Set up GitHub Secrets for AWS Credentials
To allow GitHub Actions to interact with AWS and deploy the EC2 instance, you need to store your AWS credentials as secrets in your GitHub repository.

Go to GitHub Repository > Settings > Secrets and Variables > Actions > New repository secret.
Add the following secrets:
AWS_ACCESS_KEY_ID: Your AWS IAM access key ID.
AWS_SECRET_ACCESS_KEY: Your AWS IAM secret access key.
AWS_REGION: Your AWS region (e.g., us-east-1).
AWS_KEY_NAME: The name of your EC2 key pair (e.g., firstEC2).
2. GitHub Actions Workflow to Deploy EC2
The GitHub Actions workflow (deploy_ec2.yml) automatically deploys the EC2 instance using Terraform whenever you push changes to the main branch or merge a pull request.

3. Triggering the Workflow
To trigger the EC2 deployment:

Make changes to the repository (e.g., update Terraform files or application code).
Push changes to the main branch to trigger the GitHub Actions workflow:
bash
Copy code
git add .
git commit -m "Trigger EC2 deployment workflow"
git push origin main
Monitor the Workflow: Go to the Actions tab in your GitHub repository to see the progress of the workflow.
Once the workflow is completed, your EC2 instance will be provisioned automatically in AWS.

Part 3: Pushing a Docker Image to Amazon ECR
This part of the project demonstrates pushing a Docker image of a Python app to an Amazon ECR repository.

1. Set Up an ECR Repository
Go to the AWS Management Console > ECR (Elastic Container Registry).
Create a new repository to store your Docker images.
Note the repository URI (e.g., 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-repo).
2. GitHub Secrets for ECR Authentication
In addition to the existing secrets, add the following secret in GitHub:

AWS_ECR_REPO_URI: Your ECR repository URI (e.g., 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-repo).
3. GitHub Actions Workflow to Push Docker Image
The docker_push.yml workflow builds the Docker image, authenticates with ECR, and pushes the image to your repository.

4. Triggering the Docker Push Workflow
Modify the Python application (app/app.py) or the Dockerfile.
Push changes to the main branch to trigger the Docker build and push process:
bash
Copy code
git add .
git commit -m "Trigger Docker image build and push"
git push origin main
Monitor the Workflow: Go to the Actions tab on GitHub to see the Docker image build and push progress.
Once completed, the Docker image will be available in your ECR repository.
