---
layout: default
title: Flashcards
parent: Practice & Glossary
nav_order: 2
---

# Flashcards — NCA-AIIO

Q&A pairs organized by domain. Use these for spaced repetition (Anki-compatible format below each answer).

---

## Domain 1 — Essential AI Knowledge

**Q: What is the difference between AI, ML, and DL?**  
A: AI is the broadest term (any human-intelligence simulation). ML is AI that learns from data. DL is ML using multi-layer neural networks. Every DL system is ML; every ML system is AI, but not vice versa.

---

**Q: What does NCCL stand for and what does it do?**  
A: NVIDIA Collective Communications Library. Provides GPU-to-GPU collective operations (all-reduce, all-gather, broadcast, reduce-scatter) for distributed training across multiple GPUs and nodes.

---

**Q: What is the Transformer architecture and why did it enable modern LLMs?**  
A: The Transformer uses self-attention to process all tokens in parallel (vs sequential RNN). This enables: (1) efficient GPU utilization, (2) long-range dependency capture, (3) extreme scalability. Every major LLM (GPT, LLaMA, Gemini) is Transformer-based.

---

**Q: Name three NVIDIA software frameworks and their purposes.**  
A: TensorRT (inference optimization), Triton Inference Server (production inference serving), NeMo (LLM/ASR/TTS training and fine-tuning). Also valid: RAPIDS (GPU data science), DOCA (BlueField DPU programming), NCCL (distributed training communications).

---

**Q: What GPU family is recommended for VDI and virtual desktop workloads?**  
A: Ada Lovelace — specifically NVIDIA L4 (VDI/VPC) or L40 (Virtual Workstation). The L4 has a 72W TDP making it suitable for standard server slots.

---

**Q: What makes Blackwell different from Hopper architecturally?**  
A: Blackwell (B200) adds: (1) 2nd-gen Transformer Engine with FP4 support, (2) 5th-gen NVLink at 1.8 TB/s vs 900 GB/s, (3) 208B transistors vs Hopper's 80B, (4) hardware decompression engine, (5) Secure AI enclave. Same HBM3e memory type but more capacity and bandwidth.

---

