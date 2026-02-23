# Garden-Core Enforcer

> **Deterministic verification engine for AI-generated C/C++ code.**

## Description

**Garden-Core Enforcer** is a Clojure/Babashka engine that audits code through Clang AST and EDN contracts. It serves as the "last mile" between stochastic AI output and predictable code execution.

### How It Works

> **We don't fix AI mistakes. We create an environment where invalid code physically cannot pass the build stage.**

## 🔄 Semantic Anchor Lifecycle

```
1. Human Definition → Architect defines the task
2. Intent Formalization → Formalize as EDN contract
3. Prompt Injection → Contract + requirements to AI
4. AI Coding & Tagging → AI writes code with garden-tags
5. AST Enforcement → Validation via Clang AST
6. Certification → Deterministic safety proof
```

## 🚀 Quick Start

### 1. Environment Setup (Windows)

```powershell
scoop install babashka llvm
```

### 2. Generate AST

```powershell
clang -Xclang -ast-dump=json -fsyntax-only test.c > ast.json
```

### 3. Run Enforcer

```powershell
bb -m garden.enforcer ast.json
```

## 📋 Tagging Protocol (Garden-Tagging)

Every code block related to an Intent **MUST** be tagged:

```c
// [[garden:intent(INTENT_ID)]]
void implementation_starts_here() {
    // Your logic
}
// [[/garden:intent]]
```

### Tagging Rules

| Rule | Description |
|------|-------------|
| **No Orphans** | Never place a tag without implementation |
| **Exact ID** | `INTENT_ID` must match the key in `.edn` file |
| **Scope** | Tags wrap the smallest logical unit |
| **Vacuum Rule** | Code outside `[[garden:intent]]` is dead and discarded |

## 📂 Project Structure

```
garden-core/
├── deps.edn              # Babashka/Clojure configuration
├── src/
│   ├── enforcer.clj      # Engine: AST parsing + validation
│   └── echo.clj          # Markdown report generator
├── specs/                # Local intent prototypes (EDN)
└── scripts/              # Build utilities
```

## 🛠️ Technology Stack

| Component | Purpose |
|-----------|---------|
| **Clojure / Babashka** | Fast data processing without JVM |
| **Clang LibTooling** | Parse `-ast-dump=json` |
| **EDN** | Contract format (human + machine) |

## 📜 Manifest

1. **Code is Cheap, Meaning is Expensive** — value in intentions
2. **AI is Gas, Formal Logic is Brakes** — deterministic oversight
3. **Lisp is the Ideal Contract Language** — homoiconicity for Constitution
4. **Semantic Cage** — AI generates inside DSL rule cage
5. **Validation Instead of Hope** — check at generation time
6. **Zero-Cost Security** — contract at metaprogramming stage
7. **Death of the "Black Box"** — AI in white box of intentions

## Links

- [Source Code](https://github.com/intent-garden/core)
- [Intents Registry](../registry/index.md)
- [Agent Contract](../spec/agent_contract.md)

---

[🌿 Garden-Core](https://intent-garden.org) | [📖 Rule-Rom](https://rulerom.com)

**Bake the Future. Build the Substrate.** 🛠️⚡️
