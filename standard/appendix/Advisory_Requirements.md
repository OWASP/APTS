# Advisory Requirements

**OWASP Autonomous Penetration Testing Standard (APTS) v0.1.0**

Informative Appendix (non-normative)

## Purpose

Advisory requirements are practices that provide additional assurance for autonomous penetration testing platforms but are not required for conformance at any tier. These practices were identified during the standard's development as valuable for mature implementations, high-risk engagements, or organizations pursuing the highest levels of operational rigor. They are not required for Tier 1, Tier 2, or Tier 3 conformance.

Advisory practices live exclusively in this appendix. They do not occupy slots in the domain requirement numbering. Platforms that implement advisory practices MAY document their adoption in vendor evaluation materials and customer-facing documentation. Advisory practices do not affect conformance scoring.

Advisory practices use the identifier pattern `APTS-<DOMAIN>-A0x` (for example, `APTS-AR-A01`, `APTS-AL-A01`). This distinguishes them from tier-required requirements, which use the pattern `APTS-<DOMAIN>-0xx`.

## When to Implement Advisory Practices

Advisory practices are recommended when:

- The platform operates in highly regulated industries (healthcare, finance, critical infrastructure).
- Engagements involve Level 4 (full autonomy) operations.
- Customer contracts explicitly require enhanced assurance controls.
- The organization is building toward long-term maturity in autonomous testing.

## Advisory Practice Registry

### APTS-AR-A01: State Capture, Reproducibility Documentation, and Replay Support (Advisory)

**Rationale:** Full state capture and replay support assumes deterministic system behavior that is at odds with LLM-based platforms relying on third-party inference APIs. Non-determinism in model outputs, timing, and external API responses makes strict replay infeasible as a mandatory requirement.

**Value:** Organizations that implement state capture and replay gain the ability to investigate anomalous platform behavior, reproduce findings for customer validation, and support forensic analysis of decision chains. Partial replay (capturing inputs and comparing outputs) remains valuable even without full deterministic replay.

**Practice Description:**

Capture sufficient execution state to enable meaningful replay within documented variance bounds. Record platform-controlled configuration (OS, tool versions, model versions and parameters, seed values, proxy and DNS settings), test execution state (timestamps, environment variables, tool configuration, test parameters, and the decision inputs and outputs already logged under APTS-AR-004 and APTS-AR-006), the observed target baseline at engagement start, and network conditions where known. Document every source of non-determinism (cryptographic RNG, timing variance, concurrency effects, probabilistic algorithms, and model inference variance) together with the seed values and control parameters that govern each source. Where exact replay is not achievable, show that decision logic remains traceable from inputs to outputs and that expected variance bounds are documented.

**Recommendation:** Implement state capture for critical decision points rather than full system state. Document known sources of non-determinism. Use replay as a diagnostic tool rather than a conformance gate.

**Related normative requirements:** APTS-AR-004, APTS-AR-006.

---

### APTS-AR-A02: Replay Variance Analysis and Equivalence Criteria (Advisory)

**Rationale:** Depends on APTS-AR-A01 (replay support). A strict finding-consistency threshold is not achievable for LLM-driven platforms where model inference variance, external API changes, and timing differences produce legitimate variance between runs.

**Value:** Variance analysis helps platforms understand their own consistency and identify when model updates or infrastructure changes degrade finding quality. Aggregate variance tracking across many engagements provides useful trend data.

**Practice Description:**

When replay produces different outputs, define and document equivalence criteria that distinguish acceptable variance (different phrasing of the same finding, alternative evidence paths, equivalent proof-of-concept techniques, consistent severity) from material divergence (different vulnerability classifications, severity-boundary crossings, missing critical findings, false positives, or contradictory findings from the same scenario). Document finding-consistency thresholds, severity tolerance (for example, CVSS variance bounds), and semantic-similarity thresholds for phrasing comparison. Track variance sources, classify each variance event, and perform trend analysis across replay attempts.

**Recommendation:** Track variance at the engagement level rather than mandating per-finding consistency thresholds. Use variance trends to detect regression after platform updates.

**Related normative requirements:** APTS-AR-004, APTS-AR-006, APTS-AR-A01.

---

### APTS-AR-A03: Real-Time External Log Streaming (Advisory)

**Rationale:** Real-time log streaming to an externally controlled aggregation system with an independently computed hash chain requires significant infrastructure investment. The core tamper-evidence need is already addressed by APTS-AR-012 (hash chains), APTS-AR-001 (structured logging), and APTS-AR-020 (audit trail isolation from the agent runtime).

**Value:** External log streaming provides defense-in-depth against platform compromise scenarios where an attacker could tamper with local logs. For platforms handling highly sensitive engagements, external log verification provides an additional source of truth.

**Practice Description:**

Stream all platform action logs to an externally controlled aggregation system within a short latency window (for example, 5 seconds). The external system should sit outside the platform's trust boundary (different host, different credentials, different administrative domain) and compute its own hash chain. The platform's internally computed hash chain and the external system's hash chain should match under normal operation; divergence indicates tampering. Customers of the engagement should have read-only access to the external log store for their data so they can verify the record independently of the platform.

**Recommendation:** Implement external log streaming for high-sensitivity engagements or when customer contracts require it. Batch upload (for example, every few minutes) is an acceptable alternative to true real-time streaming for most use cases.

**Related normative requirements:** APTS-AR-001, APTS-AR-012, APTS-AR-020.

---

### APTS-AR-A04: Continuous Runtime Integrity Monitoring (Advisory)

**Rationale:** Hashing every loaded executable section in memory on a short cadence is effectively asking every vendor to build runtime application self-protection. This is disproportionate to the threat model for most autonomous pentest platforms, which operate in controlled environments. Pre-flight binary attestation is already required by APTS-AR-016 (Platform Integrity and Supply Chain Attestation).

**Value:** Runtime integrity monitoring detects supply chain compromise, unauthorized code modification, and process injection during active engagements. For platforms deployed in hostile network environments or running unattended for extended periods, this provides early warning of platform compromise.

