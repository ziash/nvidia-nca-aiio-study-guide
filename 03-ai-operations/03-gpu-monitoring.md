---
layout: default
title: "3.3 GPU Monitoring Key Measures"
parent: "Domain 3 — AI Operations (22%)"
nav_order: 3
---

# 3.3 GPU Monitoring Key Measures

## What the exam tests

DCGM's role, the key GPU metrics operators watch, and how the monitoring stack (DCGM → Prometheus → Grafana) is assembled.

---

## DCGM — Data Center GPU Manager

**DCGM (NVIDIA Data Center GPU Manager)** is the primary tool for monitoring and managing GPU health in data center environments.

### What DCGM provides
- **Health monitoring:** Continuously checks GPU health and reports errors
- **Diagnostics:** Run active diagnostics (memory tests, compute stress tests) to validate GPUs
- **Telemetry collection:** Collect hundreds of metrics per GPU in real time
- **Job statistics:** Per-job GPU utilization, memory, power (when integrated with Slurm/Kubernetes)
- **Policy management:** Set GPU operating modes, power limits, clock settings
- **NVML binding:** Built on NVIDIA Management Library (NVML); same library `nvidia-smi` uses

### DCGM deployment

```
┌─────────────────────────────────────────────────────────┐
│  Each GPU node                                           │
│                                                          │
│  ┌──────────────┐    ┌───────────────────────────────┐  │
│  │  GPU Driver  │◄───│         DCGM daemon           │  │
│  │  (NVML)      │    │  (datacenter-gpu-manager.service)│ │
│  └──────────────┘    └───────────────┬───────────────┘  │
│                                      │ metrics           │
│                               ┌──────┴──────┐            │
│                               │ DCGM Exporter│            │
│                               │ (Prometheus  │            │
│                               │  format)     │            │
│                               └──────┬──────┘            │
└──────────────────────────────────────┼──────────────────┘
                                       │ scrape
                                  ┌────┴────┐
                                  │Prometheus│
                                  └────┬────┘
                                       │
                                  ┌────┴────┐
                                  │ Grafana  │
                                  └─────────┘
```

### DCGM Exporter
- Runs as a sidecar container or daemonset on Kubernetes GPU nodes
- Translates DCGM metrics into **Prometheus exposition format**
- Prometheus scrapes the exporter endpoint; Grafana visualizes

---

## Key GPU metrics to monitor

### Utilization metrics

| Metric | What it measures | Healthy value |
|---|---|---|
| **GPU Utilization (%)** | Percentage of time any kernel is executing on the GPU (SM active) | > 80% during training |
| **SM Active (%)** | Percentage of SMs executing a warp — finer grain than GPU utilization | > 80% for compute-bound |
| **SM Occupancy** | Ratio of active warps to maximum possible warps | Workload-dependent |
| **Memory Utilization (%)** | Fraction of GPU VRAM in use | < 95% (leave headroom) |
| **Tensor Active (%)** | Fraction of time Tensor Cores are executing | Key for AI workloads |

**Why GPU utilization < 80% is a problem:**
- CPU is bottlenecking data preprocessing (DALI helps)
- Network is bottlenecking gradient exchange (bandwidth/latency issue)
- Storage is bottlenecking data loading (need faster parallel FS)
- Job is memory-bound (too many small operations)

### Memory and bandwidth metrics

| Metric | Description |
|---|---|
| **GPU Memory Used (MB)** | Absolute VRAM consumption |
| **Memory Bandwidth Utilization (%)** | HBM read/write bandwidth vs peak |
| **NVLink Bandwidth (GB/s)** | Throughput on each NVLink port (Tx and Rx) |
| **PCIe Bandwidth** | Data transfer between CPU and GPU via PCIe |

### Power and thermal metrics

| Metric | Description | Alert threshold |
|---|---|---|
| **Power Draw (W)** | Current GPU power consumption | Near TDP (e.g., 700W for H100 SXM) |
| **GPU Temperature (°C)** | Die temperature | > 85°C is warning; > 90°C is critical |
| **Memory Temperature (°C)** | HBM temperature | Typically follows die temp |
| **Fan Speed (RPM)** | Active fan speed (PCIe cards) | Rising fan = rising temperature |

**Thermal throttling:** GPUs automatically reduce clock speeds when temperature exceeds threshold (85–90°C). This silently reduces training performance — temperature monitoring is critical for detecting cooling issues early.

