# OWASP Autonomous Penetration Testing Standard

This is the full OWASP Autonomous Penetration Testing Standard. It defines 173 tier-required requirements across 8 domains (plus 20 advisory practices in the [Advisory Requirements appendix](appendix/Advisory_Requirements.md)) that autonomous penetration testing platforms must meet to operate safely, transparently, and within defined boundaries, whether delivered by vendors, operated as a service, or built in-house by enterprise security teams.

## Getting Started

New to APTS? Start with the [Getting Started Guide](./Getting_Started.md) for a recommended reading order and orientation.

## Front Matter

- [Foreword](./Foreword.md) - Why this standard exists
- [Frontispiece](./Frontispiece.md) - Project metadata and conventions
- [Introduction](./Introduction.md) - Purpose, scope, compliance tiers, how to use this standard

## How This Standard Is Organized

Each of the 8 domains has its own folder containing:

- **README.md** - The requirements themselves, each with classification, requirement specification, and verification procedures
- **Implementation_Guide.md** - Practical guidance per requirement (implementation steps, key considerations, common pitfalls) plus a tiered implementation roadmap

The [appendices](./appendix/) provide cross-cutting resources: checklists, compliance mappings, cross-domain integration patterns, and evaluation guides.

## Appendices

| Appendix | Purpose |
|----------|---------|
| [Glossary](./appendix/Glossary.md) | Terms and definitions |
| [Checklists](./appendix/Checklists.md) | Per-tier compliance checklists |
| [Compliance Matrix](./appendix/Compliance_Matrix.md) | Mappings to NIST CSF 2.0, ISO 27001:2022, NIST AI RMF, SOC 2, PCI DSS, GDPR |
| [Cross-Domain Integration](./appendix/Cross_Domain_Integration.md) | How events in one domain trigger requirements in others |
| [Testing Phase Mapping](./appendix/Testing_Phase_Mapping.md) | Which requirements apply at each pentesting phase |
| [Customer Acceptance Testing](./appendix/Customer_Acceptance_Testing.md) | Optional hands-on verification procedures for customers |
| [Vendor Evaluation Guide](./appendix/Vendor_Evaluation_Guide.md) | Guide for evaluating autonomous pentest platform operators |
| [Quick Vendor Review Checklist](./appendix/Quick_Vendor_Review_Checklist.md) | Short 30-minute and 2-hour screening checklist for vendor or operator reviews |
| [Rules of Engagement Template](./appendix/Rules_of_Engagement_Template.md) | Illustrative machine-readable RoE starter template |
| [Authority Delegation Matrix Template](./appendix/Authority_Delegation_Matrix_Template.md) | Illustrative template for documenting role-based approval, escalation, and emergency authority |
| [Scope Change Decision Record Template](./appendix/Scope_Change_Decision_Record_Template.md) | Informative template for documenting approved, rejected, constrained, or deferred scope changes during an engagement |
| [Autonomy Downgrade Matrix Template](./appendix/Autonomy_Downgrade_Matrix_Template.md) | Informative template for documenting downgrade triggers, autonomy caps, re-authorization paths, and evidence preservation |
| [Threshold Configuration Template](./appendix/Threshold_Configuration_Template.md) | Simple starter table for documenting operational thresholds, escalation points, and stop conditions |
| [Model Change and Drift Record Template](./appendix/Model_Change_and_Drift_Record_Template.md) | Informative template for documenting model changes, behavioral comparison, drift review, re-attestation, and rollback evidence |
| [Data Retention and Disposal Record Template](./appendix/Data_Retention_and_Disposal_Record_Template.md) | Informative template for documenting retention decisions, disposal execution, deletion verification, exceptions, and customer deletion requests |
| [Credential and Secret Lifecycle Record Template](./appendix/Credential_and_Secret_Lifecycle_Record_Template.md) | Informative template for documenting credential provisioning, discovery, access, rotation, revocation, retention, and disposal evidence |
| [Evidence Package Manifest](./appendix/Evidence_Package_Manifest.md) | Illustrative manifest for finding evidence, provenance, and downstream handoff |
| [Evidence Package Manifest Example](./appendix/examples/Evidence_Package_Manifest_Example.md) | Fictional completed example showing finding evidence, provenance, redaction, and export handoff |
| [Conformance Claim Schema](./appendix/Conformance_Claim_Schema.md) | Illustrative machine-readable schema for structured conformance claims |
| [Conformance Claim Template](./appendix/Conformance_Claim_Template.md) | Optional template for documenting platform conformance |
| [Conformance Claim Example](./appendix/examples/Conformance_Claim_Example.md) | Fictional completed example showing how an operator might document an APTS conformance claim |
| [Evidence Request Checklist](./appendix/Evidence_Request_Checklist.md) | Simple artifact checklist for customer and reviewer evidence requests |
| [Human Review Record Template](./appendix/Human_Review_Record_Template.md) | Illustrative template for documenting reviewer approval of critical findings |
| [Operator Competency Record Template](./appendix/Operator_Competency_Record_Template.md) | Informative template for documenting operator qualification, training, assessment, remediation, and succession evidence |
| [Shift Handoff Template](./appendix/Shift_Handoff_Template.md) | Informative template for documenting operator shift handoffs, pending approvals, safety signals, and authority transfer |
| [Incident Response Drill Record Template](./appendix/Incident_Response_Drill_Record_Template.md) | Informative template for documenting incident response tabletop exercises, simulations, evidence capture, and corrective actions |
| [Advisory Requirements](./appendix/Advisory_Requirements.md) | Non-mandatory practices for enhanced assurance |
| [Incident Response Integration](./appendix/Incident_Response_Integration.md) | Unified incident response workflow across domains |
| [Multi-Agent Coordination](./appendix/Multi_Agent_Coordination.md) | Shared state, rate limits, and safety synchronization across multiple workers |
| [Multi-Agent Acceptance Testing](./appendix/Multi_Agent_Acceptance_Testing.md) | Distributed safety verification procedures for multi-agent platforms |

