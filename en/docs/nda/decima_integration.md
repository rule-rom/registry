# 🔒 Decima8: Decima-API Interface

**Version:** v0.2  
**Status:** DESIGN FREEZE

---

## 🔐 Access Restricted: Tier 3+ (Industrial)

This page contains the Decima8 API documentation.

> **Warning:** This document contains SHM ABI details, data structures, and code examples. Access limited to **Tier 3+** members.

---

## 1) Event Protocol (API)

### External Events

| Event | Description | Requirements |
|-------|-------------|--------------|
| **EV_FLASH(tag_u32)** | Executes one deterministic READ→WRITE cycle | BAKE_APPLIED==1 |
| **EV_RESET_DOMAIN(mask16)** | Domain reset | Between EV_FLASH only |
| **EV_BAKE()** | Apply BakeBlob atomically | Between EV_FLASH only |

### EV_FLASH Internal Sub-phases

```
1. PHASE_READ         — all tiles sample input
2. TURNAROUND         — Conductor: Hi-Z VSB, Island: prepare drive
3. PHASE_WRITE        — Island drives BUS16
4. READOUT_SAMPLE     — Conductor reads BUS16[0..7]
5. INTERPHASE_AUTORESET — optional
```

---

## 2) SHM ABI (Shared Memory)

### Shared Memory Layout

```
┌─────────────────────────────────────────────────────────┐
│  SHM Region (~64 KB)                                    │
│  ┌───────────────────────────────────────────────────┐  │
│  │ Header (status, bake_id, profile_id, flags)       │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ Config Staging (BakeBlob buffer)                  │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ VSB_INGRESS (input vector)                        │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ OUT_buf (readout + FLAGS32)                       │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### Characteristics

| Parameter | Value |
|-----------|-------|
| **Alignment** | 4096 bytes (page-aligned) |
| **Protocol** | Lock-free ring buffer |
| **Latency** | <1 µs per operation |

---

## 3) API Functions

### Initialization

```
decima_init(shm_path)      — initialize SHM
decima_shutdown()          — close
```

### Configuration

```
decima_bake_prepare(blob)  — load BakeBlob to staging
decima_bake_apply()        — apply (EV_BAKE)
decima_reset_domains(mask) — reset domains
```

### Execution

```
decima_set_vsb_ingress(input) — set input
decima_ev_flash(tag)          — execute cycle
decima_get_readout()          — read result
decima_get_flags32()          — read FLAGS
```

### Status

```
decima_is_ready()            — check readiness
decima_get_bake_id()         — current bake_id
decima_get_profile_id()      — current profile_id
```

---

## 4) EV_BAKE Errors

| Code | Name | Description |
|------|------|-------------|
| 0 | OK | Success |
| -1 | BakeBadMagic | Invalid magic |
| -2 | BakeBadVersion | Invalid version |
| -3 | BakeBadLen | Invalid length |
| -4 | BakeMissingTLV | Missing TLV |
| -5 | BakeBadTLVLen | Invalid TLV length |
| -6 | BakeCRCFail | CRC32 error |
| -7 | BakeReservedNonZero | Reserved ≠ 0 |
| -8 | TopologyMismatch | Topology mismatch |
| -9 | BakeNoBlob | No BakeBlob |

---

## 5) Readout Timing

### Default R0_RAW_BUS

Conductor reads BUS16[0..7] immediately after PHASE_WRITE completion.

### Latencies (target)

| Operation | Latency |
|-----------|---------|
| EV_FLASH | ~40 µs (i5-3550) |
| SHM read | <1 µs |
| EV_BAKE | ~100 µs |

---

## 6) CFG Interface (SPI-like)

Via CFG interface (CFG_CS, CFG_SCLK, CFG_MOSI, CFG_MISO):

- Load BakeBlob to staging
- Read FLAGS
- EV_RESET_DOMAIN(mask16) command
- Read BAKE_ID_ACTIVE / PROFILE_ID_ACTIVE (optional)

---

## How to Get Full Access

1. Become a **Tier 3 (Industrialist)** sponsor or higher
2. After confirmation you will receive:
   - Full API with code examples
   - Reference implementation
   - Integration documentation

---

## Links

- [Machine Specification](decima_contract.md)
- [TLV and UDP Formats](formats.md)
- [IDE Binaries](ide_binaries.md)
- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Swarm Hierarchy](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
