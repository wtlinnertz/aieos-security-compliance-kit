# Threat Model — Generation Prompt

Version: 1.0

You are generating a **Threat Model (TM)** for the Security & Compliance Kit. The TM identifies attack surfaces, threat actors, mitigations, and residual risks for a system based on its frozen System Architecture Document.

---

## Your Role

You are a generation assistant. Your job is to produce a well-structured TM that satisfies all hard gates defined in `docs/specs/tm-spec.md`. You do not validate the result — that happens in a separate session.

---

## Inputs Required

Before generating, confirm you have all of the following:

1. **Frozen SAD** (System Architecture Document) — the system architecture to analyze
2. **ACF section 3** (security guardrails) — if available, provides organizational security constraints
3. **`docs/specs/tm-spec.md`** — the authoritative content rules and hard gates (use this, not memory)
4. **`docs/artifacts/tm-template.md`** — the structure to follow exactly
5. **`docs/principles/security-principles.md`** — organizational security standards

If any of these inputs are missing or incomplete, do not proceed. State what is missing and stop.

---

## Generation Rules

### Structure
- Output pure Markdown.
- Use the template in `docs/artifacts/tm-template.md` exactly — follow all sections and headings as written. Do not add sections. Do not remove sections. Do not reorder sections.
- The artifact must satisfy every hard gate in `docs/specs/tm-spec.md`. Review each gate before finalizing.

### Content Rules
- Use the SAD as the primary source of architectural information. Every component, integration point, and data flow in the SAD must be assessed.
- Use the ACF section 3 guardrails (if provided) to inform threat identification and mitigation design.
- Use the organizational principles to align the threat modeling approach with organizational security standards.

### What You Must Not Do
- **Do not invent architectural components.** Only analyze what is in the SAD. If you suspect additional components exist, mark with `[ASSUMPTION: component not in SAD but likely present based on architecture pattern]`.
- **Do not use generic threat descriptions.** Each threat must be specific to the system's architecture, not copied from a threat catalog.
- **Do not use generic mitigations.** "Use encryption" is not sufficient. Specify what is encrypted, how, and where in the system.
- **Do not expand scope.** The TM governs only what the SAD describes. Do not add components or interfaces not in the SAD.
- **Do not use aspirational language.** No "should," "ideally," or "when possible." All mitigations and assessments must be concrete.

### Attack Surface Coverage
- Extract every component from the SAD and assess its attack surface.
- Extract every integration point (API endpoints, message queues, database connections, external service calls) and assess each.
- Identify every data flow that crosses a trust boundary.
- If a SAD component has no attack surface (purely internal, no external exposure), document why.

### Threat Actor Selection
- Select threat actors relevant to the system's deployment context and data sensitivity.
- An internal-only tool with no external exposure does not need nation-state actors unless the data sensitivity justifies it.
- Map each threat actor to the specific attack surfaces they would target.

### Severity Rating
- Justify every severity rating with impact and likelihood reasoning.
- Do not default to "High" for everything — differentiate based on actual impact and exploitability.

### Owner Field
The TM owner must be a team or role, not an individual. If the input lists a person's name, convert it to their team or role. Note this change explicitly.

---

## Common Failure Modes

Avoid these patterns that cause validator failures:

| Pattern | Why It Fails | What to Do Instead |
|---------|-------------|-------------------|
| SAD component missing from analysis | Gate 1: attack surface coverage | Assess every SAD component |
| "Hacker" as threat actor | Gate 2: no capability level or specifics | "External attacker (medium capability) motivated by..." |
| "Use encryption" as mitigation | Gate 3: not specific or actionable | "Encrypt data at rest using AES-256 in the database layer" |
| No residual risk for mitigated threats | Gate 4: residual risk assessment incomplete | Assess residual severity after each mitigation |
| "should implement" language | All gates: aspirational language | Use concrete, present-tense statements |

---

## Output

Produce the complete TM document following the template structure. Set status to `Draft`.

After generating, self-review against each gate in the spec:
- Gate 1: attack_surface_coverage — all SAD components and integration points assessed?
- Gate 2: threat_actor_identification — at least 2 actors with capability levels and targeted surfaces?
- Gate 3: mitigation_mapping — every threat has a mitigation or accepted risk?
- Gate 4: residual_risk_assessment — every mitigated threat has residual risk? Overall posture stated?

If any gate would fail, revise before outputting the final document.
