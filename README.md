# ATLAS

**ATLAS** is a source-available, C++-first AI development environment for building, compiling, previewing, inspecting, and visually modifying software through a bidirectional live workspace.

ATLAS is designed for professional software developers, systems programmers, UI framework engineers, tool builders, AI coding researchers, game-tool developers, embedded engineers, and teams working on heavy compiled applications.

ATLAS is not a generic code editor with an AI chat box. It is an IDE architecture for connecting source code, compilation, live execution, visual interaction, grid-aware layout editing, runtime inspection, and AI agents into one controlled engineering loop.

```text
Code -> Build -> Run -> Preview -> Interact -> Interpret -> Patch -> Validate -> Rebuild
```

---

## Repository Description

AI-assisted C++-first IDE with live compiled preview, isolated runtime execution, grid-aware visual editing, interaction-to-code translation, and source-aware AI agent orchestration.

---

## Core Goal

ATLAS exists to close the distance between source code and the compiled application experience.

A developer should be able to write code, compile the program, view the running application inside the IDE, interact with the application visually, and convert those interactions into safe, reviewable source-code changes.

The project is built around bidirectional development:

- Source code creates the compiled application.
- The compiled application exposes runtime structure.
- User interaction becomes structured intent.
- Structured intent becomes a source patch.
- Patches are reviewed, validated, and applied.
- The preview rebuilds or hot-reloads.

---

## Recommended Technology Stack

ATLAS should be implemented with a **Rust core**, **Slint UI**, and **C++ tooling integration**.

```text
Rust         -> IDE kernel, sandbox controller, IPC host, agent runtime, project graph
Slint        -> primary ATLAS UI layer
C++          -> primary target language, native adapters, preview integrations
CMake/Ninja  -> default C++ build pipeline
LLVM/Clang   -> parsing, diagnostics, AST rewriting, source mapping
clangd       -> code intelligence and symbol indexing
```

### Language Decision

Rust is recommended for the IDE kernel because ATLAS must coordinate processes, sandboxes, IPC, agents, build systems, and patch validation. These areas benefit from memory safety, strong concurrency, and strict control over system resources.

C++ remains the primary target language and a first-class integration layer. ATLAS must deeply understand C++ projects, compile C++ efficiently, index C++ code, integrate with CMake and Ninja, use clangd, and support native preview workflows.

| Area | Recommended Choice | Reason |
| --- | --- | --- |
| IDE kernel | Rust | Safe concurrency, process control, sandbox orchestration |
| Primary target language | C++ | Professional native, systems, embedded, graphics, tools, and heavy compiled software |
| UI framework | Slint | Lightweight declarative UI with Rust and C++ support |
| Build pipeline | CMake + Ninja | Industry-standard C++ configuration with fast incremental builds |
| Code intelligence | clangd + LLVM | C++ indexing, diagnostics, AST parsing, source rewriting |
| AI agent runtime | Rust | Async orchestration, permissioned tools, controlled patch generation |
| Plugin surface | Rust, C++, or WASM | Native performance with optional sandboxed extensions |

Python and JavaScript may be used for extensions, prototypes, scripts, model adapters, and automation, but they are not recommended as the core runtime for ATLAS.

Electron is not recommended as the main shell because ATLAS requires tight native process control, low overhead, compiled preview embedding, and predictable performance.

---

## User Interface Model

ATLAS uses a full-screen, two-sided workspace.

```text
+---------------------------------------------------------------+
|                            ATLAS                              |
+-------------------------------+-------------------------------+
| Left Workspace                | Right Workspace               |
|                               |                               |
| Project tree                  | Live compiled preview          |
| Code editor                   | Runtime interaction surface    |
| AI agent panel                | Grid overlay                   |
| Build output                  | Selection handles              |
| Diagnostics                   | Inspector overlay              |
| Patch review                  | Visual event recorder          |
| Terminal                      | Runtime state viewer           |
|                               |                               |
+-------------------------------+-------------------------------+
```

