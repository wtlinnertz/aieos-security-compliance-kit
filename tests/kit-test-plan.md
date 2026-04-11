# Security & Compliance Kit — Test Plan

This document contains the structural integrity checks and flow scenario tests for the Security & Compliance Kit. These tests verify that the kit is complete, internally consistent, and capable of producing valid artifacts.

---

## Structural Integrity Checks

Structural checks verify that the kit's files are present, properly named, and internally consistent. These checks do not require AI — they are verifiable by inspection.

### S-01: Four-File Completeness

**Check:** Every governed artifact type has exactly four files: spec, template, prompt, validator.

| Artifact Type | Spec | Template | Prompt | Validator |
|---------------|------|----------|--------|-----------|
| TM | docs/specs/tm-spec.md | docs/artifacts/tm-template.md | docs/prompts/tm-prompt.md | docs/validators/tm-validator.md |
| SAR | docs/specs/sar-spec.md | docs/artifacts/sar-template.md | docs/prompts/sar-prompt.md | docs/validators/sar-validator.md |
| CER | docs/specs/cer-spec.md | docs/artifacts/cer-template.md | docs/prompts/cer-prompt.md | docs/validators/cer-validator.md |
| DAR | docs/specs/dar-spec.md | docs/artifacts/dar-template.md | docs/prompts/dar-prompt.md | docs/validators/dar-validator.md |

**Expected result:** All 16 files present.

---

### S-02: Hard Gate Count Alignment

**Check:** Each spec's declared hard gate count matches the validator's gate checks.

| Artifact Type | Spec Hard Gates | Validator Gates |
|---------------|----------------|----------------|
| TM | 4 | 4 |
| SAR | 5 | 5 |
| CER | 4 | 4 |
| DAR | 5 | 5 |

**Expected result:** Counts match for all four artifact types.

---

### S-03: Hard Gate Name Alignment

**Check:** Gate names in specs match gate names in validators (exact string match for JSON output field names).

| Artifact | Spec Gate Names | Validator Gate Names |
|----------|----------------|---------------------|
| TM | attack_surface_coverage, threat_actor_identification, mitigation_mapping, residual_risk_assessment | attack_surface_coverage, threat_actor_identification, mitigation_mapping, residual_risk_assessment |
| SAR | guardrail_verification, mitigation_verification, vulnerability_assessment, authentication_authorization_review, data_handling_review | guardrail_verification, mitigation_verification, vulnerability_assessment, authentication_authorization_review, data_handling_review |
| CER | mandate_coverage, evidence_completeness, gap_assessment, audit_trail | mandate_coverage, evidence_completeness, gap_assessment, audit_trail |
| DAR | dependency_inventory, vulnerability_assessment, license_compliance, risk_classification, remediation_plan | dependency_inventory, vulnerability_assessment, license_compliance, risk_classification, remediation_plan |

**Expected result:** All gate names match exactly.

---

### S-04: Prompt-to-Spec Reference Integrity

**Check:** Each generation prompt references the correct spec and template. No prompt inlines content rules.

| Prompt | References Spec | References Template | Inlines Rules? |
|--------|----------------|--------------------|----|
| tm-prompt.md | docs/specs/tm-spec.md | docs/artifacts/tm-template.md | No |
| sar-prompt.md | docs/specs/sar-spec.md | docs/artifacts/sar-template.md | No |
| cer-prompt.md | docs/specs/cer-spec.md | docs/artifacts/cer-template.md | No |
| dar-prompt.md | docs/specs/dar-spec.md | docs/artifacts/dar-template.md | No |

**Expected result:** All prompts reference correct spec and template; no inlined rules.

---

### S-05: Validator-to-Spec Reference Integrity

**Check:** Each validator references its spec as the source of truth. Validators do not reference prompts.

| Validator | References Spec | References Prompt? |
|-----------|-----------------|-------------------|
| tm-validator.md | docs/specs/tm-spec.md | No |
| sar-validator.md | docs/specs/sar-spec.md | No |
| cer-validator.md | docs/specs/cer-spec.md | No |
| dar-validator.md | docs/specs/dar-spec.md | No |

