---
layout: default
title: "3.4 Virtualizing Accelerated Infrastructure"
parent: "Domain 3 — AI Operations (22%)"
nav_order: 4
---

# 3.4 Virtualizing Accelerated Infrastructure

## What the exam tests

The three GPU virtualization methods (MIG, vGPU, passthrough), their use cases, trade-offs, and which NVIDIA products support each.

---

## Why virtualize GPUs?

A physical GPU is expensive ($10K–$30K). Without virtualization:
- One GPU per user/workload — most users can't fully utilize a GPU
- No isolation between workloads on shared infrastructure
- No quality-of-service (QoS) guarantees

GPU virtualization enables:
- **Multi-tenancy:** Multiple users/VMs/containers share one GPU
- **Isolation:** Workloads cannot interfere with each other
- **QoS:** Guaranteed resource allocation per workload
- **Better utilization:** Smaller jobs fill the GPU that a single large job would leave idle

---

## Three virtualization methods

```
┌────────────────────────────────────────────────────────────────┐
│  Physical GPU (e.g., H100 80GB)                                 │
│                                                                  │
│  Method 1: GPU Passthrough                                      │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │          VM gets the ENTIRE GPU — no sharing             │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Method 2: vGPU (NVIDIA Virtual GPU)                            │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐   │
│  │  VM 1      │ │  VM 2      │ │  VM 3      │ │  VM 4      │   │
│  │ vGPU 20GB  │ │ vGPU 20GB  │ │ vGPU 20GB  │ │ vGPU 20GB  │   │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘   │
│  (time-sliced, shared GPU — software-based partitioning)        │
│                                                                  │
│  Method 3: MIG (Multi-Instance GPU)                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │  MIG 1   │ │  MIG 2   │ │  MIG 3   │ │  MIG 4   │  ...     │
│  │ 20GB HBM │ │ 20GB HBM │ │ 10GB HBM │ │ 10GB HBM │          │
│  │ SM slice │ │ SM slice │ │ SM slice │ │ SM slice │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
│  (hardware-partitioned — fully isolated, no interference)       │
└────────────────────────────────────────────────────────────────┘
```

---

## Method 1: GPU Passthrough

### What it is
A hypervisor (VMware ESXi, KVM, Hyper-V) assigns an entire physical GPU to a single VM using PCIe passthrough (also called SR-IOV VF passthrough or VFIO).

### Characteristics
- The VM has **exclusive, dedicated** access to the full GPU
- Behaves identically to bare-metal GPU access inside the VM
- **No sharing** — one GPU, one VM
- Maximum performance — no virtualization overhead
- GPU must be removed from host and allocated to VM

### Use case
- When a workload needs the full GPU and maximum performance
- Single high-priority training job on a dedicated VM
- Legacy applications that don't support MIG or vGPU

