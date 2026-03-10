# Security & Compliance Kit — Examples

This directory will contain worked examples demonstrating the full artifact flow for each trigger point.

---

## Planned Examples

### Trigger A: Threat Model Flow
- Input: Frozen SAD for a sample web application
- Output: Generated TM with attack surfaces, threat actors, mitigations, and residual risks
- Validation: TM validator output (PASS)

### Trigger B: Security Assessment and Dependency Audit
- Input: Frozen TM + ACF section 3 + implementation evidence + dependency manifest
- Output: Generated SAR and DAR
- Validation: SAR and DAR validator outputs (PASS)

### Trigger C: Compliance Evidence
- Input: Regulatory requirements (sample SOC 2 controls) + implementation evidence
- Output: Generated CER with evidence chain
- Validation: CER validator output (PASS)

---

## How to Use Examples

Each example will include:
1. The input artifacts (frozen SAD, ACF, TM, etc.)
2. The generated artifact
3. The validator output JSON (in `validator-outputs/`)

Examples serve as reference implementations. They should pass all validators without modification.
