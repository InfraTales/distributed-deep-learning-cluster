# Cost Analysis (₹)

This document provides cost estimates for the **Distributed Deep Learning Cluster** in **Indian Rupees (₹)**.

## Production Environment

At production scale (multi-GPU training), the architecture typically costs:

| Service | Monthly Cost (₹) | Notes |
|---------|------------------|-------|
| **EC2 p4d.24xlarge** | ₹400,000–600,000 | 8x A100 GPUs per instance |
| **EC2 p3.16xlarge** | ₹200,000–350,000 | 8x V100 GPUs per instance |
| **EFA Networking** | Included | Elastic Fabric Adapter |
| **FSx for Lustre** | ₹40,000–80,000 | High-performance storage |
| **S3 (datasets)** | ₹15,000–30,000 | Training data and checkpoints |
| **CloudWatch** | ₹5,000–10,000 | Monitoring and logs |
| **Total (p4d cluster)** | **₹500,000–750,000** | ~$6,250–9,375/month |

## Training Job Costs

For on-demand training jobs:

| Cluster Size | Hourly Cost (₹) | Notes |
|-------------|-----------------|-------|
| 1x p4d.24xlarge | ₹2,500/hr | Single-node 8-GPU |
| 4x p4d.24xlarge | ₹10,000/hr | 32 GPUs distributed |
| 8x p4d.24xlarge | ₹20,000/hr | 64 GPUs distributed |
| 16x p4d.24xlarge | ₹40,000/hr | 128 GPUs distributed |

## Cost Optimization Strategies

- **Spot instances** – Up to 70% savings for fault-tolerant training
- **Reserved capacity** – 30-40% savings for predictable workloads
- **Capacity reservations** – Guarantee GPU availability
- **Mixed precision** – FP16/BF16 training reduces time by 2-3x
- **Gradient compression** – Reduce network overhead
- **Checkpoint to S3** – Resume from interruptions
- **Right-size cluster** – Match GPUs to model size

## LLM Training Estimates

| Model Size | Cluster | Training Time | Est. Cost (₹) |
|-----------|---------|---------------|---------------|
| 7B params | 8x p4d | ~1 week | ₹35,00,000 |
| 13B params | 16x p4d | ~2 weeks | ₹1,12,00,000 |
| 70B params | 64x p4d | ~4 weeks | ₹8,96,00,000 |

## Related Documentation

See the project README for architecture details.md` for configuration options.