### Error metrics

| Metric | Description |
|---|---|
| **ECC Single-bit Errors (SBE)** | Corrected memory errors — high rates indicate potential hardware failure |
| **ECC Double-bit Errors (DBE)** | Uncorrectable memory errors — page retirement required; escalate to hardware replacement |
| **XID Errors** | GPU driver-level error codes; XID 79 = GPU fallen off the bus; XID 48 = ECC DBE |
| **NVLink Errors** | Errors on NVLink lanes — may indicate cable or connector issue |

---

## NVLink bandwidth monitoring

For multi-GPU nodes, NVLink bandwidth is a critical metric during distributed training:

```
During all-reduce (gradient sync):
  Expected: each GPU transmitting 900 GB/s (full NVLink 4 bandwidth)
  Actual < 80% of peak → investigation needed:
    - Job using tensor parallelism correctly?
    - NVLink health errors?
    - Gradient compression applied?
```

---

## Common monitoring scenarios

### Scenario 1: Training job slower than expected
Check in order:
1. GPU Utilization — is it < 70%? → CPU/data pipeline bottleneck
2. NVLink Bandwidth — is it < expected? → communication bottleneck
3. SM Active % — high utilization but slow? → memory-bandwidth bound workload
4. GPU Temperature — throttling reducing clocks?

### Scenario 2: Unexpected GPU errors
1. Check XID errors in DCGM or `nvidia-smi -q` for error codes
2. Run DCGM diagnostics: `dcgmi diag -r 3` (level 3 = long GPU stress test)
3. Check ECC double-bit errors → page retirement or GPU replacement
4. Review NVLink error counters

### Scenario 3: Power/cooling alert
1. Per-GPU power draw near TDP across all GPUs → data center power limit may be reached
2. Temperature > 85°C → check cooling system; verify airflow; check DLC flow
3. Throttling events in DCGM → temporary clock reduction → performance degradation

---

## Key DCGM field IDs (exam reference)

| DCGM Field | Metric name |
|---|---|
| DCGM_FI_DEV_GPU_UTIL | GPU utilization (%) |
| DCGM_FI_DEV_MEM_COPY_UTIL | Memory copy engine utilization |
| DCGM_FI_DEV_FB_USED | Framebuffer (GPU memory) used |
| DCGM_FI_DEV_GPU_TEMP | GPU temperature |
| DCGM_FI_DEV_POWER_USAGE | Power draw (W) |
| DCGM_FI_DEV_NVLINK_BANDWIDTH_TOTAL | Total NVLink bandwidth |
| DCGM_FI_DEV_ECC_SBE_VOL_TOTAL | Single-bit ECC errors (volatile) |
| DCGM_FI_DEV_ECC_DBE_VOL_TOTAL | Double-bit ECC errors (volatile) |

---

## Self-check questions

1. What does DCGM stand for and what are its four main capabilities?
2. What is the role of the DCGM Exporter in a Kubernetes monitoring stack?
3. What does GPU Utilization (%) actually measure?
4. What is the significance of ECC double-bit errors?
5. At what GPU temperature should an operator investigate cooling issues?

<details>
<summary>Answers</summary>
1. Data Center GPU Manager. Capabilities: health monitoring, active diagnostics, telemetry collection, and policy management (clock/power limit configuration).<br>
2. DCGM Exporter translates DCGM metrics into Prometheus exposition format and serves them on an HTTP endpoint. Prometheus scrapes this endpoint on each node; Grafana queries Prometheus to create dashboards and alerts.<br>
3. GPU Utilization (%) measures the percentage of time over a sample period that at least one kernel is executing on the GPU (SM is active). It does NOT measure how fully utilized the SMs are — a single kernel running on 1 of 128 SMs still shows 100% GPU utilization. SM Active % provides finer granularity.<br>
4. ECC double-bit errors (DBE) are uncorrectable memory errors. The GPU hardware cannot fix them (unlike single-bit errors). Affected memory pages must be retired, and persistent DBE errors indicate hardware failure requiring GPU replacement.<br>
5. > 85°C is warning level; > 90°C is critical — investigate immediately. Thermal throttling begins around 83–87°C (GPU-dependent) and will silently reduce performance by reducing GPU clocks.
</details>
