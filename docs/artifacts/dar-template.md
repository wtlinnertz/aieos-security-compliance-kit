# Dependency Audit Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| DAR ID | DAR-{PROJECT}-{NNN} |
| Project Name | {project name} |
| Owner | {team or role — not an individual person} |
| Version | v{N} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Audit Date | {YYYY-MM-DD} |
| Manifest Source | {e.g., package.json, requirements.txt, go.mod} |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Dependency Inventory

**Summary:**

| Category | Count |
|----------|-------|
| Direct dependencies | {N} |
| Transitive dependencies | {N} |
| Total | {N} |

### Direct Dependencies

| Name | Version | Purpose |
|------|---------|---------|
| {dependency name} | {version} | {what it is used for} |
| {dependency name} | {version} | {what it is used for} |

### Transitive Dependencies

| Name | Version | Parent Dependency |
|------|---------|-------------------|
| {dependency name} | {version} | {direct dependency that pulls it in} |
| {dependency name} | {version} | {direct dependency that pulls it in} |

*All direct dependencies must be listed individually. For large transitive dependency trees, a summary with counts per category and a link to the full manifest is acceptable.*

---

## 3. Vulnerability Assessment

**Scan Methodology:** {vulnerability database used, scan tool, scan date}

### CVE Findings

| CVE ID | Affected Dependency | Version | Severity | Description | Patch Available |
|--------|-------------------|---------|----------|-------------|----------------|
| {CVE-YYYY-NNNNN} | {dependency name} | {version} | {Critical / High / Medium / Low} | {brief description} | {Yes — version X.Y.Z / No} |
| {CVE-YYYY-NNNNN} | {dependency name} | {version} | {Critical / High / Medium / Low} | {brief description} | {Yes — version X.Y.Z / No} |

**Severity Summary:**

| Severity | Count |
|----------|-------|
| Critical | {N} |
| High | {N} |
| Medium | {N} |
| Low | {N} |

*If no CVEs found, write: "No known CVEs found affecting project dependencies. Scan performed using {database} on {YYYY-MM-DD}."*

---

## 4. License Compliance

### License Inventory

| Dependency | License | Compatibility |
|-----------|---------|---------------|
| {dependency name} | {SPDX identifier, e.g., MIT, Apache-2.0} | {Compatible / Restricted / Incompatible / Unknown} |
| {dependency name} | {SPDX identifier} | {Compatible / Restricted / Incompatible / Unknown} |

**License Distribution Summary:**

| License Type | Count |
|-------------|-------|
| {license type} | {N} |
| {license type} | {N} |

### Incompatible Licenses

| Dependency | License | Incompatibility Reason |
|-----------|---------|----------------------|
| {dependency name} | {license} | {why it is incompatible with project license or org policy} |

*If no incompatible licenses, write: "No incompatible licenses identified."*

### Unknown Licenses

| Dependency | Review Owner | Target Resolution Date |
|-----------|-------------|----------------------|
| {dependency name} | {team or role} | {YYYY-MM-DD} |

*If no unknown licenses, write: "All dependency licenses identified."*

---

## 5. Supply Chain Risk Classification

| Dependency | Risk Level | Maintenance | Provenance | Criticality | Adoption | Justification |
|-----------|-----------|-------------|------------|-------------|----------|---------------|
| {dependency name} | {Low / Medium / High / Critical} | {Active / Unmaintained / Archived} | {Known publisher / Community / Unknown} | {Core / Utility / Dev-only} | {Widely used / Niche / New} | {brief justification} |

### High/Critical Risk Responses

| Dependency | Risk Level | Risk Response |
|-----------|-----------|---------------|
| {dependency name} | {High / Critical} | {response: monitor, replace, vendor, accept with justification} |

*If no High/Critical risk dependencies, write: "No dependencies classified as High or Critical supply chain risk."*

---

## 6. Remediation Plan

| Reference | Remediation Action | Owner | Target Date |
|-----------|--------------------|-------|------------|
| {CVE ID or risk reference} | {Upgrade / Patch / Replace / Accept} | {team or role} | {YYYY-MM-DD} |

*For accepted risks:*

| Reference | Justification | Accepted By |
|-----------|--------------|-------------|
| {CVE ID or risk reference} | {why this risk is accepted} | {approver — team or role} |

*If no remediation needed, write: "No Critical or High severity vulnerabilities or risks requiring remediation."*

---

## 7. Audit Summary

**Overall Verdict:** {Pass / Conditional Pass / Fail}

**Summary Statistics:**

| Metric | Value |
|--------|-------|
| Total Dependencies | {N} |
| Total CVEs (Critical) | {N} |
| Total CVEs (High) | {N} |
| Total CVEs (Medium) | {N} |
| Total CVEs (Low) | {N} |
| License Issues | {N} |
| High/Critical Risk Dependencies | {N} |

**Verdict Justification:** {basis for the verdict, consistent with findings above}

*For Conditional Pass:*

| Condition | Owner | Deadline |
|-----------|-------|----------|
| {condition description} | {team or role} | {YYYY-MM-DD} |
