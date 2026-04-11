# Security Principles

Version: v1.0

This document defines the organization's security standards and philosophy. It is input material for artifact generation within the Security & Compliance Kit — not a governed artifact. Adapt this file to reflect your organization's actual security policies.

---

## 1. Threat Modeling

- Every system with external interfaces or sensitive data handling must have a threat model before release.
- Threat models must be based on the system's actual architecture (SAD), not on generic templates.
- Threat actors must be relevant to the system's deployment context, data sensitivity, and user base.
- All identified threats must have a documented mitigation or an explicit, authorized risk acceptance.

## 2. Secure Development

- Input validation is required at all trust boundaries.
- Authentication and authorization must be verified for every user-facing and API endpoint.
- Secrets must not be stored in source code, configuration files committed to version control, or client-side code.
- Dependencies must be audited for known vulnerabilities before production deployment.
- Security-relevant code changes require review by a team member with security domain knowledge.

## 3. Vulnerability Management

- Critical vulnerabilities must be remediated within 7 calendar days of discovery.
- High vulnerabilities must be remediated within 30 calendar days of discovery.
- Medium and Low vulnerabilities must be tracked and remediated within 90 calendar days.
- Vulnerabilities that cannot be remediated within the target window require documented risk acceptance with authorization.

## 4. Data Protection

- Data must be classified by sensitivity: Public, Internal, Confidential, Restricted.
- Data at rest classified as Confidential or Restricted must be encrypted.
- Data in transit must be encrypted using TLS 1.2 or higher.
- PII must be handled in accordance with applicable data protection regulations.
- Data retention periods must be defined and enforced.

## 5. Supply Chain Security

- All production dependencies must be sourced from reputable registries.
- Dependencies with known Critical or High vulnerabilities must not be deployed to production without documented risk acceptance.
- License compatibility must be verified before adopting new dependencies.
- Unmaintained dependencies (no commits in 12+ months, no response to security issues) must be flagged for replacement.
