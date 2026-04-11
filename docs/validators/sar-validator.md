# Security Assessment Record — Validator

This validator evaluates a completed Security Assessment Record (SAR) against `docs/specs/sar-spec.md`. It is used in a separate AI session from the one that generated the SAR.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The completed SAR (full document)
2. `docs/specs/sar-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: guardrail_verification

Check:
- Every ACF section 3 guardrail has a verification entry
- Each entry has: guardrail reference, verification status (from enumerated values: verified, partial, not verified, not applicable), and evidence
- Evidence is concrete — not an assertion (e.g., "we follow best practices" fails; a specific code reference passes)
- "Not applicable" entries include justification

**Pass:** Every ACF section 3 guardrail has a verification entry with status and concrete evidence; "not applicable" entries are justified.
**Fail:** Any ACF section 3 guardrail missing a verification entry; any "verified" status without concrete evidence; "not applicable" without justification; evidence is an assertion rather than a concrete reference.

---

### Gate 2: mitigation_verification

Check:
- Every mitigation from the frozen TM has a verification entry
- Each entry has: TM threat ID, TM mitigation description, verification status (implemented, partial, not implemented, deferred), and evidence or deferral justification
- "Deferred" entries include: reason, risk acceptance approver, and target implementation date
- "Partial" entries include: what is implemented, what remains, and completion plan

**Pass:** Every TM mitigation has a verification entry with status and evidence; deferred entries have all required fields; partial entries describe what is done and what remains.
**Fail:** Any TM mitigation missing a verification entry; "implemented" without evidence; "deferred" without risk acceptance or target date; "partial" without description of remaining work.

---

### Gate 3: vulnerability_assessment

Check:
- All vulnerabilities discovered during assessment are documented
- Each vulnerability has: vulnerability ID (V-NNN format), description, severity (from enumerated scale), affected component, and disposition (fixed, mitigated, accepted, deferred)
- No unaddressed Critical or High severity vulnerabilities remain
- If no vulnerabilities were found, the assessment methodology is stated

**Pass:** All vulnerabilities documented with required fields; no unaddressed Critical/High vulnerabilities; if none found, methodology is cited.
**Fail:** Vulnerabilities mentioned but not individually catalogued; Critical/High vulnerabilities without disposition; severity ratings absent; "no vulnerabilities" without methodology reference.

---

### Gate 4: authentication_authorization_review

Check:
- Authentication flows are described and verified with evidence
- Authorization model is described and verified with evidence
- Session management is addressed (creation, maintenance, termination)
- Each mechanism has a verification status with evidence

**Pass:** Authentication flows described and verified; authorization model described and verified; session management addressed with evidence.
**Fail:** Authentication flows not described; authorization model absent; session management not addressed; verification without evidence.

---

### Gate 5: data_handling_review

Check:
- Data classification is stated for each data type the system handles
- Data at rest protection is verified with evidence for each classified data type
- Data in transit protection is verified with evidence
- Data retention and disposal policy is stated
- PII or sensitive data handling is explicitly verified

**Pass:** Data classified; at rest and in transit protection verified with evidence; PII handling verified; retention policy stated.
**Fail:** Data classification absent; protection claimed without evidence; PII handling not addressed; retention policy absent.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "guardrail_verification": "PASS | FAIL",
    "mitigation_verification": "PASS | FAIL",
    "vulnerability_assessment": "PASS | FAIL",
    "authentication_authorization_review": "PASS | FAIL",
    "data_handling_review": "PASS | FAIL"
  },
  "blocking_issues": [
    {
      "gate": "<gate_name>",
      "description": "<what specifically failed>",
      "location": "<section or field where the failure is>"
    }
  ],
  "warnings": [
    {
      "description": "<non-blocking observation>",
      "location": "<section>"
    }
  ],
  "completeness_score": "<0-100>"
}
```

**Interpretation rules:**
- Any gate failure -> `"status": "FAIL"`
- `blocking_issues` lists exactly the failures — no additional content
- `warnings` are non-blocking; they do not affect status
- `completeness_score` is advisory; it does not override gate results
- If all gates pass, `blocking_issues` is an empty array

---

## Validator Constraints

- Do not suggest how to fix failures
- Do not redesign or improve the SAR
- Do not evaluate writing quality beyond spec requirements
- Do not accept assertions as evidence
- Evaluate only what is explicitly present in the document
