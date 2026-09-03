# Getting Started with APTS

This guide provides a recommended reading order and orientation for new readers of the OWASP Autonomous Penetration Testing Standard. It does not create requirements. All requirements are defined in the domain READMEs.

## Who Is This Standard For?

APTS serves three primary audiences:

- **Platform operators** (vendors, service providers, and enterprise security teams building or running internal platforms). Use APTS to design governance controls into your platform.
- **Customers** (CISOs, procurement teams, and security leaders) evaluating or deploying autonomous pentesting platforms. Use APTS as procurement criteria and ongoing governance requirements.
- **Security professionals** reviewing autonomous pentesting platforms. Use APTS requirements and verification procedures as a reference framework.

## Recommended Reading Order

### Step 1: Understand the Framework

Read the [Introduction](Introduction.md) first. It explains the standard's purpose, the eight domains, and compliance tiers. Pay particular attention to tier definitions, as they drive subsequent decisions.

### Step 2: Know the Vocabulary

Skim the [Glossary](appendix/Glossary.md) to familiarize yourself with APTS-specific terminology. Key terms include the four autonomy levels (L1 Assisted through L4 Autonomous, defined in the Graduated Autonomy domain), the four asset criticality categories (Critical, Production, Non-Production, Unknown, defined in APTS-SE-005), and the distinction between MUST, SHOULD, and MAY per RFC 2119.

### Step 3: Survey the Requirements

Read the [Checklists](appendix/Checklists.md) appendix for your target tier. This provides a compact view of every requirement organized by tier and domain. You do not need to understand each requirement deeply at this stage; the goal is to see the full scope.

### Step 3.5: Understand Cross-Domain Dependencies

Before reading individual domains, skim the [Cross-Domain Integration](appendix/Cross_Domain_Integration.md) appendix. APTS domains are interdependent: a single event (such as a kill-switch activation or a scope violation) can trigger requirements across four or more domains simultaneously. Understanding these dependencies prevents the common mistake of implementing domains in isolation. The appendix includes event-triggered dependency maps, a suggested implementation sequence, and scenario walk-throughs.

### Step 4: Dive into Domains

Read the domain READMEs that are most relevant to your role:

| If You Are... | Start With These Domains |
|---------------|--------------------------|
| A platform architect | Scope Enforcement (SE), Safety Controls (SC), Graduated Autonomy (AL) |
| A security operations lead | Human Oversight (HO), Safety Controls (SC), Auditability (AR) |
| A compliance officer | Auditability (AR), Reporting (RP), Supply Chain Trust (TP); also see [Compliance Matrix](appendix/Compliance_Matrix.md) for regulatory framework mappings |
| A CISO evaluating vendors or an internal platform | All domains via [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) |
| An enterprise security team building an internal autonomous pentest platform | All domains. Start with Scope Enforcement (SE), Safety Controls (SC), Human Oversight (HO), then Auditability (AR) |
| A security professional reviewing a platform | Scope Enforcement (SE), Safety Controls (SC), Auditability (AR), Manipulation Resistance (MR), Reporting (RP), then the Verification subsections for the claimed tier |
| An AI/ML engineer | Manipulation Resistance (MR), Graduated Autonomy (AL), Supply Chain Trust (TP) |

Each domain README contains the full normative requirements with rationale and verification procedures. Framework mappings (NIST CSF, ISO 27001, NIST AI RMF, SOC 2, PCI DSS, GDPR) are in the [Compliance Matrix](appendix/Compliance_Matrix.md). Domains are not independent; refer to [Cross-Domain Integration](appendix/Cross_Domain_Integration.md) to understand how requirements in your focused domains connect to others.

### Step 5: Plan Implementation

Each domain includes an Implementation Guide with practical guidance for every requirement. These guides provide implementation approaches, key considerations, and common pitfalls. They are informative (non-normative); the domain READMEs are the authoritative source.

### Step 6: Apply the Standard

Depending on your role:

