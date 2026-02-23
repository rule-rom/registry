# 🔒 Decima8: TLV and UDP Formats

**Version:** v0.2  
**Status:** DESIGN FREEZE

---

## 🔐 Access Restricted: Tier 3+ (Industrial)

This page contains Decima8 binary format specifications.

> **Warning:** This document contains exact data structures, field offsets, and CRC algorithms. Access limited to **Tier 3+** members.

---

## 1) Bake Binary TLV Spec v0.2

### General Rules

- **Endian:** Little-endian
- **Alignment:** 4-byte boundary
- **CRC32:** IEEE (zlib/crc32) over entire blob

### Header (28 bytes)

```
BakeBlobHeader:
  magic         char[4]  = "D8BK"
  version       u16      = 2.0
  flags         u32
  total_len     u32
  bake_id       u32
  profile_id    u32
  reserved      u32
```

### TLV Sections

| Section | Description |
|---------|-------------|
| **TOPOLOGY** | Tile array topology (dimensions, count) |
| **TILE_PARAMS** | Per-tile parameters (thresholds, decay, domains) |
| **ROUTING_FLAGS** | Routing flags for each tile |
| **WEIGHTS** | 8×8 weight matrices for each tile |
| **RESET_MASKS** | Domain auto-reset masks |
| **READOUT_POLICY** | Readout policy configuration |
| **CRC32** | Checksum of entire blob |

> **Full TLV structures with offsets and field types available to Tier 3+ only.**

---

## 2) Load-time Validation

Bake loader must verify:

1. ✅ magic/version/total_len
2. ✅ All required TLV sections present
3. ✅ Section lengths match topology
4. ✅ CRC32 matches
5. ✅ reserved fields = 0
6. ✅ `thr_lo16 <= thr_hi16` for each tile
7. ✅ `tile_count == tile_w × tile_h`

---

## 3) UDP Protocol (packet_v1)

**Purpose:** Cascading Decima8 machines

### Packet Format

```
┌────────────────────────────────────────────────────────┐
│  UDP Packet (~37 bytes)                                │
│  ┌──────────────────────────────────────────────────┐  │
│  │ Header (magic, version, flags, frame_tag)        │  │
│  ├──────────────────────────────────────────────────┤  │
│  │ Payload (domain, pattern, masks, winner, bus)    │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

### Packet Fields

| Group | Fields |
|-------|--------|
| **Header** | magic ('D8UP'), version, flags, frame_tag |
| **Identity** | domain_id, pattern_id |
| **Control** | reset_mask16, collision_mask16 |
| **Status** | winner_tile_id, cycle_time_us, flags32_last |
| **Data** | bus16[8] — bus values |

### Flags (bitfield)

| Bit | Flag | Description |
|-----|------|-------------|
| 0 | has_winner | winner_tile_id valid |
| 1 | has_bus | bus16 valid |
| 2 | has_cycle | cycle_time_us valid |
| 3 | has_flags | flags32_last valid |

> **Exact packet structure with offsets and field types available to Tier 3+ only.**

---

## 4) Runtime FLAGS

Island returns FLAGS32:

| Bit | Flag | Description |
|-----|------|-------------|
| 0 | READY_LAST | Last cycle ready |
| 1 | OVF_ANY_LAST | Overflow in last cycle |
| 2 | COLLIDE_ANY_LAST | Collision in last cycle |

---

## 5) Key Metrics

| Component | Target |
|-----------|--------|
| **EV_FLASH (IDE)** | ~40µs (i5-3550) |
| **UDP Cascade** | ~20µs latency |

---

## How to Get Full Access

1. Become a **Tier 3 (Industrialist)** sponsor or higher
2. After confirmation you will receive:
   - Full TLV and UDP structures
   - Reference implementation
   - Integration documentation

---

## Links

- [Machine Specification](decima_contract.md)
- [Decima-API Interface](decima_integration.md)
- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Swarm Hierarchy](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
