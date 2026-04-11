# Dependency Audit Record — Validator

This validator evaluates a completed Dependency Audit Record (DAR) against `docs/specs/dar-spec.md`. It is used in a separate AI session from the one that generated the DAR.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The completed DAR (full document)
2. `docs/specs/dar-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: dependency_inventory

Check:
- All direct dependencies are listed with: name, version, type (direct), and purpose
- All transitive dependencies are listed with: name, version, and the direct dependency that pulls them in
- Total dependency count is stated (direct and transitive separately)

**Pass:** All direct dependencies listed with required fields; transitive dependencies listed with parent tracing; total counts stated.
**Fail:** Dependencies listed without versions; transitive dependencies absent or not traced to parent; total count absent; direct dependencies missing purpose.

---

### Gate 2: vulnerability_assessment

Check:
- All known CVEs affecting any dependency are listed
- Each CVE entry has: CVE ID (standard format), affected dependency (name and version), severity rating (from recognized scoring system), description, and patch availability
- If no CVEs are found, the vulnerability database and scan date are cited
- Severity ratings reference a recognized scoring system (CVSS or organizational equivalent)

**Pass:** All CVEs listed with required fields; severity uses recognized system; if none found, methodology cited.
**Fail:** CVEs mentioned but not individually listed; entries missing severity, description, or patch availability; "no vulnerabilities" without scan reference; severity without scoring system.

---

### Gate 3: license_compliance

Check:
- Every dependency has its license identified
- Each license has a compatibility assessment (compatible, restricted, incompatible, unknown)
- Incompatible licenses are flagged with explanation of incompatibility
- Unknown licenses have a review plan with owner and target date
- License distribution summary is provided (count per license type)

**Pass:** Every dependency has license; compatibility assessed; incompatibilities explained; unknowns have review plan; distribution summary present.
**Fail:** Dependencies without license identification; licenses without compatibility assessment; incompatible licenses not flagged or not explained; unknown licenses without review plan; distribution summary absent.

---

### Gate 4: risk_classification

Check:
- Every direct dependency is classified by supply chain risk level (Low, Medium, High, Critical)
- Each classification considers: maintenance status, provenance, criticality, and adoption
- Each classification includes a brief justification
- High or Critical risk dependencies have a documented risk response

**Pass:** Every direct dependency classified with justification covering the four factors; High/Critical have risk response.
**Fail:** Dependencies without classification; classification without justification; justification missing required factors; High/Critical without risk response.

---

### Gate 5: remediation_plan

Check:
- Every Critical and High severity vulnerability has a remediation entry
- Each remediation entry has: CVE ID or risk reference, remediation action (upgrade, patch, replace, accept), remediation owner (team or role), and target date
- Accepted risks have justification and approver
- If no remediation is needed, this is explicitly stated

**Pass:** Every Critical/High vulnerability has remediation with action, owner, and date; accepted risks justified; or explicit "no remediation needed" statement.
**Fail:** Critical/High vulnerabilities without remediation; entries missing owner or target date; accepted risks without justification or approver.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "dependency_inventory": "PASS | FAIL",
    "vulnerability_assessment": "PASS | FAIL",
    "license_compliance": "PASS | FAIL",
    "risk_classification": "PASS | FAIL",
    "remediation_plan": "PASS | FAIL"
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
- Do not redesign or improve the DAR
- Do not evaluate writing quality beyond spec requirements
- Do not accept partial dependency lists as complete
- Evaluate only what is explicitly present in the document
