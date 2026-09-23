
## AWS EC2 Infrastructure Provisioning with Terraform ##

> A practical Infrastructure as Code (IaC) project demonstrating automated provisioning, validation, verification, and lifecycle management of an AWS EC2 instance using Terraform.

---

## 📌 Project Overview

This project demonstrates how to use **Terraform** to provision and manage AWS infrastructure using the Infrastructure as Code (IaC) approach.

The project provisions an **Amazon EC2 instance** in the AWS Mumbai (`ap-south-1`) region. The EC2 instance uses **Amazon Linux 2023**, with the AMI retrieved dynamically through **AWS Systems Manager (SSM) Parameter Store**.

The complete infrastructure lifecycle was implemented and tested using Terraform:

```text
Initialize → Validate → Plan → Apply → Verify → Destroy
```

This project provides practical experience with cloud infrastructure automation, Terraform workflows, AWS CLI, Git, and GitHub.

## 🎯 Project Objectives

The main objectives of this project are:

* Understand the concept of Infrastructure as Code (IaC)
* Learn Terraform fundamentals
* Provision AWS infrastructure using Terraform
* Automate EC2 instance creation
* Dynamically retrieve the latest Amazon Linux AMI
* Validate infrastructure configuration before deployment
* Preview infrastructure changes using Terraform Plan
* Verify deployed resources through the AWS Console
* Manage the complete infrastructure lifecycle
* Practice Git and GitHub for Infrastructure as Code projects

# 🛠️ Technologies and Tools
| Technology / Tool             | Purpose                                              |
| ----------------------------- | ---------------------------------------------------- |
| **Terraform**                 | Infrastructure as Code and AWS resource provisioning |
| **Amazon Web Services (AWS)** | Cloud infrastructure platform                        |
| **Amazon EC2**                | Virtual compute instance                             |
| **AWS Systems Manager (SSM)** | Dynamic Amazon Linux AMI retrieval                   |
| **AWS CLI**                   | AWS authentication and command-line management       |
| **Git**                       | Version control                                      |
| **GitHub**                    | Source code repository and project hosting           |
| **VS Code**                   | Development environment                              |

# ☁️ AWS Infrastructure

The Terraform configuration provisions the following AWS resource:

Amazon EC2  
Resource: EC2 Instance  
Instance Type: t3.micro  
Operating System: Amazon Linux 2023  
AWS Region: ap-south-1 (Mumbai)  
Instance Name: Terraform-EC2  

# 🔄 Dynamic AMI Selection

Instead of using a hard-coded AMI ID, this project uses AWS Systems Manager Parameter Store to retrieve the latest Amazon Linux 2023 AMI.
```
data "aws_ssm_parameter" "amazon_linux" {
  name = "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
}
```

The retrieved AMI is then used by the EC2 resource:
```
resource "aws_instance" "web" {
  ami           = data.aws_ssm_parameter.amazon_linux.value
  instance_type = "t3.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}
```

Why use a dynamic AMI?

A hard-coded AMI ID can become outdated and AMI IDs are region-specific.

Using the AWS Systems Manager public parameter allows Terraform to retrieve the appropriate Amazon Linux AMI dynamically for the selected AWS region.

# 🏗️ Architecture

                    Developer
                        │
                        ▼
                Terraform Configuration
                       main.tf
                        │
                        ▼
                 Terraform AWS Provider
                        │
            ┌───────────┴───────────┐
            │                       │
            ▼                       ▼
     AWS SSM Parameter          Amazon EC2
          Store                   Instance
            │                       │
            │                       ▼
            └──────────────► Amazon Linux 2023

🔁 Terraform Infrastructure Lifecycle

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

# 📁 Project Structure
terraform-aws-ec2/

│

├── main.tf         # Terraform infrastructure configuration       
├── README.md        # Project documentation         
├── .gitignore      # Excludes Terraform state and generated files        
└── .terraform.lock.hcl # Terraform provider dependency lock file    

The following are intentionally excluded from Git:

* .terraform/  
* terraform.tfstate  
* terraform.tfstate.backup    

## ⚙️ Prerequisites

Before running this project, make sure the following are installed:

* AWS Account  
* AWS CLI  
* Terraform  
* Git  
* Visual Studio Code or another code editor  

## 🔐 AWS CLI Configuration

Configure AWS CLI using:

* aws configure

Provide the required AWS credentials and default region.

For this project, the AWS region is:

* ap-south-1

Verify AWS authentication:

* aws sts get-caller-identity

This confirms that the AWS CLI can successfully communicate with the AWS account.

Security Note: AWS credentials should never be committed to GitHub or stored directly inside Terraform configuration files.

# 🚀 Deployment Process
Step 1: Create the Project Directory

Create a directory for the Terraform project:
```
mkdir terraform-aws-ec2
cd terraform-aws-ec2
```

Step 2: Create Terraform Configuration

Create a file named:
```
main.tf
```

The Terraform configuration defines the AWS provider, Amazon Linux AMI lookup, EC2 instance, and Terraform outputs.

Example:
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

data "aws_ssm_parameter" "amazon_linux" {
  name = "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
}

