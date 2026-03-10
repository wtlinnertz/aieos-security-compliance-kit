# aieos-security-compliance-kit

**Layer 10 of the AIEOS system — Security & Compliance (Cross-Cutting)**

This kit governs how security threats are identified, security implementations are verified, compliance evidence is collected, and dependency risks are assessed. It is a cross-cutting kit that operates across multiple layers of the AIEOS system rather than sitting in a single sequential position in the pipeline.

---

## What This Kit Does

Security and compliance concerns arise at multiple points in the delivery lifecycle — not just before release. This kit provides structured governance for four distinct security and compliance activities:

- **Threat modeling** — What can go wrong? Who would attack this system? What are the attack surfaces? (triggered after system architecture is defined)
- **Security assessment** — Is the implementation actually secure? Have guardrails been followed? (triggered after code is complete)
- **Compliance evidence** — Can we prove we meet regulatory requirements? Is the evidence auditable? (triggered by compliance mandate, any time)
- **Dependency auditing** — Are our dependencies safe? Are licenses compatible? Is supply chain risk managed? (triggered before release)

---

## Artifact Types

This kit produces four governed artifact types:

| Artifact | ID Prefix | Purpose |
|----------|-----------|---------|
| Threat Model (TM) | TM-{PROJECT}-{NNN} | Per-initiative threat analysis: attack surfaces, threat actors, mitigations, residual risks |
| Security Assessment Record (SAR) | SAR-{PROJECT}-{NNN} | Pre-release security review verifying guardrails and TM mitigations |
| Compliance Evidence Record (CER) | CER-{PROJECT}-{NNN} | Auditable compliance evidence chain for regulatory mandates |
| Dependency Audit Record (DAR) | DAR-{PROJECT}-{NNN} | Dependency vulnerability, license, and supply chain risk audit |

Each governed artifact type has exactly four governing files: spec, template, prompt, validator.

---

## Cross-Cutting Nature

Unlike most AIEOS kits, this kit does not have a single upstream/downstream position. Artifacts are triggered at different points:

| Artifact | Trigger Point | Pipeline Position |
|----------|---------------|-------------------|
| TM | SAD frozen (during EEK phase) | After Layer 4 architecture |
| SAR | Code complete | Between EEK and REK |
| CER | Compliance mandate identified | Any time |
| DAR | Pre-release | Between EEK and REK |

The kit can be adopted independently without requiring other AIEOS kits.

---

## Quick Start

1. Read `docs/playbook.md` — the complete process definition with trigger points
2. Read `docs/how-to-use-with-ai.md` — session setup and AI tool guidance
3. See `docs/entry-from-eek.md` — boundary briefing for the primary entry point

---

## Repository Structure

```
docs/
  principles/          # Organizational security and compliance policy (input material)
  specs/               # Content rules and hard gates per artifact type
  artifacts/           # Templates
  prompts/             # AI generation prompts
  validators/          # Quality gate definitions
  playbook.md          # Process definition with trigger points
  index.md             # Documentation entry point
  how-to-adapt.md      # Organizational adoption guidance
  how-to-use-with-ai.md # AI tool usage guide
  governance-model.md  # AIEOS structural rules (reference)
  entry-from-eek.md    # Boundary briefing from Engineering Execution Kit
examples/              # Worked examples
tests/
  kit-test-plan.md     # Structural integrity checks and flow scenarios
CLAUDE.md              # AI operating instructions
```

---

## AIEOS Layer

| Layer | Kit | Status |
|-------|-----|--------|
| 2. Product Intelligence | `aieos-product-intelligence-kit` | Built |
| 4. Engineering Execution | `aieos-engineering-execution-kit` | Built |
| 5. Release & Exposure | `aieos-release-exposure-kit` | Built |
| 6. Reliability & Resilience | `aieos-reliability-resilience-kit` | Built |
| 7. Insight & Evolution | `aieos-insight-evolution-kit` | Built |
| 8. Operational Diagnostics | `aieos-operational-diagnostics-kit` | Built |
| **10. Security & Compliance** | **`aieos-security-compliance-kit`** | **Built** |

See `aieos-governance-foundation/docs/layer-model.md` for the full layer model.
