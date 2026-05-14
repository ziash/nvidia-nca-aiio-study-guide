---
layout: default
title: Self-Quiz
parent: Practice & Glossary
nav_order: 3
---

# Self-Quiz — Original Practice Questions

50 original exam-style questions. No dumps. Answers at the bottom.

**Instructions:** Set a 60-minute timer. Read each question fully before answering. Some questions are multiple-select (indicated).

---

## Domain 1 — Essential AI Knowledge

**1.** Which NVIDIA framework is specifically designed for training, fine-tuning, and deploying large language models, automatic speech recognition, and text-to-speech models?

- A) TensorRT  
- B) RAPIDS  
- C) NeMo  
- D) Triton Inference Server

---

**2.** A data scientist wants GPU-accelerated equivalents of pandas and scikit-learn to speed up data preprocessing on an NVIDIA GPU cluster. Which NVIDIA product addresses this?

- A) NCCL  
- B) RAPIDS  
- C) TensorRT  
- D) DOCA

---

**3.** Which statement correctly describes the relationship between AI, ML, and Deep Learning?

- A) ML is a subset of DL; DL is a subset of AI  
- B) AI is a subset of ML; ML is a subset of DL  
- C) DL is a subset of ML; ML is a subset of AI  
- D) All three are independent, parallel fields

---

**4.** What two resources do scaling laws (Chinchilla) indicate primarily determine the quality of a trained language model?

- A) Number of layers and number of attention heads  
- B) Compute (FLOPS) and dataset size (tokens)  
- C) GPU memory and interconnect bandwidth  
- D) Batch size and learning rate

---

**5.** An organization needs to run LLM inference alongside professional visualization (ray tracing, rendering) on a single GPU server. Which GPU family best meets this requirement?

- A) Hopper (H100)  
- B) Ada Lovelace (L40S)  
- C) Blackwell (B200)  
- D) Grace CPU

---

**6.** What is the primary purpose of TensorRT in an AI deployment pipeline?

- A) Training large language models  
- B) Collective communications for distributed training  
- C) Optimizing trained models for inference on NVIDIA GPUs  
- D) Monitoring GPU health in production

---

**7.** Which statement about NVIDIA NGC is correct?

- A) NGC is a paid cloud service for running AI training jobs  
- B) NGC is a free catalog of GPU-optimized containers and pre-trained models  
- C) NGC is NVIDIA's BMC management platform  
- D) NGC requires an NVIDIA AI Enterprise license

---

**8.** During transformer model training, which operation consumes the most GPU memory?

- A) Storing model weights in FP32  
- B) Storing all intermediate activations for backpropagation  
- C) Loading training data from storage  
- D) Gradient clipping

---

**9.** Which GPU is recommended for Virtual Desktop Infrastructure (VDI) workloads in the NVIDIA data center GPU use-case matrix?

- A) H100  
- B) B200  
- C) L40S  
- D) L4

---

**10.** The NVIDIA B200 GPU introduced which new precision format not present in Hopper?

- A) INT8  
- B) FP8  
- C) BF16  
- D) FP4

---

## Domain 2 — AI Infrastructure

**11.** A 70B parameter model is to be served in FP16 precision. What is the minimum GPU memory required to hold the model weights?

- A) 35 GB  
- B) 70 GB  
- C) 140 GB  
- D) 280 GB

---

**12.** What is the key advantage of scale-up (multi-GPU within one node) compared to scale-out (multi-node)?

- A) Higher fault tolerance  
- B) Infinite horizontal scalability  
- C) Much higher inter-GPU bandwidth and lower latency (NVLink)  
- D) Lower cost per GPU

---

**13.** How many NVSwitch chips does the DGX H100 contain?

- A) 2  
- B) 4  
- C) 8  
- D) 16

---

**14.** What does PUE of 1.15 indicate?

- A) 15% of IT power is wasted on cooling  
- B) 15% of total power goes to IT equipment  
- C) Total facility power is 15% higher than IT equipment power  
- D) The facility is 15% below average efficiency

