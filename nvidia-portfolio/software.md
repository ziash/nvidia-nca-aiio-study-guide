---
layout: default
title: Software Stack
parent: NVIDIA Portfolio Reference
nav_order: 5
---

# NVIDIA Software Stack Reference

See [Domain 1.1](../01-essential-ai-knowledge/01-nvidia-software-stack/) for full explanations. This page is a quick-reference index.

---

## Stack layers

```
Application Layer
  NeMo · RAPIDS · Triton IS · TensorRT-LLM · Merlin · Clara · Morpheus

Middleware / Frameworks
  PyTorch · TensorFlow · JAX (on NGC) · NeMo · RAPIDS

Inference Optimization
  TensorRT · TensorRT-LLM

Serving
  Triton Inference Server

Distributed Communication
  NCCL (GPU↔GPU collective ops)

Compute Primitives
  cuDNN · cuBLAS · cuSPARSE · DALI · cuDF

Platform
  CUDA · CUDA Runtime · CUDA Toolkit
  
GPU Management
  DCGM · nvidia-smi · NVML

Infrastructure / DPU
  DOCA SDK (BlueField programming)
  
Enterprise Platform
  NVIDIA AI Enterprise · NGC (container/model registry)
  Base Command Platform · Fleet Command
```

---

## Quick reference table

| Product | Category | Purpose |
|---|---|---|
| **CUDA** | Foundation | GPU compute programming model; all NVIDIA AI runs on CUDA |
| **cuDNN** | Library | Optimized DL primitives (conv, pooling, normalization) |
| **cuBLAS** | Library | GPU-accelerated BLAS (matrix operations) |
| **NCCL** | Library | Multi-GPU collective communications (all-reduce, broadcast) |
| **DALI** | Library | GPU-accelerated data augmentation pipeline for training |
| **RAPIDS** | Framework | GPU-accelerated data science (cuDF, cuML, cuGraph) |
| **TensorRT** | Optimizer | Inference model optimization; FP16/INT8/FP8/FP4 |
| **TensorRT-LLM** | Optimizer | LLM inference with paged KV cache, continuous batching |
| **Triton IS** | Server | Production inference serving; HTTP/gRPC; dynamic batching |
| **NeMo** | Framework | LLM/ASR/TTS training and fine-tuning |
| **Merlin** | Framework | End-to-end recommendation system (cuDF + HugeCTR + Triton) |
| **Clara** | Platform | Medical imaging AI |
| **BioNeMo** | Platform | Drug discovery / protein structure |
| **Morpheus** | Platform | Cybersecurity AI (real-time network analytics) |
| **NGC** | Registry | GPU-optimized container images and pre-trained models |
| **NVIDIA AI Enterprise** | Suite | Full-stack software with enterprise SLA; vGPU support |
| **Base Command** | Management | DGX cluster management, job scheduling, software lifecycle |
| **Fleet Command** | Management | Edge + cloud AI deployment management |
| **DCGM** | Monitoring | GPU health, telemetry, diagnostics |
| **DOCA** | DPU SDK | BlueField DPU programming framework |
| **NVLink / NVSwitch** | Interconnect | GPU-to-GPU high-bandwidth fabric (hardware) |
| **GPU Operator** | Kubernetes | Automates GPU software stack on K8s |
| **MIG Manager** | Kubernetes | Configures MIG partitions in K8s |

---

## NGC container tags (exam awareness)

NVIDIA NGC containers are tagged as:  
`nvcr.io/nvidia/<framework>:<version>-py3`

Example:
```
nvcr.io/nvidia/pytorch:24.01-py3
nvcr.io/nvidia/tensorflow:24.01-tf2-py3
nvcr.io/nvidia/tritonserver:24.01-py3
```

The tag format includes NVIDIA monthly release (YY.MM) + framework version + Python version. These are tested against a specific CUDA + driver combination listed in the container's README.
