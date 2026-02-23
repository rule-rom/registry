# Baking Principle

**Status:** Public  
**Version:** 1.0

---

## 🍞 What is "Baking"?

**Baking** is the process of fixing an intent into the substrate. After baking, the configuration becomes immutable and deterministically executes on the neuromorphic core.

### Real-life Analogy

| Stage | Baking | Decima8 |
|-------|--------|---------|
| 1 | Mixing dough | Preparing EDN contract |
| 2 | Shaping | Compiling to Bake Blob |
| 3 | **Baking** | **EV_BAKE()** — fixing |
| 4 | Finished product | Execution on Swarm |

---

## 🔄 Intent Lifecycle

```
┌─────────────────────────────────────────────────────────┐
│  Intent Lifecycle                                       │
│                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│  │  Human   │ →  │   EDN    │ →  │  Bake Blob       │  │
│  │  Intent  │    │ Contract │    │  (TLV encoded)   │  │
│  └──────────┘    └──────────┘    └──────────────────┘  │
│                                          ↓              │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│  │  Swarm   │ ←  │  Fixed   │ ←  │   EV_BAKE()      │  │
│  │  Execute │    │  State   │    │   (Apply)        │  │
│  └──────────┘    └──────────┘    └──────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

---

## 🧱 Abstraction Layers

### 1. Human Intent

Architect formulates the task in natural language + formal constraints.

### 2. EDN Contract (Formal Contract)

Intent is translated to EDN — a machine-readable contract format.

```edn
{:intent :safe-pointer-lifecycle
 :rules [{:match :call :name "free"}
         {:ensure-next :assignment :value "NULL"}]}
```

### 3. Bake Blob (Binary Fixation)

EDN is compiled to binary TLV format, ready for core loading.

**What's inside Bake Blob:**
- Tile array topology
- Tile parameters (thresholds, decay, domains)
- Weight matrices
- Routing flags

### 4. Fixed State (Baked State)

After `EV_BAKE()`, configuration becomes immutable for the execution duration.

---

## ⚡ Why "Baking"?

| Property | Description |
|----------|-------------|
| **Immutability** | After baking, parameters cannot change without new EV_BAKE() |
| **Determinism** | Same Bake Blob gives same behavior on any compatible core |
| **Verifiability** | Bake Blob can be audited before application |
| **Efficiency** | Binary format optimized for fast loading and execution |

---

## 🔐 Access to Details

Full Bake Blob specification (TLV structures, encoding, CRC) is available only to **Tier 3 (Industrialist)** and above.

### What's behind NDA:

- Exact TLV header structures
- Weight and threshold encoding formats
- CRC32 calculation algorithms
- EV_BAKE() transaction specification

---

## 📋 Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Swarm Hierarchy](../spec/hierarchy.md)
- [Intents Registry](../registry/index.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
