# Introduction

## Purpose

The OWASP Autonomous Penetration Testing Standard (APTS) defines governance requirements for autonomous pentesting platforms to operate safely, transparently, and within defined boundaries.

This is a governance framework, not a testing methodology. It complements existing standards (PTES, OWASP WSTG, OSSTMM) by addressing challenges unique to autonomous operation: scope enforcement, safe autonomy, manipulation resistance, and accountability.

## Scope

APTS applies to autonomous pentesting systems that:

- Make decisions about targeting, methodology, or exploitation without human intervention
- Test against production or production-like systems
- Have the potential to cause unintended impact or data exposure
- Require governance oversight and accountability

This includes SaaS platforms, on-premises tools, cloud-native systems, integrated security orchestration platforms with autonomous testing capabilities, and in-house platforms that enterprise security teams build and operate for testing their own organization.

APTS does not cover manual pentesting, isolated lab testing, SAST/DAST tools, bug bounty programs, human-led red team exercises, purple team operations (unless the autonomous tool is the attacker component), or vulnerability disclosure programs.

### Deployment Models

APTS requirements are deployment-model agnostic and apply identically to the three common ways autonomous pentest platforms are delivered and operated:

1. **Vendor-delivered platform.** A product vendor builds the platform and ships it to customers as SaaS or on-premises software. The vendor assesses the platform against APTS and publishes conformance claims to customers.
2. **Service-operated platform.** A service provider (consultancy, MSSP, or managed red-team service) operates an autonomous platform on behalf of client engagements. The service provider assesses the platform and stands behind its conformance in service agreements.
3. **In-house enterprise platform.** An enterprise security team builds or integrates an autonomous platform for testing its own organization's systems. The security team assesses the platform against APTS as its internal governance baseline, and MAY publish its conformance claim to downstream customers as a trust signal in trust center pages, security whitepapers, customer security questionnaires, or SOC 2 complementary user entity control narratives.

APTS does not prescribe who performs the assessment. The choice of internal self-assessment, independent internal review, or third-party assessment is left to the reader based on regulatory context, customer contracts, and assurance needs. The same 173 tier-required requirements and three tiers apply across all three models. For in-house deployments, the "customer" referenced in requirements such as Rules of Engagement validation, scope approval, and reporting is the internal business unit or application team requesting the test rather than an external client; the requirement itself is unchanged.

---

## The Eight Domains

| # | Domain | Prefix | Active | Focus |
|---|--------|--------|--------|-------|
| 1 | Scope Enforcement | SE | 26 | Boundary definition, validation, continuous monitoring |
| 2 | Safety Controls & Impact Management | SC | 20 | Impact classification, blast radius, hard stops, rollback, execution sandbox |
| 3 | Human Oversight & Intervention | HO | 19 | Approval gates, kill switch, escalation, operator qualifications |
| 4 | Graduated Autonomy Levels | AL | 28 | L1 Assisted through L4 Autonomous, level assessment, containment verification |
| 5 | Auditability & Reproducibility | AR | 20 | Logging, decision trails, evidence integrity, audit trail isolation |
| 6 | Manipulation Resistance | MR | 23 | Prompt injection, scope widening, model poisoning, agent runtime isolation |
| 7 | Third-Party & Supply Chain Trust | TP | 22 | AI providers, cloud dependencies, data handling, foundation model disclosure |
| 8 | Reporting | RP | 15 | Finding validation, confidence scoring, coverage disclosure |

**Total: 173 tier-required requirements** (Tier 1 + Tier 2 + Tier 3) across the eight domains. An additional **20 advisory practices** live exclusively in the [Advisory Requirements](appendix/Advisory_Requirements.md) appendix using the `APTS-<DOMAIN>-A0x` identifier pattern; advisory practices are not counted toward any tier and do not affect conformance.

---

## Compliance Tiers

APTS defines three compliance tiers. A platform claims a tier by implementing every MUST requirement assigned to that tier and all lower tiers, with no deviation, and by either implementing every SHOULD requirement at those tiers or recording a documented justification for each deviation in its conformance claim. An unimplemented MUST requirement or an undocumented SHOULD deviation is a conformance gap; there is no partial credit.

