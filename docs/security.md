# Security Overview

This document summarizes the security posture of the **Distributed Deep Learning Cluster**.

## Defense-in-Depth

The cluster applies multiple layers of security controls:

### Identity & Access (IAM)

- Least-privilege IAM roles for EC2 instances
- Separate roles for training, data access, and model storage
- Instance profiles with scoped S3 and FSx permissions
- SSM Session Manager for secure access (no SSH keys)

### Network Isolation

- Private VPC subnets for all GPU instances
- EFA traffic isolated within placement group
- No public IP addresses on training nodes
- NAT Gateway for outbound package downloads
- Security groups restricting inter-node communication

### Data Protection

- **Training data**: S3 SSE-KMS encryption
- **FSx Lustre**: Encryption at rest and in transit
- **Model checkpoints**: Encrypted S3 storage
- **EFA traffic**: Encrypted within VPC

### Compute Security

- Amazon Linux 2 with security patches
- No SSH access – SSM only
- IMDSv2 required for instance metadata
- EBS encryption for root volumes

## Model Security

- Model artifacts encrypted in S3
- Signed model packages for deployment
- Access logging for model downloads
- Version control for training scripts

## Compliance Considerations

The architecture supports:

- SOC 2 Type II controls
- Data residency requirements
- IP protection for proprietary models

> For detailed security configurations, see `SECURITY.md` in the project root.
