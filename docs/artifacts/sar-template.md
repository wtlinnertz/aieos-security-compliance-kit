# Security Assessment Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| SAR ID | SAR-{PROJECT}-{NNN} |
| System Name | {system name} |
| Owner | {team or role — not an individual person} |
| Version | v{N} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| TM Reference | {TM-{PROJECT}-{NNN}} |
| ACF Reference | {ACF version providing section 3 guardrails} |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Assessment Scope

**Components Assessed:**
- {component or service name}
- {component or service name}

**Assessment Methodology:**
- {code review / automated scanning / manual testing / configuration review}

**Assessment Date Range:** {YYYY-MM-DD} to {YYYY-MM-DD}

**Exclusions:**

| Excluded Component | Justification |
|-------------------|---------------|
| {component name} | {why it was excluded from assessment} |

*If no exclusions, write: "No components excluded from assessment."*

---

## 3. Guardrail Verification

### ACF section 3.{N}: {Guardrail Description}

| Field | Value |
|-------|-------|
| Guardrail Reference | ACF section 3.{N} |
| Verification Status | {Verified / Partial / Not Verified / Not Applicable} |
| Evidence | {specific code reference, test result, configuration setting, or documentation} |
| Notes | {additional context} |

### ACF section 3.{N}: {Guardrail Description}

| Field | Value |
|-------|-------|
| Guardrail Reference | ACF section 3.{N} |
| Verification Status | {Verified / Partial / Not Verified / Not Applicable} |
| Evidence | {specific code reference, test result, configuration setting, or documentation} |
| Notes | {additional context} |

*One entry per ACF section 3 guardrail. "Not Applicable" must include justification in Notes.*

---

## 4. Mitigation Verification

### T-{NNN}: {Mitigation Description from TM}

| Field | Value |
|-------|-------|
| TM Threat ID | T-{NNN} |
| TM Mitigation | {mitigation description from frozen TM} |
| Verification Status | {Implemented / Partial / Not Implemented / Deferred} |
| Evidence | {specific evidence of implementation} |

*For "Deferred" entries:*

| Field | Value |
|-------|-------|
| Deferral Reason | {why this mitigation is deferred} |
| Risk Accepted By | {team or role} |
| Target Implementation Date | {YYYY-MM-DD} |

*For "Partial" entries:*

| Field | Value |
|-------|-------|
| Implemented | {what is implemented} |
| Remaining | {what remains to be done} |
| Completion Plan | {plan to complete, with target date} |

*One entry per TM mitigation.*

---

## 5. Vulnerability Assessment

### V-001: {Vulnerability Description}

| Field | Value |
|-------|-------|
| Vulnerability ID | V-001 |
| Description | {vulnerability description} |
| Severity | {Critical / High / Medium / Low} |
| Affected Component | {component name} |
| Disposition | {Fixed / Mitigated / Accepted / Deferred} |
| Evidence | {evidence of fix, mitigation, or acceptance justification} |

### V-002: {Vulnerability Description}

| Field | Value |
|-------|-------|
| Vulnerability ID | V-002 |
| Description | {vulnerability description} |
| Severity | {Critical / High / Medium / Low} |
| Affected Component | {component name} |
| Disposition | {Fixed / Mitigated / Accepted / Deferred} |
| Evidence | {evidence of fix, mitigation, or acceptance justification} |

*If no vulnerabilities were found:*
*"No vulnerabilities discovered during assessment. Assessment methodology: {methodology reference}."*

---

## 6. Authentication and Authorization Review

**Authentication Flows:**

| Flow | Protocol | Credential Handling | Verification Status | Evidence |
|------|----------|-------------------|-------------------|----------|
| {flow name} | {protocol} | {how credentials are handled} | {Verified / Not Verified} | {evidence} |

**Authorization Model:**

| Mechanism | Description | Verification Status | Evidence |
|-----------|-------------|-------------------|----------|
| {mechanism} | {description} | {Verified / Not Verified} | {evidence} |

**Session Management:**

| Aspect | Implementation | Verification Status | Evidence |
|--------|---------------|-------------------|----------|
| Session creation | {description} | {Verified / Not Verified} | {evidence} |
| Session maintenance | {description} | {Verified / Not Verified} | {evidence} |
| Session termination | {description} | {Verified / Not Verified} | {evidence} |

---

## 7. Data Handling Review

**Data Classification:**

| Data Type | Classification | At Rest Protection | In Transit Protection |
|-----------|---------------|-------------------|----------------------|
| {data type} | {Public / Internal / Confidential / Restricted} | {protection method + evidence} | {protection method + evidence} |

**PII/Sensitive Data:**

| Data Type | Handling Verification | Evidence |
|-----------|----------------------|----------|
| {PII data type} | {how it is handled} | {evidence} |

**Data Retention and Disposal:**

| Policy Aspect | Description |
|--------------|-------------|
| Retention Period | {how long data is retained} |
| Disposal Method | {how data is disposed of} |
| Evidence | {evidence of policy implementation} |

---

## 8. Assessment Verdict

**Overall Verdict:** {Pass / Conditional Pass / Fail}

**Verdict Justification:** {basis for the verdict, consistent with findings above}

*For Conditional Pass:*

| Condition | Owner | Deadline |
|-----------|-------|----------|
| {condition description} | {team or role} | {YYYY-MM-DD} |

*For Fail:*

| Blocking Issue | Severity | Required Action |
|---------------|----------|----------------|
| {issue description} | {Critical / High} | {action required} |
