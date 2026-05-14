---
layout: default
title: Exam Guide & Study Strategy
parent: Exam Overview
nav_order: 1
---

# Exam Guide & Study Strategy

## Exam facts

| Item | Detail |
|---|---|
| Full name | NVIDIA Certified Associate: AI Infrastructure and Operations |
| Short code | NCA-AIIO |
| Delivered by | Pearson VUE (online proctored) |
| Questions | 50 multiple choice / multiple select |
| Duration | 60 minutes |
| Fee | $125 USD |
| Validity | 2 years from passing date |
| Retake policy | 14-day wait after first fail; 60-day wait after second fail |

## Recommended prerequisites (NVIDIA-issued)

Complete the free self-paced course **"AI Infrastructure and Operations Fundamentals"** on NVIDIA DLI (~7 hours). Focus units:

- Unit 1 — AI Transformation Across Industries
- Unit 2 — Introduction to Artificial Intelligence
- Unit 4 — Accelerating AI With GPUs
- Unit 5 — AI Software Ecosystems
- Unit 7.1 — Data Center Platform
- Unit 7.4 — Data Center Transformation with DPUs
- Unit 8 — Networking for AI
- Units 10–11 — Energy-Efficient Computing
- Unit 12.4 — AI in the Cloud
- Unit 13 — AI Data Center Management and Monitoring
- Unit 14 — Orchestration, MLOps, and Job Scheduling

## Suggested reading (from official study guide)

NVIDIA TensorRT · NVIDIA GPU Operator · DCGM · Base Command · MIG · BlueField DPU offload · DGX SuperPOD reference architecture · DGX H100 system intro · InfiniBand key features · NVSwitch · Network IO · Kubernetes · Slurm · Docker · Out-of-Band management · Run:ai · IBM/SAS ML · Intel CPU vs GPU

## Study strategy

### By domain weight

Since Domain 2 (AI Infrastructure) carries 40% of the exam, prioritize it. Spend roughly:
- ~3 sessions on Domain 2 (especially networking 2.7–2.9 and DPU 2.10)
- ~2 sessions on Domain 1 (NVIDIA product portfolio is frequently tested)
- ~1 session on Domain 3

### High-yield topics

These topics are disproportionately likely to appear based on the official study guide emphasis:

1. **NVLink vs NVSwitch** — know what each does and which systems use them
2. **InfiniBand vs RoCE** — use cases, speed tiers (HDR/NDR/800G), RDMA
3. **MIG vs vGPU vs passthrough** — when to use each
4. **DPU three functions** — offload, accelerate, isolate
5. **AI Fabric vs N-S network** — E-W high-bandwidth vs N-S management/user access
6. **DCGM metrics** — SM utilization, memory bandwidth, NVLink throughput, temperature
7. **GPU architecture families** — Blackwell, Hopper, Ada Lovelace, Grace CPU — which GPU for which workload

### Exam-day tips

- Read all options before selecting — many questions have two plausible answers
- "NVIDIA-specific" questions almost always want the NVIDIA brand answer, not the generic one
- Time is tight (72 seconds/question) — flag and return rather than getting stuck
- Know acronyms cold: RDMA, RoCE, DPU, MIG, NCCL, DCGM, NGC, TRT, NVL
