# Dependency Audit Record — Specification

Version: v1.0

The Dependency Audit Record (DAR) audits project dependencies for known vulnerabilities, license compliance, and supply chain risk. It can be generated at any point in the delivery lifecycle; it is recommended before release.

---

## What This Artifact Is Not

- **Not a bill of materials (BOM) generator.** The DAR analyzes and assesses dependencies — it does not merely list them. A raw dependency tree is an input, not the output.
- **Not a vulnerability scanner.** The DAR documents and assesses vulnerability scan results. The scanning itself is performed by external tools whose output feeds into the DAR.
- **Not a license agreement.** The DAR identifies and classifies licenses for compliance assessment. Legal interpretation of license terms is outside the DAR's scope.

---

## Purpose

The DAR serves four roles:

1. **Dependency inventory** — Provides a complete accounting of all direct and transitive dependencies
2. **Vulnerability assessment** — Maps known CVEs to dependencies with severity ratings and remediation plans
3. **License compliance** — Identifies all licenses and flags incompatible or restricted licenses
4. **Supply chain risk** — Classifies dependencies by supply chain risk level based on maintenance, provenance, and criticality

---

## Upstream Dependencies

- Project dependency manifest (complete dependency tree including transitive dependencies)
- Vulnerability database scan results (CVE database output)
- License analysis output (automated or manual license identification)
- Organizational principles: `compliance-principles.md` — defines acceptable licenses and vulnerability thresholds

---

## Required Sections

1. Document Control
2. Dependency Inventory
3. Vulnerability Assessment
4. License Compliance
5. Supply Chain Risk Classification
6. Remediation Plan
7. Audit Summary

---

## Content Rules

### Document Control
**Rules**
- DAR ID must be present (format: DAR-{PROJECT}-{NNN})
- Project name must be present
- Owner must be present as a team or role, not an individual
- Version must be present (format: v{N})
- Status must be one of: Draft | Validated | Frozen
- Audit date must be present
- Dependency manifest source must be stated (e.g., package.json, requirements.txt, go.mod)

**Failure Examples**
- DAR ID absent
- Audit date absent
- Manifest source absent
- Owner listed as an individual person's name

### Dependency Inventory
**Rules**
- All direct dependencies must be listed with: name, version, type (direct or transitive), and purpose (what it is used for)
- All transitive dependencies must be listed with: name, version, and the direct dependency that pulls them in
- Total dependency count must be stated (direct and transitive separately)
- If the dependency tree is too large to list individually, a summary with counts per category and a link to the full manifest is acceptable, but direct dependencies must always be listed individually

**Failure Examples**
- Only direct dependencies listed
- Dependencies listed without versions
- Transitive dependencies not traced to their parent
- Total count absent

### Vulnerability Assessment
**Rules**
- All known CVEs affecting any dependency must be listed
- Each CVE entry must state: CVE ID, affected dependency (name and version), severity rating (Critical, High, Medium, Low — using CVSS or equivalent), description, and whether a patched version is available
- If no CVEs are found, this must be explicitly stated with the vulnerability database and scan date cited
- Severity ratings must use a recognized scoring system (CVSS, or organizational equivalent)

**Failure Examples**
- CVEs mentioned but not individually listed
- CVE entries without severity ratings
- "No known vulnerabilities" without scan methodology reference
- Severity ratings without scoring system reference

### License Compliance
**Rules**
- Every dependency must have its license identified
- Each license entry must state: the dependency name, the license type (e.g., MIT, Apache-2.0, GPL-3.0, BSD-2-Clause), and the compatibility assessment (compatible, restricted, incompatible, unknown)
- Incompatible licenses must be flagged with an explanation of why they are incompatible with the project's license or organizational policy
- Unknown licenses must be flagged for manual review with an owner and target resolution date
- A summary of license distribution must be provided (count per license type)

**Failure Examples**
- Dependencies without license identification
- Licenses identified but not assessed for compatibility
- Incompatible licenses not flagged
- Unknown licenses without review plan

### Supply Chain Risk Classification
**Rules**
- Every direct dependency must be classified by supply chain risk level: Low, Medium, High, Critical
- Classification must consider: maintenance status (actively maintained, unmaintained, archived), provenance (known publisher, community project, unknown origin), criticality to the project (core functionality, utility, development-only), and popularity/adoption (widely used, niche, new)
- Each classification must include a brief justification
- Dependencies classified as High or Critical risk must have a risk response documented

**Failure Examples**
- Dependencies without risk classification
- Classification without justification
- High/Critical risk dependencies without risk response
- Only direct dependencies classified (transitive high-risk dependencies must also be flagged)

### Remediation Plan
**Rules**
- Every Critical and High severity vulnerability must have a remediation entry
- Each remediation entry must state: the CVE ID or risk reference, the remediation action (upgrade, patch, replace, accept), the remediation owner (team or role), and the target date
- Accepted risks must include justification and approver
- If no remediation is needed, this must be explicitly stated

**Failure Examples**
- Critical/High vulnerabilities without remediation plan
- Remediation entries without owners
- Remediation entries without target dates
- Accepted risks without justification

### Audit Summary
**Rules**
- Overall audit verdict must be stated: Pass (acceptable risk level), Conditional Pass (acceptable with noted conditions), or Fail (unacceptable risk level)
- Summary statistics must be stated: total dependencies, total CVEs by severity, total license issues, total high/critical risk dependencies
- If Conditional Pass: conditions must be listed with owners and deadlines
- The verdict must be consistent with the findings

**Failure Examples**
- Verdict absent
- Summary statistics absent
- Verdict contradicts findings

---

## Format Requirements

- CVE IDs must follow the standard format (CVE-YYYY-NNNNN)
- Severity ratings must use the enumerated scale: Critical, High, Medium, Low
- License types must use SPDX identifiers where available
- Risk levels must use the enumerated scale: Low, Medium, High, Critical
- All dates must be in ISO 8601 format (YYYY-MM-DD)

---

## Completeness Rules

- All seven sections must be present and non-empty
- All direct and transitive dependencies listed
- All known CVEs documented
- All licenses identified and assessed
- All direct dependencies risk-classified
- Critical/High vulnerabilities have remediation plans

---

## Hard Gates

1. **dependency_inventory** — All direct dependencies listed with name, version, type, and purpose; all transitive dependencies listed with parent dependency; total count stated
2. **vulnerability_assessment** — All known CVEs listed with CVE ID, affected dependency, severity rating, description, and patch availability; if no CVEs found, scan methodology and date cited; severity uses recognized scoring system
3. **license_compliance** — Every dependency has license identified; each license has compatibility assessment; incompatible licenses flagged with explanation; unknown licenses have review plan with owner and date; license distribution summary provided
4. **risk_classification** — Every direct dependency classified by supply chain risk level with justification considering maintenance, provenance, criticality, and adoption; High/Critical risk dependencies have documented risk response
5. **remediation_plan** — Every Critical and High severity vulnerability has remediation entry with action, owner, and target date; accepted risks have justification and approver; if no remediation needed, explicitly stated