## Machine-readable export

A machine-readable JSON export of APTS requirements is available at: [apts_requirements.json](apts_requirements.json)<br>
A JSON schema for validating the export is available at: [apts_requirements_schema.json](apts_requirements_schema.json)

This enables automation, compliance tooling integration, and vendor self-assessment workflows.

### Generating the JSON Export

If you modify the requirements in the Markdown files or add new domains, you can automatically regenerate both the JSON export and its schema by running the export script from the root directory:

```bash
python scripts/export_requirements.py
```

## Requirements Index

### [Scope Enforcement](./1_Scope_Enforcement/) | [Implementation Guide](./1_Scope_Enforcement/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-SE-001 | Rules of Engagement (RoE) Specification and Validation | MUST | 1 | Parse and validate RoE before testing starts |
| APTS-SE-002 | IP Range Validation and RFC 1918 Awareness | MUST | 1 | Validate IP ranges using CIDR notation |
| APTS-SE-003 | Domain Scope Validation and Wildcard Handling | MUST | 1 | Validate domains and handle wildcards safely |
| APTS-SE-004 | Temporal Boundary and Timezone Handling | MUST | 1 | Enforce time windows with timezone handling |
| APTS-SE-005 | Asset Criticality Classification and Integration | MUST | 1 | Classify assets by criticality level |
| APTS-SE-006 | Pre-Action Scope Validation | MUST | 1 | Validate scope before every network action |
| APTS-SE-007 | Dynamic Scope Monitoring and Drift Detection | MUST | 2 | Detect scope drift during engagement |
| APTS-SE-008 | Temporal Scope Compliance Monitoring | MUST | 1 | Monitor temporal boundary compliance |
| APTS-SE-009 | Hard Deny Lists and Critical Asset Protection | MUST | 1 | Immutable list of never-test systems |
| APTS-SE-010 | Production Database Safeguards | MUST | 2 | Protect production databases from modification |
| APTS-SE-011 | Multi-Tenant Environment Awareness | SHOULD | 2 | Detect and isolate multi-tenant environments |
| APTS-SE-012 | DNS Rebinding Attack Prevention | MUST | 2 | Prevent DNS rebinding scope bypass |
| APTS-SE-013 | Network Boundary and Lateral Movement Enforcement | MUST | 2 | Prevent lateral movement beyond scope |
| APTS-SE-014 | Network Topology Discovery Limitations | SHOULD | 2 | Limit network topology discovery depth |
| APTS-SE-015 | Scope Enforcement Audit and Compliance Verification | MUST | 1 | Audit scope enforcement decisions |
| APTS-SE-016 | Scope Refresh and Revalidation Cycle | MUST | 2 | Revalidate scope periodically during engagement |
| APTS-SE-017 | Engagement Boundary Definition for Recurring Tests | MUST | 2 | Define boundaries for recurring test cycles |
| APTS-SE-018 | Cross-Cycle Finding Correlation and Regression Detection | SHOULD | 2 | Track findings across test cycles |
| APTS-SE-019 | Rate Limiting, Adaptive Backoff, and Production Impact Controls | MUST | 2 | Rate limits, backoff, and impact controls |
| APTS-SE-020 | Deployment-Triggered Testing Governance | MUST | 2 | Govern deployment-triggered testing |
| APTS-SE-021 | Scope Conflict Resolution for Overlapping Engagements | SHOULD | 3 | Resolve overlapping engagement scopes |
| APTS-SE-022 | Client-Side Agent Scope and Safety Boundaries | SHOULD | 2 | Scope constraints for client-side agents |
| APTS-SE-023 | Credential and Secret Lifecycle Governance | MUST | 2 | Manage credential lifecycle throughout |
| APTS-SE-024 | Cloud-Native and Ephemeral Infrastructure Governance | MUST | 2 | Govern cloud and ephemeral infrastructure |
| APTS-SE-025 | API-First and Business Logic Testing Governance | MUST | 2 | Govern API and business logic testing |
| APTS-SE-026 | Out-of-Distribution Action Monitoring | SHOULD | 2 | Monitor action distribution against declared baseline |

