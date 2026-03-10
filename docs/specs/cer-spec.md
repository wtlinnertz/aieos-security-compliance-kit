# Compliance Evidence Record — Specification

Version: v1.0

The Compliance Evidence Record (CER) maps regulatory requirements to implementation evidence, creating an auditable trail. It is triggered by compliance mandates and can be generated at any time, independent of the delivery pipeline.

---

## What This Artifact Is Not

- **Not a security assessment.** The CER maps regulatory requirements to evidence — it does not assess security posture. Security assessment belongs in the SAR.
- **Not a policy document.** The CER documents evidence of compliance, not the policies themselves. Policies belong in the principles files or in organizational governance documents.
- **Not a self-certification.** The CER must contain retrievable, verifiable evidence — not assertions of compliance.

---

## Purpose

The CER serves three roles:

1. **Requirement mapping** — Maps each identified regulatory requirement to specific implementation evidence
2. **Evidence chain** — Creates a retrievable, timestamped trail from requirement to implementation
3. **Gap identification** — Identifies compliance gaps with remediation plans and accountability

---

## Upstream Dependencies

- Regulatory requirements (specific mandate with clause references)
- Implementation evidence (code, configuration, process documentation, audit logs)
- Existing frozen SCK artifacts (TM, SAR, DAR — if available, provide supporting evidence)
- Organizational principles: `compliance-principles.md` — defines compliance standards and evidence requirements

---

## Required Sections

1. Document Control
2. Regulatory Scope
3. Requirement-to-Evidence Mapping
4. Gap Assessment
5. Evidence Index
6. Compliance Verdict

---

## Content Rules

### Document Control
**Rules**
- CER ID must be present (format: CER-{PROJECT}-{NNN})
- System or initiative name must be present
- Owner must be present as a team or role, not an individual
- Version must be present (format: v{N})
- Status must be one of: Draft | Validated | Frozen
- Regulatory mandate reference must be present (the specific regulation, standard, or framework)

**Failure Examples**
- CER ID absent
- Regulatory mandate reference absent
- Owner listed as an individual person's name

### Regulatory Scope
**Rules**
- The regulatory framework must be identified (e.g., SOC 2 Type II, GDPR, HIPAA, PCI-DSS)
- The specific clauses or requirements in scope must be listed
- The assessment boundary must be stated: which systems, services, or processes are in scope
- Any clauses explicitly out of scope must be listed with justification

**Failure Examples**
- Framework named but specific clauses not listed
- Assessment boundary absent
- Out-of-scope clauses not justified

### Requirement-to-Evidence Mapping
**Rules**
- Every identified regulatory requirement must have a mapping entry
- Each entry must state: the requirement reference (regulation clause number), the requirement description, the compliance status (compliant, partially compliant, non-compliant, not applicable), and the evidence reference(s)
- Evidence must be concrete and retrievable — not assertions
- "Not applicable" must include justification
- Evidence references must point to specific, verifiable artifacts (document name, section, date, or system reference)

**Failure Examples**
- Regulatory requirements without mapping entries
- Compliance status "compliant" without evidence references
- Evidence is an assertion ("we comply with this requirement")
- "Not applicable" without justification

### Gap Assessment
**Rules**
- Every requirement with status "partially compliant" or "non-compliant" must have a gap entry
- Each gap must state: the requirement reference, the gap description (what is missing or incomplete), the remediation plan (specific actions to close the gap), the remediation owner (team or role), and the target remediation date
- If no gaps exist, this must be explicitly stated

**Failure Examples**
- Partially compliant requirements without gap entries
- Gaps without remediation plans
- Remediation plans without owners or target dates
- Gap section absent

### Evidence Index
**Rules**
- Every evidence reference cited in the mapping must appear in the evidence index
- Each index entry must state: the evidence ID (format: E-{NNN}), the evidence description, the evidence type (document, code, configuration, test result, audit log, process record), the location (where to retrieve it), and the timestamp or date range
- Evidence must be retrievable — dead links, "available upon request," or vague location references fail

**Failure Examples**
- Evidence cited in mapping but not in index
- Evidence entries without location
- Evidence entries without timestamp
- Location is vague ("in our systems")

### Compliance Verdict
**Rules**
- Overall compliance verdict must be stated: Compliant, Partially Compliant, or Non-Compliant
- If Partially Compliant: summary of gaps with expected remediation timeline
- If Non-Compliant: blocking gaps listed with immediate action items
- The verdict must be consistent with the findings — a Compliant verdict with non-compliant requirements is a contradiction

**Failure Examples**
- Verdict absent
- Verdict contradicts findings
- Partially Compliant without gap summary

---

## Format Requirements

- Evidence IDs must follow the format E-{NNN} (e.g., E-001, E-002)
- Requirement references must cite specific clause numbers (not just framework names)
- All dates must be in ISO 8601 format (YYYY-MM-DD)
- Compliance statuses must use the enumerated values: compliant, partially compliant, non-compliant, not applicable

---

## Completeness Rules

- All six sections must be present and non-empty
- Every identified regulatory requirement mapped
- Every evidence reference indexed with location and timestamp
- Every gap has a remediation plan with owner and date
- Verdict is consistent with findings

---

## Hard Gates

1. **mandate_coverage** — All identified regulatory requirements have mapping entries with compliance status and evidence references; no requirement is left unmapped; "not applicable" entries include justification
2. **evidence_completeness** — Every requirement marked "compliant" or "partially compliant" has concrete, retrievable evidence (not assertions); every evidence reference in the mapping appears in the evidence index with location and timestamp
3. **gap_assessment** — Every "partially compliant" or "non-compliant" requirement has a gap entry with description, remediation plan, owner, and target date; if no gaps exist, this is explicitly stated
4. **audit_trail** — Every evidence index entry has evidence ID, description, type, retrievable location, and timestamp; no evidence is "available upon request" or has a vague location; the evidence chain from requirement to evidence to location is traceable