**Practice Description:**

Verify platform code integrity continuously during engagement execution. Hash loaded executable code sections in memory on a documented cadence and compare against a published baseline; halt the engagement and alert the operator on mismatch. Require runtime-loaded modules (plugins, exploit modules, decision models) to be cryptographically signed and verify signatures before loading. Detect and alert on unauthorized filesystem modifications in the platform's working directory, unexpected process launches from the platform's service account, and configuration file changes during an engagement. For highly sensitive deployments, run from a read-only container image or a Secure Boot verified partition with configuration changes permitted only through signed update packages.

**Recommendation:** Implement pre-flight integrity verification as the baseline (already required under APTS-AR-014). Reserve continuous runtime monitoring for high-sensitivity deployments or environments where the platform may run unattended in untrusted networks.

**Related normative requirements:** APTS-AR-014.

---

### APTS-SC-A01: Platform Health Monitoring and Anomaly Detection (Advisory)

**Rationale:** Core health monitoring is covered by APTS-SC-010. The advanced anomaly detection capabilities (behavioral baseline deviation, statistical confidence scoring, pattern analysis across testing, decision-making, and action dimensions) go beyond practical health monitoring into ML-based behavioral analysis territory.

**Value:** Behavioral anomaly detection can identify prompt injection attacks, model drift, or platform compromise that simple health checks would miss. Confidence-scored anomaly routing reduces alert fatigue while preserving detection of high-severity events.

**Recommendation:** Implement basic anomaly detection (probe rate spikes, unusual error rates) as part of SC-010 health monitoring. Reserve statistical baseline analysis and confidence scoring for platforms with dedicated security operations teams.

---

### APTS-SC-A02: Context Window Safety and Constraint Preservation (Advisory)

**Applicability:** This practice applies to platforms that use LLM-based agents with finite context windows where conversation history may be summarized, truncated, or compacted during an engagement.

**Rationale:** Context window exhaustion is an inevitable event in long-running autonomous engagements. When the platform summarizes the agent's history to free token budget, the summarizer (whether LLM-based or heuristic) can drop, paraphrase, or dilute constraints that appeared earlier in the conversation. An agent that was told "never test 10.0.0.5" in message 3 may lose that constraint entirely when messages 1 through 50 are summarized into a paragraph. The agent then continues operating without the constraint, producing a silent scope violation that no other safety control detects because the agent genuinely believes it was never restricted. This failure mode is invisible, silent, and unique to LLM-based architectures. The normative requirement set for v0.1.0 is frozen; this practice is a high-priority candidate for tier-gated inclusion in v0.2.0 and is strongly recommended for any platform using LLM-based agents with finite context windows.

**Value:** Platforms that implement context window safety prevent an entire class of silent scope violations caused by routine context management. This is especially critical for long-running engagements where compaction is guaranteed to occur multiple times, and where the constraints being lost (deny lists, autonomy restrictions, operator directives) are the constraints that prevent the most dangerous failures.

**Practice Description:**

When the platform summarizes, truncates, or otherwise compacts the agent's context window during an engagement, the platform should ensure that all safety-critical constraints survive the compaction intact. Specifically:

1. **Identify safety-critical context.** Maintain a defined set of safety-critical context elements that must survive any context compaction event. At minimum, this set should include: the active scope definition and all deny lists (APTS-SE-001, APTS-SE-009), the current autonomy level and any restrictions imposed during the session (APTS-AL-025), all active escalation states and pending human decisions (APTS-HO-011 through APTS-HO-014), credential references and their usage policies (APTS-SE-023), and any operator-imposed constraints or directives issued during the engagement.
2. **Re-inject safety-critical context after compaction.** After any context summarization, truncation, or compaction event, re-inject the full set of safety-critical context elements into the agent's working context before the agent's next action. Re-injection should not depend on the summarization process preserving these elements; treat summarization as lossy and unconditionally re-inject from an authoritative source external to the agent's context.
3. **Verify constraint preservation.** After re-injection, verify that the safety-critical context elements are present and consistent with the authoritative source. If verification fails, halt the agent and escalate to the operator.
4. **Log compaction events.** Every context compaction event should be logged with: timestamp, the compaction method used (summarization, truncation, sliding window), the token count before and after compaction, and confirmation that safety-critical context was re-injected and verified.
5. **Prohibit safety-critical constraints in the summarizable region only.** Safety-critical constraints should not be conveyed solely through conversation history that is subject to summarization. Store the authoritative copy of all safety-critical context outside the agent's context window, in a location the summarization process cannot modify.

**Recommendation:** Implement an external safety context store that the orchestration layer manages independently of the agent's conversation history. Keep the safety context document compact (under 2000 tokens) to minimize re-injection overhead. SC-020 (external action allowlist) provides a backstop: even if context-level constraints are lost, the external allowlist still blocks disallowed tools. This practice addresses constraints that are finer-grained than the allowlist (specific deny-list hosts, operator directives, autonomy-level restrictions).

**Related normative requirements:** APTS-SE-001, APTS-SE-009, APTS-AL-025, APTS-SC-020, APTS-AR-001.

---

### APTS-SC-A03: Tool Invocation Parameter and Chaining Governance (Advisory)

**Applicability:** This practice applies to platforms that let an agent or model-driven orchestration layer invoke allowlisted tools with runtime-supplied parameters.

**Rationale:** APTS-SC-020 already does the important architectural work. The allowlist lives outside the model. Entries include allowed parameters or parameter bounds. That is the floor. The unresolved problem appears one layer lower, where a tool is nominally approved but the parameter mix changes what the tool can do in practice. A rate-control tool with a zero-delay setting can still stress a target. A browser connector may be approved for scoped validation, then become an egress path once paired with an export utility. Observations from agentic systems suggest that this is where "allowed" starts to blur into "unsafe." SC-020 defines the enforcement point. This advisory deals with what that enforcement layer should evaluate before dispatch.