- **Platform builders (vendors, service providers, and enterprise security teams running internal platforms)**: Assess your platform against your target tier using the [Checklists](appendix/Checklists.md). Use the [Compliance Matrix](appendix/Compliance_Matrix.md) to understand alignment with external frameworks. The verification model and assessment options are described in the [Introduction](Introduction.md#compliance-tiers).
- **Customers**: Use the [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) to review operator-provided evidence and conformance assessment claims. For additional assurance, the [Customer Acceptance Testing](appendix/Customer_Acceptance_Testing.md) appendix provides optional hands-on verification procedures.
- **Security professionals**: Identify the claimed tier, deployment model, autonomy level, and platform configuration first. Then use each requirement's Verification subsection to review evidence, record gaps, and identify follow-up questions. Start with the [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) when reviewing an operator claim, and use [Customer Acceptance Testing](appendix/Customer_Acceptance_Testing.md) when behavioral evidence is needed for controls such as scope enforcement, kill switches, manipulation resistance, or reporting reproducibility.

## Quick Reference: Document Map

| Document | Type | Purpose |
|----------|------|---------|
| [Introduction](Introduction.md) | Normative | Framework overview, tiers, how to use this standard |
| [Glossary](appendix/Glossary.md) | Informative | Term definitions |
| Domain READMEs (8) | Normative | Full requirements with rationale and verification |
| Implementation Guides (8) | Informative | Practical implementation guidance per requirement |
| [Checklists](appendix/Checklists.md) | Informative | Quick-reference verification checklists by tier |
| [Compliance Matrix](appendix/Compliance_Matrix.md) | Informative | Mappings to NIST CSF, ISO 27001, NIST AI RMF, SOC 2, PCI DSS, GDPR |
| [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md) | Informative | Customer-facing vendor assessment guide |
| [Customer Acceptance Testing](appendix/Customer_Acceptance_Testing.md) | Informative | Hands-on verification tests |
| [Incident Response Integration](appendix/Incident_Response_Integration.md) | Informative | Operational integration guidance |
| [Cross-Domain Integration](appendix/Cross_Domain_Integration.md) | Informative | Cross-domain trigger mappings and dependency analysis |
| [Multi-Agent Coordination](appendix/Multi_Agent_Coordination.md) | Informative | Multi-agent coordination safeguards |
| [Multi-Agent Acceptance Testing](appendix/Multi_Agent_Acceptance_Testing.md) | Informative | Multi-agent hands-on verification tests |
| [Testing Phase Mapping](appendix/Testing_Phase_Mapping.md) | Informative | Requirements mapped to pentesting lifecycle phases |
| [Evidence Request Checklist](appendix/Evidence_Request_Checklist.md) | Informative | Lightweight checklist of artifacts customers and reviewers can request |
| [Human Review Record Template](appendix/Human_Review_Record_Template.md) | Informative | Illustrative template for documenting reviewer approval of critical findings |
| [Operator Competency Record Template](appendix/Operator_Competency_Record_Template.md) | Informative | Template for documenting operator qualification, training, assessment, remediation, and succession evidence |
| [Shift Handoff Template](appendix/Shift_Handoff_Template.md) | Informative | Template for documenting operator shift handoffs, pending approvals, safety signals, and authority transfer |
| [Incident Response Drill Record Template](appendix/Incident_Response_Drill_Record_Template.md) | Informative | Template for documenting incident response tabletop exercises, simulations, evidence capture, and corrective actions |
| [Conformance Claim Schema](appendix/Conformance_Claim_Schema.md) | Informative | Illustrative machine-readable schema for structured conformance claims |
| [Conformance Claim Template](appendix/Conformance_Claim_Template.md) | Informative | Optional template for platform operators documenting conformance |
| [Evidence Package Manifest](appendix/Evidence_Package_Manifest.md) | Informative | Illustrative manifest for finding evidence, provenance, and downstream handoff |
| [Rules of Engagement Template](appendix/Rules_of_Engagement_Template.md) | Informative | Illustrative machine-readable Rules of Engagement starter template |
| [Authority Delegation Matrix Template](appendix/Authority_Delegation_Matrix_Template.md) | Informative | Illustrative template for documenting role-based approval, escalation, and emergency authority |
| [Scope Change Decision Record Template](appendix/Scope_Change_Decision_Record_Template.md) | Informative | Template for documenting approved, rejected, constrained, or deferred scope changes during an engagement |
| [Autonomy Downgrade Matrix Template](appendix/Autonomy_Downgrade_Matrix_Template.md) | Informative | Template for documenting downgrade triggers, autonomy caps, re-authorization paths, and evidence preservation |
| [Threshold Configuration Template](appendix/Threshold_Configuration_Template.md) | Informative | Simple starter table for documenting operational thresholds, escalation points, and stop conditions |
| [Model Change and Drift Record Template](appendix/Model_Change_and_Drift_Record_Template.md) | Informative | Template for documenting model changes, behavioral comparisons, drift detections, re-attestation decisions, and rollback evidence |
| [Data Retention and Disposal Record Template](appendix/Data_Retention_and_Disposal_Record_Template.md) | Informative | Template for documenting retention decisions, secure deletion, disposal verification, exceptions, and customer deletion requests |
| [Credential and Secret Lifecycle Record Template](appendix/Credential_and_Secret_Lifecycle_Record_Template.md) | Informative | Template for documenting credential provisioning, discovery, access, rotation, revocation, retention, and disposal evidence |
| [Advisory Requirements](appendix/Advisory_Requirements.md) | Informative | Best-practice requirements not gated to any compliance tier |

## Common Questions

**Q: Do I need to implement all 173 requirements?**
No. Start with Tier 1 (72 requirements). Tier 2 and Tier 3 add requirements progressively for cumulative totals of 157 and 173. An additional 20 advisory practices live in the [Advisory Requirements appendix](appendix/Advisory_Requirements.md) under the `APTS-<DOMAIN>-A0x` identifier pattern; advisory practices are not required for conformance at any tier. See [Introduction: Compliance Tiers](Introduction.md#compliance-tiers) for details.

**Q: What if my platform meets most but not all Tier 1 requirements?**
APTS does not award partial credit. A tier claim requires every MUST requirement at the claimed tier and all lower tiers to be implemented, with no deviation. Every SHOULD requirement at those tiers must be either implemented or covered by a documented justification in the conformance claim. Address MUST gaps before claiming a tier.

**Q: Are the Implementation Guides mandatory?**
No. Implementation Guides are informative. They suggest approaches but do not define requirements. The domain READMEs contain all normative requirements.

**Q: How does APTS relate to PTES, WSTG, or OSSTMM?**
APTS is a governance framework, not a testing methodology. Autonomous systems should follow established testing methodologies (PTES, WSTG, OSSTMM) while operating under APTS governance controls.

**Q: Do I need to get certified?**
No. See the [Introduction](Introduction.md#compliance-tiers) for the full verification model. Platforms are assessed against the requirements, conformance is documented, and customers may independently verify claims using the [Vendor Evaluation Guide](appendix/Vendor_Evaluation_Guide.md).

**Q: When should I re-evaluate against APTS?**
Re-evaluate after major platform changes, security incidents, autonomy level changes, material dependency changes, or when the APTS standard releases a new version. At minimum, re-evaluate annually.

**Q: How much time should I budget for APTS?**
For platform operators (vendors, service providers, and enterprise security teams running internal platforms): plan 2-3 days to read and understand requirements for your target tier, plus 1-2 days per domain for detailed implementation planning. For customers evaluating an external vendor or assessing an internal platform: plan 1-2 weeks using the Vendor Evaluation Guide to review evidence and demonstrations of key safety controls. If conducting optional Customer Acceptance Testing, add 1-2 weeks for test execution and analysis.