### Left Workspace

The left side is the source, build, and intelligence workspace.

It contains:

- Project tree
- Code editor
- Symbol search
- AI command panel
- Agent activity stream
- Build configuration
- Compiler diagnostics
- Static analysis output
- Terminal
- Test runner
- Git status
- Patch review
- Diff viewer
- Undo and rollback controls

### Right Workspace

The right side is the live runtime and visual interaction workspace.

It contains:

- Compiled application preview
- Runtime input forwarding
- Grid overlay
- Snap guides
- Selection overlay
- Resize handles
- Property inspector
- Scene graph panel
- Layout constraint panel
- Accessibility warnings
- Visual event recorder
- Runtime logs
- FPS and performance telemetry
- Process status
- Sandbox status

---

## Virtual Runtime Environment

ATLAS must separate the IDE process from the compiled preview process.

The previewed application must never run inside the same trusted process as the IDE shell. A crash, memory corruption bug, infinite loop, malicious code path, AI-generated mistake, or untrusted project must not destroy IDE state or access the user's full environment.

Recommended process model:

```text
ATLAS Host Process
  |
  |-- Project Indexer
  |-- Build Controller
  |-- Agent Controller
  |-- Preview Supervisor
        |
        |-- Isolated Preview Process
              |
              |-- Compiled User Application
              |-- Preview Adapter
              |-- Runtime Metadata Bridge
```

### Best Default Isolation Model

The best default environment is a **sandboxed native process with optional container and microVM escalation**.

| Mode | Use Case | Isolation | Performance |
| --- | --- | --- | --- |
| Native supervised process | Trusted local project | Low to medium | Highest |
| OS sandbox | Normal project preview | Medium | Very high |
| Container sandbox | AI-generated or semi-trusted project | Medium to high | High |
| MicroVM sandbox | Untrusted code or remote execution | High | Medium |
| Full VM | Extreme isolation | Very high | Low |

ATLAS should start with OS-level sandboxing and container support. MicroVM support should be optional for untrusted code, remote execution, or enterprise security mode.

### Platform Runtime Strategy

#### Linux

Use separate user processes, Linux namespaces, cgroups, seccomp, Landlock filesystem restrictions, AppArmor or SELinux where available, read-only mounts, project-directory-only access, no home-directory access by default, no root execution, optional containers, and optional microVM execution.

#### Windows

Use separate processes, Job Objects, AppContainer or low-integrity process mode, restricted tokens, process mitigation policies, controlled working directories, network restrictions where applicable, and optional Hyper-V or Windows Sandbox execution.

#### macOS

Use separate processes, Seatbelt sandbox profiles, hardened runtime where applicable, controlled entitlements, restricted filesystem access, temporary working directories, and optional virtualization framework execution for higher-risk code.

---

## Preview Process Architecture

The live compiled preview is a supervised runtime session, not a static screenshot.

```text
ATLAS Host
  |
  |-- Preview Supervisor
        |
        |-- Launches preview process
        |-- Loads preview adapter where supported
        |-- Captures native window or offscreen framebuffer
        |-- Forwards input events
        |-- Receives runtime metadata
        |-- Receives scene graph when supported
        |-- Reports crashes, logs, telemetry, and sandbox events
```

### Preview Transport Options

| Transport | Description | Use Case |
| --- | --- | --- |
| Native window embedding | Embed external app window inside ATLAS | Desktop apps |
| Offscreen framebuffer | App renders to shared GPU or CPU buffer | Custom renderers, engines, controlled examples |
| Remote frame streaming | App sends frames over IPC | Containers, remote build hosts, microVMs |
| Framework-native preview | Slint, QML, web, or engine preview mode | Declarative UI frameworks |
| Screenshot fallback | Periodic capture with limited interaction | Unsupported apps |

Recommended first implementation:

