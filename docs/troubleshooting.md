# Troubleshooting

Common issues and resolutions for the **Distributed Deep Learning Cluster**.

## GPU Issues

### 1. GPU Not Detected

**Symptom:** `nvidia-smi` shows no GPUs or CUDA errors.

**Resolution:**
- Verify instance type has GPUs (p3/p4d)
- Check NVIDIA driver installation: `nvidia-smi`
- Reinstall CUDA toolkit if needed
- Reboot instance after driver updates

### 2. GPU Out of Memory

**Symptom:** `CUDA out of memory` errors during training.

**Resolution:**
- Reduce batch size
- Enable gradient checkpointing
- Use mixed precision (FP16/BF16)
- Enable DeepSpeed ZeRO optimization
- Distribute model across more GPUs

### 3. Low GPU Utilization

**Symptom:** GPUs running at <50% utilization.

**Resolution:**
- Increase batch size
- Check data loading bottleneck – use more workers
- Verify FSx Lustre throughput
- Profile with NVIDIA Nsight

## Distributed Training Issues

### 4. NCCL Timeout

**Symptom:** Training hangs with NCCL timeout errors.

**Resolution:**
- Verify EFA is enabled on all instances
- Check security group allows EFA traffic
- Ensure all nodes in same placement group
- Set `NCCL_DEBUG=INFO` for diagnostics

### 5. Gradient Sync Failures

**Symptom:** Training crashes during all-reduce operations.

**Resolution:**
- Check network connectivity between nodes
- Verify consistent NCCL/CUDA versions
- Reduce gradient compression ratio
- Try different NCCL algorithms

### 6. Uneven GPU Memory Usage

**Symptom:** GPU 0 has higher memory than others.

**Resolution:**
- Enable DeepSpeed ZeRO Stage 3
- Use FSDP (Fully Sharded Data Parallel)
- Distribute optimizer states across GPUs

## Storage Issues

### 7. FSx Lustre Slow Performance

**Symptom:** Data loading slower than expected.

**Resolution:**
- Check FSx throughput capacity
- Verify data is striped across OSTs
- Use lazy loading for large datasets
- Pre-stage data before training

### 8. Checkpoint Save Failures

**Symptom:** Checkpoints fail to save to S3.

**Resolution:**
- Check IAM role has S3 write permissions
- Verify S3 bucket exists and is accessible
- Check disk space for local staging
- Use async checkpoint saving

## Cost Issues

### 9. Spot Instance Interruptions

**Symptom:** Training interrupted by spot termination.

**Resolution:**
- Implement checkpoint-resume logic
- Use spot instance interruption notices
- Consider capacity reservations for critical jobs
- Mix on-demand and spot instances

> For architecture details, see the project README.
