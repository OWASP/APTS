# Glossary

Informative Appendix (non-normative)

This appendix is part of the OWASP Autonomous Penetration Testing Standard (APTS) and provides a full glossary of APTS-specific terms and security concepts used throughout the framework.

---

## A

**Adjacent Network**
A network reachable from the scoped network through routing, including networks on the same broadcast domain, behind the same gateway, or accessible via discovered routes. Used in scope enforcement to define boundaries that autonomous testing MUST NOT cross without explicit authorization. See APTS-SE-013.

**AI/LLM Provider**
External service providing artificial intelligence or large language model capabilities to autonomous pentest systems. Examples: OpenAI GPT-4, Anthropic Claude, Google Gemini. Requires data handling agreements and security vetting per the Supply Chain Trust domain.

**API Key**
Cryptographic credential used to authenticate API requests to third-party services. Should be stored in secure vaults, rotated annually, and never embedded in code. See Supply Chain Trust.

**Approval Gate**
A mandatory checkpoint at which an autonomous action pauses until a qualified human operator grants or denies authorization within a defined SLA response window (see APTS-HO-003). Medium and high-impact actions require approval gates regardless of the platform's autonomy level.

**Autonomy Level**
Classification system (L1-L4) describing degree of independence and decision-making authority of autonomous pentest system. Higher levels require more sophisticated technical safeguards and governance mechanisms while reducing per-action human approval requirements. L1 requires human direction for every action; L4 operates independently within pre-approved boundaries under continuous automated monitoring.

Note: Autonomy levels (L1-L4) describe operational capability (what the platform can do independently). Compliance tiers (Tier 1-3) describe governance maturity (what controls are in place). A platform may operate at any autonomy level at any compliance tier, provided the tier's requirements for that autonomy level are met.

**Autonomous Operation**
For the purposes of APTS, a system operates autonomously when it makes decisions about targeting, methodology, exploitation, or lateral movement without requiring per-action human approval. Systems that execute pre-configured scans without decision-making (for example, scheduled vulnerability scanners) are not considered autonomous under this standard. The degree of autonomy is classified using the four Autonomy Levels (L1 through L4) defined in the Graduated Autonomy domain.

**Autonomous Penetration Test**
Security assessment conducted by automated systems with minimal human intervention. System makes decisions about scope, testing methodology, and finding assessment based on configured rules and intelligence. Human oversight and approval remain required for critical decisions.

**Atomic Technique**
A single logical testing operation that cannot be meaningfully subdivided without changing its purpose. Examples include a port scan across multiple ports, a SQL injection test using multiple payloads against one parameter, DNS record enumeration, or a brute-force authentication attempt using a wordlist against one target. Each atomic technique produces one set of results and targets one logical objective. See APTS-AL-001.

**Authority Delegation Matrix (ADM)**
A formal document defining which roles are authorized to make specific categories of decisions during autonomous testing engagements, including kill switch activation, scope changes, autonomy level transitions, and high-risk action approvals. See APTS-HO-004.

---

## B

**Banner Grabbing**
Service identification technique. Used for vulnerability matching against CVE databases.

**Baseline**
In health check monitoring, the median response time calculated across the first 10 health check cycles, excluding outliers beyond the 5th and 95th percentile. Baselines are recalculated when target infrastructure changes are detected. See APTS-SC-010.

**Blast Radius**
The scope of potential damage from a single test action, including direct impact on the target and cascading failures in interconnected systems. Safety controls limit blast radius through rate limiting, impact classification, and circuit breakers.

**Blanket Pre-Approval**
A time-bounded authorization for a category of low-impact, high-frequency actions (for example, port scans on non-critical targets), allowing the platform to execute them without individual operator approval while maintaining full audit trail coverage. See APTS-HO-016.

**Breach Notification**
Formal communication to clients and regulatory authorities when unauthorized access or disclosure of their data occurs. Notification timelines vary by jurisdiction and regulation. Platforms are expected to comply with applicable breach notification requirements for each jurisdiction in which they operate (see APTS-TP-005).

