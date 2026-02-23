# Bounds Control (BOUNDS)

**Intent ID:** `:bounds-checked-access`  
**Status:** 🚧 Draft  
**Version:** 0.1

## Description

Contract for array and buffer bounds checking. Provides deterministic validation of indices before memory access.

## EDN Specification (Draft)

```edn
{:intent :bounds-checked-access
 :entities {:array {:type "T[]" :role :bounded-buffer}
            :index {:type "size_t" :role :access-key}}
 :invariants [
   ;; Rule: index must be within [0, size)
   {:op :access :target :array :index :index
    :precondition {:and 
      [{:gte :index 0}
       {:lt :index :size}]}}
 ]
 :tags {:wrap "// [[garden:bounds-checked]]"}}
```

## Usage Example

### ✅ Valid Code

```c
// [[garden:intent(bounds-checked-access)]]
int safe_array_access(int* arr, size_t size, size_t index) {
    if (index < size) {  // Bounds check
        return arr[index];
    }
    return -1;  // Out of bounds error
}
// [[/garden:intent]]
```

### ❌ Violation

```c
// [[garden:intent(bounds-checked-access)]]
int unsafe_access(int* arr, size_t size, size_t index) {
    return arr[index];  // ERROR: no bounds check
}
// [[/garden:intent]]
```

## Development Status

This intent is under development. Contact the [High Council](../spec/hierarchy.md) to participate in testing.

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
