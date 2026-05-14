---
layout: default
title: "2.4 On-Premises vs Cloud"
parent: "Domain 2 — AI Infrastructure (40%)"
nav_order: 4
---

# 2.4 On-Premises vs Cloud AI Infrastructure

## What the exam tests

The trade-offs between running AI infrastructure on-premises vs in the cloud, and when hybrid or multi-cloud approaches are appropriate.

---

## Decision framework

```
Key questions:
  1. Do you have data sovereignty / compliance requirements?
  2. Is your workload steady-state or bursty/unpredictable?
  3. What is your CapEx vs OpEx preference?
  4. Do you need custom networking (InfiniBand) for large training?
  5. What is your team's operational maturity?
```

---

## On-Premises

### Advantages

| Factor | Detail |
|---|---|
| **Data sovereignty** | Data never leaves your facility; critical for healthcare (HIPAA), finance (SOX), government, and EU (GDPR) |
| **Latency** | Sub-millisecond to data sources; critical for real-time inference connected to on-prem systems |
| **Cost at scale** | At sustained, large-scale GPU utilization (>70%), CapEx typically beats OpEx over 3–5 year horizon |
| **Customization** | Choose exact GPU generation, network fabric (InfiniBand), cooling method; no shared-tenancy constraints |
| **Network performance** | InfiniBand at 400/800 Gbps within your cluster — not available in most public clouds |
| **Predictable performance** | No noisy-neighbor effects; dedicated hardware |

### Disadvantages

| Factor | Detail |
|---|---|
| **High upfront cost** | DGX H100 ~$400K+ per system; full cluster = tens of millions |
| **Long lead times** | GPU supply constraints; 6–18 month procurement cycles at scale |
| **Operational burden** | Requires specialized team: network engineers, sysadmins, cooling/power ops |
| **Inflexible capacity** | Over-provisioning waste; under-provisioning blocks projects |
| **Facility requirements** | Liquid cooling, power upgrades, physical security |

---

## Cloud

### Advantages

| Factor | Detail |
|---|---|
| **No CapEx** | Pay-per-use; OpEx model; immediate access to latest GPUs |
| **Elasticity** | Scale from 0 to thousands of GPUs in minutes |
| **Speed to start** | Provision an 8-GPU H100 instance in minutes |
| **Managed services** | Cloud-managed Kubernetes, databases, MLOps pipelines |
| **Geographic distribution** | Multiple regions for low-latency inference globally |
| **Latest hardware** | Cloud providers adopt new GPU generations quickly |

### Disadvantages

| Factor | Detail |
|---|---|
| **Cost at scale** | Sustained large GPU usage becomes very expensive; spot instances mitigate but add complexity |
| **Data transfer costs** | Moving large training datasets to/from cloud can be expensive and slow |
| **Limited InfiniBand** | Most cloud GPU clusters use Ethernet; for largest training runs, on-prem InfiniBand is faster |
| **Data privacy** | Regulated industries may face restrictions on cloud data processing |
| **Performance variability** | Shared infrastructure; noisy-neighbor effects possible |
| **Lock-in** | Cloud-native tools create vendor dependency |

---

## Hybrid approach

The most common enterprise architecture:

```
                    ┌──────────────────────────────┐
                    │         Cloud                 │
                    │  - Burst training capacity    │
                    │  - Dev/test environments       │
                    │  - Global inference endpoints  │
                    └──────────────┬───────────────┘
                                   │  VPN / Direct Connect
                    ┌──────────────┴───────────────┐
                    │        On-Premises            │
                    │  - Production training cluster │
                    │  - Sensitive data processing   │
                    │  - Edge inference (DGX/Jetson) │
                    │  - Core model registry         │
                    └──────────────────────────────┘
```

**Common pattern:**
- Train on-prem with your regulated/proprietary data
- Burst to cloud for extra capacity during peak demand
- Deploy inference on cloud for global reach

---

## NVIDIA solutions for each deployment

| Environment | NVIDIA product |
|---|---|
| On-premises training | DGX systems + InfiniBand + Base Command Platform |
| On-premises inference | NVIDIA-Certified Servers + Triton + TensorRT |
| Cloud training | DGX Cloud (partnership with major cloud providers) |
| Cloud inference | NGC containers on cloud GPU instances + Triton |
| Hybrid management | NVIDIA Fleet Command (edge + cloud management) |
| Edge deployment | NVIDIA Jetson / Orin modules |

---

## Comparison summary

| Dimension | On-Premises | Cloud |
|---|---|---|
| Cost model | CapEx (high upfront) | OpEx (pay per use) |
| Latency | Very low | Low to medium |
| Data control | Full | Shared / contractual |
| Scale flexibility | Low | Very high |
| Operational overhead | High | Low (managed services) |
| Latest GPU access | After procurement | Immediate |
| Best for | Sustained, large-scale, regulated | Bursty, experimental, global |

---

## Self-check questions

1. Name two scenarios where on-premises is the better choice over cloud.
2. What is the main cost advantage of cloud for AI workloads?
3. Why might a large training cluster on-premises outperform the equivalent cloud setup?
4. What NVIDIA platform manages hybrid edge + cloud GPU deployments?
5. What is the "noisy neighbor" problem in cloud AI compute?

<details>
<summary>Answers</summary>
1. Any two: data sovereignty/compliance requirements; sustained high GPU utilization where CapEx beats OpEx; need for custom InfiniBand fabric; latency-sensitive real-time inference connected to on-prem systems.<br>
2. No upfront CapEx — pay only for GPU time actually used; ability to scale to zero (no idle cost) and to burst to thousands of GPUs without procurement delays.<br>
3. On-premises InfiniBand fabric (NDR 400G/800G) provides far higher bandwidth and lower latency than cloud GPU cluster networking (typically Ethernet). For training large models with high inter-node gradient communication, InfiniBand's performance advantage directly reduces training time.<br>
4. NVIDIA Fleet Command.<br>
5. Shared physical infrastructure means other tenants' workloads can contend for CPU, memory, network, or storage resources even when GPU instances are dedicated. This causes performance variability — your training run runs slower during peak cloud utilization periods.
</details>