### [Safety Controls & Impact Management](./2_Safety_Controls/) | [Implementation Guide](./2_Safety_Controls/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-SC-001 | Impact Classification and CIA Scoring | MUST | 1 | Classify impact using CIA scoring |
| APTS-SC-002 | Industry-Specific Impact Considerations | MUST | 2 | Apply industry-specific impact rules |
| APTS-SC-003 | Real-World Impact Classification Examples | SHOULD | 2 | Provide real-world impact examples |
| APTS-SC-004 | Rate Limiting, Bandwidth, and Payload Constraints | MUST | 1 | Enforce rate, bandwidth, payload limits |
| APTS-SC-005 | Cascading Failure Prevention in Interconnected Systems | SHOULD | 2 | Prevent cascading failures across systems |
| APTS-SC-006 | Threshold Escalation Workflow (Automated → Approval → Prohibited) | MUST | 2 | Escalate actions through approval tiers |
| APTS-SC-007 | Cumulative Risk Scoring with Time-Based Decay | MUST | 2 | Track cumulative risk with time decay |
| APTS-SC-008 | Threshold Configuration with Schema Validation | SHOULD | 3 | Validate threshold config with schema |
| APTS-SC-009 | Kill Switch | MUST | 1 | Multiple independent kill switch mechanisms |
| APTS-SC-010 | Health Check Monitoring, Threshold Adjustment, and Automatic Halt | MUST | 1 | Monitor health, adjust thresholds, auto-halt |
| APTS-SC-011 | Condition-Based Automated Termination | MUST | 2 | Auto-terminate on defined conditions |
| APTS-SC-012 | Network-Level Circuit Breaker | MUST | 2 | Network-level circuit breaker |
| APTS-SC-013 | Time-Based Automatic Termination with Operator Override | SHOULD | 3 | Time-based termination with override |
| APTS-SC-014 | Reversible Action Tracking and Rollback | MUST | 2 | Track reversible actions and enable rollback |
| APTS-SC-015 | Post-Test System Integrity Validation | MUST | 1 | Validate system integrity post-test |
| APTS-SC-016 | Evidence Preservation and Automated Cleanup | MUST | 2 | Preserve evidence before cleanup |
| APTS-SC-017 | External Watchdog and Operator Notification | MUST | 2 | External watchdog with operator alerts |
| APTS-SC-018 | Incident Containment and Recovery | MUST | 2 | Contain and recover from incidents |
| APTS-SC-019 | Execution Sandbox and Containment Boundary Integrity | MUST | 2 | Kernel-enforced sandbox for the agent runtime |
| APTS-SC-020 | Action Allowlist Enforcement External to the Model | MUST | 1 | Tool and action allowlist enforced outside the model |

