# Security Assessment Record — Specification

Version: v1.0

The Security Assessment Record (SAR) is a pre-release security review artifact that documents verification of security guardrails (ACF section 3) and Threat Model mitigations against the actual implementation. It provides security clearance evidence for the release process.

---

## What This Artifact Is Not

- **Not a threat model.** The SAR verifies that mitigations identified in the TM are implemented — it does not identify new threats. New threat identification belongs in a TM revision.
- **Not a penetration test.** The SAR documents evidence-based verification of security controls. Penetration test results may be cited as evidence within the SAR but the SAR is not itself a pen test report.
- **Not a compliance record.** Regulatory compliance evidence belongs in the CER. The SAR is concerned with security implementation verification, not regulatory requirement mapping.

---

## Purpose

The SAR serves three roles:

1. **Guardrail verification** — Confirms that each security guardrail defined in ACF section 3 has been followed with concrete evidence
2. **Mitigation verification** — Confirms that each mitigation identified in the frozen TM has been implemented, partially implemented, or deferred with justification
3. **Vulnerability assessment** — Documents any vulnerabilities discovered during the assessment and their disposition

---

## Upstream Dependencies

- Frozen Threat Model (TM) — provides the mitigation list to verify
- ACF section 3 (security guardrails from EEK) — provides the guardrail list to verify
- Implementation code and/or TDD — provides the evidence base
- Organizational principles: `security-principles.md` — defines assessment standards

---

## Required Sections

1. Document Control
2. Assessment Scope
3. Guardrail Verification
4. Mitigation Verification
5. Vulnerability Assessment
6. Authentication and Authorization Review
7. Data Handling Review
8. Assessment Verdict

---

## Content Rules

### Document Control
**Rules**
- SAR ID must be present (format: SAR-{PROJECT}-{NNN})
- System name must be present
- Owner must be present as a team or role, not an individual
- Version must be present (format: v{N})
- Status must be one of: Draft | Validated | Frozen
- TM Reference must be present (the frozen TM ID this SAR verifies against)
- ACF Reference must be present (the ACF version providing security guardrails)

**Failure Examples**
- SAR ID absent
- TM Reference absent
- ACF Reference absent
- Owner listed as an individual person's name

### Assessment Scope
**Rules**
- The scope of the assessment must be stated: which components, services, and interfaces were assessed
- Assessment methodology must be stated: how evidence was gathered (code review, automated scanning, manual testing, configuration review)
- Assessment date range must be stated
- Any exclusions (components not assessed) must be listed with justification

**Failure Examples**
- Scope absent or "entire system" without component list
- Methodology absent
- Date range absent

### Guardrail Verification
**Rules**
- Every guardrail from ACF section 3 must have a verification entry
- Each entry must state: the guardrail reference (ACF section 3 clause), the verification status (verified, partial, not verified, not applicable), the evidence (specific code reference, test result, configuration setting, or documentation), and any notes
- "Not applicable" must include justification for why the guardrail does not apply
- Evidence must be concrete — assertions without evidence fail (e.g., "we follow secure coding practices" is an assertion; "input validation implemented in middleware/validation.js lines 45-120, covering all user-facing endpoints per route audit in appendix A" is evidence)

**Failure Examples**
- ACF section 3 guardrails missing from verification
- Guardrails marked "verified" without evidence
- "Not applicable" without justification
- Evidence is an assertion rather than a concrete reference

### Mitigation Verification
**Rules**
- Every mitigation from the frozen TM must have a verification entry
- Each entry must state: the TM threat ID, the TM mitigation description, the verification status (implemented, partial, not implemented, deferred), and the evidence or deferral justification
- "Deferred" must include: reason for deferral, risk acceptance by whom, and target implementation date
- "Partial" must include: what is implemented, what remains, and a plan to complete

**Failure Examples**
- TM mitigations missing from verification
- Mitigations marked "implemented" without evidence
- "Deferred" without risk acceptance or target date
- "Partial" without description of remaining work

### Vulnerability Assessment
**Rules**
- All vulnerabilities discovered during assessment must be documented
- Each vulnerability must state: a vulnerability ID (format: V-{NNN}), description, severity (Critical, High, Medium, Low), affected component, and disposition (fixed, mitigated, accepted, deferred)
- No unaddressed Critical or High vulnerabilities may remain — each must be fixed, mitigated with evidence, or explicitly accepted with authorization
- If no vulnerabilities were found, this must be explicitly stated with the assessment methodology used

**Failure Examples**
- Vulnerabilities mentioned but not catalogued
- Critical/High vulnerabilities without disposition
- Severity ratings absent
- "No vulnerabilities" without methodology reference

### Authentication and Authorization Review
**Rules**
- Authentication flows must be described and verified: how users authenticate, what protocols are used, how credentials are handled
- Authorization model must be described: how access control is enforced, what the permission model is
- Each authentication and authorization mechanism must have a verification status with evidence
- Session management must be addressed: how sessions are created, maintained, and terminated

**Failure Examples**
- Authentication flows not described
- Authorization model absent
- Verification without evidence
- Session management not addressed

### Data Handling Review
**Rules**
- Data classification must be stated for each data type the system handles (matching TM data sensitivity classification)
- Data at rest protection must be verified with evidence for each classified data type
- Data in transit protection must be verified with evidence
- Data retention and disposal policy must be stated
- Any PII or sensitive data handling must be explicitly verified

**Failure Examples**
- Data classification absent
- Data protection claimed without evidence
- PII handling not addressed
- Retention policy absent

### Assessment Verdict
**Rules**
- Overall assessment verdict must be stated: Pass (clear for release), Conditional Pass (clear with noted conditions), or Fail (not clear for release)
- If Conditional Pass: conditions must be listed with owners and deadlines
- If Fail: blocking issues must be listed
- The verdict must be consistent with the findings — a Pass verdict with unaddressed Critical vulnerabilities is a contradiction

**Failure Examples**
- Verdict absent
- Conditional Pass without conditions listed
- Verdict contradicts findings

---

## Format Requirements

- Vulnerability IDs must follow the format V-{NNN} (e.g., V-001, V-002)
- Severity ratings must use the enumerated scale: Critical, High, Medium, Low
- All verification statuses must use the enumerated values defined in each section
- Evidence must be concrete references, not assertions

---

## Completeness Rules

- All eight sections must be present and non-empty
- Every ACF section 3 guardrail verified
- Every TM mitigation verified
- All discovered vulnerabilities documented with disposition
- Authentication, authorization, and data handling explicitly reviewed

---

## Hard Gates

1. **guardrail_verification** — Every ACF section 3 guardrail has a verification entry with status and concrete evidence; no guardrail is verified without evidence; "not applicable" entries include justification
2. **mitigation_verification** — Every TM mitigation has a verification entry with status and evidence or deferral justification; deferred mitigations have risk acceptance, approver, and target date
3. **vulnerability_assessment** — All discovered vulnerabilities documented with ID, severity, component, and disposition; no unaddressed Critical or High vulnerabilities remain; if no vulnerabilities found, methodology is stated
4. **authentication_authorization_review** — Authentication flows described and verified with evidence; authorization model described and verified; session management addressed
5. **data_handling_review** — Data classification stated per data type; data at rest and in transit protection verified with evidence; PII/sensitive data handling explicitly verified; retention policy stated
