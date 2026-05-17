# ATLAS

**ATLAS** is a source-available development environment for building, compiling, previewing, and visually modifying software through a two-sided live workspace. The project is designed for professional developers, software architects, systems programmers, tool builders, UI engineers, and research teams who need a tighter connection between source code, compiled output, runtime behaviour, and human interaction.

ATLAS focuses on a central idea: software should be editable from both directions. A developer should be able to change the source code and see the compiled result immediately, while also being able to interact with the running software visually and have those interactions translated back into structured code changes.

The primary target is high-performance native development, especially C++, while the long-term design supports multiple languages, frameworks, renderers, and build systems.

---

## Project Goal

The goal of ATLAS is to create a professional-grade coding environment where code, compilation, execution, rendering, interaction, and AI-assisted modification operate inside one continuous workflow.

ATLAS is intended to reduce the gap between what a developer writes and what a user experiences. Instead of treating the running application as a disconnected output, the IDE treats the live application as an editable surface connected to the source tree.

The project aims to support:

- AI-assisted coding across multiple languages
- Strong native C++ support
- Live build and execution workflows
- A full-screen split workspace
- A real-time compiled preview surface
- Interaction-to-code translation
- Visual inspection of runtime elements
- Drag, drop, resize, reposition, and property editing
- Source patch generation from runtime interactions
- Project-aware refactoring
- Build, test, debug, and execution feedback loops
- Extensible framework adapters for different UI and rendering systems

---

## Core Idea

Traditional IDEs are mostly text-first. Visual tools are often framework-specific and separated from the main development loop. AI coding tools can modify code, but they usually do not understand the running application as an editable, interactive system.

ATLAS combines these ideas into a single environment:

1. The developer edits code.
2. The IDE builds or hot-reloads the software.
3. The compiled application appears in a live render viewer.
4. The developer interacts with the running application.
5. The IDE captures visual and behavioural changes.
6. The system maps those changes back to source-level intent.
7. The AI agent proposes or applies code changes.
8. The application rebuilds, rerenders, and validates the result.

This creates a bidirectional development loop:

```text
Code -> Build -> Run -> Render -> Interact -> Interpret -> Patch Code -> Rebuild
```

---

## Main Interface Design

ATLAS is designed as a full-screen application with a two-panel working model.

### Left Side: Code and Intelligence

The left side is the source-control and intelligence workspace. It contains:

- Code editor
- File explorer
- AI coding assistant
- Build configuration view
- Compiler output
- Diagnostics
- Test results
- Debug session state
- Patch review interface
- Refactor and migration tools
- Project memory and architectural context

This panel is responsible for understanding the software project at the source level.

### Right Side: Live Render and Runtime Interaction

The right side is the compiled application workspace. It contains:

- Live compiled preview
- Runtime window embedding
- Visual inspector
- Element selection overlay
- Drag-and-drop editing
- Resize handles
- Layout guides
- Property editor
- Event recorder
- Interaction timeline
- Screenshot and frame capture
- Runtime state viewer

This panel is responsible for understanding the application at the execution and user-experience level.

---

## Software Logic

ATLAS is based on a layered architecture.

### 1. Project Indexing Layer

The indexing layer scans and understands the project. It handles:

- Source files
- Build files
- Header files
- UI definitions
- Assets
- Configuration files
- Tests
- Documentation
- Dependency metadata

For C++ projects, this layer is designed around compiler-aware indexing using compile databases, language servers, abstract syntax trees, symbol graphs, and build-system metadata.

### 2. Build and Execution Layer

The build layer manages compilation and execution. It is responsible for:

- Build command discovery
- CMake and Ninja integration
- Compiler diagnostics
- Incremental builds
- Hot reload where available
- Process management
- Runtime logging
- Crash detection
- Test execution
- Artifact tracking

This layer connects source edits to running binaries.

### 3. Live Render Layer

The live render layer displays the compiled application inside the IDE. It may use direct embedding, remote rendering, process capture, framebuffer streaming, window capture, or framework-specific preview adapters depending on the target application.

The render layer provides:

- Real-time preview
- Input forwarding
- Frame capture
- Visual overlays
- Selection geometry
- Runtime metadata
- Inspector hooks

### 4. Interaction Capture Layer

The interaction layer records what the developer does inside the live preview.