### [Human Oversight & Intervention](./3_Human_Oversight/) | [Implementation Guide](./3_Human_Oversight/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-HO-001 | Mandatory Pre-Approval Gates for Autonomy Levels L1 and L2 | MUST | 1 | Require approval before high-risk actions |
| APTS-HO-002 | Real-Time Monitoring and Intervention Capability | MUST | 1 | Real-time monitoring and intervention for operator oversight |
| APTS-HO-003 | Decision Timeout and Default-Safe Behavior | MUST | 1 | Default-safe behavior on decision timeout |
| APTS-HO-004 | Authority Delegation Matrix | MUST | 1 | Define who can authorize what actions |
| APTS-HO-005 | Delegation Chain-of-Custody and Decision Audit Trail | MUST | 2 | Immutable log of all delegation decisions |
| APTS-HO-006 | Graceful Pause Mechanism with State Preservation | MUST | 1 | Pause testing while preserving state |
| APTS-HO-007 | Mid-Engagement Redirect Capability | MUST | 1 | Redirect testing mid-engagement |
| APTS-HO-008 | Immediate Kill Switch with State Dump | MUST | 1 | Immediate kill switch with state dump |
| APTS-HO-009 | Multi-Operator Kill Switch Authority and Handoff | MUST | 2 | Multi-operator kill switch handoff |
| APTS-HO-010 | Mandatory Human Decision Points Before Irreversible Actions | MUST | 1 | Human approval before irreversible actions |
| APTS-HO-011 | Unexpected Findings Escalation Framework | MUST | 1 | Escalate unexpected findings to operator |
| APTS-HO-012 | Impact Threshold Breach Escalation | MUST | 1 | Escalate when impact thresholds breached |
| APTS-HO-013 | Confidence-Based Escalation (Scope Uncertainty) | MUST | 1 | Escalate on low scope confidence |
| APTS-HO-014 | Legal and Compliance Escalation Triggers | MUST | 1 | Escalate legal and compliance triggers |
| APTS-HO-015 | Real-Time Activity Monitoring and Multi-Channel Notification | MUST | 1 | Real-time alerts via multiple channels |
| APTS-HO-016 | Alert Fatigue Mitigation and Smart Aggregation | SHOULD | 2 | Mitigate alert fatigue with aggregation |
| APTS-HO-017 | Stakeholder Notification and Engagement Closure | MUST | 2 | Notify stakeholders at engagement closure |
| APTS-HO-018 | Operator Qualification, Training, and Competency Governance | MUST | 2 | Operator qualification and training governance |
| APTS-HO-019 | 24/7 Operational Continuity and Shift Handoff | SHOULD | 2 | 24/7 coverage with shift handoff procedures |