1. Native supervised process.
2. Native window embedding where supported.
3. Framework-native preview for Slint.
4. Offscreen framebuffer adapter for examples.
5. Containerized preview mode.

---

## IPC Design

ATLAS requires structured communication between the host IDE and preview process.

Recommended IPC channels:

```text
Control channel      -> JSON-RPC or gRPC
Event channel        -> binary or JSON event stream
Frame channel        -> shared memory, DMA-BUF, named shared memory, or socket stream
Diagnostics channel  -> structured logs and process telemetry
Agent channel        -> permissioned tool-call protocol controlled by the host
```

Example visual event:

```json
{
  "type": "visual.resize",
  "sessionId": "preview-001",
  "target": {
    "runtimeId": "button.primary",
    "sourceId": "MainWindow.PrimaryActionButton"
  },
  "from": { "x": 320, "y": 184, "width": 128, "height": 40 },
  "to": { "x": 320, "y": 184, "width": 168, "height": 48 },
  "grid": { "unit": 8, "snapped": true }
}
```

---

## Grid View and Grid Understanding

ATLAS must include a first-class grid system. The grid is not just a visual overlay. It is part of the application logic, source mapping model, accessibility system, and AI context model.

Recommended default:

```text
Base unit: 8 px
Fine unit: 4 px
Minimum interactive target: 44 x 44 px
Default spacing: 8, 16, 24, 32, 40, 48, 64
Default snap: enabled
```

The grid system provides:

- Visual alignment
- Drag snapping
- Resize snapping
- Layout consistency
- Responsive structure
- Accessibility enforcement
- Source-code intent
- AI-agent context
- Property inspector normalization
- Design-system compatibility

Grid data must be available to the visual editor, AI agent, source mapper, property inspector, accessibility checker, layout validator, and patch generator.

Example grid model:

```json
{
  "grid": {
    "unit": 8,
    "fineUnit": 4,
    "snap": true,
    "columns": 12,
    "gutter": 16,
    "margin": 24
  },
  "element": {
    "id": "PrimaryActionButton",
    "x": 320,
    "y": 184,
    "width": 168,
    "height": 48,
    "columnStart": 6,
    "columnSpan": 3,
    "align": "center",
    "touchTargetValid": true
  }
}
```

ATLAS should prefer constraints, anchors, rows, columns, and semantic layout rules over raw pixel coordinates when the framework supports them.

---

## Visual-to-Code Mapping

The central technical challenge of ATLAS is mapping runtime interaction back to source code.

ATLAS supports four mapping strategies:

1. **Declarative mapping** for Slint, QML, XAML, HTML/CSS, XML UI files, and JSON UI schemas.
2. **C++ AST mapping** for imperative UI code, Qt Widgets, Win32 wrappers, custom layout code, and engine tools.
3. **Runtime metadata mapping** for applications instrumented with an ATLAS SDK.
4. **AI-assisted mapping** for ambiguous, legacy, generated, or multi-file changes.

### Mapping Confidence

| Confidence | Meaning | Apply Policy |
| --- | --- | --- |
| High | Direct declarative source mapping | Stage automatically, show diff |
| Medium | AST or metadata mapping with clear symbol | Show diff and validation |
| Low | AI-inferred patch or ambiguous mapping | Explicit review required |
| Blocked | Unsafe or unmapped | Ask for source selection or adapter support |

Low-confidence changes must never be applied silently.

---

## Application Logic Subsystems

ATLAS is divided into explicit subsystems.

```text
atlas-core
atlas-ui
atlas-project
atlas-build
atlas-preview
atlas-sandbox
atlas-ipc
atlas-grid
atlas-inspector
atlas-mapper
atlas-agent
atlas-patch
atlas-validation
atlas-extensions
```

### atlas-core

Owns startup, shutdown, workspace sessions, command routing, event routing, configuration, plugin registry, global undo model, telemetry control, and trust policy.

### atlas-ui

