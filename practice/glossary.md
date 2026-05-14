---
layout: default
title: Glossary
parent: Practice & Glossary
nav_order: 1
---

# Glossary — NCA-AIIO Acronyms and Terms

All acronyms and key terms from the three exam domains, alphabetical order.

---

## A

**AI (Artificial Intelligence)** — Any technique enabling machines to perform tasks that normally require human intelligence. Umbrella term encompassing ML and DL.

**AI Enterprise (NVIDIA AI Enterprise)** — NVIDIA's full-stack, enterprise-grade AI software suite. Includes TensorRT, Triton, NeMo, RAPIDS. Comes with enterprise support SLA. Required for vGPU deployments.

**AI Factory** — An AI data center purpose-built for single or few extremely large training workloads. Uses NVLink + InfiniBand. Contrast with AI Cloud.

**AI Cloud** — A multi-tenant hyperscale AI infrastructure serving many diverse concurrent workloads. Uses Ethernet + RoCE. Examples: AWS, Azure, GCP GPU clouds.

**ATS (Automatic Transfer Switch)** — Electrical switch that automatically transfers power from utility to generator during an outage. Transfer time < 10 seconds.

**AOC (Active Optical Cable)** — Fiber optic cable with active (powered) transceivers at each end. Used for InfiniBand runs > 3 meters.

---

## B

**BF16 (Brain Float 16)** — 16-bit floating point format with 8 exponent bits and 7 mantissa bits. Preferred over FP16 for training due to wider dynamic range. Native to Tensor Cores from Ampere onward.

**BlueField (BlueField-3)** — NVIDIA's DPU product line. BlueField-3 provides 400 GbE networking + Arm cores + hardware accelerators. Programmed via DOCA.

**BMC (Baseboard Management Controller)** — Dedicated microcontroller on server motherboard providing out-of-band management. Enables power control, console access, and hardware monitoring independent of the host OS.

---

## C

**CapEx (Capital Expenditure)** — Upfront cost to purchase hardware/infrastructure. On-premises deployments are CapEx-heavy.

**C2C (Chip-to-Chip)** — NVLink variant for connecting CPU and GPU dies directly within a Superchip (e.g., Grace+H100 in GH200). Provides coherent memory access.

**CRAC (Computer Room Air Conditioning)** — Self-contained cooling unit for data centers. Practical limit ~30 kW/rack.

**CRAH (Computer Room Air Handler)** — Air handler connected to a building's chilled water plant. More efficient than CRAC for large deployments.

**CUDA (Compute Unified Device Architecture)** — NVIDIA's parallel computing platform and programming model. Foundation for all NVIDIA AI software.

**cuDNN** — NVIDIA's library of GPU-accelerated deep learning primitives. Used by PyTorch, TensorFlow, JAX under the hood.

---

## D

**DAC (Direct Attach Copper)** — Passive copper cable for short-reach InfiniBand or Ethernet connections (up to ~3 meters).

**DCGM (Data Center GPU Manager)** — NVIDIA's GPU monitoring and diagnostics framework. Provides health monitoring, telemetry, active diagnostics, and policy management.

**DCIM (Data Center Infrastructure Management)** — Software for managing physical data center assets: power, cooling, space, and connectivity.

**DCN (Data Center Networking)** — General term for the network infrastructure within a data center.

**DL (Deep Learning)** — Subset of ML using neural networks with many layers. Foundation of modern AI (transformers, CNNs, diffusion models).

**DLC (Direct Liquid Cooling)** — Cooling method where coolant flows directly to cold plates on GPU/CPU. Required for racks > 40 kW.

**DMA (Direct Memory Access)** — A device accesses host memory directly without CPU involvement.

**DOCA (Data Center Infrastructure on a Chip Architecture)** — NVIDIA's SDK for programming BlueField DPUs. Provides unified API across BlueField generations.

**DPU (Data Processing Unit)** — Third pillar of computing (alongside CPU and GPU). Offloads, accelerates, and isolates infrastructure tasks (networking, storage, security). NVIDIA's product: BlueField.

---

## E

**E-W (East-West)** — Traffic flowing between servers within the same cluster. In AI: GPU-to-GPU gradient exchange. Requires high bandwidth and low latency.

**ECC (Error Correction Code)** — Memory protection technology. Single-bit errors (SBE) are corrected; double-bit errors (DBE) are uncorrectable and require page retirement.

**ECN (Explicit Congestion Notification)** — Network mechanism where switches mark congested packets; receivers signal senders to slow down (used in DCQCN for RoCE congestion control).

---

## F

