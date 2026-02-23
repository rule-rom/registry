# Type Validation (TYPE-INTENT)

**Intent ID:** `:type-safe-cast`  
**Status:** 🚧 Draft  
**Version:** 0.1

## Description

Contract for safe type casting in C/C++. Provides cast tracing and type compatibility validation.

## EDN Specification (Draft)

```edn
{:intent :type-safe-cast
 :entities {:source {:type "T*" :role :cast-from}
            :target {:type "U*" :role :cast-to}}
 :invariants [
   ;; Rule: cast must be explicit
   {:op :cast :from :source :to :target
    :require-explicit true}
   
   ;; Rule: forbid dangerous casts
   {:forbid {:cast :from :const-ptr :to :non-const-ptr}}
 ]
 :tags {:wrap "// [[garden:type-checked]]"}}
```

## Usage Example

### ✅ Valid Code

```c
// [[garden:intent(type-safe-cast)]]
void process_data(const void* data) {
    // Explicit cast preserving const
    const int* values = (const int*)data;
    process_values(values);
}
// [[/garden:intent]]
```

### ❌ Violation

```c
// [[garden:intent(type-safe-cast)]]
void unsafe_cast(const void* data) {
    // ERROR: removing const without explicit intent
    int* values = (int*)data;  // Type safety violation
}
// [[/garden:intent]]
```

## Development Status

This intent is under development. Contact the [High Council](../spec/hierarchy.md) to participate in testing.

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
