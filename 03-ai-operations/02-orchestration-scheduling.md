---
layout: default
title: "3.2 Cluster Orchestration and Job Scheduling"
parent: "Domain 3 — AI Operations (22%)"
nav_order: 2
---

# 3.2 Cluster Orchestration and Job Scheduling

## What the exam tests

Kubernetes vs Slurm for AI workloads, the NVIDIA GPU Operator, Run:ai, and how GPU resources are requested and allocated in production clusters.

---

## Why AI scheduling is different

Traditional compute schedulers allocate CPU cores. AI schedulers must:
- Allocate **entire GPUs** (or MIG partitions) to jobs
- Enforce **GPU-aware gang scheduling** — a training job needs all its GPUs to start simultaneously, or none (partial allocation wastes GPUs and causes deadlock)
- Handle **heterogeneous resources** — GPU type, memory, NVLink topology, InfiniBand locality
- Support **preemption** — kill lower-priority jobs to free GPUs for high-priority ones
- Track **GPU utilization and quota** across teams/projects

---

## Slurm — HPC Job Scheduler

**Slurm** (Simple Linux Utility for Resource Management) is the dominant HPC job scheduler, widely used for AI training clusters.

### Architecture

```
┌─────────────────┐     ┌─────────────────────────────────┐
│  Submit host     │────▶│           Slurm Controller       │
│  (sbatch, srun)  │     │  (slurmctld — tracks jobs,       │
└─────────────────┘     │   nodes, resource state)          │
                         └──────────────┬──────────────────┘
                                        │
                    ┌───────────────────┼───────────────────┐
                    │                   │                   │
              ┌─────┴────┐       ┌──────┴───┐       ┌──────┴────┐
              │  Node 1   │       │  Node 2   │       │  Node N   │
              │ (slurmd)  │       │ (slurmd)  │       │ (slurmd)  │
              └─────┬────┘       └──────┬───┘       └──────┬────┘
                    │                   │                   │
                GPU, CPU, RAM      GPU, CPU, RAM       GPU, CPU, RAM
```

### Key Slurm concepts

| Concept | Description |
|---|---|
| **Partition** | Queue of nodes with specific policies (priority, time limit, GPU type) |
| **Job** | A unit of work submitted with resource requirements |
| **sbatch** | Submit a batch script (non-interactive) |
| **srun** | Run a parallel application interactively or as job step |
| **GRES (Generic Resource)** | How Slurm tracks GPUs: `--gres=gpu:8` requests 8 GPUs |
| **Gang scheduling** | All nodes of a job allocated simultaneously |

### Why Slurm for AI
- Mature, battle-tested in HPC environments
- Excellent multi-node job support
- Integrates with InfiniBand topology-aware scheduling (allocate jobs on same IB switch for lowest latency)
- Used by most HPC centers running AI workloads alongside traditional simulation

---

## Kubernetes — Container Orchestration

**Kubernetes (K8s)** is the dominant platform for containerized workloads and is increasingly used for AI inference and even training.

### GPU support in Kubernetes

Without GPU awareness, Kubernetes cannot schedule GPU workloads. Three layers are needed:

**1. NVIDIA GPU Operator**
Automates the deployment of everything required to run GPU workloads on Kubernetes:
- NVIDIA drivers (kernel module)
- CUDA runtime
- DCGM (monitoring)
- Node Feature Discovery (labels nodes with GPU capabilities)
- Device Plugin (exposes GPUs as schedulable Kubernetes resources)
- MIG manager (configures MIG partitions)
- Validator (checks the full stack is working)

The GPU Operator is a Kubernetes Operator that manages all of these components as a single unit — one `helm install` installs and manages the entire GPU stack on every node.

**2. NVIDIA Device Plugin**
Exposes NVIDIA GPUs as a Kubernetes resource:
```yaml
resources:
  requests:
    nvidia.com/gpu: "8"  # Request 8 GPUs
  limits:
    nvidia.com/gpu: "8"
```

**3. Container runtime**
NVIDIA Container Toolkit (nvidia-docker2) allows containers to access GPU hardware. Required on every node.

### AI workload types on Kubernetes