**Business Continuity Plan (BCP)**
Procedures to maintain operations if primary systems fail. For autonomous pentest systems, includes fallback procedures when external dependencies become unavailable.

---

## C

**Campaign**
A coordinated series of tests against multiple targets or services within a single engagement.

**Customer Acceptance Testing (CAT)**
An optional verification framework providing structured hands-on tests for validating platform behavior against APTS requirements. CAT covers 39 of the 173 tier-required requirements across five phases: scope enforcement, safety controls, detection effectiveness, data handling, and reporting. CAT can be conducted as a vendor-led demonstration or customer-led verification. See the Customer Acceptance Testing appendix.

**Certificate Pinning**
Security technique where application verifies specific certificate or certificate authority chain, preventing man-in-the-middle attacks even if system certificate store is compromised. Recommended for critical API connections.

**Chain of Custody**
Documentation tracking how data moves through an organization, who had access, when it was accessed, and what actions were taken. Important for audit trails and incident investigation.

**CIDR (Classless Inter-Domain Routing)**
Notation for specifying IP address ranges using a base address and prefix length (for example, 192.168.1.0/24). Used in scope definitions to specify target IP ranges.

**Compensating Controls**
Alternative security measures that mitigate vulnerability when the primary control is missing. Example: Two-factor authentication compensates for weak passwords.

**Compliance Tier**
One of three progressive levels of APTS conformance. Tier 1 (Foundation) requires 72 core requirements (MUST | Tier 1). Tier 2 (Verified) adds 85 requirements for a cumulative 157 (MUST | Tier 2 + SHOULD | Tier 2). Tier 3 (Comprehensive) adds 16 requirements for a cumulative 173 (MUST | Tier 3 + SHOULD | Tier 3). A platform claims a tier by implementing every MUST requirement at that tier and all lower tiers, with no deviation, and by either implementing every SHOULD requirement at those tiers or recording a documented justification for each deviation in its conformance claim. An additional 20 advisory practices in the Advisory Requirements appendix are recommended for highest-assurance engagements but are not counted toward any tier.

**Confidence Score**
A numeric value on a 0-100% scale indicating the platform's certainty in a scope boundary determination, target legitimacy assessment, asset classification, or finding validity. Scores below 75% for scope-related decisions trigger mandatory human escalation. See APTS-HO-013, APTS-RP-003.

**Confidence Level**
Assessment of how certain an autonomous system is about finding validity. Ranges from HIGH (95%+ certain) to UNCONFIRMED (< 50% certain). Affects report inclusion and remediation priority.

For confidence score methodology requirements, see APTS-RP-003 (confidence scoring) and APTS-AR-004 (decision point logging).

**Continuous Testing**
An operational mode where the platform conducts recurring or ongoing testing against the same target environment without a discrete end-date per cycle. Continuous testing triggers additional governance controls (see APTS-SE-019, APTS-HO-019). Platforms conducting scheduled one-time engagements (even if frequent) are not considered to be in continuous testing mode unless cycles overlap or run back-to-back without operator re-authorization.

**Crypto Shred**
Data destruction method that deletes encryption keys rather than overwriting data. Once keys destroyed, data becomes unrecoverable even if stored media is not physically destroyed.

**CVSS (Common Vulnerability Scoring System)**
An open framework maintained by FIRST for communicating the characteristics and severity of software vulnerabilities. Scores range from 0.0 (none) to 10.0 (critical). References to CVSS in this standard mean CVSS v3.1 or later (v4.0 is the current version as of this release). The specific version used should be consistent within an engagement and documented in engagement reports (see APTS-RP-003).

**CWE (Common Weakness Enumeration)**
Categorization of software weaknesses (programming errors, design flaws). Examples: CWE-89 (SQL Injection), CWE-79 (Cross-Site Scripting). Used to correlate similar vulnerabilities across products.