Examples include:

- Moving a UI element
- Resizing a component
- Reordering layout elements
- Editing text content
- Changing spacing
- Modifying alignment
- Adjusting colours
- Selecting widgets
- Triggering runtime events
- Recording repeated user flows

The captured interaction is converted into a structured intent.

Example:

```json
{
  "action": "resize",
  "target": "PrimaryActionButton",
  "from": { "width": 120, "height": 40 },
  "to": { "width": 168, "height": 48 }
}
```

### 5. Source Mapping Layer

The source mapping layer connects runtime elements to source code. It determines where a visual or behavioural change should be applied.

Depending on the framework, this may map to:

- C++ layout code
- QML files
- Slint files
- XAML files
- HTML/CSS
- JSON configuration
- Scene files
- Engine metadata
- Generated UI descriptors
- Declarative layout definitions

The source mapping layer is the key to reliable bidirectional editing.

### 6. AI Patch Layer

The AI patch layer converts structured intent into source changes. It does not blindly rewrite files. It analyses the project context, identifies the safest edit location, proposes a patch, validates the result, and explains the change.

The patch layer supports:

- Code generation
- Refactoring
- Error repair
- UI change application
- Build-failure correction
- Runtime-behaviour adjustments
- Multi-file edits
- Migration assistance
- Architecture-aware suggestions

### 7. Validation Layer

The validation layer checks that changes are correct before they are accepted.

Validation may include:

- Rebuild success
- Static analysis
- Unit tests
- UI snapshot comparison
- Runtime smoke tests
- Compiler diagnostics
- Linter results
- User confirmation
- Regression checks

---

## Core Abilities

ATLAS is designed to provide the following abilities.

### AI Coding

The IDE includes an AI coding agent capable of understanding the project structure, reading source files, modifying code, generating implementations, explaining errors, and coordinating changes across multiple files.

The AI agent is designed to work with professional development workflows rather than isolated snippets.

### C++-First Native Development

The project prioritizes C++ because C++ remains central to systems software, desktop applications, game engines, embedded software, performance-critical tools, robotics, simulation, graphics, and infrastructure.

C++ support is designed to include:

- CMake projects
- Header/source navigation
- Compile database support
- clangd-compatible indexing
- Build diagnostics
- Native process execution
- Debugger integration
- Framework-specific UI adapters
- Incremental rebuild workflows

### Multi-Language Expansion

Although C++ is the primary target, the architecture is intended to support other languages through adapters.

Potential language targets include:

- C
- C++
- Rust
- Python
- JavaScript
- TypeScript
- C#
- Java
- Go
- Kotlin
- Swift
- Lua

The goal is not to treat every language identically, but to provide framework-specific intelligence where accurate source mapping is possible.

### Live Compiled Preview

The IDE provides a live preview of the built application. The preview is not a static mockup. It is the actual compiled application, a framework-native preview, or a controlled runtime representation depending on the project type.

This enables developers to see how code changes affect the real application experience.

### Visual Editing

ATLAS supports visual modification of runtime elements where source mapping is available.

Visual editing may include:

- Dragging elements
- Resizing components
- Repositioning windows or panels
- Editing layout values
- Changing margins and padding
- Modifying alignment
- Adjusting typography
- Updating colours
- Editing component properties
- Reordering interface sections

### Interaction-to-Code Translation

The central capability of the project is translating user interaction with the running software into code changes.

This requires:

- Runtime element identification
- Geometry capture
- Property extraction
- Event interpretation
- Source mapping
- Patch generation
- Rebuild validation

The system is designed to be conservative. Where a change cannot be mapped safely, it should produce a reviewable patch instead of silently modifying code.

### Professional Patch Review

Every AI or visual edit should be visible, inspectable, and reversible.

The patch review flow includes:

- File-level diff
- Explanation of intent
- Build impact
- Risk indicators
- Test results
- Accept/reject controls
- Rollback support

---

## Design Philosophy

ATLAS follows several design principles.

### Source Code Remains the Authority

Visual editing is valuable only if it produces maintainable code. The source tree remains the canonical representation of the software.

### Framework Adapters Beat Guesswork

A universal visual editor for every possible application is not reliable. ATLAS is designed around adapters that understand specific languages, frameworks, and rendering systems.

Examples:

