---
layout: default
title: "Domain 3 — AI Operations (22%)"
nav_order: 5
has_children: true
permalink: /domain3/
---

# Domain 3 — AI Operations (22%)

Covers the day-2 operations of an AI data center: how you manage, schedule, monitor, and virtualize GPU resources once the hardware is running.

## Subdomains

| # | Topic | File |
|---|---|---|
| 3.1 | AI data center management and monitoring essentials | [datacenter-management](01-datacenter-management/) |
| 3.2 | Cluster orchestration and job scheduling | [orchestration-scheduling](02-orchestration-scheduling/) |
| 3.3 | GPU monitoring key measures | [gpu-monitoring](03-gpu-monitoring/) |
| 3.4 | Virtualizing accelerated infrastructure | [virtualization](04-virtualization/) |

**Study tip:** Know the difference between DCGM (telemetry/diagnostics), Base Command (DGX cluster management), and Slurm/Kubernetes (job scheduling). Know the three MIG GPU instance profiles on A100/H100 and when to use MIG vs vGPU vs passthrough.
