---

layout: col-sidebar
title: OWASP Autonomous Penetration Testing Standard (APTS)
tags: apts, autonomous penetration testing standard
level: 2
type: documentation
pitch: A governance standard for autonomous penetration testing platforms

---

![OWASP Logo](assets/images/OWASP_APTS_Logo.png)

<p align="center">
  <a href="https://owasp.org/APTS/"><img src="https://img.shields.io/badge/OWASP-Incubator%20Project-blue" alt="OWASP Incubator Project"></a>
  <a href="https://creativecommons.org/licenses/by-sa/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg" alt="License: CC BY-SA 4.0"></a>
  <a href="https://github.com/OWASP/APTS"><img src="https://img.shields.io/badge/Version-0.1.0-green" alt="Version: 0.1.0"></a>
  <a href="./CONTRIBUTING.md"><img src="https://img.shields.io/badge/Contributions-Welcome-brightgreen" alt="Contributions Welcome"></a>
  <a href="https://owasp.slack.com/archives/C0B1EJMT2GY"><img src="https://img.shields.io/badge/Slack-Join%20Chat-611f69?logo=slack&logoColor=white" alt="Slack"></a>
</p>


## What is OWASP APTS?

A governance standard for autonomous penetration testing platforms. It defines what these systems must do to operate safely, transparently, and within defined boundaries, whether they are delivered by vendors, operated as a service, or built in-house by enterprise security teams for testing their own organization.

APTS is not a testing methodology. It complements PTES, OWASP WSTG, and OSSTMM by addressing the problems unique to autonomous operation: scope enforcement, safe autonomy, manipulation resistance, and accountability.

## 173 Tier-Required Requirements Across 8 Domains

| Domain | Prefix | Requirements | Description |
|--------|--------|--------------|-------------|
| [Scope Enforcement](./standard/1_Scope_Enforcement/) | SE | 26 | Defining, validating, and enforcing testing boundaries |
| [Safety Controls](./standard/2_Safety_Controls/) | SC | 20 | Impact classification, blast radius, kill switches, rollback, execution sandbox |
| [Human Oversight](./standard/3_Human_Oversight/) | HO | 19 | Approval gates, dashboards, escalation, operator qualifications |
| [Graduated Autonomy](./standard/4_Graduated_Autonomy/) | AL | 28 | Four autonomy levels (L1 Assisted through L4 Autonomous) |
| [Auditability](./standard/5_Auditability/) | AR | 20 | Logging, decision trails, evidence integrity, audit trail isolation |
| [Manipulation Resistance](./standard/6_Manipulation_Resistance/) | MR | 23 | Prompt injection, adversarial inputs, scope widening defense |
| [Supply Chain Trust](./standard/7_Supply_Chain_Trust/) | TP | 22 | AI provider trust, data handling, multi-tenancy isolation, foundation model disclosure |
| [Reporting](./standard/8_Reporting/) | RP | 15 | Finding validation, confidence scoring, coverage disclosure |

## Compliance Tiers

- **Tier 1 (Foundation)**: 72 requirements. The platform will not test outside scope, can be stopped immediately, and provides an audit trail.
- **Tier 2 (Verified)**: 85 additional (157 cumulative). Full transparency, tamper-proof audit trails, and independently verifiable findings.
- **Tier 3 (Comprehensive)**: 16 additional (173 cumulative). Highest assurance for critical infrastructure and L4 autonomous operations.

Twenty additional advisory practices live exclusively in the [Advisory Requirements appendix](./standard/appendix/Advisory_Requirements.md) under the `APTS-<DOMAIN>-A0x` identifier pattern. Advisory practices are not counted toward any tier and do not affect conformance.

APTS has no certification body, no mandatory third-party audit, and no fee. Platforms are assessed against the requirements and conformance is documented. The standard does not prescribe who performs the assessment; internal self-assessment, independent internal review, and external third-party assessment are all valid approaches, and the choice is left to the reader.

## How to Reference

Requirements use the format `APTS-XX-NNN` where `XX` is the domain prefix and `NNN` is the requirement number (for example, `APTS-SE-001`). For versioned references in contracts or evaluations, use `APTS-v0.1.0-SE-001`.

## Quick Start: Where to Begin

Use this reader path to choose the shortest route through APTS based on your role and goal after reviewing the framework overview, domains, and tier structure above.

![APTS reader path flowchart](assets/images/apts-reader-flow.svg)

| Role | Start with | Then use | Outcome |
|------|------------|----------|---------|
| **New to APTS** | [Introduction](./standard/Introduction.md), [Glossary](./standard/appendix/Glossary.md) | [Getting Started guide](./standard/Getting_Started.md) | Understand the framework and scope |
| Vendor or platform builder | [Introduction](./standard/Introduction.md), [Glossary](./standard/appendix/Glossary.md), then [Checklists](./standard/appendix/Checklists.md) for target tier | Domain READMEs, Implementation Guides, [Conformance Claim Template](./standard/appendix/Conformance_Claim_Template.md), [Evidence Package Manifest](./standard/appendix/Evidence_Package_Manifest.md) | Documented conformance and evidence package |
| Enterprise internal team | [Introduction](./standard/Introduction.md), [Glossary](./standard/appendix/Glossary.md), then [Getting Started](./standard/Getting_Started.md) | Core domains (SE, SC, HO, AR), templates | Internal governance baseline |
| CISO or procurement lead | [Introduction](./standard/Introduction.md), [Glossary](./standard/appendix/Glossary.md), then [Vendor Evaluation Guide](./standard/appendix/Vendor_Evaluation_Guide.md) | [Evidence Request Checklist](./standard/appendix/Evidence_Request_Checklist.md), [Evidence Package Manifest](./standard/appendix/Evidence_Package_Manifest.md), conformance claim | Vendor evaluation decision |
| Security reviewer or auditor | [Introduction](./standard/Introduction.md), [Glossary](./standard/appendix/Glossary.md), then claimed tier and [Checklists](./standard/appendix/Checklists.md) | Domain verification sections, [Customer Acceptance Testing](./standard/appendix/Customer_Acceptance_Testing.md) | Independent review findings |
| Contributor | [CONTRIBUTING.md](./CONTRIBUTING.md) | Existing issues and PRs | Small, reviewable PR |

This orientation aid is informative and does not create or modify APTS requirements.

The full standard with all requirements, verification procedures, checklists, and appendices is in the [`standard/`](./standard/) folder.

## Contributing

This standard is open for community contributions. Whether it is improving requirement clarity, adding implementation examples, fixing errors, or translating the standard, all contributions are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started and [GOVERNANCE.md](./GOVERNANCE.md) for project roles and decision-making. Translations are maintained in [`standard/translations/`](./standard/translations/).

Report sensitive content issues (incorrect security guidance, documentation of insecure patterns) via [SECURITY.md](./SECURITY.md).

Join the discussion on [OWASP Slack](https://owasp.slack.com/archives/C0B1EJMT2GY) in the `#project-apts` channel.

## Project Leads

* [Jinson Varghese Behanan](https://www.linkedin.com/in/JinsonVarghese/)
* [Shikhil Sharma](https://www.linkedin.com/in/shikhil-sharma/)

## License

[CC BY-SA 4.0](./LICENSE.md). Copyright 2026 The OWASP Foundation.