**Expected result:** All validators reference the correct spec; none reference prompts.

---

### S-06: Template Section Alignment

**Check:** Each template's section headings match the required sections listed in the corresponding spec.

| Artifact | Spec Required Sections | Template Sections |
|----------|----------------------|-------------------|
| TM | Document Control, System Overview, Attack Surface Analysis, Threat Actor Identification, Threat Analysis, Mitigation Mapping, Residual Risk Assessment | sections 1-7 (all present) |
| SAR | Document Control, Assessment Scope, Guardrail Verification, Mitigation Verification, Vulnerability Assessment, Authentication and Authorization Review, Data Handling Review, Assessment Verdict | sections 1-8 (all present) |
| CER | Document Control, Regulatory Scope, Requirement-to-Evidence Mapping, Gap Assessment, Evidence Index, Compliance Verdict | sections 1-6 (all present) |
| DAR | Document Control, Dependency Inventory, Vulnerability Assessment, License Compliance, Supply Chain Risk Classification, Remediation Plan, Audit Summary | sections 1-7 (all present) |

**Expected result:** All template sections match spec required sections.

---

## Flow Scenario Tests

Flow scenarios verify that the kit's artifacts, when produced in order with appropriate inputs, pass validation. These tests require AI execution.

---

### F-01: Threat Model Generation and Validation

**Scenario:** Given a frozen SAD describing a web application with API, database, and external service integration, generate a TM and validate it.

**Setup:**
- Provide: a frozen SAD with 3+ components, 2+ integration points, and data flow descriptions
- Provide: security-principles.md
- Generate TM using the TM prompt
- Validate TM using the TM validator

**Expected outcomes:**
- TM: all 4 gates PASS
- Every SAD component appears in the attack surface analysis
- At least 2 threat actors identified with capability levels
- Every threat has a mitigation or accepted risk
- Overall risk posture stated

**Key gate to verify:** Gate 1 (attack_surface_coverage) — confirm every SAD component and integration point appears in the attack surface analysis.

---

### F-02: Security Assessment After Threat Model

**Scenario:** Given a frozen TM, ACF section 3 guardrails, and implementation evidence, generate a SAR and validate it.

**Setup:**
- Use a frozen TM from F-01
- Provide: ACF section 3 with 3+ guardrails
- Provide: implementation evidence (code references, test results)
- Generate SAR using the SAR prompt
- Validate SAR using the SAR validator

**Expected outcomes:**
- SAR: all 5 gates PASS
- Every ACF section 3 guardrail has a verification entry with evidence
- Every TM mitigation has a verification entry
- Auth flows and data handling explicitly reviewed
- Assessment verdict consistent with findings

**Key gate to verify:** Gate 2 (mitigation_verification) — confirm every TM mitigation from the frozen TM has a verification entry.

---

### F-03: Dependency Audit

**Scenario:** Given a project dependency manifest with known CVEs and multiple license types, generate a DAR and validate it.

**Setup:**
- Provide: dependency manifest with 10+ direct dependencies and transitive dependencies
- Provide: vulnerability scan results with at least 1 Critical/High CVE
- Provide: license analysis with at least 1 incompatible or unknown license
- Generate DAR using the DAR prompt
- Validate DAR using the DAR validator

**Expected outcomes:**
- DAR: all 5 gates PASS
- All dependencies listed with versions
- All CVEs mapped with severity ratings
- Incompatible licenses flagged with explanation
- Critical/High vulnerabilities have remediation plans
- Audit verdict consistent with findings

**Key gate to verify:** Gate 5 (remediation_plan) — confirm every Critical/High vulnerability has a remediation entry with action, owner, and date.

---

## Notes

- All structural checks (S-01 through S-06) should be verified before running flow scenarios.
- Flow scenarios F-01 through F-03 cover the three most common operational patterns.
- A compliance evidence flow scenario (CER generation and validation) may be added as organizations adopt specific regulatory frameworks.
- The cross-cutting nature of this kit means flow scenarios may involve inputs from multiple other kits.
