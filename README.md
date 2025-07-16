# Provision and Configure AWS EC2 Instance using Terraform and Ansible

## Overview

This project demonstrates how to **provision** an AWS EC2 instance using **Terraform** and then **configure** it using **Ansible**. The workflow is fully automated and ideal for DevOps engineers, cloud learners, and anyone looking to implement Infrastructure as Code (IaC) and Configuration Management best practices.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Step 1: Provision Infrastructure with Terraform](#step-1-provision-infrastructure-with-terraform)
- [Step 2: Configure EC2 with Ansible](#step-2-configure-ec2-with-ansible)
- [Accessing Your EC2 Instance](#accessing-your-ec2-instance)
- [Troubleshooting](#troubleshooting)
- [References](#references)

---

## Installation

### Prerequisites

Before you begin, ensure you have the following installed:

- **AWS Account:** [Sign up here](https://aws.amazon.com/free/) if you don't have one.
- **AWS CLI:** [Installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- **Terraform:** [Installation guide](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- **Ansible:** [Installation guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- **Python 3:** Required for Ansible (often pre-installed on most systems).

**Configure AWS credentials:**

```sh
aws configure
```

---

## Project Structure

```
How-To-Provision-and-Configure-an-AWS-EC2-Instance-using-Terraform-and-Ansible/
├── ansible/
│   ├── ansible.cfg         # Ansible configuration
│   └── playbook.yaml       # Main Ansible playbook
├── inventory.tftpl         # Template for dynamic Ansible inventory
├── inventory.tf            # Terraform resource to generate inventory
├── main.tf                 # Main Terraform configuration
├── output.tf               # Terraform outputs (public IPs, etc.)
├── variables.tf            # Terraform variables
├── .ssh/                   # Generated SSH keys (after apply)
├── README.md               # This file
└── ...
```

---

## Step 1: Provision Infrastructure with Terraform

Terraform will:

- Create a VPC, subnet, and security groups
- Launch one or more EC2 instances
- Generate an SSH key pair for Ansible
- Output a dynamic Ansible inventory file

**Run the following commands:**

```sh
# Initialize Terraform
terraform init

# (Optional) Preview changes
terraform plan

# Apply configuration (provisions AWS resources)
terraform apply
# Type 'yes' when prompted
```

**After completion:**

- SSH key is saved to `.ssh/ansible-ssh-key.pem`
- Ansible inventory file (`inventory.ini`) is generated with EC2 public IPs

---

## Step 2: Configure EC2 with Ansible

Ansible will:

- Connect to EC2 instances using the generated SSH key
- Install and start Nginx (or other software as defined in the playbook)

**Run the following commands:**

```sh
# Ensure SSH key permissions are correct
chmod 600 .ssh/ansible-ssh-key.pem

# Run the Ansible playbook
ansible-playbook -i inventory.ini ansible/playbook.yaml
```

---

## Accessing Your EC2 Instance

SSH into your instance using:

```sh
ssh -i .ssh/ansible-ssh-key.pem ubuntu@<instance-public-ip>
```

Find the public IP in the Terraform output or `inventory.ini`.

---

## Customisation

- **Number of EC2 Instances:** Edit `instance_count` in `variables.tf`.
- **Instance Type/AMI:** Edit `instance_type` and `ami` in `variables.tf`.
- **Ansible Tasks:** Edit `ansible/playbook.yaml` to add or modify configuration steps.

---

## Troubleshooting

- **SSH Permission Denied:** Ensure `.ssh/ansible-ssh-key.pem` has `chmod 600` permissions and you are using the correct username (e.g., `ubuntu` for Ubuntu AMIs).
- **Ansible Connection Errors:** Check that the EC2 security group allows SSH (port 22) from your IP.
- **Terraform Errors:** Double-check your AWS credentials and region settings.

---

## References

- [LinkedIn Article: How to Provision & Configure AWS EC2 Instance using Terraform & Ansible](https://www.linkedin.com/pulse/how-provision-configure-aws-ec2-instance-using-terraform-%D0%B8%D0%B2%D0%B0%D0%BD%D0%BE%D0%B2/)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Ansible Documentation](https://docs.ansible.com/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
