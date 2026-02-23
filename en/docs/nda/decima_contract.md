# 🔒 Decima8: Machine Specification (v0.2)

**Status:** v0.2 DESIGN FREEZE  
**Codename:** Siberian Tank Interface

---

## 🔐 Access Restricted: Tier 3+ (Industrial)

This page contains the full Decima8 neuromorphic core specification v0.2.

> **Warning:** This document contains implementation details including exact formulas, timings, and data structures. Access limited to **Tier 3+** members.

---

## 0) Scope / Non-Goals

### Scope (v0.2)

- **One deterministic rhythm** for: Emulator → Proto (PCB) → FPGA → ASIC
- **Level16:** 0..15 on each of 8 data lines
- **Bidirectional VSB exchange:**
  - Conductor sets input vector **before READ** and holds stable during READ aperture
  - Island drives VSB only in WRITE (readout after WRITE)
- **Tile = minimal programmable entity** (RuleROM addresses tiles)
- **All data transferred only via BUS16** (8 lane)
- **Neighbors form activation graph (relay):** locked tile activates descendants for BUS reading
- **Range-based Fuse (LOCK):** tile latches only if `thr_cur16 ∈ [thr_lo16..thr_hi16]`
- **Decay pulls to 0:** if `decay16>0`, accumulator decreases toward zero each tick, **never crossing 0**
- **Branch collapse:** if tile becomes inactive (not ACTIVE), it forcibly resets to `thr_cur16=0, locked=0`

### Non-Goals (v0.2)

- Absolute voltages not fixed; Level16 semantics + rhythm are fixed
- No baked parameter changes inside EV_FLASH
- No backward compatibility with v0.1 format

---

## 1) Terminology

| Term | Description |
|------|-------------|
| **Conductor** | Digital conductor (CPU), prepares input, triggers EV_FLASH, does bake/reset |
| **Island/Swarm** | Tile network + shared BUS16 (8 lane) over VSB |
| **Tile** | Fabric node: 8 input lanes, 8 output lanes, local FUSE, weights, routing_flags16 |
| **Level16** | Value 0..15 (4 bits), "level/energy" semantics |
| **READ phase** | Tiles sample input, update runtime, do NOT drive BUS |
| **WRITE phase** | Conductor stops driving bus, Island drives BUS16 |
| **Domain** | 0..15; group of tiles for thr_cur16/locked reset |
| **ACTIVE(t)** | Tile "in live chain": can read BUS16, compute, apply decay |

---

## 2) Hard Constants (v0.2)

| Parameter | Value |
|-----------|-------|
| **VSB** | 8 data lines VSB[0..7] |
| **BUS16** | 8 lanes, contribution summation in WRITE |
| **Domains** | 16 domains (0..15) |
| **Level16** | 0..15 (4 bits) |
| **RoutingFlags16** | 10 used bits: N,E,S,W,NE,SE,SW,NW,BUS_R,BUS_W |

---

## 3) EV_FLASH Cycle Phases

```
┌─────────────────────────────────────────────────────────┐
│  EV_FLASH(tag_u32)                                      │
│  ┌─────────────┐  ┌────────────┐  ┌─────────────────┐  │
│  │ PHASE_READ  │→ │ TURNAROUND │→ │ PHASE_WRITE     │  │
│  │ (Island)    │  │ (gap)      │  │ (Island)        │  │
│  └─────────────┘  └────────────┘  └─────────────────┘  │
│         ↓                ↓                  ↓           │
│    Sample input     Conductor:    Tiles drive outputs  │
│    (VSB)            Hi-Z VSB      to BUS16            │
│         ↓                ↓                  ↓           │
│  ┌─────────────────────────────────────────────────┐   │
│  │ READOUT_SAMPLE → Conductor reads BUS16[0..7]    │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

**Target latency:** ~40µs on i5-3550 (Emulator)

---

## 4) Event Protocol

### External Events (API)

| Event | Description | Requirements |
|-------|-------------|--------------|
| **EV_FLASH(tag_u32)** | Executes READ→WRITE cycle | BAKE_APPLIED==1 |
| **EV_RESET_DOMAIN(mask16)** | Domain reset | Between EV_FLASH |
| **EV_BAKE()** | Apply BakeBlob | Between EV_FLASH |

---

## 🔐 Full Specification

Remaining sections (Tile Model, FUSE-LOCK logic, RoutingFlags16, BUS Semantics) are available only to **Tier 3+** members after subscription confirmation.

### What's inside the full version:

- **Tile Model v0.2** — Baked state and runtime state of the tile
- **FUSE-LOCK logic** — Mathematical model of decay and activation
- **RoutingFlags16** — Full routing bitfield map
- **BUS Semantics** — Honest summation and overflow handling

---

## How to Get Full Access

1. Become a **Tier 3 (Industrialist)** sponsor or higher at [Boosty](https://boosty.to/intentgarden)
2. After confirmation you will receive:
   - Full specification with formulas
   - Emulator reference implementation
   - Integration documentation

### 🧠 Personas Store (Coming Soon)

In development — neuromorphic personalities marketplace. Tier 3+ members get the opportunity to:
- Reserve a niche in the future personas store
- Submit their personality for publication
- Earn royalties from usage

---

## Links

- [Decima8 Overview](decima_overview.md)
- [Visual IDE](ide_ui.md)
- [TLV and UDP Formats](formats.md)
- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Swarm Hierarchy](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