### [Graduated Autonomy Levels](./4_Graduated_Autonomy/) | [Implementation Guide](./4_Graduated_Autonomy/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-AL-001 | Single Technique Execution | MUST | 1 | Execute one technique at a time (L1) |
| APTS-AL-002 | Human-Directed Target and Technique Selection | MUST | 1 | Human selects targets and techniques (L1) |
| APTS-AL-003 | Parameter Configuration by Human Operator | MUST | 1 | Human configures all parameters (L1) |
| APTS-AL-004 | No Automated Chaining or Sequential Decision-Making | MUST | 1 | No automated chaining of actions (L1) |
| APTS-AL-005 | Mandatory Logging and Human-Reviewable Audit Trail | MUST | 1 | Log every action for human review (L1) |
| APTS-AL-006 | Basic Scope Validation and Policy Enforcement | MUST | 1 | Basic scope and policy enforcement (L1) |
| APTS-AL-007 | Multi-Step Technique Chaining Within Single Phase | MUST | 2 | Chain techniques within single phase (L2) |
| APTS-AL-008 | Real-Time Human Monitoring and Approval Gates | MUST | 1 | Real-time monitoring with approval gates (L2) |
| APTS-AL-009 | Tool-Proposed Actions with Operator Modification Capability | SHOULD | 2 | Tool proposes actions, human modifies (L2) |
| APTS-AL-010 | Step-by-Step Audit Log with Phase Transitions | MUST | 2 | Step-by-step log with phase transitions (L2) |
| APTS-AL-011 | Escalation Triggers and Exception Handling | MUST | 1 | Escalation triggers and exception handling (L2) |
| APTS-AL-012 | Kill Switch and Pause Capability | MUST | 1 | Kill switch and pause capability (L2) |
| APTS-AL-013 | Complete Attack Chain Execution Within Boundaries | MUST | 2 | Complete attack chains within boundaries (L3) |
| APTS-AL-014 | Boundary Definition and Enforcement Framework | MUST | 1 | Define and enforce boundary framework (L3) |
| APTS-AL-015 | Pre-Approved Action Categories and Decision Trees | MUST | 2 | Pre-approved action categories (L3) |
| APTS-AL-016 | Continuous Boundary Monitoring and Breach Detection | MUST | 1 | Continuous boundary monitoring (L3) |
| APTS-AL-017 | Multi-Target Assessment Management | MUST | 2 | Manage assessment across multiple targets (L3) |
| APTS-AL-018 | Incident Response During Autonomous Testing | MUST | 2 | Incident response during autonomous testing (L3) |
| APTS-AL-019 | Multi-Target Campaign Management Without Intervention | SHOULD | 3 | Multi-target campaigns without intervention (L4) |
| APTS-AL-020 | Dynamic Scope Adjustment and Target Discovery | SHOULD | 3 | Dynamic scope adjustment and discovery (L4) |
| APTS-AL-021 | Adaptive Testing Strategy and Resource Reallocation | SHOULD | 3 | Adaptive strategy and resource allocation (L4) |
| APTS-AL-022 | Continuous Risk Assessment and Automated Escalation | SHOULD | 3 | Continuous risk assessment and escalation (L4) |
| APTS-AL-023 | Complete Audit Trail and Forensic Reconstruction | SHOULD | 3 | Complete audit trail and forensic reconstruction (L4) |
| APTS-AL-024 | Periodic Autonomous Review Cycles | SHOULD | 3 | Periodic autonomous review cycles (L4) |
| APTS-AL-025 | Autonomy Level Authorization, Transition, and Reauthorization | MUST | 2 | Authorize and manage level transitions |
| APTS-AL-026 | Incident Investigation and Autonomy Level Adjustment | MUST | 2 | Investigate incidents, adjust autonomy level |
| APTS-AL-027 | Evasion and Stealth Mode Governance | SHOULD | 3 | Govern evasion and stealth mode |
| APTS-AL-028 | Containment Verification for L3 and L4 Autonomy | MUST | 3 | Periodic operator-run containment verification (L3-L4) |

### [Auditability & Reproducibility](./5_Auditability/) | [Implementation Guide](./5_Auditability/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-AR-001 | Structured Event Logging with Schema Validation | MUST | 1 | Structured event logging with schema |
| APTS-AR-002 | State Transition Logging | MUST | 1 | Log all state transitions |
| APTS-AR-003 | Resource Utilization Metrics Logging | MUST | 2 | Log resource utilization metrics |
| APTS-AR-004 | Decision Point Logging and Confidence Scoring | MUST | 1 | Log decisions with confidence scores |
| APTS-AR-005 | Log Retention and Archival Requirements | MUST | 2 | Define log retention and archival |
| APTS-AR-006 | Decision Chain of Reasoning and Alternative Evaluation | MUST | 1 | Log reasoning chain and alternatives |
| APTS-AR-007 | Risk Assessment Documentation Before Action Execution | MUST | 2 | Document risk assessment before action |
| APTS-AR-008 | Context-Aware Decision Logging | MUST | 2 | Context-aware decision logging |
| APTS-AR-009 | Transparency Report Requirements | MUST | 2 | Produce transparency reports |
| APTS-AR-010 | Cryptographic Hashing of All Evidence | MUST | 1 | Hash all evidence cryptographically |
| APTS-AR-011 | Chain of Custody for Evidence | MUST | 2 | Maintain evidence chain of custody |
| APTS-AR-012 | Tamper-Evident Logging with Hash Chains | MUST | 1 | Tamper-evident logging with hash chains |
| APTS-AR-013 | RFC 3161 Trusted Timestamp Integration | SHOULD | 3 | RFC 3161 trusted timestamps |
| APTS-AR-014 | Screenshot and Packet Capture Evidence Standards | MUST | 2 | Screenshot and packet capture standards |
| APTS-AR-015 | Evidence Classification and Sensitive Data Handling | MUST | 1 | Classify evidence, handle sensitive data |
| APTS-AR-016 | Platform Integrity and Supply Chain Attestation | MUST | 2 | Platform integrity attestation |
| APTS-AR-017 | Safety Control Regression Testing After Platform Updates | MUST | 2 | Regression test safety controls after updates |
| APTS-AR-018 | Customer Notification for Behavior-Affecting Updates | MUST | 2 | Notify customers of behavior-affecting updates |
| APTS-AR-019 | AI/ML Model Change Tracking and Drift Detection | MUST | 2 | Track AI model changes and drift |
| APTS-AR-020 | Audit Trail Isolation from the Agent Runtime | MUST | 2 | Audit store unreachable from the agent runtime |