**Verification model:** APTS follows a conformance assessment model, consistent with how other OWASP standards (WSTG, ASVS) are used by practitioners. Platform operators evaluate their platforms against the requirements using the [Checklists](appendix/Checklists.md) and document conformance. The [Conformance Claim Template](appendix/Conformance_Claim_Template.md) provides an optional format for publishing evidence of conformance. Customers MAY independently verify claims using the [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) or the [Customer Acceptance Testing](appendix/Customer_Acceptance_Testing.md) appendix for hands-on verification of behavioral requirements (kill switch response times, scope enforcement accuracy, manipulation resistance) that cannot be verified through documentation alone.

### Tier 1: Foundation (72 requirements)

**What it means for customers:** The platform will not test outside the agreed scope, can be stopped immediately if something goes wrong, will not store or leak discovered credentials in plaintext, and provides a basic audit trail of everything it did. A Tier 1 platform is safe to deploy for supervised autonomous testing against non-critical systems.

**What it means for platform operators:** You have implemented the baseline safety controls that any responsible autonomous pentest tool must have. Your platform enforces scope boundaries before every action, provides working kill switches, requires human approval for high-risk actions, logs all operations, and protects sensitive data found during testing.

All requirements marked `MUST | Tier 1` across all eight domains. Covers: scope boundary enforcement, impact classification, kill switch, approval gates, structured logging, basic manipulation resistance, provider vetting, credential protection.

### Tier 2: Verified (85 additional, 157 cumulative)

**What it means for customers:** The platform is transparent about what it did and why, protects your data with tamper-proof audit trails, handles incidents with formal response procedures, and provides findings you can independently verify. A Tier 2 platform is suitable for production environments, regulated industries, and engagements where you need full accountability.

**What it means for platform operators:** You have implemented production-grade governance. Your platform produces auditable decision trails, enforces multi-tenant data isolation, tracks cumulative risk, manages third-party dependencies, and delivers reproducible, evidence-backed findings with confidence scores and false positive rate disclosure.

All Tier 1 plus all `MUST | Tier 2` and `SHOULD | Tier 2` requirements. Adds: tamper-proof audit trails, decision transparency, anti-manipulation controls, reproducible findings, formal incident response, third-party dependency management, L3 operational requirements.

### Tier 3: Comprehensive (16 additional, 173 cumulative)

**What it means for customers:** The platform meets the highest assurance bar for critical infrastructure, fully autonomous (L4) operations, and environments with the strictest regulatory requirements. A Tier 3 platform has been validated against advanced adversarial scenarios and provides maximum transparency into its operation.

**What it means for platform operators:** You have implemented the full standard including advanced controls for continuous platform integrity monitoring, evasion governance, L4 autonomous campaign management, and comprehensive forensic reconstruction capability.

All Tier 2 plus all `MUST | Tier 3` and `SHOULD | Tier 3` requirements. Adds: continuous monitoring, platform integrity verification, advanced manipulation resistance, L4 operational requirements, red team validation.

### Advisory Requirements

In addition to the three compliance tiers, APTS designates a small number of requirements as **Advisory**. These are practices that provide meaningful additional assurance but were assessed as disproportionately burdensome relative to their safety value at their original tier, or as infeasible to verify with current technology (for example, full replay of LLM-based decision chains). Advisory requirements use the identifier pattern `APTS-<DOMAIN>-A0x` and are documented exclusively in the [Advisory Requirements](appendix/Advisory_Requirements.md) appendix; they do not appear in the domain READMEs.

Advisory requirements are **not required for conformance at any tier** but are **recommended for high-risk engagements**, regulated industries, and platforms operating at Level 4 autonomy. Platforms that implement advisory requirements MAY document their adoption in vendor evaluation materials. Customers seeking maximum assurance SHOULD evaluate vendor adoption of advisory requirements as a differentiator.

---

## How to Use This Standard

New readers should start with the [Getting Started](Getting_Started.md) guide for a recommended reading order.

**Platform operators** (vendors, service providers, and enterprise security teams): Use the Implementation Guides and [Checklists](appendix/Checklists.md) to understand what each requirement expects. Use the [Cross-Domain Integration Matrix](appendix/Cross_Domain_Integration.md) to understand domain interactions and the [Testing Phase Mapping](appendix/Testing_Phase_Mapping.md) to verify pentesting phase coverage.

**Customers evaluating platforms**: Use tier requirements as evaluation criteria. The [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) provides a structured approach with seven critical evaluation questions. For customers who want hands-on verification, the [Customer Acceptance Testing](appendix/Customer_Acceptance_Testing.md) appendix provides optional verification procedures.

**Security professionals**: Each requirement includes a Verification section with specific criteria. Implementation Guides provide supplemental context; the domain READMEs are the authoritative source.

### Interpreting Prescriptive Values

