---
layout: default
title: "1.1 NVIDIA Software Stack"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 1
---

# 1.1 NVIDIA Software Stack in an AI Environment

## What the exam tests

The names, purposes, and relationships of the major layers in the NVIDIA software ecosystem — from bare-metal drivers up to application frameworks.

---

## Stack overview

```
┌─────────────────────────────────────────────────────────────┐
│              AI Applications & Services                      │
│   (LLM inference, recommendation, computer vision, etc.)    │
├──────────────┬──────────────┬──────────────┬────────────────┤
│   TensorRT   │     NeMo     │    RAPIDS    │   Triton IS    │
│  (inference  │  (LLM/ASR/  │  (data sci)  │  (inference    │
│   optim.)    │   TTS fw.)   │              │   serving)     │
├──────────────┴──────────────┴──────────────┴────────────────┤
│              NVIDIA AI Enterprise (software suite)           │
├──────────────────────────────────────────────────────────────┤
│                    NGC (Container Registry)                   │
├──────────────────────────────────────────────────────────────┤
│           CUDA / cuDNN / cuBLAS / NCCL / cuSPARSE           │
│                   (compute primitives)                        │
├──────────────────────────────────────────────────────────────┤
│              GPU Driver + CUDA Runtime                        │
├──────────────────────────────────────────────────────────────┤
│                   GPU Hardware                                │
└──────────────────────────────────────────────────────────────┘
```

---

## Layer by layer

### CUDA — the foundation

**CUDA** (Compute Unified Device Architecture) is NVIDIA's parallel computing platform and programming model. It exposes the GPU's thousands of cores to general-purpose programs via an extension to C/C++/Python.

- Introduced 2007; now the de-facto standard for GPU compute
- Every NVIDIA AI framework (PyTorch, TensorFlow, JAX) executes on CUDA underneath
- CUDA toolkit includes: compiler (nvcc), runtime libraries, debugger, profiler (Nsight)

**Key libraries built on CUDA:**

| Library | Purpose |
|---|---|
| **cuDNN** | Optimized primitives for deep neural networks (convolutions, pooling, normalization) |
| **cuBLAS** | GPU-accelerated BLAS (matrix/vector operations) |
| **NCCL** | Collective communications for multi-GPU/multi-node (all-reduce, broadcast) — critical for distributed training |
| **cuSPARSE** | Sparse matrix operations |
| **RAPIDS** | GPU-accelerated data science (see below) |

---

### NGC — NVIDIA GPU Cloud

**NGC** is NVIDIA's catalog of GPU-optimized containers, pre-trained models, and SDKs.

- Contains PyTorch, TensorFlow, JAX, TensorRT, NeMo, and others — all pre-configured and performance-tuned
- Containers are versioned and tested against specific GPU driver + CUDA combinations
- Available on-prem (pulling to your server) and directly on major cloud GPU instances
- **Free to access** — NGC is a catalog, not a cloud service

**Why it matters for enterprise AI:** Instead of manually installing and version-managing 20 interdependent libraries, you pull a single NGC container with everything pre-configured. This is the standard starting point for NVIDIA-certified AI deployments.

---

### NVIDIA AI Enterprise

**NVIDIA AI Enterprise** is a full-stack, enterprise-grade software suite that runs on NVIDIA-certified hardware (bare metal, VMware vSphere, Red Hat OpenShift, etc.).

- Includes: NVIDIA Triton Inference Server, TensorRT, NeMo, RAPIDS, CUDA-X libraries
- Comes with **enterprise support SLA** from NVIDIA (critical for production deployments)
- Licenses per GPU — annual subscription
- Enables running AI on VMs (vGPU) with the same performance guarantees as bare metal

---

### TensorRT — inference optimization

**TensorRT** is NVIDIA's SDK for high-performance deep learning inference.

- Takes a trained model (ONNX, TensorFlow, PyTorch) and optimizes it for a target GPU
- Optimizations: layer fusion, precision calibration (FP32 → FP16 → INT8 → FP4), kernel auto-tuning, memory layout optimization
- Results in 2–10× lower latency and higher throughput vs running the raw framework
- **TensorRT-LLM**: extension specifically for large language model inference (paged KV cache, in-flight batching, continuous batching)

---

### Triton Inference Server

**NVIDIA Triton Inference Server** (open source) provides a standardized HTTP/gRPC inference serving framework.

- Serves any model format: TensorRT, ONNX, PyTorch TorchScript, TensorFlow SavedModel, Python custom backends
- Dynamic batching: automatically batches concurrent requests to maximize GPU utilization
- Concurrent model execution: run multiple models simultaneously on one GPU
- Model management: load/unload models at runtime without server restart
- Integrates with Kubernetes for horizontal scaling

---

### NeMo — conversational AI framework

**NVIDIA NeMo** is an open-source framework for building, training, and fine-tuning large language models, automatic speech recognition (ASR), and text-to-speech (TTS) models.

- Built on PyTorch Lightning; distributed training via Megatron-LM
- Supports LLM fine-tuning (SFT, RLHF, PEFT/LoRA)
- Pre-trained model collections available on NGC
- NeMo Guardrails: adds safety/topic control for LLM applications in production

---

### RAPIDS — GPU-accelerated data science

**RAPIDS** is a suite of open-source libraries that accelerates data science pipelines entirely on GPU:

| Library | Equivalent | Purpose |
|---|---|---|
| cuDF | pandas | DataFrame operations on GPU |
| cuML | scikit-learn | ML algorithms on GPU |
| cuGraph | NetworkX | Graph analytics on GPU |
| cuSpatial | GeoPandas | Geospatial analytics |

RAPIDS integrates with PyTorch and TensorFlow — GPU-accelerated preprocessing feeds directly into GPU training without CPU↔GPU round-trips.

---

## Software stack relationships (exam summary)

```
Need to run inference fast?         → TensorRT
Need to serve inference at scale?   → Triton Inference Server
Need to train/fine-tune an LLM?     → NeMo (+ NCCL for multi-GPU)
Need GPU-accelerated data prep?     → RAPIDS
Need enterprise support + vGPU?     → NVIDIA AI Enterprise
Need optimized containers/models?   → NGC
All of the above built on:          → CUDA
```

---

## Self-check questions

1. What does NCCL stand for and why is it essential for distributed training?
2. What does TensorRT do to a trained model before deployment?
3. What is the difference between NGC and NVIDIA AI Enterprise?
4. Which NVIDIA framework is used for training large language models and conversational AI?
5. An organization wants to run AI models on VMware VMs with enterprise support. Which NVIDIA product enables this?

<details>
<summary>Answers</summary>
1. NVIDIA Collective Communications Library. It provides all-reduce, broadcast, and other collective operations across GPUs on multiple nodes — essential for synchronizing gradients during distributed training.<br>
2. TensorRT optimizes the model for a specific GPU: fuses layers, calibrates precision (FP32→INT8), and auto-tunes kernels. The result is a TensorRT Engine with significantly lower latency.<br>
3. NGC is a free catalog of GPU-optimized containers and models. NVIDIA AI Enterprise is a paid software suite with enterprise SLA support that includes NGC content plus vGPU support and production-grade tools.<br>
4. NeMo (NVIDIA NeMo)<br>
5. NVIDIA AI Enterprise — it's certified on VMware vSphere (with vGPU) and provides enterprise support.
</details>
