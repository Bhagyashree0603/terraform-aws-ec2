# AWS EC2 Infrastructure Provisioning with Terraform

> Infrastructure as Code (IaC) project demonstrating automated AWS EC2 provisioning, validation, verification, and infrastructure lifecycle management using Terraform.

## 📌 Project Overview

This project demonstrates how to provision and manage AWS infrastructure using **Terraform**, an Infrastructure as Code (IaC) tool.

The project provisions an **Amazon EC2 instance** in the AWS Mumbai (`ap-south-1`) region using Terraform. Instead of hard-coding an AMI ID, the configuration dynamically retrieves the latest Amazon Linux 2023 AMI through **AWS Systems Manager (SSM) Parameter Store**.

The complete infrastructure lifecycle was tested using Terraform:

**Initialize → Validate → Plan → Apply → Verify → Destroy**

---

## 🎯 Objectives

- Understand Infrastructure as Code using Terraform
- Provision AWS resources using declarative configuration
- Automate EC2 instance creation
- Dynamically retrieve the latest Amazon Linux AMI
- Validate infrastructure changes before deployment
- Verify deployed infrastructure through AWS Console
- Manage the complete infrastructure lifecycle using Terraform
- Practice Git and GitHub version control for Infrastructure as Code

---

## 🛠️ Technologies & Tools

| Technology / Tool | Purpose |
|---|---|
| **Terraform** | Infrastructure as Code and resource provisioning |
| **AWS** | Cloud infrastructure platform |
| **Amazon EC2** | Compute infrastructure |
| **AWS SSM Parameter Store** | Dynamic Amazon Linux AMI retrieval |
| **AWS CLI** | AWS authentication and command-line management |
| **Git** | Version control |
| **GitHub** | Source code hosting |

---

## ☁️ AWS Infrastructure

The Terraform configuration provisions:

- **Amazon EC2 instance**
- **Instance type:** `t3.micro`
- **Operating System:** Amazon Linux 2023
- **AWS Region:** `ap-south-1` (Mumbai)
- **Instance Name:** `Terraform-EC2`

### AMI Selection

The project uses AWS Systems Manager Parameter Store to dynamically retrieve the latest Amazon Linux 2023 AMI:

```hcl
Architecture / Workflow
                Developer
                    │
                    ▼
              Terraform Code
                 main.tf
                    │
                    ▼
            Terraform Provider
                  AWS
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
   AWS SSM Parameter       AWS EC2
      Store               Instance
          │                   │
          │                   ▼
          └────────────► Amazon Linux 2023

---

## Infrastructure Lifecycle
terraform init
      │
      ▼
terraform validate
      │
      ▼
terraform plan
      │
      ▼
terraform apply
      │
      ▼
EC2 Instance Created
      │
      ▼
AWS Console Verification
      │
      ▼
terraform destroy
      │
      ▼
EC2 Instance Removed
---

## Project Structure
terraform-aws-ec2/
│
├── main.tf                  # Terraform infrastructure configuration
├── README.md                # Project documentation
├── .gitignore               # Prevents Terraform state files from being committed
└── .terraform.lock.hcl      # Terraform provider dependency lock file
---

## ⚙️ Prerequisites

Before running this project, install and configure:

AWS account
Terraform
AWS CLI
Git
Verify Terraform
terraform --version
Verify AWS CLI
aws --version
Configure AWS CLI
aws configure

Verify AWS authentication:

aws sts get-caller-identity
---

## 🚀 Deployment
1. Initialize Terraform

Initialize the Terraform working directory and download the required AWS provider:
terraform init
2. Format the configuration
terraform fmt
3. Validate the configuration

Check the Terraform configuration for syntax and configuration errors:

terraform validate

Expected result:

Success! The configuration is valid.
4. Review the execution plan

Preview the infrastructure changes before applying them:

terraform plan

The plan should show that Terraform intends to create one EC2 instance.

5. Provision the EC2 instance
terraform apply

Confirm the deployment by entering:

yes

Terraform then provisions the EC2 instance in AWS.

📤 Terraform Outputs

The project exposes the EC2 instance ID and public IP address as Terraform outputs.

terraform output

Example:

instance_id = "i-xxxxxxxxxxxxxxxxx"
public_ip   = "xx.xx.xx.xx"
🔍 AWS Verification

After deployment, the EC2 instance can be verified through the AWS Management Console.

The deployed instance was verified with:

Name: Terraform-EC2
Instance type: t3.micro
Region: ap-south-1
Status: Running
Status checks: 3/3 passed
🧹 Infrastructure Cleanup

After completing testing, the infrastructure can be removed using:

terraform destroy

Confirm the operation by entering:

yes

This removes the EC2 instance managed by Terraform.

The project was successfully tested through both provisioning and destruction of the infrastructure.

🔐 Security & Best Practices
AWS credentials are not stored in the Terraform source code.
Terraform state files are excluded using .gitignore.
The .terraform directory is excluded from version control.
.terraform.lock.hcl is committed to maintain consistent provider dependency versions.
AWS root credentials should not be used for regular development; IAM users or roles with least-privilege permissions are preferred.
Sensitive Terraform state should be stored securely when using Terraform in production environments.
🧠 Key Learning Outcomes

Through this project, I gained practical experience with:

Infrastructure as Code (IaC)
Terraform configuration and resource management
AWS EC2 provisioning
AWS Systems Manager Parameter Store
Dynamic AMI selection
Terraform state management
Terraform plan/apply/destroy workflow
AWS CLI
Git and GitHub
Cloud infrastructure lifecycle management
📸 Project Evidence
Terraform Apply

Screenshot showing successful EC2 provisioning using Terraform.

AWS EC2 Console

Screenshot showing the Terraform-EC2 instance running successfully in AWS.

Terraform Outputs

Screenshot showing the EC2 instance ID and public IP address returned by Terraform.

Terraform Destroy

Screenshot showing successful infrastructure cleanup.
👨‍💻 Project Status

Status: Completed ✅

Cloud Platform: AWS

Infrastructure as Code: Terraform

Version Control: Git / GitHub
data "aws_ssm_parameter" "amazon_linux" {
  name = "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
}
