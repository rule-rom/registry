# Memory Safety (C-SAFE)

**Intent ID:** `:safe-pointer-lifecycle`  
**Status:** ✅ Public  
**Version:** 1.0

## Description

This contract ensures safe pointer lifecycle in C: after calling `free()`, the pointer **must** be set to `NULL`.

## EDN Specification

```edn
{:intent :safe-pointer-lifecycle
 :rules [
   {:match :call :name "free"}
   {:ensure-next :assignment :value "NULL"}
   {:forbid-after :read-access}
 ]
 :tags ["[[garden:free]]"]}
```

## Full Specification (Resource Guard)

```edn
{:intent :resource-guard
 :entities {:ptr {:type "void*" :role :managed-handle}}
 :invariants [
   ;; Rule: after free, pointer MUST be NULL
   {:op :call :fn "free" :args [:ptr]
    :must-follow {:op :assign :target :ptr :value "NULL"}}

   ;; Rule: no read access after free
   {:op :access :target :ptr :mode :read
    :forbidden-after {:op :call :fn "free"}}]
 :tags {:wrap "// [[garden:scoped]]"}}
```

## Usage Example

### ✅ Valid Code

```c
// [[garden:intent(safe-pointer-lifecycle)]]
void cleanup_resource(void* ptr) {
    if (ptr != NULL) {
        free(ptr);
        ptr = NULL;  // Required!
    }
}
// [[/garden:intent]]
```

### ❌ Violation

```c
// [[garden:intent(safe-pointer-lifecycle)]]
void unsafe_cleanup(void* ptr) {
    free(ptr);
    // ERROR: ptr not set to NULL
    // Violation: must-follow {:op :assign :target :ptr :value "NULL"}
}
// [[/garden:intent]]
```

## Validation Rules

| Rule | Description | Status |
|------|-------------|--------|
| `must-follow` | After `free()` must be `ptr = NULL` | Required |
| `forbidden-after` | No read access after `free()` | Required |
| `match` | Match `free()` call | Required |

## Tools

- **Enforcer:** Babashka script for AST validation
- **Echo:** Markdown report generator

## Links

- [Garden-Core Enforcer](../tools/enforcer.md)
- [Agent Contract](../spec/agent_contract.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
