# Compliance Evidence Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| CER ID | CER-{PROJECT}-{NNN} |
| System/Initiative Name | {system or initiative name} |
| Owner | {team or role — not an individual person} |
| Version | v{N} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Regulatory Mandate | {specific regulation, standard, or framework} |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Regulatory Scope

**Regulatory Framework:** {e.g., SOC 2 Type II, GDPR, HIPAA, PCI-DSS}

**Requirements In Scope:**

| Clause | Requirement Summary |
|--------|-------------------|
| {clause number} | {brief description of the requirement} |
| {clause number} | {brief description of the requirement} |

**Assessment Boundary:** {which systems, services, or processes are in scope for this assessment}

**Requirements Out of Scope:**

| Clause | Justification |
|--------|--------------|
| {clause number} | {why this clause is out of scope} |

*If no clauses are out of scope, write: "All clauses within the identified framework are in scope."*

---

## 3. Requirement-to-Evidence Mapping

### {Clause Number}: {Requirement Description}

| Field | Value |
|-------|-------|
| Requirement Reference | {clause number} |
| Requirement Description | {description of what the regulation requires} |
| Compliance Status | {Compliant / Partially Compliant / Non-Compliant / Not Applicable} |
| Evidence Reference(s) | {E-NNN, E-NNN — references to Evidence Index} |
| Notes | {additional context} |

### {Clause Number}: {Requirement Description}

| Field | Value |
|-------|-------|
| Requirement Reference | {clause number} |
| Requirement Description | {description of what the regulation requires} |
| Compliance Status | {Compliant / Partially Compliant / Non-Compliant / Not Applicable} |
| Evidence Reference(s) | {E-NNN, E-NNN — references to Evidence Index} |
| Notes | {additional context} |

*One entry per identified regulatory requirement. "Not Applicable" must include justification in Notes.*

---

## 4. Gap Assessment

### Gap for {Clause Number}

| Field | Value |
|-------|-------|
| Requirement Reference | {clause number} |
| Gap Description | {what is missing or incomplete} |
| Remediation Plan | {specific actions to close the gap} |
| Remediation Owner | {team or role} |
| Target Date | {YYYY-MM-DD} |

*One entry per partially compliant or non-compliant requirement.*

*If no gaps exist, write: "No compliance gaps identified. All in-scope requirements are compliant."*

---

## 5. Evidence Index

| Evidence ID | Description | Type | Location | Timestamp |
|-------------|-------------|------|----------|-----------|
| E-001 | {evidence description} | {Document / Code / Configuration / Test Result / Audit Log / Process Record} | {retrievable location} | {YYYY-MM-DD or date range} |
| E-002 | {evidence description} | {Document / Code / Configuration / Test Result / Audit Log / Process Record} | {retrievable location} | {YYYY-MM-DD or date range} |

*Every evidence reference cited in section 3 must appear here. Location must be specific and retrievable.*

---

## 6. Compliance Verdict

**Overall Verdict:** {Compliant / Partially Compliant / Non-Compliant}

**Verdict Justification:** {basis for the verdict, consistent with findings above}

*For Partially Compliant:*

**Gap Summary:**
- {number} requirements partially compliant
- {number} requirements non-compliant
- Expected remediation timeline: {date or range}

*For Non-Compliant:*

**Blocking Gaps:**

| Clause | Gap | Immediate Action |
|--------|-----|-----------------|
| {clause number} | {gap description} | {action required} |