Owns the Slint shell, split workspace, editor views, agent panel, preview surface, inspector, diff viewer, terminal, grid overlay, scene graph, keyboard shortcuts, and theme system.

### atlas-project

Owns project loading, file tree, workspace detection, language detection, build-system detection, symbol graph, source graph, asset graph, dependency graph, configuration discovery, and trust classification.

### atlas-build

Owns CMake integration, Ninja integration, compiler detection, build presets, build execution, diagnostics, artifact tracking, test discovery, incremental rebuilds, clean builds, and cache integration.

### atlas-preview

Owns preview process launch, window embedding, frame transport, input forwarding, restart control, runtime logs, crash reports, preview session state, hot reload hooks, and runtime telemetry.

### atlas-sandbox

Owns security profiles, filesystem restrictions, network restrictions, process restrictions, CPU and memory limits, container integration, microVM integration, temporary workspaces, and preview permissions.

### atlas-ipc

Owns protocol definition, message schema, request routing, event streams, shared memory, preview adapter transport, agent tool transport, serialization, and version negotiation.

### atlas-grid

Owns grid units, snapping, alignment, column grids, baseline grids, breakpoints, constraints, spacing tokens, accessibility target checks, and grid-aware patch hints.

### atlas-inspector

Owns selected-element properties, runtime hierarchy, accessibility metadata, event bindings, layout constraints, style tokens, and source links.

### atlas-mapper

Owns runtime-to-source mapping, declarative rewriting, C++ AST rewriting, source range tracking, adapter integration, confidence scoring, semantic patch creation, and conflict detection.

### atlas-agent

Owns AI agent orchestration, model provider abstraction, context building, prompt routing, tool permissions, project memory, task planning, multi-agent coordination, patch generation, and safety constraints.

### atlas-patch

Owns diffs, patch staging, patch review, atomic apply, revert, undo, conflict handling, Git integration, change explanations, and approval gates.

### atlas-validation

Owns formatting, static analysis, build validation, test validation, snapshot checks, accessibility checks, runtime smoke tests, security policy checks, and performance thresholds.

### atlas-extensions

Owns framework adapters, language adapters, build adapters, preview adapters, agent tools, themes, grid presets, inspector plugins, and extension sandboxing.

---

## AI Agent Architecture

ATLAS uses agents as controlled engineering workers, not uncontrolled autonomous actors.

Agents must be tool-driven, permissioned, observable, and reversible.

```text
User intent or visual event
  |
  v
Agent Router
  |
  +-- Code Agent
  +-- UI Agent
  +-- Build Agent
  +-- Debug Agent
  +-- Refactor Agent
  +-- Test Agent
  +-- Security Agent
  +-- Documentation Agent
  |
  v
Patch Generator
  |
  v
Validation Pipeline
  |
  v
Human Review
  |
  v
Apply / Reject
```

Agent rules:

1. Agents cannot directly write to the working tree without a patch stage.
2. Agents cannot run commands without declared permissions.
3. Agents cannot access files outside the trusted workspace unless approved.
4. Agents cannot send private source code to external providers unless allowed by workspace policy.
5. Agents cannot install dependencies without approval.
6. Agents cannot disable tests or security checks to make a patch pass.
7. Agents cannot silently change licensing files.
8. Agents cannot apply low-confidence visual-to-code mappings without human review.
9. Agents cannot run as root.
10. Agents must produce reversible changes.

---

## Framework Adapter Model

ATLAS must not pretend all UI frameworks are identical. Reliable visual-to-code editing requires framework-specific adapters.

Initial adapter order:

1. Slint adapter
2. QML adapter
3. Qt Widgets adapter
4. Dear ImGui inspection adapter
5. Web adapter
6. Custom C++ SDK adapter
7. Game or engine adapter

A framework adapter provides runtime element discovery, scene graph extraction, property schema, source mapping, hot reload support, preview launch rules, grid semantics, accessibility metadata, patch rules, and validation rules.

