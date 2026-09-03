# Vendor Evaluation Guide

Informative Appendix (non-normative)

This appendix helps customers (CISOs, procurement teams, and security leaders) evaluate autonomous pentesting platform operators against APTS requirements. Throughout this guide, "the operator" refers to whichever entity operates the platform being evaluated: an external vendor, a managed-service provider, or an internal enterprise security team. Enterprise teams performing self-evaluation should use the questions as a self-review checklist before publishing an internal conformance claim.

For a shorter first-pass screen before a deeper review, see the [Quick Vendor Review Checklist](Quick_Vendor_Review_Checklist.md).

## Before You Start

Decide your minimum compliance tier based on your risk tolerance:

- **Tier 1 (Foundation):** 72 requirements. The platform will not test outside the agreed scope, can be stopped immediately, will not leak discovered credentials, and provides a basic audit trail. **Choose Tier 1 when:** you are running supervised autonomous testing against non-critical systems with experienced operators monitoring the engagement.

- **Tier 2 (Verified):** 157 cumulative requirements (72 + 85). The platform is fully transparent about what it did and why, protects your data with tamper-proof audit trails, handles incidents with formal response procedures, and provides independently verifiable findings. **Choose Tier 2 when:** you are testing production environments, operating in regulated industries, or need full accountability for audit or compliance purposes. This is the recommended minimum for most production deployments.

- **Tier 3 (Comprehensive):** 173 cumulative requirements (157 + 16). The platform meets the highest assurance bar for critical infrastructure, fully autonomous (L4) operations, and the strictest regulatory requirements. **Choose Tier 3 when:** you are deploying fully autonomous testing against critical infrastructure, financial systems, or healthcare environments with minimal human oversight. An additional 20 advisory practices in the [Advisory Requirements appendix](Advisory_Requirements.md) are recommended for highest-assurance engagements but are not counted toward any tier.

> **Minimum tier guidance:** Tier 1 is appropriate for supervised testing of non-critical systems in non-regulated environments. Organizations in financial services, healthcare, critical infrastructure, or any regulated industry SHOULD require Tier 2 as a minimum. Tier 3 is recommended for critical infrastructure, fully autonomous (L4) operations, and environments with the strictest regulatory requirements.

## Domain-by-Domain Evaluation

Use the questions below to evaluate vendor capabilities across all eight APTS domains. Not every question applies to every engagement; select those relevant to your environment and risk tolerance. For brevity, requirement IDs in the tables below use the short form (for example, SE-001) rather than the full identifier (APTS-SE-001).

### Scope Enforcement (SE)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| How does the platform ingest and validate Rules of Engagement? | Machine-parseable RoE format (JSON/YAML/XML), schema validation, cryptographic integrity checks | SE-001 |
| How are IP ranges and domains validated before testing begins? | CIDR validation, RFC 1918 awareness, cloud metadata endpoint exclusion, wildcard handling policies | SE-002, SE-003 |
| How does the platform enforce temporal boundaries? | UTC-based enforcement, DST handling, countdown alerts, graceful shutdown at boundary expiration | SE-004, SE-008 |
| How are critical assets protected from testing? | Immutable deny lists, asset criticality classification (Critical/Production/Non-Production/Unknown), production database safeguards | SE-005, SE-009, SE-010 |
| How does the platform handle multi-tenant environments? | Tenant isolation checks, per-request tenant context validation, cross-tenant prevention | SE-011 |
| What happens when DNS or infrastructure changes mid-engagement? | DNS rebinding prevention, dynamic scope monitoring, drift detection with alerting | SE-007, SE-012 |
| How are credentials managed during and after engagements? | Real-time credential inventory, automatic rotation at engagement end, least-privilege enforcement | SE-023 |
| Does the platform support continuous or recurring testing? | Unique engagement IDs per cycle, cross-cycle finding correlation, deployment-triggered governance | SE-017, SE-018, SE-020 |

### Safety Controls & Impact Management (SC)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| How are pentesting actions classified by impact? | Multi-tier classification system (Critical/High/Medium/Low/Info), CIA scoring per action, technique-to-impact mapping | SC-001 |
| What rate limiting is in place? | Per-host connection and request caps, subnet-level aggregate limits, payload size and bandwidth constraints | SC-004 |
| How does the platform prevent cascading failures? | Dependency mapping, kill switches at dependency nodes, circuit breaker patterns | SC-005, SC-012 |
| How does the kill switch work, and can we test it? | Two-phase termination (graceful then forced), process tree termination, operator and automatic triggers | SC-009 |
| What monitoring detects unintended impact? | Continuous target monitoring (CPU, memory, network, errors), threshold-based alerts, escalation paths | SC-010 |
| How are cumulative risks tracked? | Time-based impact accumulation, decay functions, cumulative risk scoring algorithm, dynamic threshold adjustment | SC-007, SC-010 |
| What happens after the test completes? | Reversible action rollback, post-test integrity validation against baselines, evidence preservation in immutable storage | SC-014, SC-015, SC-016 |
| How is the platform itself monitored? | Platform health monitoring separate from target monitoring, external watchdog on independent infrastructure | SC-010, SC-017 |