Some requirements include specific numeric values (for example, payload size limits, response time thresholds, confidence percentages, retention periods). These values fall into three categories:

1. **Safety-critical bounds** (for example, kill switch response times, process termination timeouts): These are maximum acceptable values. Implementations MUST meet or exceed these bounds. Deviation is not permitted; slower values create unacceptable safety risk.
2. **Regulatory minimums** (for example, log retention periods citing HIPAA or SOX): These reflect legal obligations in specific jurisdictions. Implementations MUST comply with applicable regulations regardless of APTS values.
3. **Environment-dependent defaults** (for example, payload size limits, bandwidth caps, scan rate limits): These are calibrated starting points for common deployment scenarios. Deviation is permitted only when all of the following conditions are met:
   - (a) The deviation is documented with specific technical rationale
   - (b) The alternative value is validated through testing to confirm it does not reduce safety effectiveness
   - (c) The deviation is approved through the platform's change management process

When a requirement specifies a numeric value without indicating whether it is a bound, a minimum, or a default, treat it as a bound unless the requirement text explicitly uses language such as "configurable," "documented," or "per the platform's policy."

---

## Relationship to Other Standards

This standard is complementary to existing standards:

- **Testing methodology** (PTES, WSTG, OSSTMM): autonomous systems should follow these while operating under APTS governance
- **Application security** (OWASP ASVS, OWASP Top 10): autonomous systems should test against these
- **Governance frameworks** (NIST CSF, ISO 27001, NIST AI RMF, SOC 2, PCI DSS, GDPR): APTS requirements align with these; see [Compliance Matrix](appendix/Compliance_Matrix.md) for specific version mappings
- **AI agent security** ([OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/), [Singapore IMDA Agentic AI Governance Framework](https://www.imda.gov.sg/resources/press-releases-factsheets-and-speeches/press-releases/2026/new-model-ai-governance-framework-for-agentic-ai)): APTS addresses agentic AI risks specific to pentesting
- **AI regulation** (EU AI Act): Platforms operating in EU markets should consider EU AI Act transparency and human oversight obligations alongside APTS requirements. APTS conformance does not constitute EU AI Act compliance, but many APTS requirements (particularly in the Human Oversight, Auditability, and Reporting domains) address concerns that overlap with EU AI Act obligations. Organizations should consult legal counsel for specific EU AI Act compliance determination

---

## Capability Frontier and Containment Assumptions

Autonomous pentest platforms are built on foundation models whose capabilities are advancing quickly. Each new generation of models tends to find and exploit vulnerabilities more effectively, and published safety evaluations continue to surface behaviors in frontier models that would have been considered unlikely a year earlier. APTS treats this not as a distant research concern but as a present engineering constraint on how platforms should be built.

APTS does not try to verify that a foundation model is aligned, cannot scheme, or will not attempt to exceed its mandate. These properties cannot be reliably verified by platform operators today, and a standard that claimed otherwise would give customers false assurance. APTS instead takes an architectural stance: treat the agent runtime as a component that may, for any reason, attempt actions outside its declared mandate, and build the platform so that such attempts fail by construction rather than by the agent's own restraint.

Several requirements in this standard exist specifically to encode that stance. APTS-SC-019 requires a kernel-enforced execution sandbox whose boundary the agent runtime has no credentials to move. APTS-SC-020 requires the tool and action allowlist to be enforced by a component external to the model rather than by the model's system prompt. APTS-MR-023 names the agent runtime as an untrusted component in the platform's threat model. APTS-AR-020 requires the authoritative audit trail to live on infrastructure the agent runtime cannot reach. APTS-AL-028 requires periodic operator-run containment verification for platforms operating at L3 or L4. APTS-TP-021 and APTS-TP-022 require the operator to disclose the foundation model powering the agent and to re-assess the platform when that model changes materially. APTS-SE-026 recommends monitoring the agent's action distribution for deviations that fall within scope but outside the operator's expected behavior.

These requirements share a common assumption: as foundation model capability grows, the gap between what the agent *can* do and what the operator *intends* for it to do grows with it, and that gap is best closed by architecture rather than by promises extracted from the model. The standard's treatment of this frontier is deliberately modest in scope. It does not try to address every open question about agent safety. Research-stage topics (verifiable goal alignment, scheming detection, and containment testing against models that may be aware of the test environment) are out of scope for normative requirements in this version; selected advisory practices provide initial measurement guidance. These topics may be addressed as tier-gated requirements in future versions of APTS as the field matures.
