# 🔒 Access Restricted: Tier 6 (High Council)

## RTL and Silicon Topology

This page contains RTL code and silicon topology for Decima8 ASIC.

### What's Inside

- **RTL Source Code** — Full Verilog/SystemVerilog source
- **Synthesis Scripts** — Synthesis scripts
- **Place & Route** — Placement and routing data
- **GDSII** — Final layout for tape-out
- **Simulation Models** — Verification models

### RTL Structure

```
rtl/
├── top/
│   ├── decima8_top.v      # Top module
│   ├── tile_array.v       # Tile array (16×16)
│   ├── bus16_controller.v # BUS16 controller
│   └── vsb_interface.v    # VSB interface
├── tile/
│   ├── tile_core.v        # Tile core
│   ├── fuse_logic.v       # Fuse logic
│   ├── decay_unit.v       # Decay unit
│   ├── weight_matrix.v    # Weight matrix (8×8)
│   └── routing_logic.v    # Routing logic
├── utils/
│   ├── clamp16.v          # Clamp to 0..15
│   ├── signed_mul.v       # Signed multiply
│   └── accumulator.v      # Accumulator
└── testbenches/
    ├── tb_tile.v          # Tile testbench
    ├── tb_array.v         # Array testbench
    └── tb_top.v           # Top testbench
```

### Die Layout

```
┌─────────────────────────────────────────────────────────┐
│  Decima8 ASIC Die Layout (25 mm² est.)                  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Tile Array (16×16)                               │  │
│  │  ┌─────┬─────┬─────┬─────┐                        │  │
│  │  │ 0,0 │ 0,1 │ ... │ 0,15│                        │  │
│  │  ├─────┼─────┼─────┼─────┤                        │  │
│  │  │ ... │ ... │ ... │ ... │ 256 tiles total       │  │
│  │  ├─────┼─────┼─────┼─────┤                        │  │
│  │  │15,0 │15,1 │ ... │15,15│                        │  │
│  │  └─────┴─────┴─────┴─────┘                        │  │
│  │                                                   │  │
│  │  BUS16 lanes (8) — horizontal across array        │  │
│  │  VSB lanes (8) — vertical across array            │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  Periphery:                                             │
│  - CFG Interface (SPI-like)                             │
│  - EV_FLASH Controller                                  │
│  - READOUT Logic                                        │
│  - Domain Reset Logic                                   │
└─────────────────────────────────────────────────────────┘
```

### Access Requirements

| Tier | Access |
|------|--------|
| Tier 3-4 | ❌ Not available |
| Tier 5 (Swarm Node) | ❌ Not available |
| Tier 6 (High Council) | ✅ Full RTL + topology access |

### High Council Rights

**Tier 6** receive:

- ✅ Full RTL and topology access
- ✅ IP ASIC share (proportional to contribution)
- ✅ Direct voting rights on Rule-ROM evolution
- ✅ Exclusive architectural audit from **Root Authority**
- ✅ Veto rights on specification changes v0.2+

### How to Get Access

1. Become a **Tier 6 (High Council Elder)** sponsor — 1M+ ₽
2. After payment confirmation you will receive:
   - Access to private RTL repository
   - License agreement
   - Personal access to **Root Authority**

---

## Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Swarm Hierarchy](../spec/hierarchy.md)
- [ASIC Fund](../support/asic_fund.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
