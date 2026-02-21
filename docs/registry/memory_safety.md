# Intent: C-SAFE-01 (Memory Safety & Bounds Enforcement) 🛡️🌿

## 📜 Definition
This Intent defines a strict deterministic contract for array access and buffer management in C/C++. It eliminates stochastic "guessing" of buffer sizes and pointer validity.

## 🏗️ AI Agent Implementation Rules
When implementing code under this Intent, the AI Agent **MUST** follow these structural constraints:

1. **Mandatory Bounds Checking**: Every array subscript access (`buffer[i]`) must be wrapped in an explicit `if` statement or an `assert` that validates `index < buffer_size`.
2. **Garden Tags**: Every block of code fulfilling this Intent must be wrapped in:
   ```c
   // [[garden:intent(C-SAFE-01)]]
   ... implementation ...
   // [[/garden:intent(C-SAFE-01)]]
