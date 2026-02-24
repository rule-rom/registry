# 🪗 Decima8 IDE: Visual Baking

**Status:** Public  
**Version:** 1.0

---

## 🎯 What is Decima8 IDE?

**Decima8 IDE** is a visual environment for baking neuromorphic personalities. Here you manually configure tiles, weights, and thresholds while observing recognition in real-time.

---

## 🏗️ IDE Interface

![Decima8 IDE Interface](../img/ide1.png)

*Visual baking environment: input patterns accordion, tile swarm, params and solutions panels*

---

## 🧩 Interface Components

### 🛠 Control Panel

| Button | Function |
|--------|----------|
| **▶ Play** | Start EV_FLASH cycle |
| **⏸ Pause** | Pause between cycles |
| **⏹ Stop** | Stop and reset domains |
| **🔁 Auto-Bake** | Automatic baking for pattern |
| **⚙ Swarm Params** | Global settings (size, domains, timings) |

### 🪗 Accordion (Input Patterns)

**VSB Tape** — visual representation of input data:
- **8 columns** = 8 VSB lines[0..7]
- **Values 0..15** = Level16 (Arabic numerals)
- **Tape scrolls** — data fed per tick

**Pattern Example:**
```
Tick 1: [5, 10, 3, 8, 12, 0, 7, 15] → Recognize "5"
Tick 2: [0, 2, 7, 1, 9, 4, 6, 3]    → Recognize "0"
```

### 🕸 Swarm (Tiles)

**Tile array visualization:**
- **Each tile** = one neuron with local memory
- **Color** = activity (thr_cur16)
- **Border** = locked status (fuse latched)
- **Arrows** = children activation directions (N,E,S,W...)

**Display Modes:**
- **Weights** — 8×8 weight matrix
- **Activation** — current thr_cur16
- **Routing** — routing flags

### 🎛 Tile Params Panel

Click on a tile to open editor:

| Parameter | Description | Range |
|-----------|-------------|-------|
| **BUS_R** | Read bus (ACTIVE source) | 0/1 |
| **BUS_W** | Write to bus (WRITE phase) | 0/1 |
| **Threshold** | Fuse range [thr_lo..thr_hi] | -32768..+32767 |
| **Decay** | Decay strength to zero | 0..32767 |
| **Pattern ID** | Pattern to recognize | 0..32767 |
| **Domain** | Reset group | 0..15 |
| **Priority** | Winner on collision | 0..255 |
| **Directions** | Children activation (N,E,S,W,NE,SE,SW,NW) | 8 bits |

### 🎯 Solutions Panel

**Recognized patterns output:**
- **Pattern** — recognized pattern ID
- **Confidence** — confidence (0..1)
- **Tile ID** — which tile made decision

**Output Example:**
```
[EV_FLASH #127]
→ Pattern "5" (conf: 0.87) from Tile #23
→ Pattern "3" (conf: 0.62) from Tile #45
→ Pattern "8" (conf: 0.45) from Tile #12
```

---

## 🔄 Workflow

### 1. Load Pattern

```
1. Open pattern (.d8p file)
2. Accordion fills with input data
3. Swarm shows current tile state
```

### 2. Configure Tiles

```
1. Click on tile in swarm
2. Configure thresholds, weights, decay
3. Set activation directions
4. Repeat for all tiles
```

### 3. Baking

```
1. Press 🔁 Auto-Bake
2. IDE adjusts weights for pattern
3. Fix configuration (EV_BAKE)
4. Save as .d8p
```

### 4. Recognition

```
1. Press ▶ Play
2. Tape moves through swarm
3. Solutions panel shows recognized patterns
4. Analyze confidence
```

---

## 🔐 Access

| Feature | Public | Tier 3 | Tier 4 |
|---------|--------|--------|--------|
| **IDE Binaries** | ❌ | ✅ | ✅ |
| **Patterns DB** | ❌ | ✅ | ✅ |
| **Auto-Bake** | ❌ | ✅ | ✅ |
| **SHM Integration** | ❌ | ❌ | ✅ |

---

## 🤗 AI Agent (Coming Soon)

In development — AI agent for automatic weight and topology tuning:

- **Task:** You specify pattern bank and target metrics
- **Agent:** Runs the machine, adjusting weights and topology
- **Result:** Ready-baked personality for the store

---

## 📋 Links

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Swarm Hierarchy](../spec/hierarchy.md)
- [Patterns Database](patterns_db.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