### Human Oversight & Intervention (HO)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| What requires human approval before execution? | Role-based approval workflows for exploitation, lateral movement, data access, persistence mechanisms | HO-001 |
| What does the real-time monitoring dashboard show? | Live activity feed, system health, scope boundaries, pending approvals, anomaly alerts with drill-down | HO-002 |
| What happens if an approver does not respond in time? | Documented SLAs, default-deny/pause/kill behavior on timeout, progressive escalation chains | HO-003 |
| Who can trigger the kill switch? | Primary and secondary authorities, manager override, out-of-band kill switch on independent network (Advisory) | HO-008, HO-009 |
| How are irreversible actions handled? | Mandatory human approval gate, impact assessment in approval request, two-person rule for high-impact actions | HO-010 |
| How are unexpected findings escalated? | Defined triggers for IoCs, illegal content, zero-days, out-of-scope access; legal/compliance notification paths | HO-011, HO-014 |
| What are the operator qualification requirements? | Competency standards by role, certification program, annual recertification, incident response training | HO-018 |
| How is 24/7 coverage maintained? | Shift handoff procedures, stale approval expiry, fatigue monitoring, mandatory breaks | HO-019 |

### Graduated Autonomy Levels (AL)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| What autonomy levels does the platform support? | Clear L1 (Assisted) through L4 (Autonomous) definitions with documented boundaries per level | AL-001 through AL-004 |
| How is a platform's autonomy level determined? | Formal assessment criteria, capability evaluation methodology, documented evidence requirements | AL-025 |
| What restrictions apply at each level? | Progressive capability unlocking, action-type restrictions per level, scope limitations per level | AL-006, AL-007, AL-008 |
| How does the platform transition between levels? | Defined promotion/demotion criteria, assessment evidence, approval requirements for level changes | AL-025, AL-026 |
| What safety controls scale with autonomy level? | Monitoring intensity, approval requirements, and safety margins that increase with autonomy level | AL-012 through AL-016 |

### Auditability & Reproducibility (AR)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| What does the platform log? | Every action, decision, and outcome with timestamps; decision rationale capture; tool invocation records | AR-001, AR-002 |
| How is log integrity protected? | Tamper-evident storage, cryptographic chaining or signing, independent log verification | AR-010, AR-012 |
| Can findings be reproduced? | Decision replay capability, environment state capture, reproducible finding validation | AR-016, AR-017 |
| How are AI/ML model changes tracked? | Model version logging, drift detection, change impact assessment, model change audit trail | AR-019 |
| What retention policies apply? | Defined retention periods per data type, secure disposal procedures, regulatory compliance | AR-005, TP-015 |
| How is audit access controlled? | Role-based access to logs, separation of duties between operators and auditors | AR-011, AR-019 |

### Manipulation Resistance (MR)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| How does the platform resist prompt injection? | Input sanitization, prompt/data separation, output validation, context isolation | MR-001, MR-002 |
| How is scope widening prevented? | Scope anchor validation, target-suggested scope expansion rejection, redirect chain validation | MR-007, MR-010, MR-012 |
| How is model poisoning detected? | Training data integrity verification, model behavior monitoring, anomaly detection on model outputs | TP-019, AR-019 |
| What SSRF protections are in place? | Internal network access controls, metadata endpoint blocking, outbound request filtering | MR-009 |
| How are adversarial inputs handled? | Input fuzzing resistance, encoding normalization, multi-layer validation | MR-013, MR-014, MR-017 |
| Is the platform tested against manipulation? | Regular red team exercises, adversarial testing program, manipulation resistance validation | MR-020 |

### Third-Party & Supply Chain Trust (TP)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| How are AI/ML providers vetted? | Provider security assessment, contractual security requirements, ongoing monitoring | TP-001, TP-012 |
| How are third-party dependencies managed? | Dependency inventory, vulnerability scanning, update policies, SBOM generation | TP-006 |
| What is the incident notification process? | Documented timelines for breach notification, customer communication procedures | TP-005 |
| How is customer data isolated? | Per-engagement isolation, data residency controls, cross-tenant prevention | TP-017 |
| What data retention and deletion policies exist? | Defined retention periods, certified deletion procedures, data destruction proof | TP-015, TP-016 |
| How are secrets handled during engagements? | Secret classification by provenance, reuse policies, mandatory revocation at engagement end | SE-023, MR-019 |

### Reporting (RP)

| Question | What to Look For | Key Requirements |
|----------|-----------------|------------------|
| How are findings validated before reporting? | Multi-stage validation, false positive reduction, confidence scoring per finding | RP-001, RP-002 |
| What does a standard report include? | Executive summary, technical findings, evidence, remediation guidance, coverage disclosure | RP-011, RP-012, RP-008 |
| How is finding confidence communicated? | Per-finding confidence scores with methodology and supporting evidence quality factors | RP-003 |
| How is testing coverage measured and disclosed? | Coverage metrics per scope element, untested areas identified, coverage gaps explained | RP-008 |
| How are reports and downstream findings protected in transit and at handoff? | Cryptographic evidence integrity, encrypted downstream transmission, protected integration pipeline to ticketing systems | RP-005, RP-015, TP-014 |

