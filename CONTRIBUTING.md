# Contributing to ATLAS

ATLAS is source-available under the Business Source License 1.1.

Contributions should preserve the core project direction:

- C++-first development
- Rust core architecture
- Slint primary UI
- Isolated preview runtime
- Grid-aware visual editing
- Source-aware patch generation
- Human-reviewed AI agent changes
- Professional validation pipeline

---

## Contribution Areas

- Rust core systems
- Slint UI
- C++ build tooling
- clangd and LLVM integration
- Preview process supervision
- Sandboxing
- Grid engine
- Visual-to-code mapping
- Framework adapters
- AI agent orchestration
- Tests
- Documentation

---

## Code Requirements

- Keep host and preview process boundaries explicit.
- Do not introduce direct working-tree mutation from agents.
- Prefer structured events over ad hoc strings.
- Add tests for mapper and grid changes.
- Keep security defaults restrictive.
- Keep patches minimal and reviewable.
- Preserve licensing headers where present.