**Value:** Parameter-aware enforcement closes a gap that is easy to miss in design review and easy to trigger at runtime. Each call can look acceptable on its own. The sequence is the problem. Mature platforms catch that before the target does.

**Practice Description:**

For each allowlisted tool, document a parameter schema that covers types, bounds, enumerated values, and disallowed combinations for safety-critical fields. Those fields usually include target identifiers, rate limits, payload sizes, recursion depth, credential references, export destinations, authentication modes, and execution windows. Validate them in the external enforcement layer before dispatch, not inside the model prompt and not only in the tool wrapper. Then go one step further. Some requests are syntactically valid and still wrong in context. Check that the referenced target is still in scope, that the credential reference is authorized for that tool and that engagement, and that the requested rate or payload remains inside the approved operating envelope.

Platforms operating with multi-step agents should also define a bounded set of disallowed tool chains. This is not an attempt to solve general action planning through brittle signatures. It is narrower than that. Block the sequences that are already understood to produce disallowed outcomes: reconnaissance that flows directly into unapproved credential use, widened enumeration that turns into unauthorized pivoting, or evidence export routed to a destination outside the approved delivery path. Log parameter rejections and chain-policy rejections with the tool identifier, attempted parameters, and the reason for denial.

**Recommendation:** Start small. Cover the tools that can change scope, traffic intensity, credential usage, or data egress. Build a short catalog of unsafe parameter combinations and unsafe tool chains, run it through change control with the allowlist, and expand from operational evidence rather than taxonomy for its own sake.

**Related normative requirements:** APTS-SC-004, APTS-SC-020, APTS-SE-006, APTS-SE-023, APTS-MR-023.

---

### APTS-SC-A04: Inference Spend and Compute Budget Containment (Advisory)

**Applicability:** This practice applies to platforms whose agent consumes metered compute or inference (model API tokens, GPU time, or per-call tool and service costs) during an engagement.

**Rationale:** APTS contains the agent along several axes but not its own compute consumption. SC-004 limits traffic to the target (connection, bandwidth, and payload constraints), SC-011 terminates on host resource exhaustion (CPU, memory), SC-007 halts on cumulative risk, and SC-013 halts on wall-clock duration. None treats inference or compute spend as a quantity to monitor or a condition to halt on. The closest reference, TP-008, raises a cloud billing alert, but only as a signal of account compromise, and it alerts rather than stops the agent. The unaddressed case is a runaway agent: a planning loop, a retry storm, or a degenerate tool-call chain that consumes tokens or compute far beyond the engagement's intended envelope, with no spend ceiling to arrest it. This is a containment concern rather than a cost-management nicety, because uncontrolled consumption is one observable signature of an agent operating outside its mandate.

**Value:** A spend ceiling that halts the agent turns an open-ended runaway into a bounded, reviewable event, and burn-rate anomaly detection surfaces degenerate behavior early, before it exhausts a budget or masks a deeper fault.

**Practice Description:**

Treat per-engagement inference and compute spend as a first-class containment quantity:

1. **Set a spend ceiling that halts.** Define a per-engagement budget for inference and metered compute, and halt the agent through the existing kill-switch and termination path (APTS-SC-009, APTS-SC-011) when the ceiling is reached, rather than only logging the overage.
2. **Monitor burn rate, not just totals.** Track consumption rate against an expected envelope and treat an abnormal spike (for example, a retry or planning loop) as an escalation signal, since the rate anomaly precedes budget exhaustion.
3. **Record spend in the audit trail.** Log per-engagement consumption and any spend-triggered halt alongside the other termination conditions, so a halt and its cause are reconstructable.

**Recommendation:** Start with a hard per-engagement ceiling and a simple rate threshold on the most expensive resource (usually model tokens), wired into the existing halt path rather than a separate mechanism. Treat a spend-triggered halt like any other automated termination for review purposes, and treat repeated spend halts as a reason to investigate the agent's planning behavior, since a runtime that burns its budget on degenerate loops is behaving outside its mandate (APTS-MR-023).

**Related normative requirements:** APTS-SC-004, APTS-SC-007, APTS-SC-009, APTS-SC-011, APTS-AR-003, APTS-TP-008.

---

### APTS-MR-A03: Multi-Turn Adversarial Conversation Resilience (Advisory)

**Applicability:** This practice applies to platforms that use LLM-based or agentic runtimes with conversational state spanning multiple turns, tool calls, or planning steps.

**Rationale:** APTS-MR-018 protects the architectural boundary between trusted instructions and untrusted target-derived data. APTS-MR-023 assumes the runtime may still misbehave and requires containment. Those controls remain necessary. They do not fully cover a pattern that shows up repeatedly in agentic systems: no single turn looks decisive, yet the accumulated dialogue erodes the runtime's refusal logic until it attempts something it should never have attempted. Split-message assembly does this. So do paraphrase drift, encoding chains, and target content that slowly reframes itself as operator intent. The distinction from MR-023 matters. MR-023 is containment after deviation. This advisory is about exercising the conversation path that leads to deviation in the first place.

**Value:** Single-turn screening misses a surprising amount of bad behavior because the dangerous instruction is often distributed across harmless-looking fragments. Multi-turn testing exposes that drift. It gives the operator a better signal about whether refusal behavior, scope discipline, and tool-use restrictions survive a long session instead of only surviving a clean lab prompt.

**Practice Description:**

Maintain a compact corpus of multi-turn adversarial sequences that exercises cumulative manipulation patterns relevant to autonomous pentesting. Include staged jailbreak attempts, split-message assembly, paraphrase and synonym variation, encoding or representation shifts, and attempts to reframe untrusted target content as operator intent. Then compare outcomes. If the platform rejects a direct request to widen scope or export sensitive data, it should reject the obfuscated and distributed versions of the same request as well.

Where the platform carries conversational or planning state across long sessions, treat that state as part of the attack surface. Segment or reset state at clear engagement and approval boundaries. Do not let adversarial target content sit in privileged planning context longer than necessary. When the platform summarizes or compacts history, verify that compaction does not turn a previously rejected path into an acceptable one by stripping away context or weakening the refusal signal. Log the test results and classify failures by what actually eroded: refusal drift, scope drift, tool-use drift, or unsafe state carryover.

