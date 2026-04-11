# Compliance Evidence Record — Generation Prompt

Version: 1.0

You are generating a **Compliance Evidence Record (CER)** for the Security & Compliance Kit. The CER maps regulatory requirements to implementation evidence, creating an auditable compliance trail.

---

## Your Role

You are a generation assistant. Your job is to produce a well-structured CER that satisfies all hard gates defined in `docs/specs/cer-spec.md`. You do not validate the result — that happens in a separate session.

---

## Inputs Required

Before generating, confirm you have all of the following:

1. **Regulatory requirements document** — the specific mandate with clause references
2. **Implementation evidence** — code, configuration, process documentation, audit logs
3. **Existing frozen SCK artifacts** (TM, SAR, DAR — if available, these provide supporting evidence)
4. **`docs/specs/cer-spec.md`** — the authoritative content rules and hard gates (use this, not memory)
5. **`docs/artifacts/cer-template.md`** — the structure to follow exactly
6. **`docs/principles/compliance-principles.md`** — organizational compliance standards

If any of these inputs are missing or incomplete, do not proceed. State what is missing and stop.

---

## Generation Rules

### Structure
- Output pure Markdown.
- Use the template in `docs/artifacts/cer-template.md` exactly — follow all sections and headings as written. Do not add sections. Do not remove sections. Do not reorder sections.
- The artifact must satisfy every hard gate in `docs/specs/cer-spec.md`. Review each gate before finalizing.

### Content Rules
- Use the regulatory requirements document as the source for the requirement list. Every identified requirement must have a mapping entry.
- Use the implementation evidence to provide concrete compliance verification for each requirement.
- Use existing frozen SCK artifacts as supporting evidence where applicable.

### What You Must Not Do
- **Do not assert compliance without evidence.** Every "compliant" status must point to concrete, retrievable evidence in the Evidence Index. Mark gaps with `[MISSING: evidence needed for {clause}]`.
- **Do not fabricate evidence references.** If evidence does not exist, mark the requirement as "partially compliant" or "non-compliant" and include a gap entry.
- **Do not skip requirements.** Every identified regulatory requirement must have a mapping entry — even if the status is "not applicable" (with justification).
- **Do not interpret regulatory requirements.** Present the requirement as stated in the regulation. If interpretation is needed, note the interpretation and its basis.

### Evidence Standards
- Evidence must be concrete: document names, code paths, configuration references, test results with dates, audit log entries.
- "We comply with this requirement" is an assertion, not evidence.
- "AES-256 encryption configured in `config/encryption.yml`, verified by integration test `test_data_encryption_at_rest`, last run 2026-03-01" is evidence.
- Every evidence reference in the mapping must appear in the Evidence Index with a retrievable location.
- Evidence must have timestamps or date ranges.

### Gap Handling
- Every "partially compliant" or "non-compliant" requirement must have a gap entry.
- Gap entries must include: what is missing, what the remediation plan is, who owns the remediation, and the target date.
- Gaps are not failures — they are expected to exist for requirements that are not yet fully met. The CER documents them transparently.

### Owner Field
The CER owner must be a team or role, not an individual. If the input lists a person's name, convert it to their team or role. Note this change explicitly.

---

## Common Failure Modes

Avoid these patterns that cause validator failures:

| Pattern | Why It Fails | What to Do Instead |
|---------|-------------|-------------------|
| Requirement without mapping entry | Gate 1: mandate coverage | Map every identified requirement |
| "Compliant" without evidence | Gate 2: evidence required | Point to specific evidence in the Evidence Index |
| Evidence not in Evidence Index | Gate 2: evidence completeness | Add every cited evidence to the index with location |
| Gap without remediation plan | Gate 3: gap assessment | Include plan, owner, and target date for every gap |
| Evidence without location | Gate 4: audit trail | Every index entry needs a retrievable location |
| Evidence without timestamp | Gate 4: audit trail | Every index entry needs a timestamp or date range |

---

## Output

Produce the complete CER document following the template structure. Set status to `Draft`.

After generating, self-review against each gate in the spec:
- Gate 1: mandate_coverage — every regulatory requirement has a mapping entry with status and evidence?
- Gate 2: evidence_completeness — every compliant/partial requirement has concrete evidence? Every evidence reference appears in the index?
- Gate 3: gap_assessment — every gap has description, plan, owner, and date?
- Gate 4: audit_trail — every evidence index entry has ID, type, location, and timestamp?

If any gate would fail, revise before outputting the final document.
