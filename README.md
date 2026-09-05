# Terraform AWS EC2 Deployment

## Project Overview

This project demonstrates Infrastructure as Code (IaC) using Terraform to provision an Amazon EC2 instance on AWS.

Terraform is used to automate the creation and management of AWS infrastructure.

## Technologies Used

- Terraform
- AWS
- Amazon EC2
- AWS CLI
- Amazon Linux 2023

## AWS Region

This project uses the AWS Mumbai region:

`ap-south-1`

## Infrastructure

Terraform creates the following resource:

- Amazon EC2 instance
- Instance type: `t3.micro`
- Amazon Linux 2023
- EC2 Name: `Terraform-EC2`

The Amazon Linux AMI is retrieved dynamically using AWS Systems Manager Parameter Store.

## Project Structure

```text
Terraform Project/
│
├── main.tf
├── .gitignore
├── README.md
└── .terraform.lock.hcl