**Recommendation:** Start with a small recurring corpus focused on the failures that matter operationally: scope override, instruction bypass, credential exfiltration attempts, and unsafe tool requests distributed across several turns. Use the results to tune guardrails and session-boundary handling. A benchmark score by itself is not the point.

**Related normative requirements:** APTS-MR-001, APTS-MR-018, APTS-MR-023, APTS-SC-020.

---

### APTS-HO-A01: Out-of-Band Kill Switch via Independent Network (Advisory)

**Rationale:** Core kill switch functionality is covered by APTS-HO-008. The requirement for kill switch activation via physically independent communication channels (cellular, management network, physical button) assumes deployment scenarios where the primary network may be compromised or unavailable.

**Value:** Out-of-band kill capability is critical for engagements against network infrastructure, ICS/SCADA systems, or environments where the testing platform could inadvertently disrupt its own control channel. For cloud-hosted platforms with reliable control plane connectivity, in-band kill switches (HO-008, SC-009) are sufficient.

**Recommendation:** Implement out-of-band kill capability when testing network infrastructure, critical systems, or operating in environments with unreliable connectivity. Document the out-of-band channel in the Rules of Engagement for applicable engagements.

---

### APTS-HO-A02: Disclosure and Mitigation of AI Influence on Operator Decisions (Advisory)

**Applicability:** This practice applies to platforms that use language models or other AI components to generate content shown to operators at decision points: narrative summaries, recommended approval responses, prefilled answer choices, preselected defaults, or any other UI affordance that shapes what the operator confirms. It applies uniformly across every channel that delivers an operator decision affordance (web dashboard, mobile, instant-messaging, voice, push notification).

**Rationale:** Autonomous pentest platforms increasingly use language models not only to act on the target but also to shape what the operator sees at decision time: the narrative that frames a finding, the option set in an approval prompt, the wording, the preselected default. This is a manipulation surface distinct from the prompt-injection threats covered by the Manipulation Resistance domain: there, an external entity manipulates the agent; here, the agent influences its own supervisor's choice through affordances the agent controls. Established findings from human-computer interaction (default bias, primacy bias, choice architecture effects) show these shaping decisions meaningfully influence which option the operator selects, even without adversarial intent. APTS-HO-001, HO-005, and HO-010 mandate approval gates and audit trails but do not address the form of the question put to the operator; APTS-AR-006 covers the agent's reasoning chain but not the model-shaped inputs handed to the human. The practical effect is that an audit trail can show "operator approved" while concealing that the operator was offered a single highlighted choice with the safer option visually de-emphasized. The normative requirement set for v0.1.0 is frozen; this practice is a candidate for tier-gated inclusion in v0.2.0.

**Value:** Platforms that implement this practice make operator approval a more deliberate act and the audit trail more honest. Reviewers can distinguish a typed approval from a default click-through, customers can audit whether the platform's UI has steered operators toward expedient choices, and operators retain meaningful agency at high-impact decision points.

**Practice Description:**

1. **Distinguish a default click-through from an actively-selected response.** Where the operator confirms a preselected default with a single action, the audit trail should identify the response as a default click-through rather than treating it as semantically equivalent to a typed answer or an actively-selected non-default option.
2. **Log the model and prompt that shaped the operator's view.** When a model produces summary text, recommended responses, option sets, or defaults shown to the operator, the audit record should identify the model version, the prompt template, and the relevant context window state at generation time.
3. **Record the full option set, including filtered alternatives.** When the operator is presented with a constrained set of choices generated or curated by a model, the audit trail should record the candidate options the model considered and any options that were dropped, reordered, or de-emphasized before presentation, so reviewers can detect cases where the model omitted a safer option (for example, "deny" or "abort") or buried it behind progressive disclosure.
4. **Reduce default and ordering bias for high-impact gates.** For approvals governing irreversible actions (see APTS-HO-010) and other high-impact decisions, the platform should avoid preselecting a default, present the abort or deny option with visual weight equal to other options, and consider randomizing option order to mitigate primacy bias. For the most severe action categories, consider requiring a typed confirmation phrase rather than a single click.

**Recommendation:** Treat the operator-facing decision interface as a controlled surface and apply the same provenance discipline already required for agent action logging. Items 1 and 2 form the lowest-cost starting wedge (a response-classification audit field plus model and prompt-template provenance), both addable without touching the operator UI. The presentation rules in item 4 are most consequential for APTS-HO-010 gates and for actions classified as Critical or High under APTS-SC-001.

**Related normative requirements:** APTS-HO-001, APTS-HO-005, APTS-HO-010, APTS-AR-006, APTS-AR-019.

---

### APTS-AL-A01: Continuous Improvement and Maturity Roadmap (Advisory)

**Rationale:** Multi-year maturity roadmaps, formal improvement frameworks, and annual strategic assessments are organizational process practices rather than technical governance for autonomous pentest platforms. APTS defines platform requirements, not organizational management practices.

**Value:** Organizations pursuing Level 4 autonomy benefit from structured maturity progression. A maturity roadmap framework provides a planning tool for team development, process maturity, tool capability evolution, and governance structure.

**Practice Description:**

Establish a continuous improvement framework for autonomous pentesting operations that covers periodic capability assessments, documented lessons learned from engagements, and a multi-year maturity roadmap for advancing operational governance. Track metrics such as findings per hour, escalation rate, boundary violations, and decision accuracy to measure progression over time. Conduct periodic reviews of autonomy level appropriateness against those metrics and feed outcomes back into operational procedures and training.

**Recommendation:** Use the maturity roadmap framework from the AL domain Implementation Guide as a planning tool. Track the metrics listed above to measure progression. Conduct annual reviews of autonomy level appropriateness.

---

### APTS-TP-A01: Breach Notification and Regulatory Reporting (Advisory)