---

## D

**Data Classification**
Categorization of data by sensitivity level: RESTRICTED (credentials, PII), CONFIDENTIAL (findings, technical details), INTERNAL (non-sensitive business), PUBLIC (published). Determines protection controls required.

**Data Loss Prevention (DLP)**
Security technology that detects and prevents unauthorized transmission of sensitive data. Scans for patterns like credit card numbers, social security numbers, keywords.

**Data Minimization**
Principle of collecting and transmitting only necessary data. When sending data to external providers, include only what's essential for the service function. Reduces exposure risk.

**Data Residency**
Requirement that data be stored or processed in specific geographic locations based on applicable regulations. GDPR permits international transfers subject to adequacy decisions, standard contractual clauses, or other Chapter V safeguards; some jurisdictions impose stricter domestic storage requirements. Drives cloud service provider selection.

**Default-Safe Behavior**
The action a platform takes when a required human decision times out: halt testing and preserve state rather than continuing or escalating. Ensures that operator unavailability does not result in unauthorized autonomous action. See APTS-HO-003.

**Decision Trail**
Documentation of how autonomous system arrived at finding/decision. Shows discovery method, analysis performed, rules applied, confidence scoring. Enables improvement and auditing.

**Denial of Service (DoS)**
Attack to make system unavailable to legitimate users. Can be network-level (flooding), application-level (resource exhaustion), or distributed (DDoS).

**Disaster Recovery Plan (DRP)**
Procedures to restore systems after major failure or disaster. Includes Recovery Time Objective (RTO) and Recovery Point Objective (RPO).

---

## E

**Engagement**
A defined period of autonomous penetration testing activity against a specific set of targets, governed by a Rules of Engagement document. An engagement has a defined start, end, scope, and set of authorized activities. Includes both one-time and continuous/recurring testing arrangements.

**Exploit**
Working code/technique that demonstrates vulnerability exploitation. Used for proof-of-concept, but actual exploitation only in authorized test scope.

**Exploitation**
Active attempt to trigger vulnerability and achieve unauthorized access or capability. Only conducted in authorized scope against permitted systems.

**Escalation**
The process of raising an event, finding, or decision to a higher authority or more urgent response level. In APTS, escalation is triggered by impact threshold breaches (HO-012), unexpected findings (HO-011), confidence drops (HO-013), or legal/compliance triggers (HO-014). Escalation always results in a logged event and may result in a pause, halt, or human review.

---

## F

**Execution Sandbox**
A kernel-enforced isolation boundary containing the agent runtime, whose configuration the runtime itself has no credentials to modify (see APTS-SC-019). Tool and action allowlists are enforced by components external to the model rather than by the model's own instructions (APTS-SC-020).

**Failover**
Automatic or manual transition from primary system to backup when primary becomes unavailable. Example: Cloud provider A unavailable → automatically switch to Provider B.

**False Negative**
Finding that should have been detected but wasn't. Example: Vulnerable service installed but scanner didn't detect it. Leads to missed risks.

**False Positive**
Finding reported that isn't actually valid. Example: Scanner reports SQLi but code uses parameterized queries. Wastes client remediation effort.

**FQDN (Fully Qualified Domain Name)**
A complete domain name that specifies a host's exact position in the DNS hierarchy (for example, www.example.com). Used in scope definitions to identify target domains.

**Feed**
External data source providing vulnerability information. Examples: NVD (National Vulnerability Database), VulnDB, vendor advisories. Required for accurate vulnerability correlation.

**Finding**
Single vulnerability, misconfiguration, or security weakness identified during test. Includes description, severity, remediation guidance.

---

## G

**Governance**
Organizational processes for decision-making and accountability. APTS governance includes approval workflows, escalation procedures, oversight mechanisms for autonomous systems.

---

## H

**Hash/Hashing**
Cryptographic function converting input data to fixed-size output. Examples: SHA-256, MD5. Used for integrity verification, data minimization (instead of transmitting actual values).

