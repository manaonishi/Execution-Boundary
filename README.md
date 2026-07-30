# Execution Boundary
### An OS-Level Architecture for AI Governance

> **OS-level AI governance that enforces boundaries, not opinions.**

Execution Boundary is an OS-level enforcement component of the broader AI Governance OS architecture.

This work was previously published under the name *Hamecohming Framework*.

Rather than relying solely on probabilistic model alignment, it explores deterministic execution control at the operating system boundary using provenance, consent, and policy verification.

---

## The Problem

Model-level alignment is probabilistic. It can be overridden by prompting, fine-tuning, or context manipulation.

The proposed architecture moves the enforcement point below the model:

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

This repository focuses primarily on the OS-layer implementation concept of the broader governance architecture.

The broader framework, including provenance transparency, consent-aware governance, non-learning controls, democratic interoperability, accountability structures, creator protection, redistribution mechanisms, and safety-oriented execution boundaries, is being developed as part of the overall Hamecohming Framework.

This repository is intended as the public technical reference for the Execution Boundary concept.

## Status

Active research. Independent. Not affiliated with any institution.  
Feedback and issues welcome.
