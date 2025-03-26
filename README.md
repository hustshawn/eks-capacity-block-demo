# EKS Capacity Block for ML Workloads

This repository contains Terraform configurations and Kubernetes manifests for deploying and managing ML workloads on Amazon EKS using ML Capacity Blocks.

## Overview

This project provides infrastructure as code to:

1. Create an Amazon EKS cluster optimized for ML workloads
2. Configure node groups with ML Capacity Block reservations for P5en instances
3. Deploy necessary components for GPU and EFA (Elastic Fabric Adapter) support
4. Include sample ML workload deployments for GenAI models

## Prerequisites

- AWS CLI configured with appropriate permissions
- Terraform >= 1.3
- kubectl
- Helm
- An existing ML Capacity Block reservation ID

## Architecture

The infrastructure includes:

- VPC with public and private subnets across multiple AZs
- EKS cluster (v1.32)
- Node groups:
  - P5en node group using ML Capacity Block reservation
  - Optional G5 node group for smaller GPU workloads
  - Default node group for cluster services
- NVIDIA device plugin for GPU support
- AWS EFA device plugin for high-performance networking
- Sample ML workload deployments

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/hustshawn/eks-capacity-block-demo
cd eks-capacity-block
```

### 2. Configure your ML Capacity Block reservation

Create a `dev.auto.tfvars` file with your ML Capacity Block reservation ID and AWS region:

```hcl
capacity_reservation_id = "cr-xxxxxxxxxxxxxxxxx"
aws_region = "ap-northeast-1"
```

### 3. Initialize and apply Terraform

```bash
terraform init
terraform plan -out=planfile
terraform apply planfile
```

### 4. Configure kubectl

After successful deployment, configure kubectl to connect to your cluster:

```bash
aws eks --region <your-region> update-kubeconfig --name ml-capacity-block
```

### 5. Deploy ML workloads

Deploy sample ML workloads using the provided manifests:

```bash
kubectl apply -f ds-p5en.yaml
```

## Sample Deployments

This repository includes sample Kubernetes manifests for:

- DeepSeek-R1 model deployment with SGLang
- NVIDIA Triton server deployment

## Customization

You can customize the deployment by modifying:

- `main.tf` - Core infrastructure settings
- `eks.tf` - EKS cluster and node group configurations
- `helm.tf` - Helm chart deployments
- Kubernetes manifests for specific workloads

## Cleanup

To destroy all resources created by this project:

```bash
terraform destroy
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