---

**15.** Which cooling method is required for racks exceeding 40–50 kW power density?

- A) Hot-aisle/cold-aisle containment with CRAC units  
- B) Direct Liquid Cooling (DLC)  
- C) Rear-door fan walls  
- D) Immersion cooling only

---

**16.** What is the fundamental difference between E-W (East-West) and N-S (North-South) traffic in an AI cluster?

- A) E-W is encrypted; N-S is plaintext  
- B) E-W is GPU-to-GPU training traffic requiring RDMA; N-S is user/management traffic using standard TCP  
- C) E-W uses Ethernet; N-S uses InfiniBand  
- D) E-W is higher latency than N-S

---

**17.** NVIDIA states the key measurement for an AI-optimized network is:

- A) Peak bandwidth utilization  
- B) 99th percentile packet latency  
- C) How long an AI training job takes from start to finish  
- D) Total throughput in Gbps

---

**18.** What does RDMA's "kernel bypass" feature provide?

- A) Allows the OS kernel to skip memory validation for faster transfers  
- B) Enables data transfer without involving the OS kernel, reducing latency and CPU utilization  
- C) Bypasses encryption for lower overhead  
- D) Allows GPUs to skip PCIe and use NVLink for network transfers

---

**19.** Which NVIDIA products must work together to enable RoCE adaptive routing and congestion control?

- A) ConnectX-8 SuperNIC + Quantum-X800 switch  
- B) BlueField-3 DPU + Spectrum-4 switch  
- C) H100 GPU + ConnectX-7 NIC  
- D) DOCA + NCCL

---

**20.** InfiniBand NDR provides what data rate per port?

- A) 200 Gbps  
- B) 400 Gbps  
- C) 800 Gbps  
- D) 1,600 Gbps

---

**21.** What is the purpose of NVIDIA SHARP in the Quantum-X800 InfiniBand platform?

- A) Adaptive routing to avoid congested switch ports  
- B) Hardware security acceleration for encrypted communications  
- C) Performing collective operations (e.g., all-reduce) inside the switch fabric  
- D) Compressing packets to increase effective bandwidth

---

**22.** Which storage type is most appropriate for active training datasets on a large GPU cluster with hundreds of concurrent data workers?

- A) Local NVMe (per node)  
- B) NFS over 1GbE  
- C) Parallel distributed file system (Lustre, WEKA)  
- D) Object storage (S3-compatible)

---

**23.** The DPU's "Isolate" function protects the infrastructure by:

- A) Encrypting all data in transit between GPU and CPU  
- B) Moving infrastructure control/data plane to a separate DPU domain that operates independently if the host OS is compromised  
- C) Creating isolated VLAN segments for each tenant  
- D) Partitioning GPU memory into isolated regions

---

**24.** The GB300 NVL72 contains how many Blackwell Ultra GPUs and Grace CPUs?

- A) 8 GPUs, 4 CPUs  
- B) 36 GPUs, 72 CPUs  
- C) 72 GPUs, 36 CPUs  
- D) 128 GPUs, 64 CPUs

---

**25.** What does "co-packaged optics" (CPO) enable in NVIDIA Photonics switches?

- A) Running optical transceivers in a separate switch chassis  
- B) Integrating optical components directly in the switch ASIC package, reducing power and enabling higher port density  
- C) Connecting CPUs and GPUs via fiber instead of PCIe  
- D) Using photonic quantum computing for switching decisions

---

**26.** An AI Factory uses NVLink for scale-up and which technology for scale-out across nodes?

- A) Standard Ethernet  
- B) InfiniBand  
- C) RoCE  
- D) PCIe Gen6

---

**27.** NVIDIA BlueField-3 provides how much networking bandwidth?

- A) 100 GbE  
- B) 200 GbE  
- C) 400 GbE  
- D) 800 GbE

---

**28.** Which two factors determine GPU selection for an AI training workload? *(select two)*

