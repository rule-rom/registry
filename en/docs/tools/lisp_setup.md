# Babashka & Clojure

> **Clojure-based tools for automation and validation in the Rule-ROM ecosystem.**

## Setup

Clojure-based tools for task automation in the Rule-ROM ecosystem.

## Requirements

- **Java 11+** (for Babashka)
- **Babashka** (scripting Clojure without JVM)

## Installation

### Windows (via Scoop)

```powershell
scoop install babashka
```

### Linux

```bash
curl -s https://raw.githubusercontent.com/babashka/babashka/master/install | sudo bash
```

### macOS (via Homebrew)

```bash
brew install babashka
```

## Verify Installation

```bash
bb --version
```

## Usage in Rule-ROM

### Run Enforcer

```powershell
# Generate AST
clang -Xclang -ast-dump=json -fsyntax-only test.c > ast.json

# Validate
bb -m garden.enforcer ast.json
```

### Run Echo (Report Generation)

```powershell
bb -m garden.echo ast.json
```

## EDN Contract Example

```edn
{:intent :safe-free
 :entities [:ptr]
 :must-set-null true
 :description "After free(), pointer must be set to NULL"}
```

## Garden-Tag Example in C

```c
// [[garden:intent(safe-free)]]
void cleanup(void* ptr) {
    free(ptr);
    ptr = NULL;
}
// [[/garden:intent]]
```

## Resources

| Resource | Description |
|----------|-------------|
| [Babashka Docs](https://book.babashka.org/) | Official documentation |
| [Clojure EDN](https://clojure.org/reference/reader) | EDN specification |
| [Garden-Core](https://github.com/intent-garden/core) | Engine source code |

## Links

- [Garden-Core Enforcer](enforcer.md)
- [Intents Registry](../registry/index.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
