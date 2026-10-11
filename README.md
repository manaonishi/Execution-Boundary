# Execution Boundary
### OS-Level Enforcement for AI Governance

> **Intelligence is not authority. AI decisions do not automatically constitute permission to execute.**

Execution Boundary is the OS-level enforcement component proposed within **Institution-to-Execution Architecture (I2EA)**, a broader architecture for AI governance.

Its purpose is to establish an enforceable boundary between decisions made by AI systems and actions performed in the real world.

Rather than placing all responsibility for safety and compliance inside AI models, Execution Boundary explores how externally defined policies, permissions, and restrictions can be enforced independently of model behavior.

This research was previously published under the name *Hamecohming Framework*. The current name reflects its role within the broader I2EA architecture.

---

## 1. Why External Execution Control Is Necessary

Model alignment is an important part of AI safety, but it cannot serve as the sole basis for governing AI execution.

Two distinct problems must be considered.

**First, even an aligned AI may not be authorized to perform a particular action.**

A model may behave as intended while operating under outdated permissions, changed regulations, revoked authority, or circumstances that differ from those at the time of its decision.

An internally valid AI decision does not necessarily constitute an institutionally authorized action.

**Second, not all AI systems can be assumed to be aligned.**

AI systems may differ in their training, safety mechanisms, deployment conditions, and operating objectives. Open-weight models, modified models, independently developed systems, and autonomous agents may not share the same alignment assumptions.

An execution-control architecture must therefore be able to operate without relying on universal model alignment.

The central principle is:

**AI may propose or decide an action, but permission to execute must remain independently enforceable.**

---

## 2. Why OS-Level Control?

Application-level security, access controls, and existing cybersecurity mechanisms remain essential.

However, AI agents may operate across applications, services, and tools, sometimes exercising permissions delegated by users or organizations.

Execution Boundary explores the operating system as an additional enforcement layer, separate from the AI model's reasoning process.

This separation offers several architectural advantages:

- Enforcement does not depend on the model voluntarily following instructions.
- Execution permissions can be checked independently of model-generated decisions.
- Selected operations can be blocked even when an application or agent attempts to proceed.
- Emergency restrictions can be applied to identified execution environments.

OS-level enforcement is not intended to replace existing security mechanisms.

It is intended to complement them by providing a final, independently enforceable boundary for operations within its technical scope.

The OS cannot inherently determine whether an action is ethically acceptable, legally authorized, or consistent with a user's intentions. Those determinations require appropriate policies, authority information, and trusted inputs from outside the enforcement mechanism.

**The role of the Execution Boundary is to enforce applicable restrictions, not to invent them.**

---

## 3. Design Principles

### Minimal OS-Level Enforcement

Operating-system performance and stability are primary design considerations.

Execution Boundary is not intended to move comprehensive AI governance analysis into the kernel.

Instead, the architecture follows a layered approach:

1. **Existing security mechanisms** handle ordinary access control, authentication, application security, and granular restrictions.
2. **External governance components** establish applicable policies, permissions, restrictions, and supporting evidence.
3. **Execution Boundary** performs a limited set of enforceable checks at supported execution points.
4. **Emergency controls** provide a means to restrict identified AI execution environments when required.

Complex policy interpretation, institutional coordination, and evidence evaluation should remain outside the kernel wherever practical.

The OS-level component should receive only the information necessary to enforce a specific restriction.

### Separation of Decision and Execution

An AI-generated decision and the authority to execute it are separate matters.

A decision may be technically feasible without being permitted under current policy, organizational authority, or user-defined restrictions.

Execution Boundary is intended to preserve this separation.

### Independent Enforcement

The enforcement mechanism must not depend on the AI model voluntarily complying with a refusal instruction.

Where a relevant operation is covered by the enforcement mechanism, execution can be denied independently of the model's reasoning or response.

### Structural Silence

*Structural Silence* describes the prevention of an unauthorized operation through the execution architecture itself.

It does not require the AI model to generate a refusal, justify a decision, or reach a particular semantic conclusion.

An operation is prevented because the required execution conditions have not been satisfied.

---

## 4. Position Within I2EA

Execution Boundary is one component of the broader **Institution-to-Execution Architecture (I2EA)**.

I2EA addresses the relationship between institutional governance, AI decisions, execution authority, evidence, and ongoing oversight.

The broader architecture considers:

- Human-readable and machine-readable policies
- Institutional authority and permissions
- Provenance and identity information
- Policy, evaluation, fact, and process tags
- Decision-state evidence
- Operational vigilance and feedback
- Independent execution enforcement

Execution Boundary is concerned specifically with the final transition from an AI-generated action request to an operation that can be performed.

It does not independently establish laws, interpret institutional requirements, or determine the legitimacy of a user's intentions.

Its purpose is to enforce restrictions supplied through an appropriately designed and trusted governance process.

