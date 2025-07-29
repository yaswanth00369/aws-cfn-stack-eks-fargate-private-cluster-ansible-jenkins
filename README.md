# EKS Fargate Cluster in Private Subnets (with EC2 Access)

This repository provisions a private Amazon EKS Cluster with Fargate profiles using CloudFormation, accessible only via an EC2 bastion host. Deployment is automated through Ansible and Jenkins CI/CD.

---

## 🔧 Technologies Used

- **CloudFormation**: VPC, EKS Cluster, Fargate profile, EC2, IAM
- **Ansible**: Automation of stack deployment
- **Jenkins**: CI/CD pipeline
- **kubectl + Helm**: Installed in EC2 for Kubernetes operations

---


## 🧱 Architecture

> ✅ GitHub → Jenkins → Ansible Agent → AWS CloudFormation → EKS Cluster Provisioning

![EKS Architecture](Arch-Diagram.drawio.png)

---

## ✅ Typical Use Case
You'd use this stack when you want:

A secure, private-only EKS cluster for production or dev.

Minimal operational overhead using Fargate (no node groups).

An EC2 jumpbox (bastion host) to control and interact with your EKS cluster (without needing a VPN).

A fully reproducible and infrastructure-as-code-based deployment pipeline.

---

## 📘 High-Level Purpose
This CloudFormation stack creates:

A custom VPC with public and private subnets across two Availability Zones.

An EKS Cluster running on Fargate, which allows Kubernetes pods to run without provisioning EC2 worker nodes.

A bastion EC2 instance in a public subnet to interact with the private EKS cluster using kubectl.

Required IAM roles, security groups, route tables, and internet/NAT gateways for networking and access.

All output values are to retrieve the EKS cluster name, EC2 instance ID, and public IP for further automation.

---

## 🧱 Main Components in This Template
🔹 1. Networking Layer (VPC & Subnets)
VPC: Custom VPC with DNS support and hostnames enabled.

Subnets: 2 public and 2 private subnets across 2 AZs.

Internet Gateway and NAT Gateway: For internet access from public and private subnets.

Route Tables & Associations: Public and private routing for correct egress traffic flow.

---

## 🔹 2. Security & Access
Security Groups:

InstanceSecurityGroup: Allows SSH (port 22) from anywhere to EC2.

EKSClusterAdditionalSecurityGroup: Allows communication between the EC2 instance and the EKS control plane on port 443.

IAM Roles:

EC2AdminRole: Grants full admin rights to the EC2 instance.

EKSClusterRole: Required role to create and manage the EKS cluster.

FargateProfileRole: Role that EKS Fargate will assume for running pods.

---

## 🔹 3. EKS Cluster & Fargate Profile
EKS Cluster:

Private endpoint only: No public access; only EC2 inside VPC can connect.

Logging enabled for api and audit types.

Uses the 2 private subnets and its own SG.

Fargate Profile:

Automatically schedules pods in specific namespaces (default, kube-system, etc.) on Fargate.

---

## 🔹 4. EKS Client EC2 Instance
Located in a public subnet with internet access.

Automatically installs:

kubectl (for managing Kubernetes)

Helm (for package management in Kubernetes)

Uses EC2AdminRole to get permissions to manage the EKS cluster.

Can be used to generate kubeconfig and connect to the private EKS cluster directly.

---

## 🔹 5. Outputs
After successful stack creation, it will print:

✅ EKSClientInstanceId — to identify the bastion EC2.

✅ EKSClientPublicIP — to SSH into the EC2 for running kubectl.

✅ EKSClusterName — for building the kubeconfig or referencing the cluster in scripts.

---