- A) GPU memory (model must fit)  
- B) Number of PCIe lanes available  
- C) Memory bandwidth  
- D) GPU height (1U vs 2U)

---

**29.** What is GPUDirect RDMA's primary benefit?

- A) Connects GPUs across data centers via fiber  
- B) Eliminates the GPU→CPU memory copy in the data transfer path, saving copy operations and reducing PCIe transactions  
- C) Provides encrypted GPU-to-GPU communication  
- D) Enables GPUs to directly access InfiniBand switches without a NIC

---

**30.** What type of data center management does a BMC provide?

- A) In-band management via the production network  
- B) Out-of-band management independent of server OS state  
- C) Application-level monitoring via SNMP  
- D) Container orchestration

---

## Domain 3 — AI Operations

**31.** What does DCGM stand for?

- A) Data Compute GPU Metrics  
- B) Data Center GPU Manager  
- C) Distributed CUDA GPU Monitor  
- D) Device Configuration and GPU Management

---

**32.** In a Kubernetes cluster, what does the NVIDIA Device Plugin provide?

- A) Physical GPU drivers for the host kernel  
- B) Exposes NVIDIA GPUs as schedulable Kubernetes resources (nvidia.com/gpu)  
- C) Monitors GPU utilization and sends alerts  
- D) Configures MIG partitions automatically

---

**33.** What is gang scheduling and why is it required for distributed AI training?

- A) Scheduling jobs in priority order to maximize GPU utilization  
- B) Allocating all nodes of a job simultaneously before the job starts, preventing partial allocation deadlocks  
- C) Grouping similar jobs together to batch process them  
- D) Scheduling GPU operations in parallel within a single node

---

**34.** Which NVIDIA tool automatically deploys the entire GPU software stack on Kubernetes nodes?

- A) DCGM  
- B) Triton Inference Server  
- C) NVIDIA GPU Operator  
- D) NVIDIA Base Command Platform

---

**35.** What is the maximum number of MIG instances on a single H100 80GB GPU?

- A) 4  
- B) 7  
- C) 8  
- D) 16

---

**36.** Which virtualization method provides the strongest isolation between GPU tenants?

- A) GPU passthrough  
- B) NVIDIA vGPU  
- C) MIG (Multi-Instance GPU)  
- D) Docker containers without virtualization

---

**37.** A training job shows GPU Utilization at 95% but Model FLOP Utilization (MFU) is only 20%. What is the most likely root cause?

- A) Network bottleneck causing gradient exchange delays  
- B) Many small CUDA kernel launches with high overhead, rather than sustained Tensor Core compute  
- C) Insufficient GPU memory  
- D) Thermal throttling

---

**38.** The monitoring stack for GPU clusters typically flows in which order?

- A) Grafana → Prometheus → DCGM Exporter → DCGM  
- B) DCGM → DCGM Exporter → Prometheus → Grafana  
- C) DCGM → Grafana → Prometheus → Alerts  
- D) Prometheus → DCGM → Grafana → DCGM Exporter

---

**39.** What ECC error type requires immediate investigation and potentially GPU page retirement?

- A) Single-bit errors (SBE) — correctable  
- B) Double-bit errors (DBE) — uncorrectable  
- C) Triple-bit errors (TBE)  
- D) Parity errors

---

**40.** Which platform is NVIDIA's purpose-built solution for managing DGX clusters, including job scheduling, software lifecycle, and monitoring?

- A) NVIDIA Fleet Command  
- B) NVIDIA Base Command Platform  
- C) Kubernetes with GPU Operator  
- D) Slurm with DCGM

---

**41.** Which scheduler is most appropriate for managing large multi-node distributed training jobs in a research/HPC environment?

- A) Kubernetes (vanilla)  
- B) Slurm  
- C) Docker Swarm  
- D) Apache Mesos

---

**42.** NVIDIA vGPU requires which software license?

- A) No license — vGPU is included with GPU hardware  
- B) NVIDIA AI Enterprise  
- C) NVIDIA DGX software  
- D) CUDA Enterprise License

