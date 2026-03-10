# Compliance Evidence Record — Validator

This validator evaluates a completed Compliance Evidence Record (CER) against `docs/specs/cer-spec.md`. It is used in a separate AI session from the one that generated the CER.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The completed CER (full document)
2. `docs/specs/cer-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: mandate_coverage

Check:
- Every identified regulatory requirement has a mapping entry
- Each entry has: requirement reference (clause number), requirement description, compliance status (from enumerated values: compliant, partially compliant, non-compliant, not applicable), and evidence reference(s)
- "Not applicable" entries include justification
- No requirement is left unmapped

**Pass:** Every identified requirement has a mapping entry with all required fields; "not applicable" entries are justified.
**Fail:** Any identified requirement missing a mapping entry; entries missing compliance status or evidence references; "not applicable" without justification.

---

### Gate 2: evidence_completeness

Check:
- Every requirement marked "compliant" or "partially compliant" has concrete, retrievable evidence — not assertions
- Every evidence reference cited in the mapping appears in the Evidence Index
- Each Evidence Index entry has: evidence ID, description, type, retrievable location, and timestamp or date range
- Evidence is concrete — "we comply" is an assertion; a specific document, code path, or test result is evidence

**Pass:** All compliant/partial requirements have concrete evidence; all evidence references appear in the index with ID, type, location, and timestamp.
**Fail:** Compliant requirements without evidence; evidence references not in the index; evidence is an assertion rather than a concrete reference; index entries missing location or timestamp.

---

### Gate 3: gap_assessment

Check:
- Every requirement with status "partially compliant" or "non-compliant" has a gap entry
- Each gap entry has: requirement reference, gap description, remediation plan, remediation owner (team or role), and target date
- If no gaps exist, this is explicitly stated

**Pass:** Every gap has all required fields; or explicit statement that no gaps exist (consistent with all requirements being compliant).
**Fail:** Partially compliant or non-compliant requirements without gap entries; gap entries missing remediation plan, owner, or target date; gaps exist but gap section is absent.

---

### Gate 4: audit_trail

Check:
- Every Evidence Index entry has: evidence ID (E-NNN format), description, evidence type, retrievable location, and timestamp or date range
- No evidence is "available upon request" or has a vague location
- The evidence chain from requirement to evidence reference to Evidence Index location is traceable

**Pass:** All index entries have the required fields; locations are specific and retrievable; timestamps are present; the chain is traceable.
**Fail:** Index entries missing location or timestamp; location is vague ("in our systems," "available upon request"); evidence chain is broken (reference in mapping does not appear in index).

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "mandate_coverage": "PASS | FAIL",
    "evidence_completeness": "PASS | FAIL",
    "gap_assessment": "PASS | FAIL",
    "audit_trail": "PASS | FAIL"
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
- Do not redesign or improve the CER
- Do not evaluate writing quality beyond spec requirements
- Do not accept assertions as evidence
- Do not interpret regulatory requirements — evaluate only that mapping exists
- Evaluate only what is explicitly present in the document
