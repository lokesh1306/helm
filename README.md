# Helm Chart for Application Deployment on Private EKS

This repository contains a Helm chart for deploying a containerized application on a private Amazon EKS cluster. The Helm chart is designed to integrate seamlessly with the SSM Private Access and EKS Private Cluster projects, ensuring that all components operate within a private VPC and leverage the secure networking setup provided by those projects.

Objective
The Helm chart facilitates the deployment of Kubernetes applications in a private EKS cluster while adhering to best practices for security, scalability, and observability. It includes configurations for:
- Secure access to AWS services like SSM, RDS (Aurora MySQL), S3 and SQS via private VPC endpoints
- Network policies to restrict traffic within the cluster 
- High availability and resource optimization with Horizontal Pod Autoscaling (HPA) and Pod Disruption Budgets (PDB)

## Features
### Modular and Configurable
- Highly customizable via values.yaml for environment-specific requirements
- Includes all Kubernetes resources required for application deployment, such as:
- - Deployments
- - Services
- - Horizontal Pod Autoscalers
- - Pod Disruption Budgets
- - Role-based Access Control (RBAC)
- - Network Policies
- - Service Accounts (works with IRSA for AWS service access)

## Integration with Related Projects
- SSM Private Access: The Helm chart assumes IAM roles and private VPC endpoints configured by the SSM project are available for secure communication with AWS services
- EKS Private Cluster: Deployed applications leverage the private EKS cluster networking and logging setup for observability and security

## Security and Observability
- - Uses private VPC endpoints for secure access to AWS services like SSM, RDS, S3, and SQS
- - Uses Service Accounts and IAM Roles for Service Accounts (IRSA) for secure access to AWS services
- - Supports role-based access control (RBAC) for secure cluster operations
- - Configures network policies to restrict ingress and egress traffic within the cluster

## Includes logging integration for monitoring application behavior.
- Scalability and Reliability
- Configurable Horizontal Pod Autoscaler (HPA) for automatic scaling
- Pod Disruption Budget (PDB) to ensure high availability during updates and maintenance

## Repository Structure
```
helm/
├── Chart.yaml                # Metadata about the Helm chart
├── values.yaml               # Default configurations for the chart
├── templates/                # Kubernetes resource templates
│   ├── deployment.yaml       # Deployment resource definition
│   ├── service.yaml          # Service resource definition
│   ├── serviceaccount.yaml   # Service account configuration
│   ├── hpa.yaml              # Horizontal Pod Autoscaler configuration
│   ├── pdb.yaml              # Pod Disruption Budget configuration
│   ├── networkpolicy.yaml    # Network policy configuration
│   ├── role.yaml             # RBAC role definition
│   ├── rolebinding.yaml      # RBAC role binding
│   ├── NOTES.txt             # Post-installation notes
│   ├── tests/                # Templates for testing the deployment
│       └── test-connection.yaml
├── .helmignore               # Files to ignore when packaging the chart
├── .github/                  # GitHub Actions for CI/CD
│   └── workflows/helm.yml    # CI workflow for Helm chart validation
```

## Infrastructure Requirements
1. SSM Private Access:
- Private VPC endpoints for SSM and other AWS services
- IAM roles for secure access to AWS resources

2. EKS Private Cluster:
- Private EKS cluster with nodes in private subnets
- VPC peering for cross-VPC communication

## CI/CD with GitHub Actions:
The included GitHub Actions workflow (.github/workflows/helm.yml) automates:
- Linting and validating the Helm chart
- Ensuring compatibility with Kubernetes standards
- Creating a tag, packaging the Helm chart and releasing it to GitHub to be accessed by the EKS Private Cluster project

## Related Projects:
### SSM Private Access (https://github.com/lokesh1306/ssm-private-access):
- Sets up private VPC networking and IAM roles used by this Helm chart

### EKS Private Cluster (https://github.com/lokesh1306/eks-private-cluster):
Provides the private EKS cluster where this Helm chart is deployed