### Limitations
- No sharing → poor utilization if the workload doesn't use the GPU fully
- No live migration of VMs (GPU state can't be migrated)
- Each physical GPU = one VM

---

## Method 2: vGPU — NVIDIA Virtual GPU

### What it is
NVIDIA vGPU is a software-based GPU virtualization technology where multiple VMs share a single physical GPU. The GPU is time-sliced — each VM gets a guaranteed time slice of GPU compute.

### How it works
- **NVIDIA Virtual GPU Manager** (vGPU Manager) runs as a plugin in the hypervisor
- The physical GPU is exposed as multiple virtual GPU instances to VMs
- Each VM installs a vGPU guest driver
- GPU resources are shared via time-multiplexing (not spatial partitioning like MIG)

### vGPU profiles (example with A100 80GB)
| Profile | Memory | Use case |
|---|---|---|
| A100-80C | 80 GB | Full GPU for compute (1 VM) |
| A100-40C | 40 GB | 2 VMs sharing one GPU |
| A100-20C | 20 GB | 4 VMs sharing one GPU |
| A100-10C | 10 GB | 8 VMs sharing one GPU |

### vGPU editions
| Edition | Workload |
|---|---|
| **vCS (Virtual Compute Server)** | AI/ML inference, HPC, data science |
| **vWS (Virtual Workstation)** | Professional graphics, design |
| **vPC (Virtual PC)** | VDI, Windows desktop |

### Use case
- VDI (Virtual Desktop Infrastructure) — multiple users sharing GPUs for graphics-accelerated virtual desktops
- Multi-tenant AI inference — many users sharing inference capacity
- Enterprise AI without dedicated per-user hardware

### Limitations
- Time-slicing: VMs take turns — potential latency spikes when many VMs compete
- Memory isolation is soft (memory partitioned, but error isolation is not hardware-enforced like MIG)
- Requires NVIDIA AI Enterprise license

---

## Method 3: MIG — Multi-Instance GPU

### What it is
**MIG (Multi-Instance GPU)** is a hardware-level GPU partitioning technology introduced with the Ampere (A100) architecture. The GPU is divided into up to **7 hardware-isolated GPU instances**, each with its own:
- Dedicated SM slice
- Dedicated HBM memory partition
- Dedicated L2 cache slice
- Dedicated memory bandwidth
- Dedicated PCIe bandwidth

### Why hardware isolation matters
In vGPU time-slicing, a runaway process on one VM can cause latency spikes for all VMs (shared hardware state, context switches). In MIG, each instance has physically separate resources — **one instance cannot affect the performance or memory of another**.

### Supported GPUs
- **A100** (SXM4 and PCIe): up to 7 MIG instances
- **H100** (SXM5 and PCIe): up to 7 MIG instances
- **A30**: up to 4 MIG instances
- **H200**: up to 7 MIG instances

### MIG instance profiles (A100 80GB example)

| Instance profile | SMs | GPU Memory | Number per GPU |
|---|---|---|---|
| 1g.10gb | 1/7 | 10 GB | 7 |
| 2g.20gb | 2/7 | 20 GB | 3 |
| 3g.40gb | 3/7 | 40 GB | 2 |
| 4g.40gb | 4/7 | 40 GB | 1 |
| 7g.80gb | 7/7 | 80 GB | 1 (full GPU in MIG mode) |

### MIG and Kubernetes
With the **NVIDIA GPU Operator** and **MIG Manager**:
```yaml
resources:
  requests:
    nvidia.com/mig-3g.40gb: "1"  # Request one 3g.40gb MIG instance
```
Each pod gets a guaranteed, isolated 40 GB partition.

### Use case
- Multi-tenant AI inference with strict QoS and isolation
- Running multiple small models simultaneously with guaranteed throughput
- Development environments where many data scientists share a production GPU cluster
- Research clusters where a single GPU can serve multiple simultaneous experiments

---

## Comparison matrix

| Dimension | Passthrough | vGPU | MIG |
|---|---|---|---|
| Isolation type | Full GPU, hardware | Time-sliced, software | Hardware-partitioned |
| Number of tenants | 1 per GPU | Up to 8–16 per GPU | Up to 7 per GPU (A100/H100) |
| Performance isolation | N/A (dedicated) | Soft (time share) | **Hard (hardware)** |
| Memory isolation | Full | Partitioned (soft) | **Partitioned (hard)** |
| Live VM migration | No | Yes (with some constraints) | No |
| Supported on | Any NVIDIA GPU | NVIDIA AI Enterprise GPUs | A100, H100, A30, H200 |
| License required | No | NVIDIA AI Enterprise | No (built into supported GPUs) |
| Best for | Max performance, 1 tenant | VDI, soft multi-tenancy | Production multi-tenancy, QoS |

---

## Decision guide

```
Is GPU isolation/QoS critical (regulated, SLA-bound)?
    YES → Use MIG (if GPU supports it) or passthrough (1 tenant)
    
Does GPU support MIG (A100/H100)?
    YES → Use MIG for multi-tenancy with hardware isolation
    NO  → Use vGPU for soft multi-tenancy
    
Do you need full GPU performance for one workload?
    YES → Passthrough (or 7g.80gb MIG profile)
    
VDI / multiple Windows desktop users?
    → vGPU (vPC or vWS profile)
```

---

## Self-check questions

1. What is the maximum number of MIG instances on an A100 or H100 GPU?
2. What is the key difference between MIG isolation and vGPU isolation?
3. Which virtualization method requires no special NVIDIA license?
4. A data science team of 4 users needs to share one H100, each with guaranteed 20 GB memory. Which method?
5. Which GPU form factor (passthrough, vGPU, MIG) supports live VM migration?

<details>
<summary>Answers</summary>
1. Up to 7 MIG instances (with the 1g.10gb profile on an A100/H100 80GB).<br>
2. MIG uses hardware partitioning — each instance has physically dedicated SMs, HBM memory, L2 cache, and bandwidth. One instance truly cannot affect another. vGPU uses software time-slicing — instances share the hardware state in turns; a misbehaving instance can cause latency spikes for others.<br>
3. GPU passthrough (and MIG) — both use drivers that come with NVIDIA's standard software. vGPU requires NVIDIA AI Enterprise licensing.<br>
4. MIG — configure 4× 2g.20gb instances on the H100. Each user gets a guaranteed, hardware-isolated 20 GB partition with no interference between users.<br>
5. vGPU (with NVIDIA Virtual GPU Manager on supported hypervisors) supports live VM migration with some constraints. Passthrough and MIG do not support live migration because the GPU state is bound to a specific physical location.
</details>
