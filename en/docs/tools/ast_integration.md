# Clang AST Integration

> **Parsing and validating C/C++ code structure via Abstract Syntax Tree.**

## Overview

Clang AST integration enables code analysis at the abstract syntax tree level, providing deterministic validation without heuristics.

## How It Works

### 1. Generate AST

Clang provides JSON tree dump via flag:

```powershell
clang -Xclang -ast-dump=json -fsyntax-only source.c > ast.json
```

### 2. Parse AST

Babashka script reads JSON and extracts:

- Function calls (`CallExpr`)
- Assignment operations (`BinaryOperator`)
- Memory access (`MemberExpr`, `ArraySubscriptExpr`)
- Type casts (`CastExpr`)

### 3. Validate Against Contracts

Extracted data is compared against EDN contracts from the registry.

## AST Example

```json
{
  "id": "0x1234",
  "kind": "CallExpr",
  "type": "void",
  "inner": [
    {
      "kind": "ImplicitCastExpr",
      "inner": [{ "kind": "DeclRefExpr", "name": "free" }]
    },
    { "kind": "ImplicitCastExpr", "name": "ptr" }
  ]
}
```

## Usage Example

### safe-free Validation

```edn
{:intent :safe-free
 :match {:kind "CallExpr" :name "free"}
 :ensure-next {:kind "BinaryOperator" :opcode "=" :value "NULL"}}
```

```c
// [[garden:intent(safe-free)]]
void cleanup(void* ptr) {
    free(ptr);
    ptr = NULL;  // ✓ Passes validation
}
// [[/garden:intent]]
```

## Tools

| Utility | Purpose |
|---------|---------|
| `clang -ast-dump=json` | Generate JSON AST |
| `bb -m garden.enforcer` | Validate against contracts |
| `bb -m garden.echo` | Generate Markdown reports |

## Links

- [Garden-Core Enforcer](enforcer.md)
- [Memory Safety](../registry/memory_safety.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
