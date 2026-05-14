---
layout: default
title: "1.5 AI Use Cases and Industries"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 5
---

# 1.5 Key AI Use Cases and Industries

## What the exam tests

Which AI capabilities apply to which industries, and which NVIDIA products/platforms enable them.

---

## Use cases by AI capability

### Generative AI
- **Text generation / LLMs:** Copilots, chatbots, summarization, code generation, document drafting
- **Image synthesis:** Product design, marketing, drug molecule generation
- **Video generation:** Entertainment, synthetic training data for autonomous vehicles
- **NVIDIA solution:** Blackwell B200/B300 (training), H100/L40S (inference); NeMo framework

### Computer Vision
- **Object detection / classification:** Quality control in manufacturing, medical imaging, autonomous vehicles
- **Video analytics:** Surveillance, retail foot-traffic, sports analytics
- **NVIDIA solution:** NVIDIA Metropolis (smart cities/retail), Holoscan (medical), L40S/L4 for inference

### Natural Language Processing (NLP)
- **Semantic search:** Enterprise knowledge retrieval, legal document review
- **Translation:** Real-time multilingual customer support
- **Sentiment analysis:** Financial trading signals, brand monitoring
- **NVIDIA solution:** NeMo, TensorRT-LLM, H100/B200

### Recommendation Systems
- **Collaborative filtering, deep learning ranking:** E-commerce, streaming, social media
- **NVIDIA solution:** NVIDIA Merlin (end-to-end recommender system framework), A100/H100 for training

### Autonomous Systems
- **Self-driving vehicles:** Perception, path planning, sensor fusion
- **Robotics:** Pick-and-place, warehouse automation, surgical robots
- **NVIDIA solution:** NVIDIA DRIVE platform (automotive), Isaac (robotics), Orin SoC

### High-Performance Computing (HPC) + AI
- **Scientific simulation:** Climate modeling, molecular dynamics, computational fluid dynamics
- **Drug discovery:** Protein structure prediction (AlphaFold-class models), virtual screening
- **NVIDIA solution:** Grace CPU, Hopper H100, DGX SuperPOD

---

## Industries and their AI applications

| Industry | Key AI applications | NVIDIA platforms |
|---|---|---|
| **Healthcare & Life Sciences** | Medical imaging diagnosis, drug discovery, genomics, robotic surgery | Clara (medical imaging), BioNeMo (drug discovery) |
| **Financial Services** | Fraud detection, algorithmic trading, risk modeling, AML | RAPIDS (data analytics), Morpheus (cybersecurity) |
| **Manufacturing** | Predictive maintenance, visual inspection, digital twins, robotics | Metropolis, Isaac, Omniverse |
| **Retail & E-commerce** | Recommendation engines, demand forecasting, cashier-less stores | Merlin (recommenders), Metropolis (vision) |
| **Telecommunications** | Network optimization, predictive maintenance, 5G RAN processing | AI-RAN, BlueField DPU for telco |
| **Energy** | Seismic analysis, equipment fault prediction, smart grid optimization | HPC clusters, DGX systems |
| **Media & Entertainment** | AI content creation, upscaling, rendering acceleration, VFX | L40S, Omniverse, DLSS |
| **Government & Defense** | Intelligence analysis, logistics optimization, autonomous systems | NVIDIA-Certified Systems, edge AI |

---

## AI Factory vs AI Cloud

From the NVIDIA course, two distinct deployment models for AI infrastructure:

**AI Factory:**
- Purpose-built for single/few massive AI training workloads
- Runs the largest AI models (trillion-parameter class)
- Uses NVLink-based scale-up fabric + InfiniBand for scale-out
- Example: NVIDIA DGX SuperPOD, hyperscaler internal training clusters

**AI Cloud:**
- Hyperscale multi-tenant service hosting many diverse AI workloads
- Serves many users simultaneously with different model sizes and task types
- Uses Ethernet-based networking (more flexible/cost-efficient at cloud scale)
- Example: AWS, Azure, GCP GPU instances

---

## Self-check questions

1. Which NVIDIA platform is designed for end-to-end recommendation system development?
2. What is the difference between an AI Factory and an AI Cloud deployment?
3. Which NVIDIA SDK targets medical imaging AI?
4. Name two AI use cases specific to the financial services industry.
5. Which NVIDIA platform handles 5G RAN processing and telco AI workloads?

<details>
<summary>Answers</summary>
1. NVIDIA Merlin<br>
2. AI Factory: purpose-built for a single or few extremely large training workloads; uses NVLink + InfiniBand; designed for maximum throughput on one job. AI Cloud: multi-tenant, diverse workloads, many concurrent users/models; uses Ethernet networking; designed for flexibility and utilization across many jobs.<br>
3. NVIDIA Clara<br>
4. Any two of: fraud detection, algorithmic trading, risk modeling, anti-money laundering (AML), credit scoring, customer churn prediction.<br>
5. NVIDIA AI-RAN; BlueField DPU is used to offload RAN processing in 5G deployments.
</details>
