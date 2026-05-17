# ATLAS AI Agents

ATLAS uses AI agents as controlled engineering assistants. Agents propose patches; they do not silently mutate the working tree.

---

## Agent Flow

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
Validation
  |
  v
Review
  |
  v
Apply or Reject
```

---

## Agent Rules

1. Agents propose patches.
2. Agents do not silently apply low-confidence edits.
3. Agents do not run shell commands without permission.
4. Agents do not access files outside the workspace by default.
5. Agents do not send private source code to external providers unless allowed by policy.
6. Agents do not disable validation to make changes pass.
7. Agents do not modify licensing files without explicit task intent.
8. Agents do not run as root.
9. Agents do not install dependencies without approval.
10. Agents must produce reversible changes.

---

## Specialist Agents

### Code Agent

Implements source code changes, fixes compiler errors, generates scaffolding, updates APIs, and edits C++ code.

### UI Agent

Translates visual interaction into layout patches, edits Slint or QML files, updates grid placement, improves accessibility metadata, and preserves layout intent.

### Build Agent

Configures CMake, fixes include paths, diagnoses compiler errors, updates build presets, and repairs build scripts.

### Debug Agent

Reads preview logs, interprets crashes, correlates runtime events with source files, and proposes runtime fixes.

### Refactor Agent

Extracts components, modernizes C++, reorganizes modules, converts imperative UI toward declarative structures, and improves architecture.

### Test Agent

Generates unit tests, mapper tests, grid tests, preview smoke tests, UI snapshot tests, and regression tests.

### Security Agent

Reviews sandbox policies, dependency changes, command execution, filesystem access, and generated code risks.

### Documentation Agent

Maintains README, architecture docs, adapter guides, API docs, examples, and roadmap files.

---

## Context Packet

Agents receive structured context rather than only raw chat text.

```json
{
  "workspace": { "root": "/project", "trust": "normal" },
  "project": { "languages": ["cpp", "slint"], "buildSystem": "cmake+ninja" },
  "selection": { "file": "src/ui/main_window.slint", "symbol": "PrimaryActionButton" },
  "preview": {
    "runtimeId": "button.primary",
    "bounds": { "x": 320, "y": 184, "width": 168, "height": 48 }
  },
  "grid": { "unit": 8, "snapped": true },
  "task": {
    "type": "visual.resize",
    "intent": "increase primary button width while preserving grid alignment"
  }
}
```

---

## Tool Permissions

| Tool | Default |
| --- | --- |
| Read workspace file | Allowed |
| Write patch stage | Allowed |
| Write working tree | Requires approval |
| Run build | Allowed after project trust |
| Run tests | Allowed after project trust |
| Run arbitrary shell command | Requires approval |
| Install dependency | Requires approval |
| Network access | Disabled unless configured |
| Read outside workspace | Denied |
| Modify license | Requires explicit task intent |
