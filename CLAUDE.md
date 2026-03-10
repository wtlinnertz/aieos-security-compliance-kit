# CLAUDE.md — Security & Compliance Kit

## What This Repository Is

This is the **Security & Compliance Kit** — an AIEOS kit that governs threat modeling, security assessment, compliance evidence collection, and dependency auditing. It is Layer 10 of the AIEOS system, operating as a cross-cutting kit that interacts with multiple layers rather than occupying a single sequential position in the pipeline.

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
tests/                 # Structural integrity checks
```

## Artifact Types

This kit produces four governed artifact types:

1. **Threat Model (TM)** — Per-initiative threat analysis identifying attack surfaces, threat actors, mitigations, and residual risks. Generated after SAD is frozen.
2. **Security Assessment Record (SAR)** — Pre-release security review verifying ACF section 3 guardrails and TM mitigations are implemented. Generated after code is complete.
3. **Compliance Evidence Record (CER)** — Auditable compliance evidence chain mapping regulatory requirements to implementation evidence. Triggered by compliance mandate at any time.
4. **Dependency Audit Record (DAR)** — Audit of dependencies for vulnerabilities, license compliance, and supply chain risk. Recommended before release.

Each artifact type has exactly four governing files: spec, template, prompt, validator.

## Key Rules

- **Specs are the source of truth** — prompts and validators reference specs, never inline rules
- **Validators judge, they do not help** — no suggestions, no redesign
- **Freeze before promote** — upstream artifacts must be frozen before downstream generation
- **Separate generation and validation** — different AI sessions to prevent self-validation bias
- **No scope expansion** — downstream artifacts must not expand scope beyond upstream
- **No inferred information** — mark missing information explicitly, do not fill gaps
- **Cross-cutting triggers** — artifacts are triggered at different pipeline points, not in a single linear flow
- **Governance model sync** — `docs/governance-model.md` is a synchronized copy of `aieos-governance-foundation/governance-model.md` (canonical authority). Do not edit kit copy directly; update `aieos-governance-foundation` first, then sync all kit copies to match exactly. See governance-model.md section 15 for versioning and change protocol.
- **Engagement Record** — SCK maintains the security and compliance section of the project's ER. Add artifact IDs as they freeze. See `docs/playbook.md` and `aieos-governance-foundation/docs/engagement-record-spec.md`.

## Artifact Flow

This kit uses trigger-based activation rather than a single linear flow:

```
Trigger A: SAD frozen (from EEK)
  → Threat Model (TM) → validate → freeze

Trigger B: Code complete (from EEK)
  → Security Assessment Record (SAR) → validate → freeze
  → Dependency Audit Record (DAR) → validate → freeze
  (SAR and DAR may run in parallel)

Trigger C: Compliance mandate identified (any time)
  → Compliance Evidence Record (CER) → validate → freeze
  (CER runs independently of other artifacts)
```

## Boundary Contracts

- **Inputs from EEK:** SAD (system architecture for TM), ACF section 3 (security guardrails for SAR), TDD (technical design for review), implementation code (for SAR)
- **Inputs from PIK:** Compliance requirements identified in PFD/DPRD (for CER)
- **Outputs to QAK:** TM and SAR can feed QGR security assessment
- **Outputs to REK:** SAR provides security clearance evidence for release
- **Outputs to RRK:** Security monitoring requirements from TM
- **Inputs from ODK:** Security-related PMR findings may trigger TM revision

## File Naming

| Type | Pattern |
|------|---------|
| Spec | `{type}-spec.md` |
| Template | `{type}-template.md` |
| Prompt | `{type}-prompt.md` |
| Validator | `{type}-validator.md` |

## Commit Message Style

Follow conventional commits: `docs: <description>`

## When Working on This Kit

- Read the playbook (`docs/playbook.md`) for the full process definition
- Read the governance model (`docs/governance-model.md`) for structural rules
- Check `docs/how-to-use-with-ai.md` for session setup instructions

## Building or Auditing AIEOS Kits

- `aieos-governance-foundation/docs/kit-structure-standard.md` — compliance checklist for building and auditing kits
- `aieos-governance-foundation/docs/philosophy.md` — design rationale for governance model decisions
- `aieos-governance-foundation/docs/layer-model.md` — layer model and kit registry
