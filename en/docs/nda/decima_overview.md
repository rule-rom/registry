# 🔒 Decima8: Machine and Network

**Status:** v0.2 DESIGN FREEZE  
**Codename:** Siberian Tank Interface

---

## 📊 Access Tiers

| Section | Public | Tier 3 | Tier 4 | Tier 5 | Tier 6 |
|---------|--------|--------|--------|--------|--------|
| [Architecture Overview](decima_overview.md) | ✅ | ✅ | ✅ | ✅ | ✅ |
| [Visual IDE](ide_ui.md) | ✅ | ✅ | ✅ | ✅ | ✅ |
| [Tiles & Weights Concept](decima_concepts.md) | ✅ | ✅ | ✅ | ✅ | ✅ |
| [Machine Specification](decima_contract.md) | ❌ | 🔐 | 🔐 | 🔐 | 🔐 |
| [TLV and UDP Formats](formats.md) | ❌ | 🔐 | 🔐 | 🔐 | 🔐 |
| [Decima-API Interface](decima_integration.md) | ❌ | 🔐 | 🔐 | 🔐 | 🔐 |
| [IDE Binaries](ide_binaries.md) | ❌ | 🔐 | 🔐 | 🔐 | 🔐 |
| [Patterns Database](patterns_db.md) | ❌ | 🔐 | 🔐 | 🔐 | 🔐 |
| [Standalone Emulator](standalone_emulator.md) | ❌ | ❌ | 🔐 | 🔐 | 🔐 |
| [Swarm-Bus API](swarm_bus_api.md) | ❌ | ❌ | 🔐 | 🔐 | 🔐 |
| [ASIC/FPGA Specification](asic_spec.md) | ❌ | ❌ | ❌ | 🔐 | 🔐 |
| [RTL and Topology](rtl_topology.md) | ❌ | ❌ | ❌ | ❌ | 🔐 |
| [Swarm-Bus Internals](swarm_internals.md) | ❌ | ❌ | ❌ | ❌ | 🔐 |

**Legend:**
- ✅ Public — open documentation
- 🔐 NDA — requires appropriate tier access

---

## 🏗️ What is Decima8?

**Decima8** is a neuromorphic computing core operating on principles of deterministic decay and threshold-based activation.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Tile** | Minimal computational unit with local weight memory and state |
| **Weights** | 8×8 connection matrix defining input signal transformation |
| **Decay** | Deterministic decrease of tile activity over time |
| **Activation Threshold** | Value range where tile "latches" into active state |
| **Baking Process** | Fixing intent into substrate — converting config to immutable state |

### Architectural Principles

```
┌─────────────────────────────────────────────────────────┐
│  Decima8 Resonance Swarm                                │
│                                                         │
│  ┌─────────────┐     ┌──────────────┐     ┌─────────┐  │
│  │   IDE /     │ →   │   Emulator   │ →   │  Swarm  │  │
│  │  Editor     │     │  (SHM IPC)   │     │  Core   │  │
│  └─────────────┘     └──────────────┘     └─────────┘  │
│         ↑                    ↑                  ↑       │
│    Configuration      Deterministic       Physical     │
│    (Bake Blob)        cycle (~40µs)       execution    │
└─────────────────────────────────────────────────────────┘
```

### Key Metrics

| Component | Metric | Status |
|-----------|--------|--------|
| **IDE / Emulator** | ~40µs per cycle | Target (i5-3550) |
| **UDP Cascade** | ~20µs latency | Target |
| **Precision** | Level16 (0..15) | Deterministic |
| **Scale** | Up to 256 tiles in array | Configurable |

---

## 🔐 How to Get Access

### Tier 3 (Industrialist) — 3,000 ₽ / ~$34 / ~¥245

**Access:**
- Full Decima8 machine specification
- Bake Blob (TLV) and UDP Protocol formats
- Decima8 IDE binaries
- Patterns database

### Tier 4 (Swarm Founder) — 15,000 ₽ / ~$170 / ~¥1,225

**Additionally:**
- Standalone emulator with SHM integration
- Swarm-Bus API for machine cascading
- License to deploy "Baked Personalities"

### Tier 5 (Swarm Node) — 100,000 ₽ / ~$1,130 / ~¥8,170

**Additionally:**
- Full ASIC/FPGA specification
- License for custom silicon integration
- Priority access to first ASIC batch

### Tier 6 (High Council) — 1M+ ₽ / ~$11,300+ / ~¥81,700+

**Additionally:**
- RTL and silicon topology
- Swarm-Bus internals at transaction level
- Voting rights on specification evolution

---

## 📋 Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Swarm Hierarchy](../spec/hierarchy.md)
- [Visual IDE](ide_ui.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
