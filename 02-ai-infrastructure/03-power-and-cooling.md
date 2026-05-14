---
layout: default
title: "2.3 Power and Cooling Requirements"
parent: "Domain 2 — AI Infrastructure (40%)"
nav_order: 3
---

# 2.3 Power and Cooling Requirements

## What the exam tests

GPU thermal design power (TDP), rack power density tiers, cooling method trade-offs, and PUE as a data center efficiency metric.

---

## GPU power (TDP) by product

| GPU | Form Factor | TDP |
|---|---|---|
| NVIDIA L4 | PCIe | 72 W |
| NVIDIA L40S | PCIe | 350 W |
| NVIDIA H100 | PCIe | 350 W |
| NVIDIA H100 | SXM5 | 700 W |
| NVIDIA B200 | SXM | ~1,000 W |
| NVIDIA GB300 NVL72 (full rack) | Rack | ~120 kW total rack |

**Key insight:** A DGX H100 (8× H100 SXM5 at 700W each) draws ~10.2 kW from the GPUs alone — plus CPU, memory, storage, networking. Total DGX H100 rack power: ~10.2 kW. A GB300 NVL72 full-rack system draws ~120 kW.

This is why AI data centers require purpose-built power infrastructure — a traditional enterprise server rack runs 3–10 kW; an AI rack runs 30–120+ kW.

---

## Rack power density evolution

```
Traditional enterprise IT:    3–10 kW / rack
AI-ready data center:        20–40 kW / rack
Modern GPU cluster:          40–80 kW / rack
DGX B200/GB300 dense:       80–120+ kW / rack
```

**Practical implication:** Most existing data centers cannot physically host modern GPU clusters without power and cooling upgrades. Colocation providers increasingly offer "AI-ready" halls with direct liquid cooling (DLC) and 40–80 kW/rack power circuits.

---

## Cooling methods

### Air cooling
- Uses CRAC/CRAH units to circulate cold air through server front intakes and exhaust hot air from rear
- Hot-aisle/cold-aisle containment improves efficiency
- **Practical limit:** ~30 kW/rack before thermal density becomes unmanageable
- Still used for L40S-class (350W PCIe) servers in standard rack deployments

### Direct Liquid Cooling (DLC)
- Water or coolant flows directly to cold plates mounted on GPU die and CPU
- Removes heat from the chip before it enters the room air
- Enables 50–80 kW/rack densities
- Required for DGX H100 SXM configurations and higher
- Rear-door heat exchangers: liquid cooling at the back of the rack, cooling hot exhaust air before it enters the room

### Immersion cooling
- Servers submerged in dielectric fluid (mineral oil or synthetic fluid)
- Single-phase: fluid stays liquid, captures heat via heat exchanger
- Two-phase: fluid boils, vapor condenses on a chiller; extremely efficient
- Enables 100+ kW/rack, virtually silent
- Higher upfront cost; specialized facilities required
- Used for ultra-dense GB300/B200 deployments

### Comparison

| Method | Max density | Cost | Retrofit ease |
|---|---|---|---|
| Air (hot/cold aisle) | ~30 kW/rack | Low | High (standard CRAC) |
| Direct Liquid Cooling | 50–80 kW/rack | Medium | Medium (plumbing required) |
| Rear-door HX | ~40 kW/rack | Medium | Medium |
| Immersion | 100+ kW/rack | High | Low (specialized facility) |

---

## PUE — Power Usage Effectiveness

**PUE** measures how efficiently a data center uses energy:

```
PUE = Total Facility Power / IT Equipment Power

Where:
  Total Facility Power = power consumed by ALL equipment (IT + cooling + lighting + UPS losses)
  IT Equipment Power = power consumed by servers, storage, networking only
```

| PUE | Rating |
|---|---|
| 1.0 | Perfect (theoretical) — 100% of power goes to IT |
| 1.1–1.2 | Excellent — hyperscale data centers (Google, Meta) |
| 1.3–1.4 | Good — purpose-built colocation |
| 1.5–1.7 | Average — older enterprise data centers |
| 2.0+ | Poor — inefficient legacy facilities |

**AI data center targets PUE < 1.2** with direct liquid cooling. Air-cooled facilities typically achieve 1.3–1.5.

**Exam trap:** Lower PUE = better efficiency. PUE of 1.0 is perfect; higher numbers mean wasted energy on cooling overhead.

---

## Power infrastructure requirements

### Power delivery
- **3-phase power** is standard for rack-scale AI deployments (more efficient than single-phase for high current)
- **UPS (Uninterruptible Power Supply):** Provides short-term battery backup during utility power transitions; typically 10–15 minutes to allow graceful shutdown
- **PDUs (Power Distribution Units):** Intelligent PDUs enable per-outlet power monitoring and remote switching
- **Redundancy:** N+1 or 2N power paths for mission-critical training clusters

### Power capacity planning
```
Total AI rack power = GPU TDP × number of GPUs
                    + CPU TDP × number of CPUs  
                    + Storage (NVMe: ~10W per drive)
                    + Networking (ConnectX-7: ~20W per NIC)
                    + Memory, fans, board overhead

Rule of thumb: multiply calculated IT load × 1.25 for cooling overhead at PUE 1.25
```

---

## Self-check questions

1. What is the TDP of the H100 in SXM5 form factor?
2. What does PUE stand for and what value indicates a highly efficient data center?
3. What cooling method is required for racks exceeding 40 kW?
4. Why can't most traditional enterprise data centers host GPU clusters without upgrades?
5. A data center has PUE of 1.6. If IT equipment consumes 1 MW, how much total power does the facility use?

<details>
<summary>Answers</summary>
1. 700W TDP (H100 SXM5).<br>
2. Power Usage Effectiveness. PUE = 1.1–1.2 indicates excellent efficiency (hyperscale class). Lower is better; 1.0 is theoretical perfect.<br>
3. Direct Liquid Cooling (DLC) — either cold-plate direct to chip or rear-door heat exchangers. Air cooling becomes impractical above ~30 kW/rack.<br>
4. Traditional data centers are designed for 3–10 kW/rack with standard air cooling. GPU racks require 30–120+ kW/rack, which exceeds both the power circuit capacity (PDU/breaker ratings) and the cooling capacity of standard CRAC units.<br>
5. 1.6 × 1 MW = 1.6 MW total facility power (0.6 MW goes to cooling, lighting, UPS losses).
</details>