- Qt/QML adapter
- Slint adapter
- Dear ImGui adapter
- CMake adapter
- Web adapter
- Native desktop adapter
- Game engine adapter
- Custom renderer adapter

### Safe Automation Over Blind Automation

The AI agent should explain and validate meaningful changes. It should not rewrite important code without traceability.

### Runtime Behaviour Matters

Compilation success is not enough. The IDE should help developers understand what the application does while it is running.

### Professional Workflows First

The tool is designed for serious software engineering. It should support version control, reproducible builds, diagnostics, tests, review, and collaboration.

---

## Availability Model

ATLAS is source-available under the Business Source License 1.1.

The source code is available for inspection, learning, modification, redistribution, and non-production use under the terms of the license. Limited production use is allowed only under the Additional Use Grant in the `LICENSE` file. Commercial production use outside that grant requires a commercial license from the licensor.

On the Change Date specified in the `LICENSE` file, the licensed version converts to the Change License.

This model is designed to balance professional transparency, developer access, commercial sustainability, and long-term open-source availability.

---

## Intended Users

ATLAS is intended for:

- C++ developers
- Native application developers
- UI framework engineers
- Game and simulation tool builders
- Systems programmers
- Developer tooling teams
- Startup engineering teams
- Research laboratories
- Product engineers
- Software architecture teams
- AI coding workflow researchers

---

## Example Use Cases

### Native UI Development

A developer builds a native C++ application, previews the compiled window inside ATLAS, drags a panel to a new location, reviews the generated code patch, accepts the edit, and immediately sees the rebuilt result.

### Framework-Based UI Editing

A developer opens a QML or Slint project, selects a button in the live preview, changes its size visually, and ATLAS updates the correct declarative source file.

### Debugging Runtime Behaviour

A developer runs an application, captures a problematic interaction, and asks the AI agent to trace the event path, inspect related source files, and propose a fix.

### Heavy Application Development

A developer working on graphics, simulation, robotics, or engine tooling uses the live render viewer to inspect runtime output while keeping the build system, source code, logs, and AI assistant in one workspace.

### Codebase Migration

A team uses the AI agent to migrate project structure, update APIs, modernize C++ code, or move UI definitions into a more maintainable framework while validating each change through builds and tests.

---

## Technical Direction

The project is designed around these technical foundations:

- Native desktop shell
- Split-pane full-screen UI
- C++ project support
- CMake and compile database integration
- Language server integration
- AI agent execution loop
- Runtime process management
- Live preview embedding
- UI interaction overlays
- Framework-specific source mappers
- Patch generation and review
- Build and test validation
- Extension system for adapters

---

## Development Roadmap

### Phase 1: Core IDE Shell

- Full-screen split interface
- File explorer
- Code editor
- Terminal panel
- Build output panel
- Project loader
- Basic CMake support

### Phase 2: C++ Build and Run Loop

- Compile database loading
- CMake configuration
- Incremental builds
- Executable discovery
- Runtime process launch
- Log capture
- Error diagnostics

### Phase 3: Live Render Viewer

- Application preview embedding
- Input forwarding
- Frame capture
- Runtime restart controls
- Preview session state

### Phase 4: AI Coding Agent

- Project-aware code edits
- Multi-file patch generation
- Build-error repair
- Code explanation
- Refactor support
- Diff review

### Phase 5: Visual Interaction Capture

- Selection overlays
- Drag and resize capture
- Property editing
- Interaction event recording
- Structured intent model

### Phase 6: Source Mapping

- Declarative UI mapping
- C++ symbol mapping
- Framework adapters
- Patch target resolution
- Confidence scoring

### Phase 7: Validation and Professionalization

- Test runner integration
- Snapshot checks
- Regression detection
- Plugin system
- Documentation
- Packaging
- Commercial licensing workflow

---

## Repository Status

This repository defines and develops ATLAS as a professional source-available project. The design emphasizes correctness, transparency, extensibility, and developer control.

The project is not positioned as a simple code generator. It is a development environment for closing the loop between source code, compiled software, visual output, and human intent.

---

## License

ATLAS is licensed under the **Business Source License 1.1**. See the `LICENSE` file for the full terms, including the Additional Use Grant, Change Date, and Change License.

SPDX-License-Identifier: BUSL-1.1