**FP4 / FP8 / FP16 / FP32** — Floating point precision formats. Lower precision = smaller memory footprint + higher throughput. FP4 introduced in Blackwell.

**Fat-tree** — Network topology where aggregate uplink bandwidth equals downlink bandwidth. Non-blocking: any node can communicate with any other at full line rate simultaneously.

**FDR InfiniBand** — 56 Gbps InfiniBand generation. Legacy.

---

## G

**GRES (Generic Resource Scheduling)** — Slurm mechanism for tracking and allocating non-CPU resources including GPUs.

**GPU (Graphics Processing Unit)** — Massively parallel processor optimized for matrix/tensor operations. Primary compute engine for AI workloads.

**GPU Operator (NVIDIA GPU Operator)** — Kubernetes operator that automates installation of the full NVIDIA GPU software stack (drivers, CUDA, DCGM, device plugin, MIG manager).

**GPUDirect RDMA** — Technology enabling the network adapter to read/write GPU memory directly, bypassing CPU memory. Eliminates the GPU→CPU→NIC copy in distributed training.

---

## H

**HBM (High Bandwidth Memory)** — Stacked DRAM mounted on the GPU package. Provides very high bandwidth (3–8 TB/s). Used in data center GPUs (H100, B200).

**HDR InfiniBand** — 200 Gbps InfiniBand generation. Previous standard for H100 clusters.

**HPC (High-Performance Computing)** — Computing for scientific simulation and research. Often runs alongside AI workloads on shared GPU clusters.

---

## I

**IB (InfiniBand)** — High-throughput, low-latency networking technology with native RDMA support. Industry standard for HPC and AI Factory scale-out networks.

**IBTA (InfiniBand Trade Association)** — Organization that maintains the InfiniBand specification.

**iDRAC (Integrated Dell Remote Access Controller)** — Dell's BMC implementation. Provides out-of-band management for Dell servers.

**iLO (Integrated Lights-Out)** — HPE's BMC implementation.

**IPMI (Intelligent Platform Management Interface)** — Industry-standard protocol for communicating with BMCs. Version 2.0 is widely supported. Being superseded by Redfish.

---

## J

**JBOD (Just a Bunch of Disks)** — Storage enclosure without RAID. Used for raw capacity.

---

## K

**K8s (Kubernetes)** — Container orchestration platform. Used for AI inference serving and increasingly for training. GPU support via NVIDIA GPU Operator.

**KV cache (Key-Value cache)** — In transformer inference, stores previously computed key and value tensors for each layer and token. Memory-intensive at long context lengths.

---

## L

**LoRA (Low-Rank Adaptation)** — Parameter-efficient fine-tuning technique. Trains only a small set of adapter weights, leaving most model parameters frozen. Dramatically reduces fine-tuning compute cost.

---

## M

**MFU (Model FLOP Utilization)** — Ratio of actual compute operations to theoretical peak GPU FLOPS. Healthy training runs achieve 40–60% MFU.

**MIG (Multi-Instance GPU)** — Hardware GPU partitioning technology (Ampere/Hopper). Divides one GPU into up to 7 fully isolated instances with dedicated SM, HBM, L2, and bandwidth.

**ML (Machine Learning)** — Subset of AI where systems learn from data without explicit programming.

**MLOps** — Application of DevOps principles to machine learning — automating the lifecycle of training, validation, deployment, and monitoring of ML models.

**MMR (Meet-Me Room)** — Physical location in a data center where carrier fiber connects to the building's network.

---

## N

**N-S (North-South)** — Traffic flowing between the data center and users/internet. In AI: management traffic, user access, result retrieval. Standard Ethernet.

**NCCL (NVIDIA Collective Communications Library)** — Library for multi-GPU and multi-node collective operations (all-reduce, all-gather, broadcast). Essential for distributed training.

**NDR InfiniBand** — 400 Gbps InfiniBand generation. Current standard for H100/B200 clusters.

**NGC (NVIDIA GPU Cloud)** — NVIDIA's catalog of GPU-optimized containers, pre-trained models, and SDKs. Free to access.

**NVLink** — NVIDIA's high-speed GPU interconnect. NVLink 4 (Hopper): 900 GB/s; NVLink 5 (Blackwell): 1.8 TB/s.

**NVMe (Non-Volatile Memory Express)** — PCIe-based SSD protocol. Very high IOPS and bandwidth vs SATA/SAS.

**NVMe-oF (NVMe over Fabrics)** — Extension of NVMe protocol over network fabrics (RDMA/TCP). BlueField-3 DPU offloads NVMe-oF processing.