**Q: What is RLHF and which phase of the AI lifecycle does it belong to?**  
A: Reinforcement Learning from Human Feedback. It belongs to the training/fine-tuning phase. Human raters rank model outputs → a reward model is trained on rankings → the LLM is fine-tuned using RL against the reward model. Used for alignment (ChatGPT's key ingredient).

---

## Domain 2 — AI Infrastructure

**Q: What is the difference between scale-up and scale-out GPU infrastructure?**  
A: Scale-up: adding GPUs to one node (connected via NVLink/NVSwitch, very high bandwidth, weak fault tolerance). Scale-out: adding nodes to a cluster (connected via InfiniBand/Ethernet, lower bandwidth per link, robust fault tolerance). Production clusters use both.

---

**Q: What is the purpose of NVSwitch in a DGX H100?**  
A: NVSwitch creates a fully non-blocking all-to-all NVLink fabric across all 8 GPUs. Any GPU can communicate with any other GPU at full NVLink bandwidth simultaneously — essential for collective operations (all-reduce). DGX H100 uses 4× NVSwitch chips.

---

**Q: What is PUE and what value indicates a world-class data center?**  
A: Power Usage Effectiveness = Total Facility Power / IT Equipment Power. 1.0 is perfect (theoretical). 1.1–1.2 is world-class (hyperscale). Higher numbers = more energy wasted on cooling/infrastructure.

---

**Q: What is the difference between AI Fabric (E-W) and control/user-access network (N-S)?**  
A: E-W (AI Fabric): GPU-to-GPU gradient exchange during training; requires high bandwidth, low latency, nonblocking topology, RDMA. N-S: user access, management, internet; loosely coupled, TCP OK, oversubscribed topology acceptable.

---

**Q: What is RDMA and why is it used in AI networks?**  
A: Remote Direct Memory Access. Allows one machine to directly read/write memory on a remote machine without involving the remote CPU or OS. Benefits: zero-copy, kernel bypass, < 1 µs latency, very low CPU overhead. Essential for InfiniBand and RoCE in AI training.

---

**Q: What hardware components are required for RoCE adaptive routing and congestion control?**  
A: BlueField-3 DPU (at each server) + Spectrum-4 switch. Both must work together. The Spectrum-4 performs adaptive routing decisions; BlueField-3 handles in-order delivery and congestion signals.

---

**Q: What are the three functions of a DPU (Data Processing Unit)?**  
A: (1) Offload — take infrastructure tasks (networking, storage, security) off the server CPU. (2) Accelerate — run those tasks faster than the CPU can using hardware accelerators. (3) Isolate — move infrastructure functions to a separate security domain that remains secure even if the host OS is compromised.

---

**Q: What is GPUDirect RDMA?**  
A: Technology that lets the network adapter (NIC) read/write GPU memory directly, bypassing CPU memory. Eliminates the GPU→CPU memory copy in distributed training, reducing PCI transactions, CPU usage, and end-to-end latency.

---

**Q: Name the three cooling methods and their approximate maximum rack power density.**  
A: Air cooling: ~30 kW/rack. Direct Liquid Cooling (DLC): 50–80 kW/rack. Immersion cooling: 100+ kW/rack.

---

**Q: What is the Quantum-X800 (Q3400-RA) switch spec?**  
A: 144 ports × 800 Gbps = 115.2 Tbps total bandwidth. 4th generation NVIDIA SHARP. Adaptive routing, congestion control. Used in AI Factory InfiniBand fabrics.

---

**Q: What InfiniBand generation provides 400 Gbps per port?**  
A: NDR (Next Data Rate) InfiniBand. XDR (Extra Data Rate, Quantum-X800) provides 800 Gbps per port.

---

**Q: How many GPUs and Grace CPUs are in the NVIDIA GB300 NVL72?**  
A: 72 Blackwell Ultra (B300) GPUs + 36 Grace CPUs. NVLink 5 connects all. Total: 20 TB GPU memory, 14 TB CPU memory, 130 TB/s NVLink bandwidth.

---

## Domain 3 — AI Operations

**Q: What does DCGM stand for and what are its main capabilities?**  
A: Data Center GPU Manager. Capabilities: (1) GPU health monitoring, (2) active diagnostics (stress tests), (3) telemetry collection (hundreds of metrics), (4) policy management (power limits, clocks).

---

**Q: What is the Slurm GRES flag for requesting 8 GPUs?**  
A: `--gres=gpu:8`

---

**Q: What is the difference between MIG and vGPU isolation?**  
A: MIG: hardware partitioning — each instance has physically dedicated SMs, HBM, L2, and bandwidth. True performance and error isolation. vGPU: software time-slicing — instances share hardware in turns. Soft isolation; one misbehaving VM can affect others.

---

**Q: What does the NVIDIA GPU Operator install on a Kubernetes cluster?**  
A: NVIDIA drivers, CUDA runtime, DCGM, NVIDIA Device Plugin (exposes GPUs as K8s resources), Node Feature Discovery, MIG Manager, NVIDIA Container Toolkit — the complete GPU software stack, managed as one unit.

---

**Q: What ECC error type is uncorrectable and requires page retirement?**  
A: Double-bit errors (DBE). Single-bit errors (SBE) are corrected by hardware. DBE indicates hardware degradation and requires action.

---

**Q: What is gang scheduling and why is it needed for AI training?**  
A: Allocating all nodes/GPUs a job needs simultaneously before the job starts. Required because: if only part of a job's GPUs are allocated, those GPUs sit idle consuming memory while waiting for the rest — wasting expensive compute and potentially creating deadlocks.

---

**Q: What are the three GPU virtualization methods and when do you use each?**  
A: (1) Passthrough: full GPU to one VM, maximum performance, no sharing. (2) vGPU: time-sliced GPU sharing across multiple VMs, requires NVIDIA AI Enterprise license, good for VDI. (3) MIG: hardware-partitioned instances with true isolation, up to 7 per GPU (A100/H100), best for multi-tenant inference with QoS.

---

**Q: At what temperature should an operator investigate GPU cooling issues?**  
A: Warning at > 85°C; critical at > 90°C. Thermal throttling (clock reduction) typically begins around 83–87°C depending on GPU model, silently degrading performance.

---

**Q: What is the difference between Redfish and IPMI?**  
A: Both are BMC management protocols. IPMI (v2.0) is a legacy binary protocol from 1998. Redfish is a modern REST/JSON-based API from DMTF (~2015). Redfish is gradually replacing IPMI on modern server platforms (Dell iDRAC9+, HPE iLO 5+).