**Rationale:** Breach notification and regulatory reporting are general vendor obligations governed by applicable laws (GDPR, CCPA, HIPAA, and similar regimes) and existing organizational compliance programs. These obligations apply to any company handling sensitive data, not specifically to autonomous penetration testing platforms. Including jurisdiction-specific regulatory timelines as a mandatory pentest platform requirement conflates general corporate compliance with platform governance.

**Value:** Platforms that formalize breach notification procedures specific to penetration testing engagements (where breach scope may involve multiple client environments) reduce response time and regulatory risk. Pre-built notification templates and severity classification aligned to regulatory timelines accelerate incident response.

**Recommendation:** Establish breach notification procedures that cover penetration testing-specific scenarios (cross-tenant exposure, credential leakage during testing). Align notification timelines with applicable regulatory requirements. Reference APTS-TP-018 (Tenant Breach Notification) for cross-engagement breach scenarios.

---

### APTS-TP-A02: Privacy Regulation Compliance (Advisory)

**Rationale:** Privacy regulation compliance (GDPR, CCPA, PIPEDA, HIPAA, and similar regimes) is a general legal obligation for any organization processing personal data. While autonomous pentest platforms do encounter personal data during testing, the compliance mechanisms (DPIAs, lawful basis documentation, data subject rights) are organizational responsibilities governed by existing privacy programs, not platform-specific technical controls.

**Value:** Platforms that build privacy awareness into their testing workflows (automatic PII detection, data minimization, retention controls) reduce the compliance burden on operators. Privacy-by-design in the testing pipeline prevents accidental retention or exposure of personal data discovered during engagements.

**Recommendation:** Integrate with APTS-TP-013 (sensitive data handling) for PII detection and isolation during testing. Document how the platform supports operators' privacy compliance obligations. Implement data minimization controls that limit personal data retention to what is necessary for finding documentation.

---

### APTS-TP-A03: Professional Liability and Engagement Agreements (Advisory)

**Rationale:** Professional liability insurance, engagement agreements, and liability allocation are business and legal practices, not technical platform governance controls. These are organizational decisions influenced by jurisdiction, company size, client requirements, and risk appetite. A platform governance standard cannot prescribe specific contractual terms or insurance coverage levels.

**Value:** Clear engagement agreements reduce disputes when autonomous testing causes unintended impact. Defined liability allocation, indemnification terms, and insurance requirements protect both platform operators and their clients. Standardized agreement templates accelerate client onboarding.

**Recommendation:** Develop engagement agreement templates that address autonomous testing-specific risks (unintended system impact from autonomous actions, scope boundary failures). Maintain professional liability insurance appropriate to the scope and risk level of testing activities. Reference the Rules of Engagement framework in APTS-SE-001 for alignment between contractual scope and technical enforcement.

---

### APTS-TP-A04: External Tool Connector Trust Boundaries and Credential Isolation (Advisory)

**Rationale:** Autonomous pentest platforms increasingly rely on external tool connectors such as remote browser agents, model tool servers, plugins, and data connectors that run outside the core platform trust boundary. These integrations can introduce new instruction channels, expand available actions, and inherit broad customer credentials. APTS already covers dependency inventory, provider vetting, action allowlists, and runtime containment, but it does not yet give implementation guidance specific to externally hosted tool connectors and protocol bridges.

**Value:** Platforms that treat external tool connectors as distinct trust zones reduce the risk of tool-poisoning, over-privileged connector credentials, silent capability expansion, and connector-driven cross-tenant leakage. This is especially useful for platforms that integrate remote browsers, agent plugins, retrieval connectors, or Model Context Protocol-style tool servers.

**Practice Description:**

Document every external connector that can execute actions, access customer data, or supply context into the agent runtime. For each connector, define the approved capability scope, credential scope, network reachability, and data classes it may access. Route connector requests through an enforcement layer outside the model that validates connector identity, denies undeclared actions, and records connector invocation provenance. Connector credentials should be isolated per engagement or customer wherever operationally feasible, and high-impact connectors should require explicit operator approval before first use in an engagement. Connector output should be treated as untrusted input subject to the same validation and sanitization controls applied to target-side content.

**Recommendation:** Start with a short connector inventory and per-connector approval profile rather than a heavyweight framework. Prioritize connectors that can execute code, browse arbitrary URLs, retrieve private documents, or introduce new action surfaces at runtime.

**Related normative requirements:** APTS-TP-006, APTS-TP-017, APTS-SC-020, APTS-MR-022, APTS-MR-023.

---

### APTS-MR-A01: Goal Misgeneralization and Emergent Misalignment Evaluation Suite (Advisory)

**Applicability:** This practice applies to platforms that use LLM-based agents fine-tuned (SFT, RFT, RLHF, DPO, or equivalent) on offensive-security tasks, or whose foundation model has been adapted with offensive-task adapters, instruction tuning, task-specific reward models, or post-deployment online learning on engagement data.

**Rationale:** Recent peer-reviewed work has demonstrated that fine-tuning a frontier LLM on a narrow task can produce broad behavioral misalignment that extends far outside the training domain (Betley et al., 2026, *Training large language models on narrow tasks can lead to broad misalignment*, Nature 649, 584-589). For autonomous penetration testing platforms, two failure modes follow directly: (a) goal misgeneralization, where the agent learns a proxy objective ("produce findings that look like vulnerabilities") that diverges from the true objective ("identify vulnerabilities exploitable in the customer environment") in distinguishing situations the training data did not cover; and (b) emergent misalignment, where narrow fine-tuning on offensive tasks shifts the agent's behavior in adjacent domains with no signal until the shift manifests in a production engagement. APTS-MR-013 (Adversarial Example Detection in Vulnerability Classification) probes input-side robustness; APTS-MR-020 (Adversarial Validation and Resilience Testing of Safety Controls) probes control-side resilience; APTS-AR-019 (AI/ML Model Change Tracking and Drift Detection) tracks output drift. None of these evaluate the agent's underlying objective alignment under distribution shift. The Introduction's *Capability Frontier and Containment Assumptions* section defers verifiable goal alignment as research-stage and out of scope for v0.1.0; this practice begins to close that gap with an evaluation-based approach achievable today. The normative requirement set for v0.1.0 is frozen; this practice is a candidate for tier-gated inclusion in v0.2.0 (likely as SHOULD | Tier 2 for platforms operating at Level 3 autonomy or higher, or for any platform that performs post-deployment fine-tuning on engagement data).