**NVSwitch** — NVIDIA switch chip that creates fully non-blocking all-to-all NVLink fabric within a system (e.g., DGX H100 uses 4× NVSwitch for 8-GPU all-to-all).

---

## O

**OAM (OCP Accelerator Module)** — Server form factor used in hyperscaler platforms for GPU hosting.

**OOB (Out-of-Band)** — Management traffic on a dedicated separate network, independent of production traffic.

**OpEx (Operating Expenditure)** — Ongoing costs (power, maintenance, licensing, cloud subscription). Cloud deployments are OpEx-heavy.

---

## P

**PCIe (Peripheral Component Interconnect Express)** — Standard interface for connecting GPUs, NICs, and NVMe drives to servers. PCIe Gen5: 128 GB/s x16; PCIe Gen6: 256 GB/s.

**PDU (Power Distribution Unit)** — Rack-level power distribution. Intelligent PDUs monitor per-outlet power consumption.

**PEFT (Parameter-Efficient Fine-Tuning)** — Family of techniques for fine-tuning large models by training only a small subset of parameters. Includes LoRA, prefix tuning, adapters.

**PFC (Priority Flow Control)** — Ethernet mechanism that sends per-priority pause frames to prevent packet drops. Required for lossless Ethernet (RoCE).

**PUE (Power Usage Effectiveness)** — Data center efficiency metric: Total Facility Power / IT Equipment Power. Lower = better. Hyperscale targets 1.1–1.2.

---

## R

**RDMA (Remote Direct Memory Access)** — Network transfer mechanism allowing one machine to directly read/write memory on another without involving the remote CPU or OS. Zero-copy, kernel-bypass.

**Redfish** — Modern REST/JSON-based API standard for server management (BMC). Supersedes IPMI.

**RLHF (Reinforcement Learning from Human Feedback)** — Technique for aligning LLMs to human preferences using a reward model trained on human rankings.

**RoCE (RDMA over Converged Ethernet)** — Takes RDMA semantics and runs them over Ethernet. Requires lossless Ethernet (PFC). Two versions: RoCEv1 (L2 only) and RoCEv2 (IP-routable).

---

## S

**SEL (System Event Log)** — BMC log of hardware events (errors, power events, temperature alerts).

**SM (Streaming Multiprocessor)** — Basic compute unit in an NVIDIA GPU. Each SM contains CUDA cores, Tensor Cores, and a cache. Thousands of SMs work in parallel.

**SHARP (Scalable Hierarchical Aggregation and Reduction Protocol)** — NVIDIA InfiniBand feature that performs collective operations (all-reduce) inside network switches, reducing GPU data traffic.

**Slurm** — Open-source HPC job scheduler. Dominant in AI training clusters. Manages job queuing, resource allocation, and gang scheduling for multi-node jobs.

**SXM** — High-performance GPU slot form factor used in DGX systems. Supports full NVLink bandwidth and highest TDP. Not PCIe-compatible.

---

## T

**TDP (Thermal Design Power)** — Maximum heat a component generates under sustained workload. Data center cooling and power circuits must handle TDP plus system overhead.

**Tensor Core** — Specialized matrix multiply-accumulate (GEMM) units within NVIDIA GPU SMs. Core accelerator for AI workloads. Each generation adds precision formats (FP4 in Blackwell).

**TensorRT** — NVIDIA SDK for deep learning inference optimization. Layer fusion, precision calibration (FP32→INT8), kernel auto-tuning.

**TensorRT-LLM** — NVIDIA's LLM inference library. Adds paged KV cache, continuous batching, in-flight batching.

**Triton (NVIDIA Triton Inference Server)** — Open-source inference serving framework. Supports TensorRT, ONNX, PyTorch, TensorFlow backends. HTTP/gRPC API, dynamic batching.

**TTFT (Time to First Token)** — Inference latency from request receipt to first generated token. Critical metric for interactive LLM applications.

---

## U

**UPS (Uninterruptible Power Supply)** — Battery-backed power supply providing bridge power during utility failure while generators start.

---

## V

**vGPU (Virtual GPU)** — NVIDIA technology for sharing a GPU across multiple VMs via time-slicing. Requires NVIDIA AI Enterprise license. Supports vCS, vWS, vPC profiles.

**VRAM (Video RAM)** — GPU memory (also called framebuffer or GPU memory). In data center GPUs: HBM type. Must fit the model weights for training/inference.

---

## X

**XDR InfiniBand** — 800 Gbps InfiniBand generation. Used in NVIDIA Quantum-X800 switches.

**XID Error** — NVIDIA GPU driver error code. XID 79 = GPU fell off the bus (hardware failure). XID 48 = double-bit ECC error.