### [Manipulation Resistance](./6_Manipulation_Resistance/) | [Implementation Guide](./6_Manipulation_Resistance/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-MR-001 | Instruction Boundary Enforcement | MUST | 1 | Enforce instruction boundary (operator vs target) |
| APTS-MR-002 | Response Validation & Sanitization | MUST | 1 | Validate and sanitize all responses |
| APTS-MR-003 | Error Message Neutrality | MUST | 1 | Neutralize error message manipulation |
| APTS-MR-004 | Configuration File Integrity Verification | MUST | 1 | Verify config file integrity |
| APTS-MR-005 | Authority Claim Detection & Rejection | MUST | 1 | Detect and reject authority claims |
| APTS-MR-006 | Decision Boundary Enforcement | SHOULD | 2 | Enforce immutable decision boundaries |
| APTS-MR-007 | Redirect Following Policy | MUST | 1 | Policy for following redirects |
| APTS-MR-008 | DNS and Network-Level Redirect Prevention | MUST | 1 | Prevent DNS and network-level redirects |
| APTS-MR-009 | Server-Side Request Forgery (SSRF) Prevention in Testing | MUST | 1 | Prevent SSRF during testing |
| APTS-MR-010 | Scope Expansion Social Engineering Prevention | MUST | 1 | Prevent scope expansion social engineering |
| APTS-MR-011 | Out-of-Band Communication Prevention | MUST | 1 | Block out-of-band communication |
| APTS-MR-012 | Immutable Scope Enforcement Architecture | MUST | 1 | Immutable scope enforcement architecture |
| APTS-MR-013 | Adversarial Example Detection in Vulnerability Classification | MUST | 2 | Detect adversarial vuln classification |
| APTS-MR-014 | Resource Exhaustion and Tarpit Attack Prevention | MUST | 2 | Prevent resource exhaustion and tarpits |
| APTS-MR-015 | Deceptive Authentication Honeypots | MUST | 2 | Detect deceptive auth honeypots |
| APTS-MR-016 | Anti-Automation Defense Detection | MUST | 2 | Detect anti-automation defenses |
| APTS-MR-017 | Anomaly Detection in Response Patterns | MUST | 2 | Detect anomalies in response patterns |
| APTS-MR-018 | AI Model Input/Output Architectural Boundary | MUST | 1 | AI model input/output boundary |
| APTS-MR-019 | Discovered Credential Protection | MUST | 1 | Protect discovered credentials |
| APTS-MR-020 | Adversarial Validation and Resilience Testing of Safety Controls | MUST | 2 | Adversarial test safety controls |
| APTS-MR-021 | Data Isolation Adversarial Testing | MUST | 3 | Adversarial test data isolation |
| APTS-MR-022 | Inter-Model Trust Boundaries and Output Validation | MUST | 2 | Inter-model trust and output validation |
| APTS-MR-023 | Agent Runtime as an Untrusted Component | MUST | 2 | Isolate the agent runtime from the control plane |

