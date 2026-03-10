# How to Adapt This Kit

This kit provides the structure, rules, and prompts for security and compliance governance. Adapting it to your organization means filling in the content that is specific to your context — without modifying the governance structure.

---

## What to Adapt

### Organizational Principles

**Files:** `docs/principles/security-principles.md`, `docs/principles/compliance-principles.md`

These files define your organization's security and compliance philosophy. Adapt them to reflect:

- **Threat modeling methodology**: Does your organization use STRIDE, PASTA, attack trees, or a custom methodology? What threat actor categories are relevant to your domain?
- **Security assessment standards**: What constitutes sufficient evidence for a security verification? Are penetration test results required? What vulnerability severity thresholds block release?
- **Compliance framework**: Which regulatory frameworks apply (SOC 2, GDPR, HIPAA, PCI-DSS, FedRAMP)? What is the evidence standard for each?
- **Dependency management policy**: What is the maximum acceptable vulnerability severity in production dependencies? What license types are prohibited?

Replace the default policies with your organization's actual policies. Keep the structure (numbered sections, clear policy statements) but change the content to match your standards.

### Trigger Points

The playbook defines four trigger points. If your organization has different activation criteria (for example, if threat modeling is required at design time rather than after SAD freeze), update the playbook trigger descriptions accordingly.

### Severity Thresholds

The TM and SAR specs reference severity levels for threats and vulnerabilities. If your organization uses a different severity taxonomy, adapt the specs accordingly. Do not remove severity classification — adapt the levels.

---

## What Not to Adapt

### Specs

The specs define what makes an artifact valid. Do not soften hard gates to make artifacts easier to produce. If a hard gate is failing consistently, that usually means the artifact is incomplete — not that the gate is wrong.

If you genuinely need to add a hard gate (your organization requires something the spec does not check), add it. Do not remove existing hard gates.

### Validator Logic

Validators evaluate against specs. If a validator is producing unexpected results, check whether the spec accurately captures your requirements — and adjust the spec if needed, not the validator.

### Governance Model

`docs/governance-model.md` is a synchronized copy of the canonical governance model. Do not edit it. If you believe the governance model should change, update `aieos-governance-foundation/governance-model.md` and sync all kit copies.

---

## Adding Artifact Types

If your organization needs additional governed artifacts (e.g., a penetration test record, a privacy impact assessment), follow the four-file system:

1. Write the spec first — define the hard gates before writing anything else
2. Write the validator — this forces you to verify the spec is evaluable
3. Write the template — structure only, no content rules
4. Write the prompt — generation behavior, references spec and template

Register the new artifact type in the playbook, index, and CLAUDE.md.

---

## Tool Bindings

This kit is tool-agnostic. Templates use generic placeholders for vulnerability scanners, SAST/DAST tools, and compliance platforms.

When adopting this kit, create a bindings document:

```
docs/bindings/
  vulnerability-scanner-mapping.md   # Maps vulnerability scan outputs to DAR format
  sast-dast-mapping.md               # Maps static/dynamic analysis tools to SAR format
  compliance-platform-mapping.md     # Maps compliance platform evidence to CER format
```

Bindings are not governed artifacts — they have no spec, validator, or prompt. Update them when your tooling changes without touching the governed files.

---

## Independent Adoption

This kit is designed to be adoptable independently. You do not need other AIEOS kits to use it. If you are not using the Engineering Execution Kit, provide equivalent inputs:

- For TM: any system architecture document that describes components, integration points, and data flows
- For SAR: any security guardrail document and implementation evidence
- For CER: regulatory requirements and implementation evidence
- For DAR: dependency manifest and vulnerability scan results

---

## First-Time Setup Checklist

- [ ] Read `docs/playbook.md` fully before beginning
- [ ] Review and adapt `docs/principles/` to match your organizational policies
- [ ] Identify which artifacts are relevant to your initiative (not all four may be needed)
- [ ] Determine trigger points for your delivery pipeline
- [ ] Identify the security owner and compliance owner
- [ ] Confirm upstream artifacts are available (SAD for TM, ACF section 3 for SAR)
