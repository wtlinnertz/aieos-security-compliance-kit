# Threat Model

---

## 1. Document Control

| Field | Value |
|-------|-------|
| TM ID | TM-{PROJECT}-{NNN} |
| System Name | {system name — must match SAD} |
| Owner | {team or role — not an individual person} |
| Version | v{N} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| SAD Reference | {SAD-{PROJECT}-{NNN}} |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. System Overview

**System Description:** {brief description sourced from SAD}

**System Boundary:**
- In scope: {systems, components, and interfaces assessed by this threat model}
- Out of scope: {systems, components, or interfaces explicitly excluded}

**Data Sensitivity Classification:**

| Data Type | Classification | Description |
|-----------|---------------|-------------|
| {data type} | {Public / Internal / Confidential / Restricted} | {brief description} |

---

## 3. Attack Surface Analysis

### 3.1 {Component or Interface Name}

| Field | Value |
|-------|-------|
| Component/Interface | {name from SAD} |
| Exposure Type | {network / file system / user input / API / message queue / other} |
| Trust Boundary | {trust boundary crossed, if any} |
| Description | {brief description of attack surface} |

### 3.2 {Component or Interface Name}

| Field | Value |
|-------|-------|
| Component/Interface | {name from SAD} |
| Exposure Type | {network / file system / user input / API / message queue / other} |
| Trust Boundary | {trust boundary crossed, if any} |
| Description | {brief description of attack surface} |

*Add additional entries for every SAD component and integration point.*

---

## 4. Threat Actor Identification

### Actor 1: {Category Name}

| Field | Value |
|-------|-------|
| Category | {category name} |
| Capability Level | {low / medium / high / advanced} |
| Motivation | {motivation description} |
| Targeted Attack Surfaces | {list of attack surfaces from section 3 this actor would target} |

### Actor 2: {Category Name}

| Field | Value |
|-------|-------|
| Category | {category name} |
| Capability Level | {low / medium / high / advanced} |
| Motivation | {motivation description} |
| Targeted Attack Surfaces | {list of attack surfaces from section 3 this actor would target} |

*Minimum 2 threat actors required. Add additional actors as appropriate for the system context.*

---

## 5. Threat Analysis

### T-001: {Threat Description}

| Field | Value |
|-------|-------|
| Threat ID | T-001 |
| Description | {threat description} |
| Attack Surface | {attack surface from section 3} |
| Threat Actor(s) | {threat actor(s) from section 4} |
| Impact | {Confidentiality / Integrity / Availability — one or more} |
| Severity | {Critical / High / Medium / Low} |
| Severity Justification | {impact and likelihood reasoning} |

### T-002: {Threat Description}

| Field | Value |
|-------|-------|
| Threat ID | T-002 |
| Description | {threat description} |
| Attack Surface | {attack surface from section 3} |
| Threat Actor(s) | {threat actor(s) from section 4} |
| Impact | {Confidentiality / Integrity / Availability — one or more} |
| Severity | {Critical / High / Medium / Low} |
| Severity Justification | {impact and likelihood reasoning} |

*At least one threat per attack surface. Add additional threats as identified.*

---

## 6. Mitigation Mapping

### T-001 Mitigation

| Field | Value |
|-------|-------|
| Threat ID | T-001 |
| Mitigation Description | {specific, actionable mitigation} |
| Mitigation Type | {Preventive / Detective / Corrective} |
| Implementation Status | {Planned / In Progress / Implemented / Accepted Risk} |

### T-002 Mitigation

| Field | Value |
|-------|-------|
| Threat ID | T-002 |
| Mitigation Description | {specific, actionable mitigation} |
| Mitigation Type | {Preventive / Detective / Corrective} |
| Implementation Status | {Planned / In Progress / Implemented / Accepted Risk} |

*If a threat is accepted without mitigation:*

| Field | Value |
|-------|-------|
| Threat ID | {T-NNN} |
| Mitigation Description | Accepted Risk |
| Justification | {why this risk is accepted} |
| Accepted By | {approver — team or role} |

*Every threat from section 5 must have an entry here.*

---

## 7. Residual Risk Assessment

### T-001 Residual Risk

| Field | Value |
|-------|-------|
| Threat ID | T-001 |
| Original Severity | {from section 5} |
| Residual Severity | {Critical / High / Medium / Low / Negligible} |
| Basis | {why the residual severity is at this level after mitigation} |

### T-002 Residual Risk

| Field | Value |
|-------|-------|
| Threat ID | T-002 |
| Original Severity | {from section 5} |
| Residual Severity | {Critical / High / Medium / Low / Negligible} |
| Basis | {why the residual severity is at this level after mitigation} |

*For Critical or High residual risks, add:*

| Field | Value |
|-------|-------|
| Escalation Path | {additional mitigation plan or escalation action} |
| Owner | {team or role} |
| Target Date | {YYYY-MM-DD} |

**Overall Residual Risk Posture:** {Acceptable / Conditionally Acceptable / Unacceptable}

**Posture Justification:** {basis for the overall risk posture assessment}
