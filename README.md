# 🚀 Node.js Application Deployment on AWS ECS using Terraform

This project demonstrates how to deploy a **Node.js Express app** to **AWS ECS (Fargate)** using **Terraform** and **AWS Elastic Container Registry (ECR)**, behind an **Application Load Balancer (ALB)**.

---

## 🏗️ Architecture Overview

**Components:**
- **Node.js** – Sample REST API (`/contacts` endpoint)
- **Docker** – Containerizes the Node.js app
- **ECR (Elastic Container Registry)** – Stores the Docker image
- **ECS (Elastic Container Service)** – Runs the container using Fargate
- **ALB (Application Load Balancer)** – Provides public access
- **Terraform** – Infrastructure as Code (IaC) for provisioning resources

---

## ⚙️ Prerequisites

Before you start, make sure you have:

- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [Docker](https://docs.docker.com/get-docker/)
- AWS account with IAM access for ECS, ECR, and ALB
- Visual Studio Code or any terminal access to your EC2/local machine

---

## 📁 Project Structure

node-terraform-ecs/
├── App/
│ ├── src/
│ │ └── app.js # Node.js Express API
│ ├── package.json
│ └── Dockerfile
├── terraform/
│ ├── main.tf # ECS + ECR + ALB configuration
│ ├── variables.tf
│ ├── provider.tf
│ └── outputs.tf
└── README.md


---

## 🐳 Step 1: Build and Tag Docker Image

```bash
# From your project root
docker build -t demo-app-ecr-repo .

##Then tag it with your AWS ECR repository URI:
docker tag demo-app-ecr-repo:latest <AWS_ACCOUNT_ID>.dkr.ecr.<region>.amazonaws.com/demo-app-ecr-repo:latest

##Step 2: Authenticate Docker with ECR
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<region>.amazonaws.com

##Step 3: Push Image to ECR
docker push <AWS_ACCOUNT_ID>.dkr.ecr.<region>.amazonaws.com/demo-app-ecr-repo:latest

##Step 4: Initialize and Apply Terraform
cd terraform
terraform init
terraform fmt -recursive
terraform validate
terraform plan
terraform apply -auto-approve

##Step 5: Access the Application
alb_dns_name = cc-demo-app-alb-xxxxxxxx.eu-central-1.elb.amazonaws.com

Open the URL in your browser:

http://<alb_dns_name>/ → should return:
Node App is running successfully!

http://<alb_dns_name>/contacts → should return the sample JSON contact list.
