# Framework for AI Governance: Hamecohming Framework

> **OS-level AI governance that enforces boundaries — not opinions.**

AI safety should not depend on whether the model makes good decisions.  
It should depend on whether execution is permitted at the system level in the first place.

This framework enforces provenance, consent, and domain constraints at the **kernel execution boundary** — deterministically, without semantic interpretation.

---

## The Problem

Model-level alignment is probabilistic. It can be overridden by prompting, fine-tuning, or context manipulation.

The Hamecohming Framework moves the enforcement point **below** the model:

```text
User/Agent Request
      ↓
  Tag Check  ←  Provenance / Domain / Consent
      ↓
 OS Boundary  ←  No semantic interpretation. Deterministic enforcement.
      ↓
Permit Execution  ──or──  Structural Silence (deny by structure, not by judgment)
```

<img src="https://raw.githubusercontent.com/overthinker-jp/umecohming-hamecohming/main/UH_OS.png" width="100%" />

---

## Core Logic

```c
if (!os.verify_domain_tag(process, action)) {
    deny_execution();  // No interpretation. No loopholes.
}
```

No model in the decision path. No prompt-injection attack surface. No "maybe".

---

## Components

| Component | Role |
|---|---|
| **Umecohming** | Attribute embedding & provenance tagging |
| **Hamecohming** | Institutional boundary definition |
| **Structural Silence** | Execution halt on unverified actions |
| **Transparency Incentives** | Redistribution logic tied to verified usage |

---

## Implementation

### Conceptual prototype (Python)

Demonstrates the boundary logic in a readable form.  
Not OS-level; intended as a specification reference.

```bash
python os/prototype/boundary.py
```

- Tag matching with tamper detection (SHA-256 rule hash)
- Structural Silence that actually halts execution (`PermissionError`)
- Persistent audit log at `logs/boundary_audit.jsonl`

### Kernel-level enforcement (eBPF/LSM)

The production implementation enforces the same boundary conditions at the kernel syscall layer — before user-space code can execute.

```bash
# Load the LSM hook (requires root)
Kernel-space implementation planned (work in progress)
```

See `/ebpf/` for the kernel-space implementation.

> **Note:** The Python prototype is a conceptual demonstration.  
> Actual OS-level enforcement requires the eBPF implementation.

---

## Threat Model / Limitations

Conceptual prototype — not a secure enforcement layer.

- User-space execution → integrity can be bypassed in a compromised runtime
- `PermissionError` → hard stop by design (no recovery path)
- Audit logs → writable and not tamper-resistant

Not flaws. Scope.

Real enforcement begins where user-space ends — at the kernel boundary (eBPF/LSM).

---

## Background

This framework emerged from a practical question:

*Why does AI safety rely on the model saying no — when the OS can simply deny execution?*

Informed by pharmacovigilance system design (adverse event detection without retraining) and applied to AI governance as an institutional architecture problem.

**90/9/1 model:**

- ~90% of AI interactions → probabilistic safety is sufficient
- ~9% → context-aware gray-zone handling
- <1% → OS-level hard stop, no exceptions

---

## Publications & Public Record

This repository focuses primarily on the OS-layer implementation concept of the Hamecohming Framework.
The broader framework — including provenance transparency, consent-aware governance, non-learning controls, democratic interoperability, accountability structures, creator protection, redistribution mechanisms, and safety-oriented execution boundaries — is documented in the public materials below.

The Hamecohming Framework is designed to explore how AI systems can remain open and interoperable while preserving verifiable provenance, human oversight, institutional accountability, and reversible safety controls at the infrastructure layer.


| Type | Link |
|---|---|
| Notion | [Open Notion](https://wise-antimatter-790.notion.site/Hamecohming-Framework-35867dd80dba8093a121f24a40b4f9a4) |

---

## Status

Active research. Independent. Not affiliated with any institution.  
Feedback and issues welcome.