resource "aws_instance" "web" {
  ami           = data.aws_ssm_parameter.amazon_linux.value
  instance_type = "t3.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}

output "instance_id" {
  value = aws_instance.web.id
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

Step 3: Initialize Terraform

Initialize the Terraform working directory:
```
terraform init
```

This downloads the required AWS provider and prepares the Terraform working directory.

Step 4: Format the Terraform Configuration

Format the Terraform configuration:
```
terraform fmt
```

This ensures that the Terraform code follows standard formatting conventions.

Step 5: Validate the Configuration

Validate the Terraform configuration:
```
terraform validate
```

Expected result:
```
Success! The configuration is valid.
```
Step 6: Create the Terraform Plan

Preview the infrastructure changes:
```
terraform plan
```
Terraform analyzes the configuration and shows the resources that will be created, modified, or destroyed.

For this project, the plan should indicate that one EC2 instance will be created.

Step 7: Provision the EC2 Instance

Deploy the infrastructure:
```
terraform apply
```

Terraform asks for confirmation.

Enter:
```
yes
```
Terraform then creates the EC2 instance in AWS.

Step 8: View Terraform Outputs

After successful deployment, retrieve the output values:

terraform output

The project provides:
```
instance_id = "i-xxxxxxxxxxxxxxxxx"
public_ip   = "xx.xx.xx.xx"
```
These outputs provide useful information about the deployed EC2 instance.

## 🔍 AWS Deployment Verification

After Terraform successfully provisions the infrastructure, the EC2 instance can be verified through the AWS Management Console.

The deployed instance was verified with:

| Property             | Value             |
| -------------------- | ----------------- |
| **Name**             | `Terraform-EC2`   |
| **Instance Type**    | `t3.micro`        |
| **Region**           | `ap-south-1`      |
| **Operating System** | Amazon Linux 2023 |
| **Status**           | Running           |
| **Status Checks**    | 3/3 passed        |

This confirms that the EC2 instance was successfully provisioned by Terraform.

# 🧹 Infrastructure Cleanup

After testing the infrastructure, it should be removed to avoid unnecessary AWS resource usage.

Run:
```
terraform destroy
```
Confirm the operation by entering:
```
yes
```
Terraform removes the EC2 instance that it created.

Successful cleanup should result in:
```
Destroy complete! Resources: 1 destroyed.
```

# 🔐 Security and Best Practices

This project follows several basic Infrastructure as Code security practices:
```
Terraform State
```

Terraform state files are excluded from Git using .gitignore.
```
.terraform/
*.tfstate
*.tfstate.*
```
Terraform state files should not normally be committed to a public GitHub repository.

# AWS Credentials

AWS credentials should never be stored in:

main.tf  
README.md  
GitHub repositories  
Screenshots  
Other project files

Use AWS CLI configuration, IAM roles, or other secure credential mechanisms instead.

* IAM Permissions

For regular development and DevOps work, avoid using AWS root credentials.

Use an IAM user or IAM role with appropriate least-privilege permissions.

# 📸 Project Evidence

The project was tested successfully using Terraform and AWS.

Recommended screenshots for project documentation:

1. Terraform Initialization

Screenshot showing:
```
terraform init
```
and successful provider initialization.

2. Terraform Validation

Screenshot showing:
```
terraform validate
``` 
with successful validation.

3. Terraform Plan

Screenshot showing:
```
Plan: 1 to add, 0 to change, 0 to destroy.
``` 
4. Terraform Apply

Screenshot showing:
```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

5. AWS EC2 Console

Screenshot showing:

* Terraform-EC2
* Instance ID
* t3.micro  
* Running state  
* 3/3 status checks
6. Terraform Outputs

Screenshot showing:
```
instance_id = "i-xxxxxxxxxxxxxxxxx"
public_ip   = "xx.xx.xx.xx"
```
7. Terraform Destroy

Screenshot showing:
```
Destroy complete! Resources: 1 destroyed.
```

# 📈 Skills Demonstrated


This project demonstrates practical knowledge of:

Infrastructure as Code

        │
        ├── Terraform
        │
        ├── AWS EC2
        │
        ├── AWS SSM
        │
        ├── AWS CLI
        │
        ├── Git
        │
        └── GitHub

    GitHub

# 🔮 Future Improvements

The current project focuses on basic EC2 provisioning. It can be extended into a more production-oriented infrastructure project by adding:

Custom AWS VPC  
Public and private subnets  
Internet Gateway   
Route tables   
Security groups   
SSH key pair configuration  
IAM roles and policies  
Terraform variables   
Terraform modules   
Terraform remote state using Amazon S3   
Multiple environments such as Development, Staging, and Production  
CI/CD automation using GitHub Actions    
Automated infrastructure testing  
Monitoring and logging using AWS services  

# 📊 Project Status

Component	Status

Terraform Configuration	✅ Completed

AWS EC2 Provisioning	✅ Completed

Dynamic AMI Selection	✅ Completed

Terraform Validation	✅ Completed

Infrastructure Deployment	✅ Completed

AWS Console Verification	✅ Completed

Infrastructure Cleanup	✅ Completed

Git Repository	       ✅ Completed

GitHub Repository	🚀 Ready to Push

# 🏁 Conclusion

This project demonstrates the use of Terraform as an Infrastructure as Code tool to automate AWS infrastructure provisioning.

An Amazon EC2 instance was successfully provisioned in the AWS Mumbai region using Terraform, with the Amazon Linux 2023 AMI dynamically retrieved through AWS Systems Manager Parameter Store.

The infrastructure was validated, planned, deployed, verified through the AWS Console, and successfully destroyed after testing.

The project provides a foundation for further DevOps and cloud automation practices such as Terraform modules, remote state management, CI/CD pipelines, networking, IAM, monitoring, and multi-environment infrastructure.