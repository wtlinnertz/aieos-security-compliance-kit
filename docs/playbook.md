# Security & Compliance Kit — Playbook

This playbook defines the process for the Security & Compliance Kit (Layer 10). Unlike sequential kits, this kit uses trigger-based activation — each artifact type is triggered at a different point in the delivery pipeline.

---

## Trigger-Based Artifact Flow

This kit does not follow a single linear flow. Artifacts are triggered independently:

```
Trigger A: SAD frozen (from EEK, Layer 4)
         │
         ▼
  Threat Model (TM) → generate → validate → freeze
         │
         ▼ (TM feeds SAR)

Trigger B: Code complete (from EEK, Layer 4)
         │
         ├──▶ Security Assessment Record (SAR) → generate → validate → freeze
         │    (requires frozen TM)
         │
         └──▶ Dependency Audit Record (DAR) → generate → validate → freeze
              (independent — can run in parallel with SAR)

Trigger C: Compliance mandate identified (any time)
         │
         ▼
  Compliance Evidence Record (CER) → generate → validate → freeze
  (independent — runs on its own schedule)
```

### Dependency Rules

- **TM** has no dependency on other SCK artifacts. It depends on a frozen SAD from EEK.
- **SAR** depends on a frozen TM (to verify mitigations are implemented). It also requires ACF section 3 guardrails from EEK.
- **DAR** has no dependency on other SCK artifacts. It can run at any time; recommended before release.
- **CER** has no dependency on other SCK artifacts. It is triggered by compliance mandates and runs on its own schedule.

---

## Upstream Interface

**Primary upstream:** Engineering Execution Kit (EEK, Layer 4)

### For Threat Model (TM)

**Input:** Frozen System Architecture Document (SAD)
**Dependency:** SAD component inventory, integration points, data flows, external interfaces

The SAD must be frozen before TM generation begins. If the SAD is absent or incomplete, do not generate the TM. Request a completed SAD from the EEK team.

### For Security Assessment Record (SAR)

**Input:** Frozen TM + ACF section 3 (security guardrails) + implementation code + TDD
**Dependency:** TM mitigations list, ACF section 3 guardrails, implementation evidence

The SAR requires both a frozen TM and completed implementation. If either is missing, do not generate the SAR.

### For Compliance Evidence Record (CER)

**Input:** Compliance requirements from PFD/DPRD (PIK, Layer 2) or external mandate
**Dependency:** Identified regulatory requirements with specific clauses

CER can be triggered at any time. It does not require EEK artifacts, though implementation evidence may be needed for certain compliance requirements.

### For Dependency Audit Record (DAR)

**Input:** Project dependency manifest (package.json, requirements.txt, go.mod, etc.)
**Dependency:** Complete dependency tree including transitive dependencies

DAR can be generated at any point; recommended before release.

---

## Trigger A — Threat Model

**Artifact:** Threat Model (TM)
**Type:** Generated
**Spec:** `docs/specs/tm-spec.md` (4 hard gates)
**Template:** `docs/artifacts/tm-template.md`
**Prompt:** `docs/prompts/tm-prompt.md`
**Validator:** `docs/validators/tm-validator.md`

### Purpose

The TM identifies attack surfaces, threat actors, mitigations, and residual risks for the system described in the SAD. It is the foundational security artifact from which the SAR validates implementation.

### Inputs

- Frozen SAD (from EEK)
- ACF section 3 (security guardrails, from EEK) — if available
- `docs/principles/security-principles.md` (organizational security policy)

### Process

1. Confirm the SAD is frozen. Do not proceed with a draft or unfrozen SAD.
2. In a new AI session, provide: frozen SAD + ACF section 3 (if available) + `docs/specs/tm-spec.md` + `docs/artifacts/tm-template.md` + `docs/principles/security-principles.md`. Use the TM prompt (`docs/prompts/tm-prompt.md`).
3. Review the generated TM. Confirm that all SAD components and integration points are assessed, threat actors are realistic for the system's context, and mitigations are actionable.
4. Validate the TM in a separate session using `docs/validators/tm-validator.md` and the spec.
5. On PASS: human review and freeze.
6. On FAIL: address blocking issues and re-generate or correct; re-validate.

### Freeze Points

- TM must be frozen before SAR generation begins (SAR verifies TM mitigations are implemented)
- Frozen TM may feed RRK security monitoring requirements

---

## Trigger B — Security Assessment Record and Dependency Audit Record

### Security Assessment Record (SAR)

**Artifact:** Security Assessment Record (SAR)
**Type:** Generated
**Spec:** `docs/specs/sar-spec.md` (5 hard gates)
**Template:** `docs/artifacts/sar-template.md`
**Prompt:** `docs/prompts/sar-prompt.md`
**Validator:** `docs/validators/sar-validator.md`

