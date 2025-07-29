# EKS Fargate Cluster in Private Subnets (with EC2 Access)

This repository provisions a private Amazon EKS Cluster with Fargate profiles using CloudFormation, accessible only via an EC2 bastion host. Deployment is automated through Ansible and Jenkins CI/CD.

---

## 🔧 Technologies Used

- **CloudFormation**: VPC, EKS Cluster, Fargate profile, EC2, IAM
- **Ansible**: Automation of stack deployment
- **Jenkins**: CI/CD pipeline
- **kubectl + Helm**: Installed in EC2 for Kubernetes operations

---