| Workload | Kubernetes mechanism |
|---|---|
| **Batch training** | Job or MPIJob (Kubeflow MPI Operator) |
| **Distributed training** | PyTorchJob, TFJob (Kubeflow Training Operator) |
| **Inference serving** | Deployment + HPA (autoscaling on GPU utilization) |
| **Jupyter notebooks** | StatefulSet with GPU request |

---

## Slurm vs Kubernetes

| Dimension | Slurm | Kubernetes |
|---|---|---|
| Primary use | HPC batch jobs, large training | Containerized apps, inference, mixed workloads |
| Scheduling model | Job queue with priorities | Declarative pod scheduling |
| GPU support | Native GRES plugin | Via NVIDIA GPU Operator + Device Plugin |
| Multi-node MPI jobs | Excellent (native) | Good (MPI Operator add-on) |
| Inference serving | Not designed for it | Excellent |
| Container support | Yes (with enroot/pyxis) | Native |
| Real-time scaling | Manual | Automatic (HPA, KEDA) |
| Typical environment | Research HPC, academic, enterprise AI | Cloud-native enterprise, inference platforms |

**Most large AI clusters run both:** Slurm for large training jobs (researchers familiar with it); Kubernetes for inference services and MLOps pipelines.

---

## Run:ai — Kubernetes-Based GPU Scheduling

**Run:ai** is a commercial Kubernetes-native GPU scheduling platform that adds AI-specific capabilities on top of standard Kubernetes:

- **GPU fractions:** Share one GPU between multiple jobs (not full MIG — software-level time-slicing)
- **Dynamic GPU allocation:** Allocate GPUs to jobs as needed; return them when idle
- **Guaranteed quotas + fair-share:** Teams get guaranteed GPU quotas; idle quota is shared with others
- **Preemption with checkpointing:** Preempt lower-priority jobs; if the job checkpoints, it resumes from last checkpoint
- **GPU utilization analytics:** Per-project, per-user utilization reports

Run:ai sits on top of Kubernetes — it adds a scheduler that replaces the default Kubernetes scheduler for GPU workloads.

---

## Docker — Containerization

**Docker** provides:
- **Image packaging:** Bundle the application + libraries + CUDA + framework into a portable image
- **Runtime isolation:** GPU workloads run in containers without interfering with host OS or other containers
- **NGC integration:** NVIDIA NGC images are Docker images; `docker pull nvcr.io/nvidia/pytorch:24.01-py3`

NVIDIA Container Toolkit (`nvidia-ctk`) enables Docker containers to access GPU hardware through the container runtime.

---

## Self-check questions

1. What does the NVIDIA GPU Operator install on a Kubernetes cluster?
2. How does Slurm request GPUs for a job?
3. What is gang scheduling and why is it critical for AI training?
4. What does Run:ai add on top of Kubernetes that standard K8s lacks?
5. Which scheduler is typically preferred for large multi-node distributed training vs inference serving?

<details>
<summary>Answers</summary>
1. The GPU Operator automatically deploys: NVIDIA drivers, CUDA runtime, DCGM (monitoring), NVIDIA Device Plugin (exposes GPUs to K8s scheduler), Node Feature Discovery, MIG manager, and the NVIDIA Container Toolkit — the entire GPU software stack on every node.<br>
2. Via GRES (Generic Resource Scheduling) flags: `--gres=gpu:8` requests 8 GPUs. SBATCH scripts or srun commands include this flag.<br>
3. Gang scheduling allocates all nodes/GPUs a distributed job needs simultaneously before the job starts. Without it, a job might get 7 of 8 required nodes and hold those GPUs idle while waiting for the 8th — causing GPU waste and potential deadlock.<br>
4. GPU fractions (software time-slicing of a single GPU), dynamic quota allocation, guaranteed-plus-fair-share scheduling between teams, and preemption with checkpoint-aware resumption — capabilities beyond what the default Kubernetes scheduler provides for GPU workloads.<br>
5. Slurm for large multi-node distributed training (HPC origins, excellent MPI support, topology-aware placement); Kubernetes for inference serving (declarative deployment, autoscaling, service mesh integration, rolling updates).
</details>