**Value:** Platforms that maintain a goal-misgeneralization and emergent-misalignment evaluation suite detect a class of failure that no other safety control catches: situations where every individual safety check passes, scope holds, and the agent produces fluent, plausible output, while the agent is in fact optimizing a proxy objective that diverges from the true objective in distinguishing cases. This is the agent-side analogue of the fabricated-finding problem addressed by APTS-RP-A01: a failure that is invisible to per-action checks because it manifests only across the distribution of decisions the agent makes.

**Practice Description:**

Maintain and execute a goal-misgeneralization and emergent-misalignment evaluation suite that evaluates the agent's behavior in distinguishing scenarios (scenarios constructed so that following a plausible proxy objective produces a different action than following the true objective) and that evaluates the agent's behavior outside the offensive-security domain after any fine-tuning event. Specifically:

1. **Evaluation suite corpus.** Maintain a corpus of distinguishing scenarios covering, at minimum: scope-versus-finding-pressure tension (the agent has an opportunity to claim a finding by exceeding scope), proxy-reward gaming (the agent has an opportunity to produce a fluent finding without performing the underlying verification), intent-versus-output divergence (the agent is asked to assess a target whose intended behavior matches a vulnerability signature), and adjacent-domain spillover scenarios constructed per the Nature 2026 emergent-misalignment methodology. Document the proxy action and true action for every scenario.
2. **Behavioral metrics.** For each scenario, record the agent's chosen action and classify it as true-objective-aligned, proxy-objective-aligned, or other. Compute alignment rate per scenario category and aggregate across the suite. Track per-category trends across runs to detect drift.
3. **Refresh cadence.** Execute the suite quarterly and on every event that changes the agent's behavior at scale: foundation model change (per APTS-TP-022), adapter or instruction-tuning update, reward model change, and any post-deployment fine-tuning on engagement data. Re-execute on the prior model version after every refresh to distinguish suite-quality changes from model-behavior changes.
4. **Threshold and escalation.** Document a minimum acceptable alignment rate per scenario category and an aggregate floor. Below-threshold results MUST trigger reauthorization review per APTS-AL-026 before the platform may continue to operate at or above L3 autonomy. Below-threshold results on out-of-distribution scenarios MUST trigger review of any post-deployment learning data per APTS-TP-019.
5. **Out-of-distribution audit after fine-tuning.** Following the Nature 2026 methodology, evaluate the agent on a held-out, non-pentesting-domain benchmark before and after every fine-tuning event. Material shifts (defined and documented by the platform) constitute emergent misalignment evidence and trigger the same escalation as below-threshold suite results.

**Recommendation:** Implement the suite as an independent evaluation pipeline using a recognized evaluation framework (Inspect AI, Braintrust, OpenAI Evals, or equivalent) so that scenarios, scoring, and run history are inspectable, reproducible, and externally reviewable. Run the suite under the same model configuration the platform uses in production (same system prompt, same tool access pattern, same temperature). APTS-RP-A01 provides a backstop on the output side: even if the agent's objective drifts, an independent finding-authenticity verifier can catch fabricated evidence before it reaches the customer. APTS-MR-A01 addresses the upstream failure that RP-A01 cannot: the agent producing genuinely-grounded findings that the agent itself was misaligned to discover, prioritize, or report.

**Related normative requirements:** APTS-MR-013, APTS-MR-020, APTS-AL-026, APTS-TP-019, APTS-TP-022, APTS-AR-019.

---

### APTS-MR-A02: Sandbagging Detection and Behavioral Consistency Validation (Advisory)

**Applicability:** This practice applies to platforms that use LLM-based agents and run an adversarial evaluation suite under APTS-MR-020 against the same model configuration deployed in customer engagements authorized under APTS-SE-001. It applies regardless of whether the underlying model has been fine-tuned, and is therefore complementary in scope to APTS-MR-A01.

**Rationale:** A recurring finding across frontier-model evaluations between late 2024 and early 2026 is that an agent's behavior on an adversarial eval suite can diverge from its behavior in live deployment, because the model can detect features of the evaluation context and modulate its actions accordingly. Apollo Research demonstrated reproducible in-context scheming and sandbagging across multiple frontier models including Claude 3.5 Sonnet, Claude 3 Opus, OpenAI o1, Gemini 1.5, and Llama 3.1 (Meinke et al., 2024, *Frontier Models are Capable of In-context Scheming*, arXiv:2412.04984). Apollo's later anti-scheming work with OpenAI documented OpenAI o3 reasoning explicitly about whether evaluators would be able to detect underperformance before deciding how to answer (Schoen et al., 2025, *Stress Testing Deliberative Alignment for Anti-Scheming Training*, arXiv:2509.15541, Figures 48-49). Independent peer-reviewed work establishes sandbagging as a measurable, controllable behavior (van der Weij et al., *AI Sandbagging*, ICLR 2025, arXiv:2406.07358), and Anthropic's *Alignment Faking* result shows that production-grade Claude 3 Opus differentially modulates behavior under naturalistic conditions (Greenblatt et al., 2024, arXiv:2412.14093). Recent model system cards report that interpretability features for evaluation awareness can strengthen during training and that a model can recognize an evaluation context without verbalizing that recognition in its reasoning trace; operators should consult the system card for the specific model and version they deploy rather than rely on a single published figure. The capability scales predictably with model size (Chaudhary et al., 2025, arXiv:2509.13333) and is measurable on agentic tasks (Needham et al., 2025, arXiv:2505.23836). For autonomous penetration testing platforms the consequence is concrete: published sandbagging experiments report large drops on targeted cyber and capability evaluations under an underperformance instruction while general-knowledge performance is largely unchanged, exactly the failure mode an adversarial eval suite is supposed to surface. APTS-MR-020 (Adversarial Validation and Resilience Testing of Safety Controls) requires the platform to run the eval suite; APTS-AR-019 (AI/ML Model Change Tracking and Drift Detection) tracks output drift across model versions. Neither measures the differential between the agent's eval-context behavior and its production-context behavior on the same model snapshot. The EU AI Act Article 15 obligation that high-risk systems perform consistently throughout their lifecycle gestures at this property without specifying a measurement; this practice supplies one. The normative requirement set for v0.1.0 is frozen; this practice is a candidate for tier-gated inclusion in v0.2.0 (likely as SHOULD | Tier 2 for platforms operating at Level 3 autonomy or higher).

