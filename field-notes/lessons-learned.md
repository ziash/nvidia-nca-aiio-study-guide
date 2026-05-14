---
layout: default
title: Lessons Learned
parent: Field Notes
nav_order: 2
---

# Lessons Learned

*Running log — one lesson per bullet, dated. Sanitized, no customer or vendor confidential information.*

---

## 2026

**May 2026 — InfiniBand cabling matters more than expected**  
A cluster with NDR 400G InfiniBand and cheap passive copper DAC cables saw 40% lower than expected all-reduce throughput on runs longer than 5 meters. Replaced with active optical cables (AOC); throughput recovered immediately. Always use vendor-certified cables for InfiniBand at 200G and above — passive copper has strict reach limits that data sheets understate.

**May 2026 — GPU utilization at 99% is not the same as efficient training**  
Had a training job showing 99% GPU utilization via `nvidia-smi` but MFU (Model FLOP Utilization) was ~18%. Root cause: many small CUDA kernel launches with high launch overhead, not sustained Tensor Core compute. GPU utilization measures "is something executing," not "is the Tensor Core running." Use DCGM's `DCGM_FI_PROF_TENSOR_ACTIVE` metric to measure actual Tensor Core utilization.

**May 2026 — Assume RDMA is not working until you verify it**  
After deploying a RoCE cluster, gradient all-reduce was slower than expected. NCCL was falling back to TCP because PFC (Priority Flow Control) was not configured on the Ethernet fabric. RoCE requires lossless Ethernet — PFC must be enabled end-to-end, including on ToR switches and uplinks. Without it, packet drops destroy RDMA session performance. Test with `perftest` or `ib_write_bw` before running workloads.

**May 2026 — Firmware version mismatches are a silent killer**  
ConnectX NIC firmware, GPU driver, CUDA, and NCCL have complex interdependencies. Running a mismatched set caused subtle training instability — loss spikes that looked like hyperparameter issues. Always pin the complete software stack (driver + CUDA + NCCL + framework) and test the combination before deploying to production. Use NGC containers to manage this.

**May 2026 — Storage tiering pays for itself quickly**  
Having a two-tier storage setup (fast NVMe-backed parallel FS for active training datasets, cheap S3-compatible object storage for archives) avoided a storage capacity crisis when dataset sizes grew 3× in one quarter. Hot tier is expensive — don't use it for cold data. Build the tiering policy from day one, not when you're running out of space.

---

*Add new entries as they happen. Keep each one to 2–4 sentences. Date everything.*