### Purpose

The SAR is the pre-release security review artifact. It verifies that ACF section 3 guardrails have been followed and that TM mitigations have been implemented. It provides security clearance evidence for the release process.

### Inputs

- Frozen TM
- ACF section 3 (security guardrails from EEK)
- Implementation code and/or TDD (technical design documents)
- `docs/principles/security-principles.md`

### Process

1. Confirm the TM is frozen and implementation is code-complete.
2. In a new AI session, provide: frozen TM + ACF section 3 + implementation evidence + `docs/specs/sar-spec.md` + `docs/artifacts/sar-template.md` + `docs/principles/security-principles.md`. Use the SAR prompt (`docs/prompts/sar-prompt.md`).
3. Review the generated SAR. Confirm that every TM mitigation has a verification entry and every ACF section 3 guardrail has been addressed.
4. Validate in a separate session using `docs/validators/sar-validator.md` and the spec.
5. On PASS: security owner reviews and freezes.
6. On FAIL: address blocking issues; re-generate or correct; re-validate.

### Freeze Points

- SAR must be frozen before it can be cited as release evidence in REK
- Frozen SAR provides security clearance for the Release Entry Record

---

### Dependency Audit Record (DAR)

**Artifact:** Dependency Audit Record (DAR)
**Type:** Generated
**Spec:** `docs/specs/dar-spec.md` (5 hard gates)
**Template:** `docs/artifacts/dar-template.md`
**Prompt:** `docs/prompts/dar-prompt.md`
**Validator:** `docs/validators/dar-validator.md`

### Purpose

The DAR audits project dependencies for known vulnerabilities, license compliance issues, and supply chain risks. It can run in parallel with the SAR.

### Inputs

- Project dependency manifest (complete dependency tree)
- Vulnerability database scan results
- License analysis output
- `docs/principles/compliance-principles.md`

### Process

1. Obtain the complete dependency manifest including transitive dependencies.
2. In a new AI session, provide: dependency manifest + vulnerability scan results + license analysis + `docs/specs/dar-spec.md` + `docs/artifacts/dar-template.md` + `docs/principles/compliance-principles.md`. Use the DAR prompt (`docs/prompts/dar-prompt.md`).
3. Review the generated DAR. Confirm that all dependencies are listed, CVEs are correctly mapped, and license classifications are accurate.
4. Validate in a separate session using `docs/validators/dar-validator.md` and the spec.
5. On PASS: security owner reviews and freezes.
6. On FAIL: address blocking issues; re-generate or correct; re-validate.

### Freeze Points

- DAR should be frozen before release; it provides dependency risk evidence for REK

---

## Trigger C — Compliance Evidence Record

**Artifact:** Compliance Evidence Record (CER)
**Type:** Generated
**Spec:** `docs/specs/cer-spec.md` (4 hard gates)
**Template:** `docs/artifacts/cer-template.md`
**Prompt:** `docs/prompts/cer-prompt.md`
**Validator:** `docs/validators/cer-validator.md`

### Purpose

The CER maps regulatory requirements to implementation evidence, creating an auditable trail. It is triggered by compliance mandates and runs on its own schedule, independent of the delivery pipeline.

### Inputs

- Regulatory requirements (specific mandate with clause references)
- Implementation evidence (code, configuration, process documentation, audit logs)
- Existing SCK artifacts (frozen TM, SAR, DAR — if available, these provide supporting evidence)
- `docs/principles/compliance-principles.md`

### Process

1. Identify the regulatory mandate and its specific requirements.
2. In a new AI session, provide: regulatory requirements + implementation evidence + any existing frozen SCK artifacts + `docs/specs/cer-spec.md` + `docs/artifacts/cer-template.md` + `docs/principles/compliance-principles.md`. Use the CER prompt (`docs/prompts/cer-prompt.md`).
3. Review the generated CER. Confirm that every regulatory requirement has concrete evidence (not assertions), gaps have remediation plans, and the evidence chain is retrievable.
4. Validate in a separate session using `docs/validators/cer-validator.md` and the spec.
5. On PASS: compliance owner reviews and freezes.
6. On FAIL: address blocking issues; re-generate or correct; re-validate.

### Freeze Points

- CER is frozen when the compliance owner approves the evidence chain
- Frozen CER is the auditable record for the identified regulatory mandate

---

## Downstream Handoffs

### To Release & Exposure Kit (REK, Layer 5)

**Handoff artifact:** Frozen SAR + frozen DAR
**Purpose:** Security clearance evidence for the release process
**What REK receives:** SAR provides evidence that security guardrails were followed and TM mitigations are implemented. DAR provides evidence that dependency risks are assessed and managed.

### To Quality Assurance Kit (QAK)