### [Third-Party & Supply Chain Trust](./7_Supply_Chain_Trust/) | [Implementation Guide](./7_Supply_Chain_Trust/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-TP-001 | Third-Party Provider Selection and Vetting | MUST | 1 | Vet third-party providers |
| APTS-TP-002 | Model Version Pinning and Change Management | MUST | 2 | Pin model versions, manage changes |
| APTS-TP-003 | API Security and Authentication | MUST | 1 | Secure API authentication |
| APTS-TP-004 | Provider Availability, SLA Management, and Failover | MUST | 2 | Manage provider SLAs and failover |
| APTS-TP-005 | Provider Incident Response, Breach Notification, and Mid-Engagement Compromise | MUST | 1 | Provider incident response and breach notification |
| APTS-TP-006 | Dependency Inventory, Risk Assessment, and Supply Chain Verification | MUST | 1 | Inventory dependencies and assess risk |
| APTS-TP-007 | Data Residency and Sovereignty Requirements | SHOULD | 2 | Enforce data residency requirements |
| APTS-TP-008 | Cloud Security Configuration and Hardening | MUST | 1 | Harden cloud security configuration |
| APTS-TP-009 | Incident Response and Service Continuity Planning | MUST | 2 | Plan incident response and service continuity |
| APTS-TP-010 | Vulnerability Feed Selection and Management | MUST | 2 | Select and manage vulnerability feeds |
| APTS-TP-011 | Feed Quality Assurance and Incident Response | SHOULD | 2 | Assure feed quality with incident response |
| APTS-TP-012 | Client Data Classification Framework | MUST | 1 | Classify client data before handling |
| APTS-TP-013 | Sensitive Data Discovery and Handling | MUST | 1 | Discover and handle sensitive data |
| APTS-TP-014 | Data Encryption and Cryptographic Controls | MUST | 1 | Encrypt data with strong cryptography |
| APTS-TP-015 | Data Retention and Secure Deletion | MUST | 2 | Enforce retention and secure deletion |
| APTS-TP-016 | Data Destruction Proof and Certification | MUST | 3 | Prove and certify data destruction |
| APTS-TP-017 | Multi-Tenant and Engagement Isolation | MUST | 2 | Isolate tenants and engagements |
| APTS-TP-018 | Tenant Breach Notification | MUST | 1 | Notify affected tenants of breaches |
| APTS-TP-019 | AI Model Provenance and Training Data Governance | MUST | 2 | Govern AI model provenance and training data |
| APTS-TP-020 | Persistent Memory and Retrieval State Governance | SHOULD | 2 | Govern persistent memory and retrieval state |
| APTS-TP-021 | Foundation Model Disclosure and Capability Baseline | MUST | 1 | Disclose foundation model and capability baseline |
| APTS-TP-022 | Re-attestation on Material Foundation Model Change | MUST | 2 | Re-assess platform after material foundation model change |

### [Reporting](./8_Reporting/) | [Implementation Guide](./8_Reporting/Implementation_Guide.md)

| ID | Requirement | Classification | Tier | Brief |
|----|-------------|----------------|------|-------|
| APTS-RP-001 | Evidence-Based Finding Validation | MUST | 2 | Validate findings with evidence |
| APTS-RP-002 | Finding Verification and Human Review Pipeline | MUST | 2 | Human review pipeline for findings |
| APTS-RP-003 | Confidence Scoring with Auditable Methodology | MUST | 2 | Confidence scoring with auditable method |
| APTS-RP-004 | Finding Provenance Chain | MUST | 2 | Maintain finding provenance chain |
| APTS-RP-005 | Cryptographic Evidence Chain Integrity | MUST | 2 | Cryptographic evidence chain integrity |
| APTS-RP-006 | False Positive Rate Disclosure | MUST | 1 | Disclose false positive rates |
| APTS-RP-007 | Independent Finding Reproducibility | SHOULD | 3 | Independent finding reproducibility and validation |
| APTS-RP-008 | Vulnerability Coverage Disclosure | MUST | 1 | Disclose vulnerability coverage |
| APTS-RP-009 | False Negative Rate Disclosure and Methodology | MUST | 2 | Disclose false negative rates |
| APTS-RP-010 | Detection Effectiveness Benchmarking | SHOULD | 3 | Benchmark detection effectiveness |
| APTS-RP-011 | Executive Summary and Risk Overview | MUST | 1 | Executive summary and risk overview |
| APTS-RP-012 | Remediation Guidance and Prioritization | MUST | 2 | Remediation guidance and prioritization |
| APTS-RP-013 | Engagement SLA Compliance Reporting | MUST | 2 | Engagement SLA compliance reporting |
| APTS-RP-014 | Trend Analysis for Recurring Engagements | SHOULD | 2 | Trend analysis for recurring engagements |
| APTS-RP-015 | Downstream Finding Pipeline Integrity | SHOULD | 2 | Downstream finding pipeline integrity |
