---
layout: default
title: "1.2 Training vs Inference"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 2
---

# 1.2 Training vs Inference Architecture Requirements

## What the exam tests

The different compute, memory, and latency profiles of training and inference workloads — and which NVIDIA hardware and software targets each.

---

## The two phases of AI deployment

```
┌──────────────────────────────────────────────────────────────┐
│  TRAINING                          INFERENCE                  │
│                                                               │
│  Objective: Find model weights     Objective: Use weights to  │
│  that minimize loss function       answer a query fast        │
│                                                               │
│  Runs: Once (or iteratively)       Runs: Millions of times    │
│  Duration: Hours to weeks          Duration: Milliseconds     │
│  Users: Data scientists            Users: End-users/apps      │
└──────────────────────────────────────────────────────────────┘
```

---

## Training requirements

### Compute profile
- **Massive FLOPS** — billions to trillions of multiply-accumulate operations per forward + backward pass
- **FP16/BF16 training** — most modern training runs in BF16 (better dynamic range than FP16) with Tensor Core acceleration
- **Gradient computation** — backpropagation requires storing all intermediate activations; memory-hungry

### Memory requirements
- **Large model memory** — GPT-4 class models require tens of TB of GPU memory in aggregate
- **High memory bandwidth** — weight loading and gradient accumulation stress HBM bandwidth
- **Distributed training requires NVLink + InfiniBand** — model parallelism, data parallelism, and pipeline parallelism spread across many GPUs

### Parallelism strategies

| Strategy | What it distributes | When to use |
|---|---|---|
| **Data parallelism** | Mini-batches across GPUs | Fits model on one GPU; scale throughput |
| **Model/tensor parallelism** | Model layers/tensors across GPUs | Model too large for one GPU |
| **Pipeline parallelism** | Layers in pipeline stages | Very deep models with micro-batch pipelining |
| **Sequence parallelism** | Long context attention | Extremely long sequences |

NCCL handles the all-reduce operations that synchronize gradients across GPUs/nodes.

### Best hardware for training
- **H100 SXM5 / B200 SXM** — high HBM bandwidth, 4th/5th gen NVLink, highest-density compute
- **DGX H100 / DGX B200** — 8 GPUs all-to-all connected via NVSwitch; optimal for large model training
- **InfiniBand (HDR/NDR/800G)** — required for multi-node training at scale; RoCE is an alternative

---

## Inference requirements

### Compute profile
- **Low latency** — user-facing applications require sub-100ms response times
- **High throughput** — serving many concurrent users simultaneously
- **INT8 / FP8 / FP4 quantization** — reduced precision maintains accuracy while doubling or quadrupling throughput vs FP16

### Memory requirements
- **KV cache** — transformer inference stores key-value tensors per token per layer; grows with context length
- **Model fits in GPU memory** — inference typically runs on a single GPU or small GPU cluster
- **Memory bandwidth matters more than FLOPS** at small batch sizes (memory-bandwidth bound)

### Inference optimization techniques

| Technique | Description |
|---|---|
| **Quantization** | FP32 → FP16 → INT8 → FP8 → FP4; reduces weight size and increases throughput |
| **Layer fusion** | Combine multiple operations into one kernel launch (TensorRT) |
| **Continuous batching** | Process tokens from multiple requests in the same forward pass as they arrive |
| **Paged KV cache** | Store KV cache in non-contiguous pages, like OS virtual memory (TensorRT-LLM) |
| **Speculative decoding** | Draft model generates candidates; target model verifies in parallel |

### Best hardware for inference
- **H100 / B200** — for large LLM inference requiring maximum throughput
- **L40S** — for inference + graphics combined; cost-efficient for medium models
- **L4** — edge/cloud inference for smaller models; very power-efficient (72W TDP)
- **TensorRT** — always use to optimize models before deploying to any NVIDIA GPU

---

## Side-by-side comparison

| Dimension | Training | Inference |
|---|---|---|
| Primary metric | Throughput (samples/sec, tokens/sec training) | Latency (TTFT, tokens/sec generation) |
| Precision | FP32, BF16, FP16 | INT8, FP8, FP4, FP16 |
| Batch size | Large (improves GPU utilization) | Small to large (dynamic batching) |
| Memory need | Very high (activations + gradients + weights) | Moderate (weights + KV cache only) |
| Communication | All-reduce across many GPUs (NCCL) | Tensor parallel across a few GPUs |
| Key NVIDIA tool | NeMo, Megatron-LM, NCCL | TensorRT, Triton, TensorRT-LLM |
| Preferred interconnect | InfiniBand / NVLink | NVLink (within node), Ethernet (scale-out) |

---

## Key terms

- **TTFT (Time to First Token):** Latency from request to first token returned — critical for interactive applications
- **Tokens/sec:** Throughput metric for generation workloads
- **MFU (Model FLOP Utilization):** Fraction of theoretical peak FLOPS actually used; good training runs achieve 40–60% MFU
- **Batch size:** Number of samples processed in parallel; larger = better GPU utilization but higher latency

---

## Self-check questions

1. Why does training require storing all intermediate activations?
2. What is the purpose of quantizing a model from FP16 to INT8 for inference?
3. Which NCCL operation synchronizes gradients across GPUs during distributed training?
4. What is the difference between model parallelism and data parallelism?
5. Which NVIDIA GPU is designed for inference + professional visualization on a single card?

<details>
<summary>Answers</summary>
1. Backpropagation computes gradients layer-by-layer from the output back to the input. Each layer needs its input activation (computed during the forward pass) to calculate its gradient. So all activations must be stored until the backward pass completes.<br>
2. INT8 uses 8-bit integers instead of 16-bit floats, halving memory usage and doubling throughput on Tensor Cores that support INT8, at minor accuracy cost (calibrated quantization minimizes this).<br>
3. All-reduce — each GPU starts with its local gradient; after all-reduce, every GPU has the sum of all gradients, enabling synchronized weight updates.<br>
4. Data parallelism: each GPU has a full copy of the model but processes different data batches; gradients are averaged at the end. Model parallelism: the model itself is split across GPUs, each holding different layers or tensor slices.<br>
5. L40S (Ada Lovelace) — handles AI inference + rendering/visualization on one card.
</details>
