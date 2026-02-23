# 🔒 Access Restricted: Tier 4+ (Swarm Founder)

## Swarm-Bus API

This page contains the Swarm-Bus API documentation.

### What's Inside

- **Swarm-Bus API Specification** — Full protocol description
- **UDP Protocol (packet_v1)** — Packet specification for machine cascading
- **SHM ABI** — Shared memory ABI
- **Event Protocol** — Event protocol (EV_FLASH, EV_RESET_DOMAIN, EV_BAKE)

### UDP Packet Format (37 bytes)

```
Offset  Size  Field              Description
──────  ────  ─────────────────  ───────────────────────────
0       4     magic              'D8UP' (0x50553844)
4       2     version            = 1
6       2     flags              bit0:has_winner, bit1:has_bus, ...
8       4     frame_tag          Unique frame ID
12      1     domain_id          Domain ID (0..15)
13      2     pattern_id         Pattern ID
15      2     reset_mask16       Domain reset mask
17      2     collision_mask16   Collision mask
19      2     winner_tile_id     Winner ID (on collision)
21      4     cycle_time_us      Cycle time in µs
25      4     flags32_last       FLAGS32 from last EV_FLASH
29      8     bus16[8]           BUS16[0..7] values
──────  ────  ─────────────────  ───────────────────────────
Total: 37 bytes
```

### Event Protocol (API)

| Event | Description | Requirements |
|-------|-------------|--------------|
| `EV_FLASH(tag_u32)` | Executes READ→WRITE cycle | BAKE_APPLIED==1 |
| `EV_RESET_DOMAIN(mask16)` | Domain reset | Between EV_FLASH |
| `EV_BAKE()` | Apply BakeBlob | Between EV_FLASH |

### Access Requirements

| Tier | Access |
|------|--------|
| Tier 3 (Industrialist) | ❌ Not available |
| Tier 4 (Swarm Founder) | ✅ API documentation + examples |
| Tier 5 (Swarm Node) | ✅ + reference implementation source |
| Tier 6 (High Council) | ✅ full access + extension rights |

### Usage Example (C++)

```cpp
// Initialize Swarm-Bus
SwarmBus bus;
bus.init("/dev/shm/decima8");

// Prepare input vector
Level16 input[8] = {5, 10, 3, 8, 12, 0, 7, 15};
bus.set_vsb_ingress(input);

// Execute EV_FLASH
auto readout = bus.ev_flash(0x12345678);

// Read result
for (int i = 0; i < 8; i++) {
    printf("BUS16[%d] = %d\n", i, readout[i]);
}
```

### How to Get Access

1. Become a **Tier 4 (Swarm Founder)** sponsor or higher
2. After payment confirmation you will receive API documentation access

---

## Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Swarm Hierarchy](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
