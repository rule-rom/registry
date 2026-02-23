# 🔒 Access Restricted: Tier 6 (High Council)

## Swarm-Bus Internals

This page contains the internal workings of Swarm-Bus at transaction and signal level.

### What's Inside

- **Bus Transaction Protocol** — Signal-level transaction protocol
- **Timing Diagrams** — Timing diagrams for all phases
- **Turnaround Logic** — VSB direction switching logic
- **Summation Circuit** — BUS16 honest summation circuit
- **Clock Domain Crossing** — Clock domain crossing

### EV_FLASH Timing Diagram

```
Cycle N:
        ┌─────────────┬────────────┬──────────────┬──────────────┐
CLK     │█████████████│████████████│██████████████│██████████████│
        └─────────────┴────────────┴──────────────┴──────────────┘
        
        ┌────────────────────────────────────────────────────────┐
VSB     │ Conductor drives input (Level16[0..7])                │
        │ Stable throughout READ aperture                        │
        └────────────────────────────────────────────────────────┘
        
        ┌─────────────┐  ┌────────────┐  ┌──────────────────────┐
Phase   │ PHASE_READ  │→ │ TURNAROUND │→ │ PHASE_WRITE          │
        │ (Island)    │  │ (gap)      │  │ (Island drives BUS)  │
        └─────────────┘  └────────────┘  └──────────────────────┘
        
        ┌────────────────────────────────────────────────────────┐
BUS16   │ High-Z (Conductor) │ Turnaround │ Island drives SUM   │
        │                    │            │ READOUT_SAMPLE      │
        └────────────────────────────────────────────────────────┘
```

### Turnaround Logic

| Signal | Conductor | Island | Description |
|--------|-----------|--------|-------------|
| VSB_DRV | 1 (READ) | 0 | Conductor drives VSB |
| VSB_DRV | 0 (TURN) | 0 | Hi-Z (gap) |
| VSB_DRV | 0 (WRITE) | 1 | Island drives VSB |

### Honest BUS16 Summation

For each lane i=0..7 in PHASE_WRITE:

```
contrib_from_all_tiles[i] = 
  Σ_{t | (routing_flags16[t] & BUS_W)!=0 && (locked self || locked_ancestor)} 
    drive_vec[t][i]

bus_raw[i]  = contrib_from_all_tiles[i]
BUS16[i]    = clamp15(bus_raw[i])
BUS_CLIP[i] = (bus_raw[i] > 15)
```

`bus_raw` range: [0..3840] (256 tiles × 15 max)

### Access Requirements

| Tier | Access |
|------|--------|
| Tier 3-5 | ❌ Not available |
| Tier 6 (High Council) | ✅ Full internals specification |

### High Council Rights

**Tier 6** receive:

- ✅ Full Swarm-Bus Internals access
- ✅ Right to modify protocol
- ✅ Direct voting rights on specification evolution
- ✅ Exclusive architectural audit

### How to Get Access

1. Become a **Tier 6 (High Council Elder)** sponsor — 1M+ ₽
2. After payment confirmation you will receive personal documentation access

---

## Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Swarm Hierarchy](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
