# Threat Model — Validator

This validator evaluates a completed Threat Model (TM) against `docs/specs/tm-spec.md`. It is used in a separate AI session from the one that generated the TM.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The completed TM (full document)
2. `docs/specs/tm-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: attack_surface_coverage

Check:
- Every component listed in the SAD (referenced via TM Document Control) is assessed for attack surface
- Every integration point (API, message queue, database connection, external service) is assessed
- Every data flow crossing a trust boundary is identified
- Each attack surface entry has: component/interface, exposure type, and trust boundary (if any)

**Pass:** All SAD components and integration points assessed; all trust boundary crossings identified; each entry has the required fields.
**Fail:** Any SAD component missing from analysis; any integration point not assessed; trust boundary crossings not identified; entries missing exposure type.

---

### Gate 2: threat_actor_identification

Check:
- At least two threat actor categories are identified
- Each threat actor has: category name, capability level (from enumerated scale: low, medium, high, advanced), motivation, and targeted attack surfaces
- Threat actors are relevant to the system's deployment context

**Pass:** At least 2 threat actors with all required fields; actors are contextually relevant.
**Fail:** Fewer than 2 threat actors; any actor missing category, capability level, motivation, or targeted surfaces; generic actors without specifics (e.g., "hacker" with no further detail).

---

### Gate 3: mitigation_mapping

Check:
- Every identified threat (from section 5) has a corresponding mitigation entry
- Each mitigation has: threat ID, mitigation description, mitigation type (preventive/detective/corrective), and implementation status
- Mitigations are specific and actionable — not generic security advice
- Threats without mitigation are documented as accepted risk with justification and approver

**Pass:** Every threat has a mitigation or accepted risk entry; all entries have the required fields; mitigations are specific.
**Fail:** Any threat without a mitigation entry; mitigations described generically ("improve security," "use encryption" without specifics); accepted risks without justification or approver; entries missing mitigation type or status.

---

### Gate 4: residual_risk_assessment

Check:
- Every threat with a mitigation has a residual risk assessment
- Each residual risk has: original threat ID, residual severity (from enumerated scale), and basis for the rating
- Any residual risk rated Critical or High has an escalation path or additional mitigation plan with owner and target date
- Overall residual risk posture is stated (acceptable, conditionally acceptable, or unacceptable)

**Pass:** Every mitigated threat has residual risk; all entries have severity and basis; Critical/High residuals have escalation; overall posture stated.
**Fail:** Any mitigated threat missing residual risk assessment; residual severity without basis; Critical/High residual risks without escalation path; no overall risk posture statement.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "attack_surface_coverage": "PASS | FAIL",
    "threat_actor_identification": "PASS | FAIL",
    "mitigation_mapping": "PASS | FAIL",
    "residual_risk_assessment": "PASS | FAIL"
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
- Do not redesign or improve the TM
- Do not evaluate writing quality beyond spec requirements
- Do not accept generic threat descriptions as equivalent to specific ones
- Evaluate only what is explicitly present in the document