---

**43.** At what GPU temperature does thermal throttling typically begin?

- A) 60°C  
- B) 75°C  
- C) 83–87°C  
- D) 100°C

---

**44.** What Slurm flag requests 4 GPUs for a training job?

- A) `--gpus=4`  
- B) `--gres=gpu:4`  
- C) `--resource=gpu:4`  
- D) `--accelerator=4`

---

**45.** What is the primary purpose of Run:ai in a GPU cluster?

- A) Replace DCGM for GPU health monitoring  
- B) Kubernetes-native GPU scheduling with AI-specific features: quotas, fractions, preemption  
- C) Serve inference via REST API  
- D) Manage InfiniBand switch configurations

---

## Mixed / Integration Questions

**46.** An organization is deploying a multi-tenant GPU cluster for 20 data scientists. They need each scientist to have a guaranteed, isolated GPU resource allocation with no performance interference between users. The cluster uses H100 GPUs. Which virtualization method is best?

- A) GPU passthrough (20 GPUs for 20 scientists)  
- B) vGPU with time-slicing  
- C) MIG with 2g.20gb profiles (3 instances per GPU, ~7 GPUs for 20 users with queuing)  
- D) Docker containers without virtualization

---

**47.** A training cluster has 99% GPU utilization but the team complains training is slow and gradient all-reduce is taking too long. The cluster uses 100 Gbps Ethernet. What should the infrastructure team investigate?

- A) Increase GPU memory  
- B) Upgrade to InfiniBand or higher-bandwidth Ethernet (400+ Gbps)  
- C) Add more CPU cores  
- D) Increase local NVMe storage

---

**48.** Which statement about NVIDIA-Certified Systems is correct?

- A) They are only available from NVIDIA directly  
- B) They validate a best baseline configuration for performance, security, and scale from partner OEMs  
- C) They require NVIDIA AI Enterprise license to operate  
- D) They are only available in PCIe form factor

---

**49.** A company needs to build an AI cluster that processes regulated healthcare data. The team wants maximum GPU performance for LLM training. Which deployment model is most appropriate?

- A) Public cloud (AWS/Azure/GCP)  
- B) On-premises with DGX systems and InfiniBand  
- C) Hybrid: training in cloud, data stored on-prem  
- D) Edge deployment using Jetson modules

---

**50.** NVIDIA Spectrum-X is described as the "World's First Ethernet Platform for AI." What specific combination of technologies enables near-InfiniBand RDMA performance over standard Ethernet?

- A) PCIe Gen6 + 800 Gbps Ethernet  
- B) RoCEv2 + NCCL-optimized adaptive routing + congestion control, requiring BlueField-3 DPU + Spectrum-4 switch  
- C) TCP over 100 Gbps Ethernet with hardware offload  
- D) InfiniBand protocol encapsulated in Ethernet frames

---

## Answers

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | C | 11 | C | 21 | C | 31 | B | 41 | B |
| 2 | B | 12 | C | 22 | C | 32 | B | 42 | B |
| 3 | C | 13 | B | 23 | B | 33 | B | 43 | C |
| 4 | B | 14 | C | 24 | C | 34 | C | 44 | B |
| 5 | B | 15 | B | 25 | B | 35 | B | 45 | B |
| 6 | C | 16 | B | 26 | B | 36 | C | 46 | C |
| 7 | B | 17 | C | 27 | C | 37 | B | 47 | B |
| 8 | B | 18 | B | 28 | A,C | 38 | B | 48 | B |
| 9 | D | 19 | B | 29 | B | 39 | B | 49 | B |
| 10 | D | 20 | B | 30 | B | 40 | B | 50 | B |

---

## Score interpretation

| Score | Readiness |
|---|---|
| 45–50 | Exam ready — schedule it |
| 40–44 | Nearly ready — review weak domains |
| 35–39 | Good foundation — 1 more week of study |
| < 35 | Focus on Domain 2 (networking + DPU) then retake |
