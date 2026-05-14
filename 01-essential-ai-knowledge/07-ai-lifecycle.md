---
layout: default
title: "1.7 AI Development and Deployment Lifecycle"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 7
---

# 1.7 AI Development and Deployment Lifecycle Software Components

## What the exam tests

The stages of the AI/ML lifecycle, which tools operate at each stage, and NVIDIA's specific products that support production ML workflows (MLOps).

---

## The AI lifecycle

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  1. Data    │───▶│  2. Model   │───▶│  3. Model   │───▶│  4. Deploy  │
│  Collection │    │  Training   │    │  Validation │    │  & Serve    │
│  & Prep     │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                  │                  │                  │
       ▼                  ▼                  ▼                  ▼
  Data pipelines     NeMo, PyTorch     Experiment         TensorRT +
  RAPIDS (GPU         Megatron-LM       tracking           Triton
  data prep)          NCCL (dist.)      MLflow             Kubernetes
                       DGX cluster      W&B                Base Command
                                                                │
                                                                ▼
                                                    ┌──────────────────┐
                                                    │  5. Monitor &    │
                                                    │  Iterate         │
                                                    │  DCGM, Grafana   │
                                                    │  model drift,    │
                                                    │  retraining      │
                                                    └──────────────────┘
```

---

## Stage 1: Data collection and preparation

### Key activities
- Data ingestion from multiple sources (databases, object storage, real-time streams)
- Cleaning, deduplication, normalization, augmentation
- Labeling (for supervised learning) — manual or AI-assisted
- Feature engineering (for traditional ML) or tokenization (for LLMs)

### NVIDIA tools
- **RAPIDS** (cuDF, cuML): GPU-accelerated data processing — pandas/scikit-learn workflows on GPU
- **DALI (Data Loading Library)**: GPU-accelerated data augmentation pipeline, removes CPU bottleneck in vision training

### Storage requirements
- Training datasets can reach petabyte scale
- Requires parallel/distributed file systems (Lustre, GPFS, WEKA) for high-throughput reads
- Hot storage (NVMe/SSD) for active training datasets; object storage (S3-compatible) for cold archive

---

## Stage 2: Model training

### Key activities
- Hyperparameter selection (learning rate, batch size, architecture choices)
- Distributed training across multiple GPUs/nodes
- Checkpointing (saving model state periodically to recover from failures)
- Experiment tracking

### NVIDIA tools
- **NeMo**: training and fine-tuning LLMs, ASR, TTS
- **Megatron-LM**: core of NVIDIA's large-scale transformer training
- **NCCL**: collective communications for multi-GPU training
- **NVIDIA Base Command Platform**: manages DGX clusters, job scheduling, monitoring during training

### Infrastructure
- DGX systems with NVSwitch (8 GPUs, all-to-all 900GB/s)
- InfiniBand fabric for multi-node (NDR 400G or 800G/port)
- Slurm or Kubernetes for job scheduling

---

## Stage 3: Model validation and experimentation

### Key activities
- Evaluate model accuracy on held-out test set
- Compare runs (experiment tracking)
- Hyperparameter tuning (manual or automated — grid search, Bayesian)
- Model interpretability / explainability

### Common tools
- **MLflow**: open-source experiment tracking, model registry, artifact management
- **Weights & Biases (W&B)**: experiment tracking, visualization, hyperparameter sweeps
- **NVIDIA Triton** model analyzer: profiles models to find optimal batch sizes

---

## Stage 4: Deployment and serving

### Key activities
- Model optimization for target hardware
- Setting up inference serving infrastructure
- A/B testing, canary deployments
- SLA definition (latency p99, throughput target)

### NVIDIA tools
- **TensorRT**: optimize model for production GPU; FP16/INT8/FP8/FP4 quantization
- **TensorRT-LLM**: specifically for LLM inference (paged KV cache, continuous batching)
- **Triton Inference Server**: production serving; dynamic batching, multi-model, multi-backend
- **NVIDIA Fleet Command**: manage inference deployments at edge/branch locations

### Deployment targets
- **Cloud:** Kubernetes pods with GPU nodes; autoscaling on KEDA/HPA based on queue depth
- **On-prem:** DGX or NVIDIA-Certified Servers; Triton + Kubernetes or bare-metal
- **Edge:** NVIDIA Jetson (embedded AI); Orin modules; Fleet Command for management

---

## Stage 5: Monitoring and iteration

### Key activities
- Track model accuracy in production (detect data drift, concept drift)
- Monitor infrastructure health (GPU utilization, temperature, memory)
- Trigger retraining when model performance degrades
- A/B test new model versions

### NVIDIA tools
- **DCGM** (Data Center GPU Manager): GPU health, utilization, error tracking
- **Prometheus + Grafana**: time-series metrics, dashboards (DCGM Exporter bridges DCGM → Prometheus)
- **NVIDIA AI Enterprise**: includes monitoring integrations and lifecycle management

---

## MLOps pipeline summary

```
Data Prep → Training → Validation → Registry → Deploy → Monitor → (retrain)
RAPIDS      NeMo       MLflow       NGC         TensorRT   DCGM
DALI        Megatron   W&B          Triton      Triton     Grafana
            NCCL                    Base Cmd    Kubernetes  MLflow
```

---

## Self-check questions

1. Which NVIDIA library handles GPU-to-GPU communication during distributed training?
2. What is the purpose of TensorRT in the deployment stage?
3. Which NVIDIA tool manages DGX cluster jobs and monitoring during training?
4. What type of storage is required for high-throughput training data access?
5. What is model drift and why does it trigger retraining?

<details>
<summary>Answers</summary>
1. NCCL (NVIDIA Collective Communications Library)<br>
2. TensorRT optimizes a trained model for a specific NVIDIA GPU — it fuses layers, calibrates precision, and auto-tunes kernels to maximize throughput and minimize latency at inference time.<br>
3. NVIDIA Base Command Platform<br>
4. Parallel/distributed file systems (Lustre, GPFS, WEKA, WekaFS) with NVMe-backed storage — they provide the high IOPS and bandwidth needed to keep GPUs fed during training.<br>
5. Model drift: the distribution of real-world input data shifts away from the training distribution, causing accuracy to degrade over time. When accuracy drops below an acceptable threshold, the model needs to be retrained on fresh data.
</details>