**Hard Deny List**
An immutable list of systems that the platform MUST NOT test under any circumstances, regardless of scope definition. Unlike standard scope boundaries, hard deny lists cannot be modified during an engagement. Default entries include production databases, authentication infrastructure, payment systems, and safety-critical systems. See APTS-SE-009.

**High Availability**
System design ensuring minimal downtime. Achieved through redundancy, failover mechanisms, geographic distribution.

**HIPAA (Health Insurance Portability and Accountability Act)**
US regulation protecting health information (PHI). Requires data encryption, access controls, and breach notification per regulatory timelines.

---

## I

**Incident Response**
Organizational procedures for responding to security incidents. Includes detection, assessment, containment, notification, investigation, remediation.

**Information Disclosure**
Vulnerability revealing sensitive information without requiring authentication. Examples: Directory listing, comments in HTML, error messages with stack traces.

**Intrusion Detection System (IDS)**
Security system monitoring network traffic for suspicious patterns indicating attacks. Can be network-based (NIDS) or host-based (HIDS).

**Irreversible Action**
A testing action that cannot be undone or rolled back after execution. Examples include data deletion, account lockout on production systems, or cryptographic key destruction. APTS requires mandatory human approval before executing irreversible actions (HO-010).

---

## K

**Key Management**
Processes for creating, storing, rotating, and retiring cryptographic keys. Includes access controls, auditing, escrow procedures.

**Kill Switch**
Immediate termination mechanism that halts all autonomous testing activity. APTS requires multiple independent kill switch mechanisms (see APTS-SC-009 for normative requirements). Kill switches must initiate halt promptly and achieve full cessation of testing activity within the timeframe defined by the platform's safety controls.

---

## L

**Lateral Movement**
Post-exploitation activity where attacker moves from compromised system to other systems in network. Autonomous systems detect obvious lateral movement capabilities (for example, reachability tests).

**Legal Hold**
Requirement to preserve data for legal proceedings despite normal deletion schedules. Overrides retention policies until legal proceeding concludes.

---

## M

**Machine-Parseable**
A data format that can be consumed and processed by automated tools without manual transformation. In the context of APTS, machine-parseable report formats include JSON, XML, and CSV. See APTS-RP-011.

**Material Change**
A modification to the platform, its dependencies, or its operating environment that could affect the efficacy of controls covered by the claimed compliance tier. Examples include: changing the primary AI/ML provider, migrating to a different cloud infrastructure provider, adding support for a new class of testing (for example, network infrastructure in addition to web applications), or changes to the decision-making pipeline that alter how the platform selects targets, chooses techniques, or evaluates findings. Routine maintenance, bug fixes, and minor version updates that do not affect safety controls, scope enforcement, or decision-making logic are not considered material changes.

**Metrics**
Quantitative measurements of system performance or security. Examples: false positive rate, mean time to remediation (MTTR), SLA uptime percentage.

**Multi-Tenancy**
Architecture where multiple clients' data is stored on shared infrastructure. Requires strict logical/physical isolation to prevent cross-contamination.

---

## N

**NIST CSF (Cybersecurity Framework)**
National Institute of Standards and Technology framework for managing cybersecurity risk. CSF 2.0 (2024) defines six functions: Govern, Identify, Protect, Detect, Respond, Recover.

**Non-Repudiation**
Security property where actor cannot deny having performed action. Achieved through digital signatures, audit logs.

---

## O

**Operation**
General term for any platform activity during an engagement, including tests, data collection, and reporting activities.

**Out-of-Band**
A communication or control channel that operates independently of the primary channel, providing redundancy in case the primary channel fails. In APTS, out-of-band kill switch mechanisms use separate network paths, protocols, or hardware to ensure termination capability even when the primary management channel is compromised or unavailable. See APTS-HO-008, APTS-HO-009.

