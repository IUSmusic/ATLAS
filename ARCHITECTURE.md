# ATLAS Architecture

ATLAS is a Rust-core, Slint-UI, C++-first AI IDE for bidirectional development between source code and live compiled software.

```text
Code -> Build -> Run -> Preview -> Interact -> Interpret -> Patch -> Validate -> Rebuild
```

---

## Core Stack

```text
Rust        -> IDE core, process orchestration, IPC, sandboxing, agents
Slint       -> ATLAS desktop UI
C++         -> primary target language, preview adapters, native plugin surface
LLVM/Clang  -> parsing, diagnostics, source rewriting
CMake/Ninja -> default build pipeline
clangd      -> indexing, completion, diagnostics, symbol intelligence
```

---

## System Diagram

```text
+-------------------------------------------------------------+
|                         ATLAS Host                          |
|                                                             |
|  atlas-ui        atlas-core       atlas-agent               |
|  atlas-project   atlas-build      atlas-patch               |
|  atlas-preview   atlas-sandbox    atlas-mapper              |
|  atlas-grid      atlas-ipc        atlas-validation          |
|                                                             |
+-----------------------------|-------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                  Isolated Preview Runtime                   |
|                                                             |
|  compiled app     preview adapter     metadata bridge       |
|                                                             |
+-------------------------------------------------------------+
```

---

## Host Process

The host process owns trusted IDE state:

- Workspace state
- Source index
- Project graph
- Build graph
- Agent permissions
- Patch staging
- Validation results
- User settings
- Security policy

The host process must remain stable if the preview process crashes.

---

## Preview Process

The preview process owns execution of the compiled application.

It may run as:

- Native supervised child process
- OS-sandboxed process
- Containerized process
- MicroVM-isolated process

The preview process exposes structured metadata to ATLAS when an adapter is available.

---

## Isolation Model

| Tier | Runtime | Use |
| --- | --- | --- |
| 0 | Native supervised process | Trusted local project |
| 1 | OS sandbox | Default normal preview |
| 2 | Container | AI-generated or semi-trusted code |
| 3 | MicroVM | Untrusted projects |
| 4 | Full VM | Rare high-risk execution |

Default: Tier 1 for normal projects, Tier 2 for AI-generated code execution.

---

## IPC Channels

```text
Control       -> JSON-RPC or gRPC
Events        -> structured event stream
Frames        -> shared memory, DMA-BUF, named shared memory, or socket stream
Diagnostics   -> structured logs
Agent tools   -> permissioned tool protocol
```

---

## Mapping Strategies

ATLAS supports four mapping paths:

1. Declarative mapping
2. C++ AST mapping
3. Runtime metadata mapping
4. AI-assisted mapping

| Confidence | Meaning | Policy |
| --- | --- | --- |
| High | Direct declarative mapping | Stage and show diff |
| Medium | AST or metadata mapping | Show diff and validate |
| Low | AI-inferred patch | Explicit review required |
| Blocked | Unsafe or ambiguous | Do not patch automatically |

---

## Subsystems

| Subsystem | Responsibility |
| --- | --- |
| atlas-core | lifecycle, command bus, event bus, trust policy |
| atlas-ui | Slint shell, panels, editor, preview surface |
| atlas-project | project loading, indexing, source graph |
| atlas-build | CMake, Ninja, diagnostics, artifacts, tests |
| atlas-preview | launch, embed, frame transport, input, telemetry |
| atlas-sandbox | filesystem, process, network, container, microVM policies |
| atlas-ipc | protocol, schema, routing, shared memory |
| atlas-grid | grid units, snapping, constraints, accessibility checks |
| atlas-inspector | properties, scene graph, source links |
| atlas-mapper | visual-to-code translation and confidence scoring |
| atlas-agent | AI agent routing, tools, context, permissions |
| atlas-patch | diffs, staging, approval, undo, Git integration |
| atlas-validation | formatting, build, tests, accessibility, runtime checks |
| atlas-extensions | adapters, tools, plugins, grid presets |