**Handoff artifact:** Frozen TM + frozen SAR
**Purpose:** Security assessment input for Quality Gate Review (QGR)
**What QAK receives:** TM provides the threat landscape; SAR provides security verification evidence.

### To Reliability & Resilience Kit (RRK, Layer 6)

**Handoff artifact:** Frozen TM
**Purpose:** Security monitoring requirements
**What RRK receives:** TM residual risks and monitoring recommendations feed into SRP security monitoring requirements.

---

## Freeze Points Summary

| Artifact | When Frozen | What It Gates |
|----------|------------|---------------|
| TM | After SAD freeze + validation | SAR generation; feeds RRK monitoring requirements |
| SAR | After code complete + validation | Release clearance evidence for REK |
| DAR | After dependency audit + validation | Release risk evidence for REK |
| CER | After compliance review + validation | Regulatory audit trail |

---

## Session Discipline

Follow these rules in every AI session:

| Rule | Why |
|------|-----|
| One artifact per session | Prevents context contamination across artifacts |
| Separate generation and validation sessions | Prevents self-validation bias |
| Include full frozen upstream artifacts | Do not summarize; the AI needs complete context |
| Include spec and template for generation sessions | The AI generates against the spec, not from memory |
| Include spec and validator for validation sessions | The validator judges against the spec, not against the generated content |

Do not ask the AI that generated an artifact to also validate it in the same session.

---

## Re-Entry Protocol

When a frozen artifact must be corrected:

1. Identify the artifact to be changed (TM, SAR, CER, or DAR).
2. Identify all downstream artifacts that reference it or depend on it.
3. Assess the impact:
   - TM change: Does it affect SAR mitigation verification? Does it change RRK monitoring requirements?
   - SAR change: Does it affect REK release clearance?
   - DAR change: Does it affect REK release risk assessment?
   - CER change: Does it affect audit trail completeness?
4. Issue a new version of the artifact.
5. Re-validate the new version.
6. Re-validate all affected downstream artifacts that reference the changed artifact.
7. Document the change in the new version's document control section with a reference to the original and the reason for the correction.

---

## TM Revision Protocol

When the system architecture changes (SAD revision) or a security-related incident occurs:

1. **Do not edit the frozen TM.** The frozen TM remains immutable.
2. Identify the trigger: SAD revision, security incident, new threat intelligence, ODK PMR finding.
3. Generate a new TM version using the updated SAD and the TM prompt.
4. Validate and freeze the new TM version.
5. Assess whether the SAR needs to be re-generated against the new TM.
6. If the SAR was already frozen and referenced the old TM, the SAR remains valid for its release. Future releases must use the new TM version.

---

## Amendment Procedure

A frozen artifact may be corrected in place without re-validation when **all** of the following criteria are met:

1. The correction does not affect any field evaluated by a hard gate.
2. The correction does not change scope, decisions, owners, or technical specifications.
3. The correction does not affect any field referenced by a downstream artifact.

**Procedure:** Make the correction and add an Amendment Log entry to the artifact's Document Control section: date, what changed, materiality criterion cited, and who authorized the change. No re-validation is required.

**If there is any ambiguity** about whether a change is non-material, treat it as material and issue a new artifact version. The amendment path must not become a workaround for the version protocol.

---

## Principle File Revision

When a principle file in `docs/principles/` changes, use the change categories defined in `aieos-governance-foundation/docs/principle-file-standard.md`:

| Change Category | Version Bump | Re-Entry Impact |
|----------------|-------------|-----------------|
| **Minor** (clarification only) | `v_.x -> v_.x+1` | No re-entry required; already-frozen artifacts remain valid |
| **Significant** (new requirement or tightened constraint) | `v1.x -> v1.x+1` | Review artifacts generated after the change against updated principles; already-frozen artifacts are grandfathered |
| **Breaking** (removal or loosening) | `vN.x -> vN+1.0` | Requires security owner authorization and documented business justification; re-entry may be warranted |

Every change to a principle file must bump the version field, even minor clarifications.

---

## Maintaining the Engagement Record

The Engagement Record (ER) is a project-level artifact that lives in the consuming project at `docs/engagement/er-{initiative}.md`. It spans all AIEOS layers and is maintained by each kit's operators as work passes through. The ER spec and format are defined in `aieos-governance-foundation/docs/engagement-record-spec.md`.

**SCK maintains the security and compliance section of the ER.**

### What to Update

| Trigger | ER update |
|---------|-----------|
| TM frozen | Add TM ID to artifact table |
| SAR frozen | Add SAR ID to artifact table |
| DAR frozen | Add DAR ID to artifact table |
| CER frozen | Add CER ID to artifact table |
| TM revised (new version) | Add new TM version to artifact table; note revision date |