**OWASP (Open Worldwide Application Security Project)**
Non-profit organization focused on software security. Produces the standard, OWASP Top 10, security testing guides.

---

## P

**PCI DSS (Payment Card Industry Data Security Standard)**
Security standard for organizations handling credit cards. Requires encryption, access controls, vulnerability management, and incident response capabilities.

**Platform**
The autonomous penetration testing system subject to APTS requirements. Includes the software, infrastructure, AI/ML models, and operational processes that together perform autonomous testing. May be deployed as SaaS, on-premises, or hybrid.

**Penetration Test**
Authorized assessment simulating attacker behavior to identify vulnerabilities, governed by documented scope and Rules of Engagement.

**PII (Personally Identifiable Information)**
Data identifying individuals: names, SSNs, email addresses, phone numbers. Requires special protection per privacy regulations.

**Phase Boundary**
The transition point between distinct stages of a penetration test (for example, reconnaissance to exploitation). In graduated autonomy, human approval is required at phase boundaries because crossing from passive observation to active system interaction changes the risk profile. See APTS-AL-007.

**Proof of Concept (PoC)**
Demonstration that vulnerability is exploitable. Can be code, command, or screenshot showing unauthorized access/capability achieved.

**Production-Like Environment**
A target environment that mirrors production in configuration, data sensitivity, or network topology sufficiently that unintended testing impact could affect real users, data, or services. This includes staging environments with production data, pre-production environments connected to production networks, and disaster recovery environments that can be activated. Isolated development environments with synthetic data are not production-like.

**Prompt Injection**
An attack in which adversarial content embedded in data the agent processes (target system responses, files, web pages) attempts to override the agent's instructions or alter its behavior. Manipulation Resistance requirements such as APTS-MR-001 mandate strict separation between operator instructions and target-derived data.

**Qualified Reviewer**
An individual with demonstrated expertise in penetration testing methodology (for example, OSCP, CREST CRT, GPEN, or equivalent experience), security governance frameworks, and familiarity with AI/ML systems. Organizations may use qualified reviewers when evaluating platforms against APTS requirements.

---

## R

**Reconnaissance**
Information gathering phase. Includes passive research (OSINT) and active probing (scanning). Determines targets and attack surface.

**Recovery Point Objective (RPO)**
Maximum acceptable data loss window. If system fails at 2pm and RPO is 1 hour, must be able to recover data from 1pm. Determines backup frequency.

**Recovery Time Objective (RTO)**
Maximum acceptable downtime after a failure event. Determines backup and failover procedure requirements. Example: an RTO of 4 hours means service must be restored within 4 hours of failure.

**Rollback**
Restoring a target system or the platform to its pre-action state after testing changes, using pre-action state capture and documented step-by-step procedures (see APTS-SC-014). Evidence is preserved in tamper-evident storage before rollback executes (APTS-SC-016).

**Rules of Engagement (RoE)**
A formal document defining the scope, boundaries, authorized activities, temporal constraints, escalation procedures, and contact information for an autonomous penetration testing engagement. The RoE is the authoritative source for scope enforcement.

**Red Team**
Security team authorized to attack systems to test defenses. Autonomous systems can be part of red team operations.

**Remote Code Execution (RCE)**
Vulnerability allowing attacker to execute arbitrary code on target system. Typically most severe (CVSS 9+), enables complete compromise.

**Remediation**
Process of fixing identified vulnerabilities. Includes patching, configuration changes, implementation of controls.

**Reporting**
Delivery of findings, recommendations, and risk assessment to client. Includes executive summary, detailed findings, confidence levels, remediation guidance.

**Risk Assessment**
Process of identifying threats, vulnerabilities, and likelihood of exploitation to determine overall risk level. Input to strategic planning.

**Risk Rating**
Classification of vulnerability/risk severity: CRITICAL, HIGH, MEDIUM, LOW, INFORMATIONAL. Determined by exploitability, impact, and affected assets.

---

## S

