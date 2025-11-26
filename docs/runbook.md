# Runbook

Operational guide for deploying, operating, and maintaining the **Distributed Deep Learning Cluster**.

## 1. Deployment

### Prerequisites

- AWS CLI configured with appropriate credentials
- Node.js 18+ and npm installed
- AWS CDK CLI installed (`npm install -g aws-cdk`)
- GPU instance quota approved (p4d/p3 instances)

### Deploy Steps

```bash
# Install dependencies
npm install

# Bootstrap CDK (first time only)
cdk bootstrap

# Deploy cluster infrastructure
cdk deploy --context environment=prod

# Note: GPU instances may take 5-10 minutes to launch
```

## 2. Training Operations

### Launch Training Job

```bash
# SSH to head node via SSM
aws ssm start-session --target i-xxxxx

# Activate environment
source /opt/conda/bin/activate pytorch

# Launch distributed training
torchrun --nproc_per_node=8 --nnodes=4 \
  --node_rank=$RANK --master_addr=$MASTER \
  train.py --config config.yaml
```

### Using DeepSpeed

```bash
deepspeed --hostfile hostfile.txt \
  --num_gpus 8 \
  train.py --deepspeed ds_config.json
```

### Using Horovod

```bash
horovodrun -np 32 -H host1:8,host2:8,host3:8,host4:8 \
  python train.py
```

## 3. Monitoring

### Key Metrics to Watch

- **GPU utilization**: Target >80% for efficiency
- **GPU memory**: Watch for OOM conditions
- **Network throughput**: EFA bandwidth utilization
- **Training loss**: Convergence monitoring
- **Checkpoint frequency**: Recovery point objectives

### Dashboards

Pre-configured dashboards for:

- GPU cluster health
- Training progress
- Network performance
- Cost tracking

## 4. Checkpointing

### Save Checkpoint

```python
# In training script
if rank == 0:
    torch.save({
        'epoch': epoch,
        'model_state_dict': model.state_dict(),
        'optimizer_state_dict': optimizer.state_dict(),
    }, f's3://bucket/checkpoints/epoch_{epoch}.pt')
```

### Resume from Checkpoint

```python
checkpoint = torch.load('s3://bucket/checkpoints/latest.pt')
model.load_state_dict(checkpoint['model_state_dict'])
```

## 5. Maintenance

### Regular Tasks

- Update NVIDIA drivers quarterly
- Patch OS security updates monthly
- Review GPU utilization weekly
- Clean up old checkpoints

### Teardown

```bash
cdk destroy --context environment=prod
```

> For troubleshooting common issues, see `docs/troubleshooting.md`.