For the broader architecture, see [I2EA](https://i2ea.org).

---

## 5. Potential Applications

### Institutional AI Governance

Organizations may need to restrict AI execution according to institutional policies, delegated authority, operational conditions, and applicable regulations.

Execution Boundary could provide an additional enforcement point for selected operations, independently of the AI model's internal behavior.

### Individual User Protection

Execution Boundary may also help **restrict unintended AI agent activities on behalf of individual users**.

The objective is to support straightforward protection without requiring users to continuously monitor agent behavior or understand complex security mechanisms.

Routine operations should remain convenient, while sensitive or consequential operations may require additional authorization or be restricted by default.

### Emergency Intervention

A future implementation could support emergency restrictions targeting identified AI models or their associated execution environments.

Such a mechanism would require a trusted association between model identity, running processes, execution contexts, and enforceable operating-system controls.

A model name alone is not sufficient for reliable OS-level enforcement.

Emergency intervention would apply only to operations and execution environments covered by the implemented enforcement mechanisms. It would not retroactively reverse completed actions or automatically stop activity on unrelated systems.

---

## 6. Technical Approach

### Conceptual Prototype: Python

The repository includes a Python prototype demonstrating selected boundary-control concepts.

```bash
python os/prototype/boundary.py
```

The prototype includes:

- Tag matching and SHA-256-based rule-hash checking
- Execution denial through `PermissionError`
- A persistent audit log at `logs/boundary_audit.jsonl`

This prototype illustrates the proposed logic. It is **not an OS-level security boundary**.

Hash comparison alone does not establish trusted provenance or tamper resistance, and the prototype's audit logs are not protected against modification by a sufficiently privileged actor.

### OS-Level Approach: eBPF/LSM

The proposed Linux implementation explores **eBPF and Linux Security Module (LSM) hooks** as mechanisms for enforcing selected execution restrictions.

The intended approach is to:

- Identify supported operations that require enforcement.
- Associate execution contexts with trusted policy and authority information.
- Apply minimal checks at appropriate kernel security hooks.
- Deny operations that fail the applicable enforcement conditions.
- Keep complex policy interpretation and governance processing outside the kernel.

The `/ebpf/` directory is designated for this implementation work.

**The existence of a kernel-level implementation directory does not establish production readiness or comprehensive enforcement coverage.**

LSM hooks do not provide universal visibility into every AI action or application-level transaction. Operations that cannot be distinguished at the relevant kernel boundary require additional integration or enforcement elsewhere.

The precise enforcement coverage, trust assumptions, performance characteristics, and supported operations remain subjects for further specification and validation.

---

## 7. Threat Model and Limitations

Execution Boundary is an architectural research project, not a complete security product.

### Current Prototype

The Python prototype does not provide a trusted enforcement boundary.

- User-space execution may be bypassed or modified in a compromised runtime.
- `PermissionError` demonstrates denial within the prototype, not kernel-enforced isolation.
- Audit logs are writable and are not tamper-resistant.
- Rule-hash checking does not independently establish the authenticity of policy inputs.

### Proposed OS-Level Enforcement

Kernel-level enforcement also has important limitations.

- Only operations covered by the selected security hooks can be controlled.
- A compromised kernel or sufficiently privileged attacker may undermine enforcement.
- Model identity and execution-context attribution require trusted mechanisms.
- Kernel-level restrictions do not automatically reveal the semantic meaning of an operation.
- Network services, remote agents, and external systems may require separate controls.
- Incorrect policies or stale authority information can lead to incorrect enforcement outcomes.
- Performance, compatibility, and failure behavior require testing before operational deployment.

The architecture does not claim to eliminate prompt injection, guarantee correct governance decisions, or provide universal control over AI systems.

Its purpose is narrower: to explore independently enforceable execution restrictions at selected technical boundaries.

---

## 8. Research Status

**Status: Active architectural research and conceptual prototyping.**

The current repository provides a public technical reference for the Execution Boundary concept.

The Python prototype demonstrates selected control mechanisms. OS-level enforcement, trusted model-to-execution attribution, emergency intervention, and integration with the broader I2EA governance architecture require further specification, implementation, and validation.

Future work includes defining enforcement requirements, supported operations, trust boundaries, policy interfaces, performance constraints, and testing methods.

This project is independently developed and is not affiliated with any institution.

Technical feedback, architectural criticism, and implementation discussions are welcome.

---

## 9. Background and Related Work

The research originated from a practical question:

*Why should preventing an AI action depend entirely on the AI deciding not to perform it?*

The approach is informed in part by the distinction between operational systems and independent monitoring found in pharmacovigilance and other safety-governance practices.

The broader I2EA research extends this distinction to institutional authority, decision-state evidence, operational vigilance, and execution control.

**Project website:** https://i2ea.org

**Author:** Mana Onishi

© 2026 Mana Onishi
