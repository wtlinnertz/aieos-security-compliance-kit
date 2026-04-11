# Compliance Principles

Version: v1.0

This document defines the organization's compliance standards and evidence philosophy. It is input material for artifact generation within the Security & Compliance Kit — not a governed artifact. Adapt this file to reflect your organization's actual compliance policies and applicable regulatory frameworks.

---

## 1. Evidence Standards

- Compliance evidence must be concrete and retrievable — not assertions of compliance.
- Evidence must be timestamped or date-ranged to establish currency.
- Evidence must be traceable: from regulatory requirement to implementation to verification.
- "We comply" is never sufficient evidence. Evidence is: a specific document, code path, configuration, test result, or audit log entry.

## 2. Regulatory Framework

- Identify all applicable regulatory frameworks before beginning compliance work.
- Map each regulatory requirement to specific clauses — do not reference frameworks at the abstract level.
- Requirements declared "not applicable" must include documented justification.
- Regulatory scope must be explicitly stated: which systems, services, and processes are covered.

## 3. Gap Management

- Compliance gaps are expected and must be documented transparently.
- Every gap must have a remediation plan with: specific actions, an owner, and a target date.
- Gaps without remediation plans are audit findings, not managed risks.
- Progress on gap remediation must be tracked and reported.

## 4. Dependency and License Compliance

- All production dependencies must have identified licenses.
- License compatibility with the project's license and organizational policy must be verified.
- Copyleft licenses (GPL family) require legal review before adoption in proprietary software.
- Dependencies with unknown or ambiguous licenses must be flagged for manual review before production use.

## 5. Audit Readiness

- Compliance evidence must be maintained in a state suitable for audit at any time.
- Evidence locations must be specific enough for an auditor to retrieve the evidence independently.
- Evidence must not depend on the availability of a specific individual — it must be retrievable by any authorized team member.
- Compliance records must be retained for the period required by the applicable regulation (or organizational policy if no regulation specifies).