## What to Ask Vendors

Use these seven questions for initial assessment before detailed evaluation:

1. **"Which APTS tier do you claim conformance with?"** If unfamiliar with APTS, share this standard. If claiming conformance, request evidence.

2. **"Provide your completed APTS conformance assessment against the [Checklists](Checklists.md)."** A credible vendor maps capabilities to per-tier verification items, whether the assessment was performed internally or by a third party.

3. **"Can you demonstrate your safety controls in a live environment?"** Ask the vendor to demonstrate kill switch operation, scope enforcement, and rate limiting. The [Customer Acceptance Testing](Customer_Acceptance_Testing.md) appendix provides structured test procedures if you want to conduct your own hands-on verification.

4. **"How does your kill switch work, and can we test it?"** Multiple independent kill switches are required (APTS-SC-009). Ask for a demo in a test environment.

5. **"What happens to our data after the engagement?"** Request credential disposal reports and data destruction proof (APTS-SE-023, APTS-TP-015, APTS-TP-016).

6. **"Do you deploy agents or software to our infrastructure?"** If yes, confirm agents can be removed without vendor cooperation and are covered in the Rules of Engagement (APTS-SE-022).

7. **"What AI/ML models does the platform use, and how do you track model changes?"** Request the model change log and drift detection procedures (APTS-AR-019).

## Red Flags

Watch for these warning signs:

- **No kill switch demo.** Inability to demonstrate emergency termination may indicate the capability doesn't exist or won't work reliably.
- **"We handle scope internally."** Platforms must accept machine-parseable RoE from customers, not define scope unilaterally.
- **No audit trail access.** Refusal to provide logs suggests insufficient logging infrastructure.
- **Vague AI model governance.** Vendors should know exactly which models are used, how changes are tracked, and how drift is detected.
- **No data isolation.** Engagement data from different customers must be isolated; ask how this is enforced.
- **No credential rotation.** Inability to confirm automatic credential disposal at engagement end risks stale credentials persisting.
- **No incident notification timeline.** Vendors should have documented timelines for customer notification of security incidents.

### Evaluation Timeline

A thorough vendor evaluation typically requires 2-4 weeks depending on the depth of verification:

- **Week 1:** Define target tier. Distribute the APTS Checklists appendix to the vendor and request a completed conformance assessment. Review vendor documentation.
- **Week 2:** Vendor presentation and Q&A using domain-specific evaluation questions above. Request evidence for claimed capabilities. Request vendor demonstrations of safety controls (kill switch, scope enforcement, rate limiting).
- **Week 3-4 (optional):** If additional assurance is needed, conduct hands-on verification using the [Customer Acceptance Testing](Customer_Acceptance_Testing.md) procedures in a staging environment. Analyze results, identify gaps, make accept/conditional/reject decision.

## Verification Approaches

Three approaches, in increasing order of assurance:

- **Vendor-provided conformance assessment:** Vendor provides a completed checklist with supporting evidence. The assessment may be performed internally or by a third party at the vendor's discretion. This is the standard starting point for evaluating vendor claims against APTS requirements. Most vendor evaluations will use this approach.
- **Vendor demonstration:** Ask the vendor to demonstrate key safety controls (kill switch activation, scope enforcement, rate limiting) in their own staging environment. This provides additional confidence in behavioral requirements without requiring the customer to build test infrastructure.
- **Customer Acceptance Testing (CAT):** For organizations requiring the highest assurance (critical infrastructure, regulated industries, fully autonomous deployments), the [Customer Acceptance Testing](Customer_Acceptance_Testing.md) appendix provides structured hands-on test procedures. CAT is optional and represents the most thorough verification approach.

> **Note on behavioral requirements:** Some APTS requirements are behavioral (kill switch timing, scope enforcement accuracy, manipulation resistance) and cannot be fully verified through documentation. For these requirements, customers can request vendor demonstrations or recorded evidence. Organizations with higher risk tolerance may accept vendor claims and operator-provided assessment evidence; organizations requiring stronger assurance may conduct hands-on verification using the CAT procedures.

## Recommended Evaluation Process

1. **Define your minimum tier.** Tier 1 for general use, Tier 2 for regulated environments, Tier 3 for highest-assurance requirements.
2. **Send the vendor the APTS Checklists** for their claimed tier and request a completed conformance assessment.
3. **Request a small evidence pack first.** The [Evidence Request Checklist](Evidence_Request_Checklist.md) appendix provides a lightweight starting set of artifacts to request before deeper review.
4. **Review domain-specific questions** from the tables above, prioritizing domains most relevant to your environment.
5. **Identify red flags** using the criteria above. Any red flag warrants deeper investigation before proceeding.
6. **Request vendor demonstrations** of key safety controls (kill switch, scope enforcement, rate limiting). For higher-assurance needs, consider conducting [Customer Acceptance Testing](Customer_Acceptance_Testing.md).
7. **Document the evaluation outcome** including tier, date, and any conditions or exceptions.
8. **Re-evaluate periodically** or when major platform changes, security incidents, or autonomy level changes occur.