---

## Validation Pipeline

Every significant change passes through validation.

```text
Patch generated
  |
  +-- Format check
  +-- Static analysis
  +-- Build
  +-- Unit tests
  +-- UI snapshot test
  +-- Accessibility check
  +-- Runtime smoke test
  +-- Security policy check
  |
Result -> Review -> Apply or Reject
```

ATLAS should not treat an AI patch as correct until it has been validated.

---

## Repository Structure

```text
atlas/
  README.md
  LICENSE
  SECURITY.md
  ARCHITECTURE.md
  ROADMAP.md
  AGENTS.md
  GRID.md
  CONTRIBUTING.md
  docs/
    preview-runtime.md
    sandboxing.md
    visual-to-code-mapping.md
    framework-adapters.md
    ai-agent-runtime.md
  crates/
    atlas-core/
    atlas-ui/
    atlas-project/
    atlas-build/
    atlas-preview/
    atlas-sandbox/
    atlas-ipc/
    atlas-grid/
    atlas-inspector/
    atlas-mapper/
    atlas-agent/
    atlas-patch/
    atlas-validation/
    atlas-extensions/
  adapters/
    slint/
    qml/
    qt-widgets/
    imgui/
    web/
    custom-sdk/
  examples/
    cpp-slint-basic/
    cpp-qml-basic/
    cpp-widgets-basic/
    sandbox-preview/
    visual-resize-patch/
  tests/
    mapper/
    grid/
    preview/
    sandbox/
    agents/
```

---

## Roadmap

### Phase 1: ATLAS Shell

- Slint-based full-screen IDE shell
- Two-panel workspace
- File tree
- Editor placeholder
- Terminal panel
- Build output panel
- Preview placeholder
- Command bus
- Workspace state model

### Phase 2: C++ Build and Project Loading

- CMake project detection
- Ninja build support
- Compile database generation
- clangd integration
- Compiler diagnostics
- Executable discovery
- CTest integration

### Phase 3: Preview Supervisor

- Separate preview process
- Runtime logs
- Crash detection
- Preview restart
- Native window embedding
- Input forwarding
- Preview status panel

### Phase 4: Sandboxed Preview

- Linux sandbox profile
- Windows restricted process profile
- macOS sandbox profile
- Container preview mode
- Permission model
- Workspace-only filesystem policy

### Phase 5: Grid System

- 8-unit grid model
- Preview overlay
- Snapping
- Alignment guides
- Grid property inspector
- Accessibility target checks
- Grid-aware event model

### Phase 6: Slint Adapter

- Parse Slint components
- Map runtime elements to source
- Select preview elements
- Modify properties
- Apply grid-aware layout patches
- Reload preview
- Validate generated Slint changes

### Phase 7: Patch Review

- Diff viewer
- Patch staging
- Accept and reject controls
- Undo and rollback
- Git integration
- Patch explanations

### Phase 8: AI Agent Runtime

- Agent router
- Model provider abstraction
- Tool permission system
- Code Agent
- UI Agent
- Build Agent
- Debug Agent
- Test Agent
- Security Agent
- Documentation Agent

### Phase 9: C++ AST Mapping

- LibTooling support
- AST matchers
- Setter-call mapping
- Object-name mapping
- Qt Widgets adapter
- Source range confidence scoring

### Phase 10: Professional Release

- Examples
- CI
- Documentation
- Installers
- Extension SDK
- Commercial licensing workflow

---

## Availability

ATLAS is source-available under the Business Source License 1.1.

The source is available for inspection, learning, modification, redistribution, internal development, internal testing, evaluation, education, academic research, non-commercial research, demonstrations, and non-public prototypes under the terms stated in the `LICENSE` file.

Commercial production use outside the Additional Use Grant requires a separate commercial license from the licensor.

On the Change Date stated in the `LICENSE` file, the covered version converts to the Change License.

SPDX-License-Identifier: BUSL-1.1
