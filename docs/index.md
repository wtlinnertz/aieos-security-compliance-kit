# Security & Compliance Kit — Documentation Index

This kit governs threat modeling, security assessment, compliance evidence collection, and dependency auditing. It is Layer 10 of the AIEOS system, operating as a cross-cutting kit.

---

## Start Here

| Document | Purpose |
|----------|---------|
| `playbook.md` | Process definition with trigger points — read this first |
| `how-to-use-with-ai.md` | AI session setup and tool guidance |
| `how-to-adapt.md` | Organizational adoption guidance |
| `governance-model.md` | AIEOS structural rules (reference) |
| `entry-from-eek.md` | Boundary briefing when arriving from Engineering Execution Kit |

---

## Artifact Governing Files

### Trigger A — Threat Model (TM)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/tm-spec.md` | Content rules and 4 hard gates |
| Template | `artifacts/tm-template.md` | TM structure |
| Prompt | `prompts/tm-prompt.md` | Generation instructions |
| Validator | `validators/tm-validator.md` | Pass/fail evaluation |

### Trigger B — Security Assessment Record (SAR)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/sar-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/sar-template.md` | SAR structure |
| Prompt | `prompts/sar-prompt.md` | Generation instructions |
| Validator | `validators/sar-validator.md` | Pass/fail evaluation |

### Trigger B — Dependency Audit Record (DAR)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/dar-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/dar-template.md` | DAR structure |
| Prompt | `prompts/dar-prompt.md` | Generation instructions |
| Validator | `validators/dar-validator.md` | Pass/fail evaluation |

### Trigger C — Compliance Evidence Record (CER)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/cer-spec.md` | Content rules and 4 hard gates |
| Template | `artifacts/cer-template.md` | CER structure |
| Prompt | `prompts/cer-prompt.md` | Generation instructions |
| Validator | `validators/cer-validator.md` | Pass/fail evaluation |

---

## Principles

| File | Purpose |
|------|---------|
| `principles/security-principles.md` | Threat modeling standards, secure development practices, vulnerability management |
| `principles/compliance-principles.md` | Regulatory compliance philosophy, evidence standards, audit readiness |

---

## Guides

| Document | Purpose |
|----------|---------|
| `entry-from-eek.md` | Boundary briefing when arriving from the Engineering Execution Kit |

---

## Tests

`tests/kit-test-plan.md` — S-01 to S-06 structural checks + F-01 to F-03 flow scenarios
