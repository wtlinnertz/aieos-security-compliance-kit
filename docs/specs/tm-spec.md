# Threat Model — Specification

Version: v1.0

The Threat Model (TM) is a per-initiative threat analysis that identifies attack surfaces, threat actors, mitigations, and residual risks for a system. It is generated after the System Architecture Document (SAD) is frozen and serves as the foundational security artifact from which the Security Assessment Record verifies implementation.

---

## What This Artifact Is Not

- **Not a security policy document.** The TM analyzes threats to a specific system — it does not define organizational security standards. Those belong in the principles files.
- **Not a penetration test report.** The TM identifies threats based on architecture review. Penetration testing produces evidence that feeds the SAR, not the TM.
- **Not a compliance checklist.** Regulatory compliance mapping belongs in the CER. The TM is concerned with threats and mitigations, not regulatory requirements.

---

## Purpose

The TM serves three roles:

1. **Threat identification** — Systematically identifies what can go wrong, who would cause it, and how
2. **Mitigation mapping** — Ensures every identified threat has a documented mitigation or an explicit risk acceptance
3. **Residual risk documentation** — Makes remaining risks visible with severity ratings so stakeholders can make informed decisions

---

## Upstream Dependencies

- Frozen System Architecture Document (SAD) — provides component inventory, integration points, data flows
- ACF section 3 (security guardrails) — if available, provides organizational security constraints
- Organizational principles: `security-principles.md` — defines threat modeling methodology and security standards

---

## Required Sections

1. Document Control
2. System Overview
3. Attack Surface Analysis
4. Threat Actor Identification
5. Threat Analysis
6. Mitigation Mapping
7. Residual Risk Assessment

---

## Content Rules

### Document Control
**Rules**
- TM ID must be present (format: TM-{PROJECT}-{NNN})
- System name must be present and match the SAD system name
- Owner must be present as a team or role, not an individual
- Version must be present (format: v{N})
- Status must be one of: Draft | Validated | Frozen
- SAD Reference must be present (the frozen SAD ID this TM is based on)

**Failure Examples**
- TM ID absent
- System name does not match SAD
- Owner listed as an individual person's name
- SAD Reference absent

### System Overview
**Rules**
- Brief description of the system under analysis (sourced from SAD)
- System boundary must be stated: what is in scope and what is out of scope for this threat model
- Data sensitivity classification must be stated for each major data type the system handles

**Failure Examples**
- System overview absent
- No boundary statement
- Data sensitivity not classified

### Attack Surface Analysis
**Rules**
- Every component listed in the SAD must be assessed for attack surface
- Every integration point (API, message queue, database connection, external service) must be assessed
- Every data flow crossing a trust boundary must be identified
- Each attack surface entry must state: the component or interface, the exposure type (network, file system, user input, etc.), and the trust boundary it crosses (if any)

**Failure Examples**
- SAD components missing from analysis
- Integration points not assessed
- Data flows across trust boundaries not identified
- Attack surface entries without exposure type

### Threat Actor Identification
**Rules**
- At least two threat actor categories must be identified
- Each threat actor must have: a category name, a capability level (low, medium, high, advanced), a motivation, and the attack surfaces they are most likely to target
- Threat actors must be relevant to the system's deployment context (an internal-only tool does not need nation-state actors unless justified)

**Failure Examples**
- Only one threat actor identified
- Threat actors without capability levels
- Generic "hacker" without specifics
- Threat actors not mapped to attack surfaces

### Threat Analysis
**Rules**
- At least one threat must be identified per attack surface entry
- Each threat must have: a threat ID (format: T-{NNN}), a description, the attack surface it targets, the threat actor(s) who would exploit it, the potential impact (confidentiality, integrity, availability), and a severity rating (Critical, High, Medium, Low)
- Severity ratings must be justified with impact and likelihood reasoning

**Failure Examples**
- Attack surfaces with no identified threats
- Threats without severity ratings
- Severity ratings without justification
- Threats not mapped to attack surfaces or threat actors

### Mitigation Mapping
**Rules**
- Every identified threat must have a corresponding mitigation entry
- Each mitigation must state: the threat ID it addresses, the mitigation description (specific and actionable), the mitigation type (preventive, detective, corrective), and the implementation status (planned, in progress, implemented, accepted risk)
- If a threat has no mitigation, it must be explicitly documented as an accepted risk with justification and an approver
- Mitigations must be specific — "use encryption" is not sufficient; "encrypt data at rest using AES-256 via the application-level encryption service" is

**Failure Examples**
- Threats with no mitigation entry
- Mitigations described generically ("improve security")
- Accepted risks without justification or approver
- Mitigation type absent

### Residual Risk Assessment
**Rules**
- Every threat with a mitigation must have a residual risk assessment
- Each residual risk must state: the original threat ID, the residual severity after mitigation (Critical, High, Medium, Low, Negligible), and the basis for the residual severity rating
- Any residual risk rated Critical or High must have an explicit escalation path or additional mitigation plan with owner and target date
- Overall residual risk posture must be stated (acceptable, conditionally acceptable, unacceptable)

**Failure Examples**
- Mitigated threats without residual risk assessment
- Residual severity without basis
- Critical/High residual risks without escalation path
- No overall risk posture statement

---

## Format Requirements

- Threat IDs must follow the format T-{NNN} (e.g., T-001, T-002)
- Severity ratings must use the enumerated scale: Critical, High, Medium, Low
- Capability levels must use the enumerated scale: low, medium, high, advanced
- All tables must use consistent column structure

---

## Completeness Rules

- All seven sections must be present and non-empty
- Every SAD component and integration point assessed
- At least two threat actors identified
- At least one threat per attack surface
- Every threat has a mitigation or accepted risk entry
- Every mitigated threat has a residual risk assessment

---

## Hard Gates

1. **attack_surface_coverage** — All SAD components and integration points are assessed; every data flow crossing a trust boundary is identified; each entry has component/interface, exposure type, and trust boundary
2. **threat_actor_identification** — At least two relevant threat actor categories identified; each has category name, capability level, motivation, and targeted attack surfaces
3. **mitigation_mapping** — Every identified threat has a mitigation or accepted risk with justification; mitigations are specific and actionable with type and implementation status
4. **residual_risk_assessment** — Every mitigated threat has residual risk with severity and basis; Critical/High residual risks have escalation path or additional mitigation plan; overall risk posture stated
