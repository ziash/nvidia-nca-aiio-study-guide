---
layout: default
title: "2.6 Facility Requirements"
parent: "Domain 2 — AI Infrastructure (40%)"
nav_order: 6
---

# 2.6 Facility Requirements for AI Data Centers

## What the exam tests

The physical infrastructure requirements — power, cooling, space, connectivity, and security — needed to host GPU-dense AI workloads.

---

## Why AI changes facility requirements

Traditional enterprise data centers were designed for 3–10 kW/rack with air cooling. Modern AI racks draw 30–120+ kW — this is not just "more of the same," it requires a fundamentally different facility design.

```
Traditional enterprise rack:  3–10 kW      → standard air cooling, standard PDU
AI-optimized rack (H100):    ~30 kW         → high-density air or rear-door DLC
AI rack (B200/DGX):          60–80 kW       → direct liquid cooling (mandatory)
GB300 NVL72 rack:            ~120 kW        → purpose-built liquid infrastructure
```

---

## Power infrastructure

### Power feed
- **3-phase power** is standard for high-density racks (more efficient for high current loads)
- **Per-rack circuit:** 30 kW racks require 30A @ 208V 3-phase circuits; higher for denser racks
- **Transformer proximity:** Step-down transformers should be close to the row to minimize voltage drop

### UPS (Uninterruptible Power Supply)
- Provides battery backup during utility outages or generator switchover
- AI training jobs take minutes to hours — UPS is not a substitute for a generator; it bridges the gap
- Typical UPS runtime: 10–15 minutes at full load
- Types: Double-conversion online UPS (clean power, zero transfer time) preferred for GPU clusters

### Redundancy tiers (Uptime Institute)
| Tier | Redundancy | Expected availability |
|---|---|---|
| Tier I | None (N) | 99.671% |
| Tier II | N+1 | 99.741% |
| Tier III | N+1 concurrently maintainable | 99.982% |
| Tier IV | 2N fully fault-tolerant | 99.995% |

AI training clusters commonly target **Tier III** — maintainable without downtime. Full Tier IV adds significant cost that's often not justified if training jobs can checkpoint and resume.

### Generators
- Diesel or natural gas generators provide sustained power beyond UPS runtime
- Transfer time: manual (> 30s) or automatic transfer switch (ATS, < 10s)
- Generator sizing: must cover full IT load plus cooling equipment

---

## Cooling infrastructure

*(See [2.3 Power and Cooling](03-power-and-cooling/) for full detail)*

### Facility-level cooling components
- **CRAC (Computer Room Air Conditioning):** Traditional raised-floor units; air-based; practical to ~30 kW/rack
- **CRAH (Computer Room Air Handler):** Works with building chilled water plant; more efficient than self-contained CRAC
- **Chilled Water Plant:** Chillers, cooling towers, pumps; backbone of large data center cooling
- **Direct Liquid Cooling infrastructure:** CDUs (Coolant Distribution Units), manifolds, facility chilled water supply/return

### Hot/cold aisle containment
- Separates cold intake air from hot exhaust air
- Cold-aisle containment: encloses cold aisles with doors/roof; hot air freely exits to room return
- Hot-aisle containment: encloses hot aisles; cold air floods the room

---

## Space and physical layout

### Raised floor
- Traditional: raised floor provides plenum for cold air distribution and cable management
- Not required with overhead cabling and row-based cooling (in-row coolers)
- AI clusters often use overhead cable management with floor drains (for liquid cooling leak management)

### Floor load capacity
- Standard data center floor: 100–150 kg/m² (about 300–450 lbs/ft²)
- GPU racks with liquid cooling can exceed this: DGX H100 weighs ~350 kg
- **Structural assessment is required** before installing AI racks in existing facilities

### Rack unit (U) planning
- 1U = 1.75 inches; standard rack is 42U
- DGX H100 = 10U
- GB300 NVL72 = full rack (purpose-built)
- Leave space for top-of-rack switches, patch panels, and cable management

---

## Connectivity

### External network connectivity
- **Multiple diverse fiber paths** from different providers — no single point of failure at the building
- **Cross-connects** in the Meet-Me Room (MMR) to carriers and internet exchanges
- Bandwidth: 100G, 400G uplinks for clusters with high external data transfer needs

### Internal network runs
- InfiniBand requires low-loss fiber or copper (DAC) up to 3m, active optical cables (AOC) for longer runs
- Fat-tree topologies may require structured cabling from compute to spine switches — plan cable path before rack installation

---

## Physical security

| Layer | Measures |
|---|---|
| **Perimeter** | Fenced facility, vehicle barriers, security guard |
| **Building access** | Biometric or badge reader entry, mantrap/airlock |
| **Data center floor** | Separate badge zone; camera coverage (CCTV); visitor log |
| **Rack level** | Lockable rack doors; cage-based isolation for multi-tenant |
| **OOB management** | Dedicated IPMI/BMC network physically separated from production |

---

## Summary checklist before deploying AI racks

- [ ] Confirm rack power circuit capacity (30–120 kW per rack as required)
- [ ] Verify floor load rating supports GPU rack weights
- [ ] Determine cooling method (air / rear-door DLC / direct DLC / immersion)
- [ ] Confirm chilled water supply is available (if DLC)
- [ ] Plan cable management for InfiniBand high-density connections
- [ ] Confirm redundant power feeds (UPS + generator)
- [ ] Assess external bandwidth for training data ingestion and model uploads
- [ ] Verify physical security meets compliance requirements

---

## Self-check questions

1. What facility rating (Uptime Tier) is most commonly targeted for AI training clusters?
2. Why does floor load capacity matter for GPU rack deployments?
3. What is the purpose of hot-aisle/cold-aisle containment?
4. What bridges the gap between utility power failure and generator startup?
5. For racks above 40 kW, what cooling method is typically required?

<details>
<summary>Answers</summary>
1. Tier III — concurrently maintainable, 99.982% availability. Provides redundancy without the full cost of Tier IV, and training jobs can checkpoint/resume if needed.<br>
2. GPU racks (e.g., DGX H100 at ~350 kg) can exceed the structural floor load rating of standard data center floors (typically 100–150 kg/m²). Exceeding this risks structural damage. A load assessment and possibly structural reinforcement are required.<br>
3. Hot/cold aisle containment prevents cold supply air from mixing with hot exhaust before it cools servers. Without containment, hot and cold air mix, reducing cooling effectiveness and forcing over-provisioning of cooling capacity.<br>
4. UPS (Uninterruptible Power Supply) — provides battery-backed power for 10–15 minutes while the generator starts up and stabilizes.<br>
5. Direct Liquid Cooling (DLC) — cold plates on GPUs/CPUs, fed by facility chilled water. Rear-door heat exchangers are also used in the 30–50 kW range.
</details>
