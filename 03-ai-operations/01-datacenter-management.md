---
layout: default
title: "3.1 AI Data Center Management and Monitoring"
parent: "Domain 3 — AI Operations (22%)"
nav_order: 1
---

# 3.1 AI Data Center Management and Monitoring Essentials

## What the exam tests

Out-of-band management, BMC/IPMI, NVIDIA Base Command Platform, and the tools used to manage large-scale AI infrastructure day-to-day.

---

## Two management planes

```
┌────────────────────────────────────────────────────────────────┐
│  In-Band Management                                             │
│  • Uses production network (same cables as workload traffic)    │
│  • Requires the server OS to be running                         │
│  • Tools: SSH, SNMP, Kubernetes API, Slurm CLI                  │
│                                                                  │
│  Out-of-Band (OOB) Management                                   │
│  • Separate dedicated management network                        │
│  • Works regardless of server OS state                          │
│  • Can power cycle, image, diagnose before OS loads             │
│  • Tools: BMC, IPMI, Redfish, iDRAC (Dell), iLO (HPE)          │
└────────────────────────────────────────────────────────────────┘
```

---

## BMC — Baseboard Management Controller

**BMC** is a dedicated microcontroller on every server motherboard that provides out-of-band management capability.

### What BMC enables
- **Power control:** Power on/off, hard reset, graceful shutdown — even when OS is hung
- **Serial console access (SOL):** See server boot messages and OS console remotely
- **Hardware sensor monitoring:** CPU/memory/fan/power sensor readings
- **Remote firmware updates:** BIOS/UEFI updates without physical access
- **Hardware event logging:** SEL (System Event Log) records hardware errors
- **Remote media mounting:** Boot from ISO image over network

### BMC protocols
| Protocol | Purpose |
|---|---|
| **IPMI (Intelligent Platform Management Interface)** | Industry-standard protocol for BMC communication; v2.0 widely supported |
| **Redfish** | Modern REST API replacing IPMI; JSON-based; part of DMTF standard |
| **iDRAC** | Dell's BMC implementation (iDRAC9 supports Redfish) |
| **iLO** | HPE's BMC implementation |

### OOB network design
- Dedicated 1GbE management port per server (separate physical NIC from production)
- Connected to a separate management switch (VLAN-isolated or physically separate)
- Accessible from management VLAN only — not routable from production
- Provides access even during kernel panic, BIOS POST, or physical maintenance

---

## NVIDIA Base Command Platform

**NVIDIA Base Command Platform** is the AI infrastructure management software for DGX-based clusters.

### What it provides
- **Cluster management:** Provision, configure, and monitor DGX systems at scale
- **Job scheduling:** Integrated workload scheduler for AI training jobs
- **Software lifecycle:** Deploy and update OS, drivers, CUDA, NGC containers across the cluster
- **Monitoring:** GPU utilization, health, alerts, capacity planning dashboards
- **Multi-cluster support:** Manage multiple DGX clusters from a single pane of glass

### Relationship to NGC
Base Command Platform orchestrates jobs that run NGC containers. You define a training job pointing at an NGC container and dataset; Base Command handles scheduling, resource allocation, and monitoring.

### Who uses it
Enterprise customers with DGX H100/B200 clusters. It's NVIDIA's answer to the operational complexity of managing hundreds of DGX systems running AI workloads 24/7.

---

## Data Center Management Tools

### DCIM — Data Center Infrastructure Management

DCIM platforms (e.g., Sunbird, Nlyte, Vertiv) provide:
- Asset inventory (physical rack location, connections)
- Power monitoring per rack/server
- Cooling/thermal management
- Capacity planning
- Change management

For AI data centers, DCIM integration with GPU-level telemetry (via DCGM) creates a full stack from physical power consumption to per-GPU compute utilization.

### Monitoring stack

```
GPU Hardware Metrics
       │
       ▼
DCGM (Data Center GPU Manager)
       │
       ▼
DCGM Exporter (Prometheus format)
       │
       ▼
Prometheus (time-series collection)
       │
       ▼
Grafana (dashboards & alerting)
```

See [3.3 GPU Monitoring](03-gpu-monitoring/) for full DCGM details.

---

## Management network topology

```
                ┌─────────────────┐
                │  Management     │
                │  Workstation    │
                └────────┬────────┘
                         │
                ┌────────┴────────┐
                │   OOB Switch    │  ← 1GbE dedicated
                │  (mgmt-only)    │
                └────────┬────────┘
              ┌──────────┴───────────┐
    ┌─────────┴──┐            ┌──────┴──────┐
    │  BMC (DGX1) │  ...      │  BMC (DGX N) │
    └─────────────┘            └─────────────┘
    (powered, always            (powered, always
     accessible)                 accessible)
```

Even during BIOS POST, OS crash, or network card failure — the BMC is reachable via the dedicated OOB network.

---

## Self-check questions

1. What is the difference between in-band and out-of-band management?
2. What can you do with a BMC that you cannot do via SSH into the OS?
3. What modern protocol is replacing IPMI for BMC communication?
4. Which NVIDIA platform manages DGX clusters including job scheduling and software lifecycle?
5. Why is a dedicated OOB network required rather than using the production network for BMC access?

<details>
<summary>Answers</summary>
1. In-band management uses the production network and requires the server OS to be running. Out-of-band management uses a separate dedicated network (connected to the BMC) and works regardless of OS state — even if the server is powered off, crashed, or in BIOS.<br>
2. With BMC you can: power cycle/reset the server even if the OS is hung; access the serial console during boot before the OS loads; read hardware sensors (temperature, fan, power) independently of OS; update firmware remotely; mount virtual media over the network for OS reinstallation.<br>
3. Redfish — a modern REST/JSON-based API defined by DMTF that replaces the older binary IPMI protocol.<br>
4. NVIDIA Base Command Platform.<br>
5. If the production network is used for both workload traffic and management, a network failure or misconfiguration could cut off management access entirely. A dedicated OOB network ensures management access is always available regardless of production network state.
</details>
