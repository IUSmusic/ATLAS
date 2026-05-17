# ATLAS Security

ATLAS runs compiled user applications and AI-generated code paths. Security is a primary design boundary.

---

## Security Principle

The IDE host must not trust the preview process.

```text
Trusted:    ATLAS host, patch store, project index
Untrusted:  compiled preview process, generated binaries, external project scripts
Controlled: AI agents, build tools, dependency installers
```

---

## Process Separation

ATLAS runs the compiled application in a separate supervised process. The preview process may crash, hang, allocate excessive memory, or execute unsafe code. ATLAS must remain stable.

---

## Isolation Tiers

| Tier | Runtime | Use |
| --- | --- | --- |
| 0 | Native supervised process | Trusted local project |
| 1 | OS sandbox | Default normal preview |
| 2 | Container | AI-generated or semi-trusted code |
| 3 | MicroVM | Untrusted projects |
| 4 | Full VM | Rare high-risk execution |

---

## Filesystem Policy

Default preview access:

- Workspace read/write allowed
- Build directory allowed
- Temporary directory allowed
- Home directory denied
- SSH keys denied
- Browser profiles denied
- Cloud credentials denied
- System directories denied except required runtime/toolchain paths

---

## Network Policy

Default preview network access:

- Disabled unless project profile allows it
- Dependency installation requires approval
- Agent network access controlled separately
- External model access controlled by workspace policy

---

## Agent Security

Agents must not run as root, modify files outside the workspace, install dependencies without approval, hide generated changes, disable tests silently, change license terms without explicit task intent, or execute unknown binaries without sandboxing.

---

## Preview Permission Profile

```json
{
  "filesystem": {
    "workspace": "read-write",
    "home": "deny",
    "tmp": "read-write"
  },
  "network": "deny",
  "process": {
    "maxMemoryMB": 2048,
    "maxCpuPercent": 200
  },
  "sandbox": "os"
}
```
