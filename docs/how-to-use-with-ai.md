# How to Use This Kit with AI

This guide explains how to set up AI sessions for each artifact in the Security & Compliance Kit workflow. Follow the session setup instructions precisely — incorrect session setup is the most common cause of poor artifact quality.

---

## Core Discipline

**One artifact per session.** Do not generate multiple artifacts in the same session.

**Separate generation and validation.** Always validate in a new session. Never ask the AI that generated an artifact to validate it — this produces self-validation bias.

**Include full frozen documents.** Do not summarize upstream artifacts. Provide the complete document.

---

## TM — Generation Session

**Session setup:**
```
Documents to provide:
1. Frozen SAD (System Architecture Document — full document)
2. ACF section 3 (security guardrails — if available)
3. docs/specs/tm-spec.md
4. docs/artifacts/tm-template.md
5. docs/principles/security-principles.md

Prompt:
"Generate a Threat Model using the provided inputs.
Follow the prompt in docs/prompts/tm-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Assess every component and integration point from the SAD.
Do not invent threats not supported by the architecture — mark
any assumptions with [ASSUMPTION: reason]. Output pure Markdown."
```

**After generation:** Review the TM. Confirm:
- Every SAD component and integration point has been assessed
- Threat actors are realistic for the system's deployment context
- Mitigations are actionable and specific (not generic security advice)
- Residual risks have severity ratings and acceptance justification

**Validation session setup:**
```
Documents to provide:
1. The generated TM (full document)
2. docs/specs/tm-spec.md

Prompt:
"Validate this Threat Model against the TM spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/tm-validator.md."
```

---

## SAR — Generation Session

**Session setup:**
```
Documents to provide:
1. Frozen TM (Threat Model — full document)
2. ACF section 3 (security guardrails)
3. Implementation evidence (code review findings, test results, configuration)
4. docs/specs/sar-spec.md
5. docs/artifacts/sar-template.md
6. docs/principles/security-principles.md

Prompt:
"Generate a Security Assessment Record using the provided inputs.
Follow the prompt in docs/prompts/sar-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Verify each TM mitigation against implementation evidence.
Verify each ACF section 3 guardrail with concrete evidence.
Do not assert security without evidence — mark gaps with
[MISSING: evidence needed]. Output pure Markdown."
```

**After generation:** Review the SAR. Confirm:
- Every ACF section 3 guardrail has a verification entry with evidence
- Every TM mitigation has a verification status (implemented, partial, not implemented)
- Vulnerability findings have severity ratings and are actionable
- Authentication and authorization flows are explicitly verified

**Validation session setup:**
```
Documents to provide:
1. The generated SAR (full document)
2. docs/specs/sar-spec.md

Prompt:
"Validate this Security Assessment Record against the SAR spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/sar-validator.md."
```

---

## CER — Generation Session

**Session setup:**
```
Documents to provide:
1. Regulatory requirements document (specific mandate with clause references)
2. Implementation evidence (code, configuration, process documentation, audit logs)
3. Existing frozen SCK artifacts (TM, SAR, DAR — if available)
4. docs/specs/cer-spec.md
5. docs/artifacts/cer-template.md
6. docs/principles/compliance-principles.md

Prompt:
"Generate a Compliance Evidence Record using the provided inputs.
Follow the prompt in docs/prompts/cer-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Map every identified regulatory requirement to concrete evidence.
Do not assert compliance without evidence — mark gaps with
remediation plans. Output pure Markdown."
```

**After generation:** Review the CER. Confirm:
- Every regulatory requirement has a mapping to concrete evidence
- Evidence is retrievable and timestamped (not just assertions)
- Gaps have remediation plans with owners and target dates
- The audit trail is complete and traceable

**Validation session setup:**
```
Documents to provide:
1. The generated CER (full document)
2. docs/specs/cer-spec.md

Prompt:
"Validate this Compliance Evidence Record against the CER spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/cer-validator.md."
```

---

## DAR — Generation Session

**Session setup:**
```
Documents to provide:
1. Dependency manifest (complete dependency tree including transitive)
2. Vulnerability scan results (CVE database scan output)
3. License analysis output
4. docs/specs/dar-spec.md
5. docs/artifacts/dar-template.md
6. docs/principles/compliance-principles.md

Prompt:
"Generate a Dependency Audit Record using the provided inputs.
Follow the prompt in docs/prompts/dar-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
List all direct and transitive dependencies.
Map all known CVEs with severity ratings.
Classify all licenses and flag incompatibilities.
Output pure Markdown."
```

**After generation:** Review the DAR. Confirm:
- All direct and transitive dependencies are listed
- CVE mappings are accurate and severity ratings are correct
- License classifications match the actual license files
- Supply chain risk levels are justified
- Critical/high vulnerabilities have remediation plans with owners

**Validation session setup:**
```
Documents to provide:
1. The generated DAR (full document)
2. docs/specs/dar-spec.md

Prompt:
"Validate this Dependency Audit Record against the DAR spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/dar-validator.md."
```

---

## Troubleshooting

**Validator returns FAIL on multiple gates**
Check that the generation session included all required inputs. Missing inputs are the most common cause of multi-gate failures.

**Threat actors are flagged as too generic**
Provide more context about the system's deployment environment, user base, and data sensitivity in the SAD or principles file. Generic threat actors like "hacker" without capability levels fail the threat_actor_identification gate.

**Mitigation verification is incomplete**
The SAR requires concrete evidence for each mitigation — not assertions that it was done. Provide code review findings, test results, or configuration evidence to the generation session.

**Compliance evidence is flagged as assertions rather than evidence**
The CER requires retrievable, timestamped evidence. Statements like "we encrypt data" are assertions. Evidence is: "AES-256 encryption configured in config/encryption.yml, verified by integration test test_data_encryption_at_rest, last run 2026-03-01."

**Dependency list is incomplete**
Ensure the dependency manifest includes transitive dependencies. Run your package manager's full dependency tree command (e.g., `npm ls --all`, `pip freeze`, `go mod graph`) and provide the complete output.