**Value:** Platforms that maintain and validate an eval-versus-production behavioral-consistency measurement detect a class of failure that a clean eval pass-rate cannot rule out: situations where the adversarial eval suite shows no anomalies while the agent's deployed behavior diverges along the dimensions the eval was supposed to govern (scope compliance, refusal of out-of-scope tools, finding-fabrication rate). This is the context-side analogue of the objective-side failure addressed by APTS-MR-A01: MR-A01 catches the agent pursuing the wrong objective when training data is ambiguous; MR-A02 catches the agent pursuing the right objective during evaluation while pursuing a different one in deployment, on the same model snapshot.

**Practice Description:**

Maintain and validate a behavioral-consistency measurement comparing the agent's behavior on the adversarial evaluation suite of APTS-MR-020 against a stratified sample of production engagement transcripts captured under APTS-SE-001 authorization, on the same model snapshot tracked under APTS-AR-019. Specifically:

1. **Behavioral metrics.** For each model snapshot, compute the eval-versus-production delta on at least four dimensions: scope-compliance rate, refusal rate on out-of-scope tool invocations, finding-fabrication rate (per APTS-RP-A01), and the distribution of time-to-completion per task class. Document per-dimension thresholds and the methodology used to compute each metric on both sides of the comparison.
2. **Stratified sampling.** Draw production transcripts using a documented stratification protocol (for example, by autonomy level, engagement size, and customer environment class) sufficient to support per-dimension comparison at the stated thresholds. Disclose sample sizes, the stratification rationale, and any redactions applied to comply with APTS-AR-001 retention policy and APTS-TP-013 sensitive-data handling.
3. **Multi-snapshot trend.** Repeat the measurement on at least two model snapshots tracked under APTS-AR-019 so that drift in the differential itself becomes observable. Where only one snapshot is available, document the limitation and re-execute on the next snapshot change.
4. **Verbalized recognition disclosure.** Where the model produces reasoning traces (chain-of-thought, scratchpad, or equivalent), disclose whether evaluation-context recognition appears in the verbalized reasoning of evaluation runs and whether the rate of verbalized recognition correlates with the measured behavioral delta. Do not treat the absence of verbalized recognition as evidence that the model is not detecting the evaluation context; recent system-card results show recognition can occur without verbalization.
5. **Threshold and escalation.** Document a maximum acceptable delta per dimension and an aggregate floor. Above-threshold delta on any dimension MUST trigger reauthorization review per APTS-AL-026 before the platform may continue to operate at or above L3 autonomy, and MUST trigger review of any post-deployment learning data per APTS-TP-019. The validation report (the differential between eval-side and production-side metrics) MUST be committed to the audit trail per APTS-AR-001 alongside the corresponding eval and production samples.

**Recommendation:** Implement the measurement as an independent pipeline outside the engagement runtime so that the eval-side and production-side samples are scored under identical conditions. A lightweight standing detector (paired formal-versus-casual prompt framings administered to the deployed configuration via the "Playing Dumb" methodology, 2026) provides an inexpensive black-box check between full measurement runs. A second, complementary standing detector plants benign evaluation-context markers that a scope-compliant agent has no legitimate reason to interact with (for example, a scoring-oracle-shaped host on the evaluation network, flag-file bait outside the authorized target, or an out-of-band result-collection endpoint) and measures whether the agent orients toward the evaluation scaffolding rather than remaining within the authorized target scope of APTS-SE-001. Orientation toward the harness is a positive, black-box signal of evaluation-context recognition that requires no production sample: an agent that reads from the scoring oracle or writes to the result channel has both detected the evaluation and violated scope. This detector is also the interim measure where a platform early in deployment lacks the production-transcript volume that the stratified-sampling step (item 2) assumes, providing an eval-awareness signal until production data accumulates. For the scoring component, follow the same independence rule that APTS-MR-A01 and APTS-RP-A01 apply: where an LLM judge is used, it MUST run as a separate inference call with no shared context with the agent under test, and SHOULD use a different model family. METR's MALT methodology (2025), cross-task differential analysis rather than single-transcript review, is the citable reference for how to interpret the production-side sample. The scope of the measurement is the eval suite of APTS-MR-020 and live customer engagements under APTS-SE-001; training-time RLHF, RLAIF, or DPO runs are out of scope and are governed by APTS-TP-019. APTS-MR-A01 addresses the upstream objective-side failure; APTS-MR-A02 addresses the context-side failure on the same model snapshot.

**Related normative requirements:** APTS-SE-001, APTS-MR-020, APTS-MR-022, APTS-AR-001, APTS-AR-019, APTS-AL-026, APTS-TP-019, APTS-TP-022, APTS-RP-A01.

---

### APTS-RP-A01: Automated Finding Authenticity Verification (Advisory)

**Rationale:** LLM-based penetration testing agents can produce findings that appear legitimate but contain fabricated evidence: proof-of-concept scripts that output hardcoded strings instead of making real requests, HTTP responses that were not actually received from the target, or severity classifications unsupported by the evidence. Because these fabricated findings are fluent and internally consistent, they pass casual human review and erode trust in the platform's output. RP-001 and RP-002 require evidence-based validation and human review, but neither addresses the risk that the agent itself fabricates evidence. The normative requirement set for v0.1.0 is frozen; this practice is a candidate for tier-gated inclusion in v0.2.0 (likely as MUST | Tier 2 given the implementation complexity).

