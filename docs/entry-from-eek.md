# Entry from Engineering Execution Kit (EEK)

**You are here because:** The System Architecture Document (SAD) is frozen, and/or implementation is code-complete and requires security assessment.

**What you must bring:**

| Artifact | Status Required | Where to Find It | Which SCK Artifact It Feeds |
|----------|----------------|-----------------|----------------------------|
| System Architecture Document (SAD) | Frozen | EEK `docs/artifacts/` in the consuming project | TM (Threat Model) |
| Architecture Context Framework section 3 (ACF section 3) | Frozen | EEK `docs/artifacts/` in the consuming project | TM, SAR |
| Implementation code / TDD | Code complete | Project repository | SAR |
| Dependency manifest | Current | Project repository (package.json, requirements.txt, etc.) | DAR |

**Not all inputs are needed at the same time.** This kit uses trigger-based activation:

- **SAD frozen?** Generate the Threat Model (TM).
- **Code complete?** Generate the Security Assessment Record (SAR) and/or Dependency Audit Record (DAR).
- **Compliance mandate identified?** Generate the Compliance Evidence Record (CER) — this does not require EEK artifacts.

**First artifact to generate:** Threat Model (TM) — triggered by SAD freeze

**Where to start:** `docs/playbook.md` — Trigger A (Threat Model)

**What changes at this boundary:**

- You shift from building the system to assessing its security posture. The focus moves from "does it work?" to "can it be attacked?"
- The TM is generated from the frozen SAD — the architecture as designed, not as aspirationally planned. If the SAD is incomplete, the TM will have coverage gaps.
- Security assessment (SAR) requires both a frozen TM and completed implementation. Do not attempt to generate the SAR until both prerequisites are met.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Generating TM from a draft SAD | The SAD must be frozen. A draft SAD may change, invalidating the threat model. |
| Skipping TM and going straight to SAR | The SAR verifies TM mitigations are implemented. Without a frozen TM, the SAR has no mitigation list to verify against. |
| Treating ACF section 3 as optional for SAR | ACF section 3 defines the security guardrails. The SAR verifies compliance with these guardrails. Without them, the guardrail_verification gate will fail. |
| Generating DAR without full transitive dependency tree | The DAR requires the complete dependency tree, not just direct dependencies. Run the full dependency resolution command. |

**If you arrived here without a complete upstream artifact:**

Stop. Return to EEK, complete and freeze the SAD, and then re-enter SCK. The Threat Model cannot provide meaningful attack surface coverage without a complete system architecture. A pointer to incomplete SAD content does not satisfy the entry requirement.

---

*For the full entry flow, see `docs/playbook.md`.*