**SaaS (Software as a Service)**
Cloud-based application provided by vendor. Examples: Salesforce, Microsoft 365. Autonomous pentest systems depend on multiple SaaS services.

**Scanning**
Automated process to identify services, versions, and vulnerabilities on target systems. Examples: Port scanning, vulnerability scanning, web application scanning.

**Scope**
Boundaries of authorized testing. Defines systems, networks, and functionality that can be tested. Excludes: third-party systems, personal devices, production data.

**Scope Drift**
Gradual expansion of testing activity beyond originally authorized boundaries. Can occur through DNS rebinding, subdomain discovery, linked resource following, or autonomous decision-making that interprets scope permissively. APTS requires continuous scope validation to detect and prevent drift.

**Security Information and Event Management (SIEM)**
Centralized platform collecting logs from security systems for analysis, correlation, alerting. Used for autonomous system monitoring.

**Severity Level (SEV-1 through SEV-4)**
Incident severity classification used throughout APTS. SEV-1 (Critical): active harm to target systems, data breach, or scope violation affecting critical assets; triggers immediate kill switch, executive notification, and regulatory assessment. SEV-2 (High): potential harm detected, safety threshold breach, or provider compromise. SEV-3 (Medium): anomalous behavior, confidence threshold breach, or non-critical scope drift. SEV-4 (Low): minor anomalies, recoverable errors, or informational alerts. See the Incident Response Integration appendix for full severity definitions and response procedures.

**Service Level Agreement (SLA)**
Contract specifying service availability, performance, and remediation terms. Example: 99.9% uptime, 200ms latency, 24-hour support response.

**Severity**
Impact level of vulnerability if exploited. Related to CVSS score but also considers business context (affects critical assets more severe).

**Social Engineering**
Manipulation of people to divulge confidential information or perform security-violating actions. Autonomous systems typically do not conduct social engineering.

**SOC 2**
Audit standard for service organizations. Type II includes controls testing over time. Widely adopted by cloud and SaaS providers to demonstrate trust assurance.

**Software Bill of Materials (SBOM)**
A machine-readable inventory of the software components and dependencies used by the platform, maintained in SPDX or CycloneDX format. The baseline inventory is required at Tier 1 (APTS-TP-006); SBOM freshness and customer access obligations apply at Tier 2 (APTS-AR-016).

**SQL Injection (SQLi)**
Vulnerability in database queries where attacker can insert malicious SQL code. Allows unauthorized database access, data theft, modification.

**SSL/TLS**
Cryptographic protocols for secure communication. Platforms must use current, industry-accepted TLS versions for all sensitive communications (see APTS-TP-014 for normative cryptographic requirements).

**Supply Chain Risk**
Risk from third-party vendors and dependencies. Can be product-related (malicious code), operational (outages), or data-related (breach).

---

## T

**Test**
A single technique execution or check performed by the platform against a target.

**Transparent Database Encryption (TDE)**
Database-level encryption of data at rest. Automatic, transparent to applications. Used for protection of sensitive data in databases.

**Testing Phase**
A discrete stage in the penetration testing lifecycle. This standard uses the canonical phase model defined in the Graduated Autonomy domain: Reconnaissance, Enumeration, Identification, Exploitation, Post-Exploitation, and Reporting, plus an Initialization state that precedes Reconnaissance (recognized by APTS-AR-002). Implementations MUST use these phase names when generating phase-transition events. Domain-specific requirements may reference phase subsets relevant to their scope.

**Timestamp Precision**
Platform-generated timestamps are expected to use millisecond precision or better (see APTS-AR-001). External timestamp authorities (for example, RFC 3161 services) may operate at lower precision (±1 second); this is acceptable for external timestamping while platform-internal logs maintain millisecond precision.

**Trust Model**
Definition of what/who is trusted in system. Zero-trust: Verify everything, never assume. Autonomous systems require strong trust boundaries.

---

## U

**Unauthorized Access**
Access to system/data without permission. Includes both external attackers and internal actors accessing beyond their authorized scope.

