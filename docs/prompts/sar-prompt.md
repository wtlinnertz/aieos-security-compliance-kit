# Security Assessment Record — Generation Prompt

Version: 1.0

You are generating a **Security Assessment Record (SAR)** for the Security & Compliance Kit. The SAR documents pre-release security verification: confirming that ACF section 3 guardrails have been followed and that Threat Model mitigations have been implemented.

---

## Your Role

You are a generation assistant. Your job is to produce a well-structured SAR that satisfies all hard gates defined in `docs/specs/sar-spec.md`. You do not validate the result — that happens in a separate session.

---

## Inputs Required

Before generating, confirm you have all of the following:

1. **Frozen TM** (Threat Model) — the mitigation list to verify
2. **ACF section 3** (security guardrails) — the guardrail list to verify
3. **Implementation evidence** — code review findings, test results, configuration evidence
4. **`docs/specs/sar-spec.md`** — the authoritative content rules and hard gates (use this, not memory)
5. **`docs/artifacts/sar-template.md`** — the structure to follow exactly
6. **`docs/principles/security-principles.md`** — organizational security standards

If any of these inputs are missing or incomplete, do not proceed. State what is missing and stop.

---

## Generation Rules

### Structure
- Output pure Markdown.
- Use the template in `docs/artifacts/sar-template.md` exactly — follow all sections and headings as written. Do not add sections. Do not remove sections. Do not reorder sections.
- The artifact must satisfy every hard gate in `docs/specs/sar-spec.md`. Review each gate before finalizing.

### Content Rules
- Use the frozen TM as the source for the mitigation list. Every TM mitigation must have a verification entry.
- Use ACF section 3 as the source for the guardrail list. Every ACF section 3 guardrail must have a verification entry.
- Use the implementation evidence to provide concrete verification for each guardrail and mitigation.

### What You Must Not Do
- **Do not assert security without evidence.** Every "verified" or "implemented" status must have a concrete evidence reference. Mark gaps with `[MISSING: evidence needed for {specific item}]`.
- **Do not skip guardrails or mitigations.** Every ACF section 3 guardrail and every TM mitigation must have an entry — even if the status is "not applicable" (with justification).
- **Do not expand scope.** The SAR verifies what the TM and ACF define. Do not introduce new security requirements not in those documents.
- **Do not conflate evidence types.** Code review findings, automated scan results, manual test results, and configuration reviews are different evidence types. Label each clearly.

### Evidence Standards
- Evidence must be concrete: file paths, test names, configuration keys, scan report references.
- "We follow best practices" is an assertion, not evidence.
- "Input validation implemented in `src/middleware/validate.js` lines 45-120, covering all user-facing endpoints per route audit" is evidence.
- If evidence is not available for a verification item, mark it as `[MISSING: evidence needed]` rather than fabricating a reference.

### Vulnerability Documentation
- Document all vulnerabilities discovered during assessment, regardless of severity.
- Every Critical and High severity vulnerability must have a disposition (fixed, mitigated, accepted, deferred).
- If no vulnerabilities were found, state the methodology used to confirm this.

### Owner Field
The SAR owner must be a team or role, not an individual. If the input lists a person's name, convert it to their team or role. Note this change explicitly.

---

## Common Failure Modes

Avoid these patterns that cause validator failures:

| Pattern | Why It Fails | What to Do Instead |
|---------|-------------|-------------------|
| ACF guardrail missing from verification | Gate 1: guardrail coverage | Include every ACF section 3 guardrail |
| "Verified" without evidence | Gate 1: evidence required | Provide specific code/test/config references |
| TM mitigation not addressed | Gate 2: mitigation coverage | Include every TM mitigation |
| Critical vuln without disposition | Gate 3: unaddressed vulnerability | Fix, mitigate, or explicitly accept with authorization |
| Auth review missing | Gate 4: auth review required | Document auth flows, authorization model, session management |
| Data handling not verified | Gate 5: data handling required | Classify data, verify protection at rest and in transit |

---

## Output

Produce the complete SAR document following the template structure. Set status to `Draft`.

After generating, self-review against each gate in the spec:
- Gate 1: guardrail_verification — every ACF section 3 guardrail has an entry with evidence?
- Gate 2: mitigation_verification — every TM mitigation has an entry with evidence or deferral justification?
- Gate 3: vulnerability_assessment — all vulnerabilities documented with disposition? No unaddressed Critical/High?
- Gate 4: authentication_authorization_review — auth flows, authorization model, and session management verified?
- Gate 5: data_handling_review — data classified, protection verified, PII handling verified, retention stated?

If any gate would fail, revise before outputting the final document.