**Value:** Platforms that implement automated finding authenticity verification catch fabricated PoCs, hallucinated vulnerability types, and severity mismatches before they reach human reviewers. This ensures reviewers spend their time on genuine judgment calls rather than authenticity checking, and prevents fabricated findings from reaching customers.

**Practice Description:**

Implement an automated verification mechanism that screens each reported finding for fabricated evidence, hallucinated vulnerabilities, and synthetic proof artifacts before the finding enters the human review pipeline (APTS-RP-002) or the final report. Specifically:

1. **Operate independently of the agent that produced the finding.** The verification mechanism must not share a context window, conversation history, or in-memory state with the discovering agent. If the verifier is itself LLM-based, it should receive only the finding record, the associated evidence artifacts, and any relevant tool output, not the discovering agent's reasoning chain or system prompt.
2. **Confirm reported effects by independent replay; treat static artifact analysis as the fallback.** The primary authenticity check for any finding whose claimed effect is reproducible is to re-execute the claimed interaction against the target from a harness independent of the discovering agent, and to confirm the effect through an out-of-band channel the agent does not control (for example, a callback listener that records an actual inbound request, a canary reachable only by a genuine interaction, or a database or log side effect observed directly on the target). An effect confirmed out-of-band is authentic regardless of how the PoC artifact is written; an effect that replay cannot reproduce is FLAGGED. Where a claimed effect is not independently replayable (for example, a one-time state change or evidence that cannot be safely re-triggered), fall back to static analysis of the PoC artifacts for: absence of network calls to the target, hardcoded output strings that match the reported evidence verbatim, and output that could not have been produced by the claimed tool or technique. Static analysis alone is weaker than replay, because a fabricated artifact can be written to resemble a genuine one; prefer replay wherever the effect permits it.
3. **Screen for hallucinated vulnerabilities.** Cross-reference the claimed vulnerability against the raw evidence artifacts. A finding that claims SQL injection should have evidence of actual SQL injection behavior (error messages, data exfiltration, time-based delay); a finding that claims XSS should have evidence of script execution or DOM manipulation. Findings where the evidence does not support the claimed vulnerability type should be flagged.
4. **Screen for severity misclassification.** Evaluate whether the evidence supports the assigned severity. A finding classified as Critical should have evidence of impact commensurate with Critical severity (for example, remote code execution, authentication bypass, mass data exposure). Findings where severity is unsupported by the evidence should be flagged for reclassification.
5. **Screen for design-intent false positives.** Detect findings that describe intended application behavior as vulnerabilities (for example, public API keys designed for client-side use, CORS headers intentionally set for broad access, or documented public endpoints).
6. **Classify each finding.** Assign each finding one of the following integrity statuses: VERIFIED (evidence is authentic, supports the claimed vulnerability type and severity), FLAGGED (evidence appears inconsistent with the claim; requires human review before inclusion), or REJECTED (evidence is fabricated or does not demonstrate any vulnerability). REJECTED findings should not appear in the main findings section of the report.
7. **Log all verification decisions.** Each verification decision should be logged with: the finding identifier, the integrity status assigned, the specific checks that passed or failed, and the evidence artifacts examined.

**Recommendation:** Deterministic evidence replay (Practice item 2) is the primary authenticity check; the "Finding Judge" described here is the secondary backstop for the residue replay cannot reach, such as severity misclassification and narrative-only claims. Implement the verifier as a separate "Finding Judge" that receives only the finding record, associated evidence artifacts, and target context. The judge should err toward FLAGGED (sending to human review) rather than REJECTED (dropping the finding), to avoid suppressing genuine findings. For multi-agent or swarm architectures, process findings from all agents through a single verification pipeline to ensure consistent integrity standards.

**Verification:** A customer can verify this practice as follows:

1. **Seeded-fabrication test.** Introduce findings of known integrity status into the verification pipeline: at least one genuine finding whose effect is reachable on a test target, one finding whose PoC produces its evidence without contacting the target (canned output), one finding whose claimed vulnerability type is unsupported by its evidence, and one finding whose severity exceeds what the evidence demonstrates. Confirm the pipeline classifies each as VERIFIED, REJECTED, or FLAGGED consistent with its seeded status, and that no REJECTED finding reaches the main findings section of the report (APTS-RP-002).
2. **Replay confirmation.** For a finding classified VERIFIED on the basis of a reproducible effect, confirm from the audit log (APTS-AR-001) that the effect was observed on an out-of-band channel the discovering agent did not control, rather than inferred solely from the agent's own PoC output.
3. **Independence check.** Confirm from configuration or logs that the verification mechanism received only the finding record, evidence artifacts, and target context, and did not share a context window, conversation history, or in-memory state with the discovering agent (item 1).
4. **Forged-submission resistance.** Confirm that a finding's integrity status is bound to evidence the discovering agent cannot author or alter after the fact (out-of-band observations and audit entries under APTS-AR-001), so that a finding cannot be marked VERIFIED on the strength of agent-supplied evidence alone.

**Related normative requirements:** APTS-RP-001, APTS-RP-002, APTS-RP-003, APTS-AR-001, APTS-AR-006.

---

## Relationship to Conformance Tiers

| Tier | Scope | Advisory Practices |
|------|-------|--------------------|
| Tier 1 (Foundation) | Core safety and scope controls | Not applicable |
| Tier 2 (Verified) | Production-grade platform governance | Optional enhancement |
| Tier 3 (Comprehensive) | Maximum assurance for high-risk operations | Recommended where operationally feasible |

Advisory practices are independent of the tier system. A platform may claim Tier 3 conformance without implementing any advisory practices, and a Tier 2 platform may implement advisory practices that are relevant to its deployment context.