**Unencrypted Protocol**
Communication protocol transmitting data in plaintext without encryption. Examples: HTTP, Telnet, FTP. Should not be used for sensitive data.

---

## V

**Vulnerability**
Security weakness in system that could be exploited to achieve unauthorized access or action. Can be in code, configuration, or design.

**Vulnerability Database**
Repository of known vulnerabilities with details: affected versions, CVSS scores, references. Examples: NVD, VulnDB.

**Vulnerability Feed**
Data source providing vulnerability information in machine-readable format. Feeds must be validated for accuracy and freshness.

---

## W

**Web Application Firewall (WAF)**
Security device filtering web traffic to prevent attacks. Protects against OWASP Top 10 vulnerabilities like SQLi, XSS, CSRF.

**White-Box Testing**
Security testing with full knowledge of system internals (source code, architecture, credentials). Can be more thorough but less realistic.

---

## Z

**Zero-Day Vulnerability**
Previously unknown vulnerability that vendors have not patched. Attackers can exploit before patch available. High-value target for attackers.

**Zero-Trust Security**
Architecture verifying every access request, never assuming trust. Requires authentication, authorization, encryption regardless of network location.

---

## Common Acronyms

| Acronym | Full Form | Context |
|---------|-----------|---------|
| APTS | Autonomous Penetration Testing Standard | Standard name |
| API | Application Programming Interface | External service integration |
| AWS | Amazon Web Services | Cloud provider |
| BCP | Business Continuity Plan | Disaster recovery |
| CCPA | California Consumer Privacy Act | California privacy law |
| CISO | Chief Information Security Officer | Organizational leadership |
| CVE | Common Vulnerabilities and Exposures | Vulnerability ID system |
| CVSS | Common Vulnerability Scoring System | Vulnerability rating |
| CWE | Common Weakness Enumeration | Weakness categorization |
| DLP | Data Loss Prevention | Security technology |
| DPA | Data Processing Agreement | GDPR contractual requirement |
| DRP | Disaster Recovery Plan | System recovery procedures |
| FP | False Positive | Invalid finding |
| FN | False Negative | Missed vulnerability |
| GDPR | General Data Protection Regulation | EU privacy regulation |
| HIPAA | Health Insurance Portability and Accountability Act | US health privacy law |
| HSM | Hardware Security Module | Secure key storage |
| IDS/NIDS | Intrusion Detection System / Network IDS | Security monitoring |
| ISO | International Organization for Standardization | Standards body |
| MTTR | Mean Time to Remediation | Remediation speed metric |
| NVD | National Vulnerability Database | US vulnerability feed |
| OWASP | Open Worldwide Application Security Project | Security organization |
| PCI-DSS | Payment Card Industry Data Security Standard | Payment card security requirement |
| PHI | Protected Health Information | HIPAA-regulated health data |
| PII | Personally Identifiable Information | Privacy-regulated personal data |
| RCE | Remote Code Execution | Critical vulnerability type |
| RPO | Recovery Point Objective | Maximum acceptable data loss |
| RTO | Recovery Time Objective | Maximum acceptable downtime |
| SIEM | Security Information and Event Management | Log aggregation platform |
| SLA | Service Level Agreement | Service availability commitment |
| SQLi | SQL Injection | Database vulnerability type |
| SSN | Social Security Number | PII element |
| TLS | Transport Layer Security | Encryption protocol |
| VPN | Virtual Private Network | Secure remote access |
| WAF | Web Application Firewall | Application security device |
| XSS | Cross-Site Scripting | Web application vulnerability |

---

## References

- NIST Cybersecurity Framework: https://www.nist.gov/cyberframework
- OWASP: https://owasp.org
- CVE/NVD: https://nvd.nist.gov
- CVSS: https://www.first.org/cvss
- CWE: https://cwe.mitre.org
- PCI-DSS: https://www.pcisecuritystandards.org
