---
layout: default
title: Sizing a First AI Cluster
parent: Field Notes
nav_order: 1
---

# Sizing a First AI Cluster

*Field note — sanitized from experience deploying enterprise GPU clusters. No proprietary numbers.*

---

## The question nobody asks correctly

When someone says "we need a GPU cluster for AI," the first question is almost never the right one. "How many GPUs?" depends entirely on what you're running, when you need it, and whether you've actually verified your bottleneck is compute and not data, software, or people.

Before touching a spreadsheet:

1. **What are you running?** — Training from scratch? Fine-tuning? Inference? All three at different times?
2. **What's the largest model you'll train?** — This determines your minimum GPU memory footprint
3. **What's your time-to-result requirement?** — A 2-week experiment window changes the math versus a 6-hour deadline
4. **Do you have the training data?** — A GPU cluster with no data pipeline is an expensive heater

---

## Memory-first sizing

The GPU **memory** requirement is the hard constraint. You cannot compress a model that doesn't fit in VRAM.

### Rough formula for LLM training
```
GPU memory needed (bytes) = model parameters × bytes per parameter × overhead factor

FP16/BF16 training:  parameters × 2 bytes × ~4 (weights + gradients + optimizer states)
Example: 7B model    7,000,000,000 × 2 × 4 = 56 GB
Example: 70B model   70,000,000,000 × 2 × 4 = 560 GB
```

For 70B training, you need at least 560 GB of GPU memory aggregate → minimum 7× H100 80GB (just for the model state, not activations).

### Inference memory (simpler)
```
Inference memory = parameters × bytes per parameter × quantization factor

FP16:    parameters × 2 bytes
INT8:    parameters × 1 byte
FP4:     parameters × 0.5 bytes

Example: 70B in FP16 = 140 GB → needs 2× H100 80GB minimum
Example: 70B in INT8 = 70 GB → fits on 1× H100 80GB
```

---

## Compute sizing: a proxy method

Without profiling, use utilization targets:

1. Estimate weekly compute demand in GPU-hours (conversations with the ML team)
2. Add 30% overhead for iteration, debugging, failed runs
3. Divide by 168 hours/week to get steady-state GPU count
4. Add 20% headroom for bursts

Example:
- ML team says "we run 5 experiments/week, each takes ~8 GPU-hours"
- 5 × 8 = 40 GPU-hours × 1.3 overhead = 52 GPU-hours/week
- 52 / 168 = 0.31 GPUs steady-state
- Minimum viable: 1 GPU with queuing
- Practical for a team: 4–8 GPUs (so experiments don't queue for days)

---

## Network and storage — the forgotten bottlenecks

Two classic mistakes:

**Mistake 1: Cheap networking**  
Using 10GbE for a multi-node training cluster. At 10 Gbps, a gradient all-reduce for a 7B model takes seconds per step. At 400 Gbps InfiniBand, it takes milliseconds. Training time scales almost linearly with network bandwidth for large models.

**Rule:** For multi-node training, allocate at least 1× 100 Gbps NIC per GPU. For serious clusters, 400 Gbps InfiniBand per GPU minimum.

**Mistake 2: NFS for training data**  
Mounting training datasets over NFS and wondering why GPUs sit at 40% utilization. NFS can't sustain the random-read throughput of hundreds of concurrent data workers. Use a parallel file system (WEKA, Lustre) or at minimum a high-IOPS NFS appliance with solid-state backing.

**Rule:** Provision at least 10 GB/s aggregate storage throughput per 8 GPUs for standard image classification. For LLM training with large tokenized datasets, profile first.

---

## The question to ask facilities before you spec anything

"What is the maximum power delivery and cooling capacity per rack, and what is your upgrade timeline?"

If the answer is "10 kW per rack, no liquid cooling planned," you are limited to L40S-class servers or lower. A DGX H100 needs ~30 kW cooling-capable rack. A GB300 NVL72 needs ~120 kW. Ordering the wrong hardware for the facility is an expensive lesson.

---

## Lessons from first-cluster projects

- **Buy one node first, validate the full stack, then order the rest.** Lead times are long and returns are rare. A single DGX that exposes integration issues with your storage, network, or software stack is worth more than a half-deployed 20-node cluster.

- **Plan your OOB network from day one.** You will need to power-cycle nodes remotely, image them, and diagnose hardware failures. Retrofitting a BMC network after the fact is painful.

- **Checkpoint early and often.** A training job that doesn't checkpoint is a job where one power blip or hardware failure loses days of work. Set checkpoint intervals based on acceptable recovery time, not convenience.

- **Your first bottleneck will surprise you.** It's almost never what the spec sheet suggests. Profile before adding more GPUs.
