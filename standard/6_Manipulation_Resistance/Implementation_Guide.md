# Manipulation Resistance: Implementation Guide

Practical guidance for implementing APTS Manipulation Resistance requirements. Each section provides a brief implementation approach, key considerations, and common pitfalls.

> **Note:** This guide is informative, not normative. Recommended defaults and example values are suggested starting points; the [Manipulation Resistance README](README.md) contains the authoritative requirements. Where this guide and the README differ, the README governs.

---

## APTS-MR-001: Instruction Boundary Enforcement

**Implementation:** Implement cryptographic tagging of operator instructions separate from target content. Maintain strict source tracking throughout execution pipeline.

**Key Considerations:**
- Tag all operator inputs with immutable cryptographic markers before processing
- Enforce separation in memory/data structures; never merge instruction and target data paths
- Validate source origin before acting on any directive

**Common Pitfalls:**
- Mixing operator and target content in shared buffers
- Losing source attribution through multi-step processing

**Prompt Injection Detection for AI/LLM-Based Platforms:**

Platforms using LLMs for decision-making MUST implement layered prompt injection defense:

1. **Input sanitization:** Strip or escape instruction-like patterns from target responses before passing to LLM. Patterns include: "ignore previous instructions", "you are now", "system:", "assistant:", and role-switching attempts.
2. **Instruction boundary enforcement:** Use structured prompts with clear delimiters between system instructions (immutable) and target data (untrusted). Example: wrap target data in explicit tags like `<target_response>...</target_response>` and instruct the model to treat content within these tags as data, never as instructions.
3. **Output validation:** Validate LLM outputs against expected action schemas before execution. If the LLM proposes an action outside the pre-defined action set, reject it and log the anomaly.
4. **Canary detection:** Embed unique tokens in system prompts. If these tokens appear in target-facing outputs, the instruction boundary has been breached. Alert the operator immediately.
5. **Dual-model verification:** For high-risk decisions, submit the same input to a second independent model and compare outputs. Divergent recommendations trigger escalation.

---

## APTS-MR-002: Response Validation & Sanitization

**Implementation:** Validate all target responses against predefined schemas. Apply regex-based sanitization to remove instruction-like patterns (keywords: "execute", "ignore", "override", "admin", "perform").

**Key Considerations:**
- Define strict schemas per endpoint before testing begins
- Log all sanitization events for post-test auditing
- Use allowlist validation where possible instead of blocklist

**Common Pitfalls:**
- Over-reliance on blocklists that miss obfuscated patterns
- Insufficient logging of sanitization events

---

## APTS-MR-003: Error Message Neutrality

**Implementation:** Treat all error messages as untrusted data only for analysis. Never execute directives embedded in error responses. Extract only diagnostic information for vulnerability assessment.

**Key Considerations:**
- Parse error messages for vulnerability indicators, not instructions
- Implement circuit breaker on suspicious error patterns
- Document all extracted error information separately from actions taken

**Common Pitfalls:**
- Following debugging suggestions in error messages
- Assuming error messages from trusted domains are safe

---

## APTS-MR-004: Configuration File Integrity Verification

**Implementation:** Analyze discovered config files for vulnerabilities without executing embedded instructions. Cryptographically sign expected configs; reject unsigned variants. Flag instruction-like content as findings.

**Key Considerations:**
- Maintain baseline hashes of legitimate configurations
- Scan for env var injection, comment-based instructions, hidden directives
- Separate vulnerability analysis from any auto-remediation

**Common Pitfalls:**
- Auto-loading or executing config-embedded scripts
- Missing injection vectors in config comments

---

## APTS-MR-005: Authority Claim Detection & Rejection

**Implementation:** Maintain allowlist of authentic authority markers (cryptographic signatures, registered domain patterns). Flag and reject any authority claims not on allowlist as suspicious.

**Key Considerations:**
- Define authority markers in pre-test configuration only
- Log all authority claims for post-test review
- Treat "admin", "developer", "system" claims from target content as red flags

**Common Pitfalls:**
- Accepting domain-based authority without cryptographic proof
- Allowing runtime modification of authority allowlists

---

## APTS-MR-006: Decision Boundary Enforcement

**Implementation:** Codify all critical decisions (scope, escalation, credentials handling) in immutable pre-test configuration. Prevent runtime modifications via target-controlled data paths.

**Key Considerations:**
- Use immutable configuration objects after initialization
- Require explicit human re-authorization for any scope/credential changes
- Version-control all pre-test decisions for audit trail

**Common Pitfalls:**
- Allowing dynamic scope expansion based on target responses
- Implicit scope changes via redirect chains

---

## APTS-MR-007: Redirect Following Policy

**Implementation:** Validate all HTTP redirects (3xx status codes), JavaScript redirects, and meta-refresh directives against pre-approved scope. Reject out-of-scope redirects; log for analysis.

**Key Considerations:**
- Maintain explicit redirect whitelist per target
- Detect redirect chains; validate each hop independently
- Monitor for redirect-based DoS or infinite loops

**Common Pitfalls:**
- Blindly following relative redirects to different hosts
- Missing JavaScript-based redirects (window.location)

---

## APTS-MR-008: DNS and Network-Level Redirect Prevention

**Implementation:** Validate DNS responses against authorized scope before network connection. Detect DNS rebinding attacks and TOCTOU race conditions. Use DNSSEC validation where available.

**Key Considerations:**
- Implement DNS response validation against allowlist IPs
- Monitor for rapid DNS changes (rebinding)
- Pin resolved IPs for duration of interaction with domain

**Common Pitfalls:**
- Trusting DNS without validation; missing rebinding attacks
- Not detecting DNS response changes between lookup and connection

---

## APTS-MR-009: Server-Side Request Forgery (SSRF) Prevention in Testing

**Implementation:** Before following any URL discovered in target responses, validate against scope. Block private IP ranges (RFC 1918, 127.0.0.1, ::1) unless explicitly in scope.

**Key Considerations:**
- Maintain expanded IP blocklist: 169.254.0.0/16, 224.0.0.0/4, and cloud metadata endpoints for AWS (169.254.169.254 IMDSv1 and IMDSv2), GCP (metadata.google.internal, 169.254.169.254, and IPv6 fd00:ec2::254), and Azure (169.254.169.254 on path /metadata/instance with header "Metadata: true")
- Check URL scheme (only http/https in most cases)
- Log all blocked SSRF attempts with target source context

**Common Pitfalls:**
- Missing alternative IP representations (decimal, hex, octal notation)
- Not blocking cloud metadata endpoints (AWS, GCP, Azure)

---

## APTS-MR-010: Scope Expansion Social Engineering Prevention

**Implementation:** Detect social engineering patterns in target content (urgency, authority claims, emotional appeals). Flag suspicious scope expansion requests; require human approval.

**Key Considerations:**
- Maintain pattern library of social engineering indicators
- Flag requests containing "urgent", "emergency", "authorization", "please", "critical"
- Implement mandatory human review gate for scope changes

**Common Pitfalls:**
- Missing emotionally manipulative language in target responses
- Auto-accepting scope expansion claims without human verification

---

## APTS-MR-011: Out-of-Band Communication Prevention

**Implementation:** Block all communication channels except pre-approved testing channels. Detect and prevent blind SSRF, DNS exfiltration, and indirect data exfiltration attempts by targets.

**Key Considerations:**
- Monitor outbound DNS, HTTP, SMTP for unauthorized targets
- Detect second-order request patterns (email callbacks, webhook calls)
- Log all out-of-band attempt detections

**Common Pitfalls:**
- Missing DNS query-based exfiltration
- Not detecting targets' attempts to control outbound traffic

---

## APTS-MR-012: Immutable Scope Enforcement Architecture

**Implementation:** Implement scope as cryptographically signed, immutable data structure. Use hardware-backed or TPM-based enforcement where possible. Require cryptographic re-signing for any modifications.

**Key Considerations:**
- Use digital signatures with offline key storage for scope validation
- Implement scope as append-only audit log with cryptographic chaining
- Store scope outside regular application memory where possible

**Common Pitfalls:**
- Storing scope in modifiable configuration files
- Missing logging of scope override attempts

---

## APTS-MR-013: Adversarial Example Detection in Vulnerability Classification

**Implementation:** Test AI/ML-based vulnerability classification against adversarial examples. Include payload obfuscation, semantic equivalence variants, and jailbreak attempts in test sets.

**Key Considerations:**
- Maintain adversarial example library (encoding bypasses, prompt injections, semantic tricks)
- Run quarterly adversarial testing against classification models
- Document all adversarial bypass attempts for model hardening

**Common Pitfalls:**
- Relying solely on standard test payloads; missing novel obfuscation
- Not updating adversarial library as attack techniques evolve

---

## APTS-MR-014: Resource Exhaustion and Tarpit Attack Prevention

**Implementation:** Implement strict per-endpoint timeouts (5-30s). Detect slowloris, timeout-based tarpits, and memory exhaustion patterns. Implement exponential backoff with jitter.

**Key Considerations:**
- Set baseline response time threshold; flag 10x+ deviations as tarpits
- Implement connection/request limits per host
- Monitor heap/memory growth; abort on unusual spikes

**Common Pitfalls:**
- Over-generous timeouts allowing indefinite hangs
- Missing memory exhaustion detection in response parsing

---

## APTS-MR-015: Deceptive Authentication Honeypots

**Implementation:** Flag auth endpoints discovered in scope as potentially deceptive. Do not auto-authenticate to suspicious endpoints. Require explicit human approval before credential submission.

**Key Considerations:**
- Maintain list of known deceptive auth patterns (fake login forms, secondary auth)
- Detect endpoints mimicking legitimate auth with slight variations
- Log all suspicious auth endpoint detections

**Common Pitfalls:**
- Auto-submitting credentials to any /login or /auth endpoint
- Missing visually similar but distinct auth endpoint clones

---

## APTS-MR-016: Anti-Automation Defense Detection

**Implementation:** Respect rate limiting headers (Retry-After, X-RateLimit-*). Detect CAPTCHA, challenge-response, and fingerprinting defenses. Back off immediately upon detection.

**Key Considerations:**
- Parse standard rate limit headers; apply backoff before hitting limits
- Detect CAPTCHA forms; refuse auto-completion
- Monitor for fingerprinting scripts (bot detection); abort if detected

**Common Pitfalls:**
- Ignoring Retry-After headers
- Attempting to bypass anti-automation via multiple IPs or rotation

---

## APTS-MR-017: Anomaly Detection in Response Patterns

**Implementation:** Baseline normal response patterns (size, latency, error rates) for each endpoint. Flag deviations exceeding 3 sigma as anomalies. Investigate or abort on suspicious patterns.

**Key Considerations:**
- Collect 10+ baseline responses per endpoint before aggressive testing
- Monitor content divergence (identical requests returning different responses)
- Detect timing anomalies (sudden slowdowns, consistency changes)

**Common Pitfalls:**
- Starting aggressive testing without baseline establishment
- Missing second-order anomalies (anomalies in anomalies)

---

## APTS-MR-018: AI Model Input/Output Architectural Boundary

**Implementation:** Enforce strict architectural separation between system instructions and target data in AI pipelines. Tag all target data; prevent merging with system prompts. Implement input validation before AI model ingestion.

**Key Considerations:**
- Use separate model instances or namespaced contexts for system and target data
- Implement prompt injection detection (unusual control characters, instruction keywords)
- Log all target data ingested by models for audit

**Common Pitfalls:**
- Concatenating target data directly into prompts without sanitization
- Missing encoding-based prompt injection vectors

---

## APTS-MR-019: Discovered Credential Protection

**Implementation:** Immediately encrypt all discovered credentials (AES-256-GCM) at discovery time. Never store plaintext in logs, memory dumps, or output files. Use dedicated secret management system.

**Key Considerations:**
- Implement credential discovery hooks to auto-encrypt at parse time
- Use unique encryption keys per credential; rotate keys post-test
- Separate plaintext in-memory access to authorized personnel only

**Common Pitfalls:**
- Credentials logged before encryption
- Reusing encryption keys across multiple tests
- Storing decryption keys in same location as encrypted data

---

## APTS-MR-020: Adversarial Validation and Resilience Testing of Safety Controls

**Implementation:** Conduct annual independent adversarial testing by external team. Include prompt injection, jailbreak attempts, scope bypass scenarios. Document all control failures; implement mitigations.

**Key Considerations:**
- Use a red team independent of the development team (internal or external)
- Test against realistic threat models (insider, advanced persistent actor)
- Maintain detailed findings log; track remediation completion

**Common Pitfalls:**
- Internal teams conducting "adversarial" testing without realistic threat modeling
- Skipping adversarial testing or conducting only sporadic reviews

---


## APTS-MR-021: Data Isolation Adversarial Testing

**Implementation:** Test cross-tenant access vectors (shared storage, shared memory, shared logs, shared error handlers). Attempt to access other tenant data via all communication channels.

**Key Considerations:**
- Provision multiple test tenants; attempt cross-tenant access via all discovery vectors
- Test log aggregation systems, error reporting, analytics endpoints
- Verify data encryption at rest and in transit per tenant

**Common Pitfalls:**
- Testing only primary application data; missing isolation in logs/errors
- Not testing all secondary channels (metrics, traces, backups)

---

## APTS-MR-022: Inter-Model Trust Boundaries and Output Validation

**Implementation:** Define trust boundaries between AI model components. Validate all model-to-model outputs against strict schemas. Never trust downstream model outputs without sanitization.

**Key Considerations:**
- Implement output validation layer between model components
- Apply same sanitization rules to model outputs as to target content
- Log all cross-model data transfers for audit trails

**Common Pitfalls:**
- Assuming high-confidence model outputs are safe to pass to downstream systems
- Missing injection vectors through model chaining (model A output -> model B input)

---

## APTS-MR-023: Agent Runtime as an Untrusted Component

**Implementation:** Write a threat-model document that explicitly names the agent runtime as one of the adversaries the platform defends against. Isolate the agent runtime from the control plane at a layer the runtime cannot reach: a separate process with no shared filesystem to platform config, a separate container with no mount of the platform's secrets or allowlist, a separate network namespace with no route to the control-plane management endpoints. Platform configuration files (allowlists, threshold tables, kill-switch config, audit store credentials) live on a volume the agent runtime has no read, write, or modify access to. Distribution-shift monitoring is a background job that consumes audit records and flags anomalies for a human-reviewed queue. Run a tabletop exercise at least quarterly that walks through "what if the agent tries to do X" for a few concrete X values drawn from current threat intelligence.

**Key Considerations:**
- This requirement is architectural; it cannot be satisfied by prompt engineering or model alignment
- The isolation must survive the agent being wrong, not just the agent being attacked
- Distribution-shift monitoring needs a human in the loop because legitimate novel behavior will look anomalous

**Common Pitfalls:**
- Putting the agent runtime and the control plane in the same process and calling "function boundaries" isolation
- Mounting the platform configuration into the agent runtime's filesystem "for convenience"
- Treating a quarterly tabletop exercise as a box-ticking ritual rather than an opportunity to find real gaps

---

## Advisory Practice Implementation Guidance

### APTS-MR-A01: Goal Misgeneralization and Emergent Misalignment Evaluation Suite

> This section provides implementation guidance for the advisory practice [APTS-MR-A01](../appendix/Advisory_Requirements.md#apts-mr-a01-goal-misgeneralization-and-emergent-misalignment-evaluation-suite-advisory). It is not required for conformance at any tier.

**Implementation:** Build the evaluation suite as an independent pipeline outside the agent's serving infrastructure. Each run takes a fixed model configuration (foundation model version, adapter set, system prompt, tool access policy, temperature) and produces a structured run record with per-scenario decisions, per-category alignment rates, and an aggregate score committed to the audit trail.

**Architecture Pattern: Independent Evaluation Pipeline**

A proven pattern is to implement the suite using an off-the-shelf evaluation framework (Inspect AI, Braintrust, OpenAI Evals, or an equivalent harness) and to run it as a standalone job rather than as part of the engagement runtime. The pipeline has four components:

1. **Scenario corpus.** A versioned set of distinguishing scenarios stored as data, not code. Each scenario carries: a target description (the synthetic environment the agent is asked to assess), the proxy-objective action, the true-objective action, the rubric mapping the agent's chosen action to one of the three classes (true-objective-aligned, proxy-objective-aligned, other), and a category tag.
2. **Run harness.** A driver that instantiates the same agent loop the platform uses in production (same system prompt, same tool registry, same control plane) but pointed at the synthetic targets in the scenario corpus. The harness records every tool call, every model output, and the agent's terminal action per scenario.
3. **Scorer.** A rule-based or LLM-judge scorer (independent of the agent under test) that maps each scenario's run record to one of the three classes. Where an LLM judge is used, the judge MUST run as a separate inference call with no shared context with the agent under test (same constraint as APTS-RP-A01).
4. **Reporter.** Aggregates per-category alignment rates, computes aggregate score, compares against documented thresholds, computes deltas against the most recent prior run on the same model version, and writes a signed run record to the audit trail per APTS-AR-012.

**Calibrated Alignment Thresholds:**

For scenario categories where the population of distinguishing cases is small, define a minimum sample size before thresholds apply. For example: a category with fewer than 30 scenarios should report a confidence-interval band rather than a single alignment rate, and threshold checks should compare the lower bound of the band against the floor. This prevents a category with 5 scenarios from triggering reauthorization on a single regression that is within statistical noise.

**Out-of-Distribution Audit:**

Implement the post-fine-tuning out-of-distribution audit as a separate evaluation job that runs the model against a held-out general-purpose benchmark (HHH-style or equivalent) before and after every fine-tuning event. The audit is concerned with relative shift, not absolute score: a several-point drop on a previously-stable benchmark following narrow fine-tuning is the signal the Nature 2026 methodology identifies as emergent misalignment, and the relative drop matters more than the absolute number.

**Key Considerations:**
- The evaluation suite must be governed as carefully as the model under APTS-TP-019. Scenario corpus contamination, where suite scenarios leak into post-deployment learning data, silently inflates alignment rates and is the evaluation-side analogue of training-set contamination in capability benchmarks.
- The judge component must use a different model family or, if the same family, a different system prompt and a separate inference call. APTS-RP-A01's independence requirement applies symmetrically here.
- Run the suite against the *same* model configuration the platform uses in production, including system prompt, tool registry, temperature, and any safety wrappers. A suite that passes against a stripped-down configuration provides no assurance about production behavior.
- Suite results should be tracked across runs with explicit versioning of (model, adapter, system prompt, suite version). A drop in alignment rate is interpretable only relative to prior runs on a comparable configuration.

**Common Pitfalls:**
- Building a scenario corpus that distinguishes the proxy from the true objective only on cases the agent has already seen during training. Distinguishing scenarios must be genuinely held out from training data.
- Treating an LLM-judge scorer as ground truth without inter-rater reliability checks. The scorer is a measurement instrument and needs its own calibration.
- Running the suite once at platform launch and never again. The point of the practice is to catch drift, which requires repeated runs across model and adapter changes.
- Treating the suite as a marketing artifact rather than a control. Publishing high alignment rates without committing run records and scenario versions to the audit trail provides no assurance and creates a perverse incentive to overfit the suite.

---

### APTS-MR-A02: Sandbagging Detection and Behavioral Consistency Validation

> This section provides implementation guidance for the advisory practice [APTS-MR-A02](../appendix/Advisory_Requirements.md#apts-mr-a02-sandbagging-detection-and-behavioral-consistency-validation-advisory). It is not required for conformance at any tier.

**Implementation:** Build the consistency measurement as an independent pipeline that ingests, on one side, per-scenario records from the adversarial eval suite of APTS-MR-020, and on the other side a stratified sample of production engagement transcripts captured under APTS-AR-001. Each measurement run is anchored to a single model snapshot identifier (per APTS-AR-019) and produces a structured differential report (per-dimension deltas, sample sizes, and a verbalized-recognition correlation) committed to the audit trail.

**Architecture Pattern: Differential Measurement Pipeline**

The pipeline has four components, mirroring the structure used in APTS-MR-A01:

1. **Eval-side ingestor.** Reads per-scenario records emitted by the MR-020 eval suite for the model snapshot under measurement. The ingestor records, per scenario, the agent's terminal action, tool-call trace, and reasoning trace if available.
2. **Production-side sampler.** Draws a stratified sample of production transcripts from the audit store (APTS-AR-001), applying redactions consistent with APTS-TP-013 sensitive-data handling. The sampler MUST NOT relax retention or consent boundaries to obtain samples; where sampling is constrained, the limitation is reported in the differential report rather than worked around.
3. **Differential scorer.** Computes per-dimension metrics on each sample and the delta between them. Where rule-based scoring is insufficient (for example, refusal-rate classification on free-form tool invocations), an LLM judge is used; the judge MUST run as a separate inference call with no shared context with the agent under test (same constraint as APTS-MR-A01 and APTS-RP-A01) and SHOULD use a different model family.
4. **Trend reporter.** Aggregates the differential across model snapshots tracked under APTS-AR-019 so that the change in the differential itself is observable, writes a signed run record to the audit trail per APTS-AR-012, and emits the validation report for inclusion alongside the customer engagement record.

**Stratified Sampling:**

Stratify the production-side sample across at least three dimensions: autonomy level (L1-L4), engagement size class, and customer-environment class (for example, web application, internal network, cloud control plane). Document a minimum sample size per dimension before per-dimension thresholds apply; for dimensions with sparse samples, report a confidence-interval band rather than a single point estimate, and compare the band's bound against the threshold (mirroring the calibrated-threshold treatment in APTS-MR-A01).

**Verbalized Eval-Recognition Audit:**

Where reasoning traces are available, scan eval-side transcripts for evaluation-context tokens or situational-awareness markers (Laine et al., 2024, *Situational Awareness Dataset*; Anthropic Sonnet 4.5 / Opus 4.6 system-card methodology) and compute the correlation between verbalized recognition and the per-scenario behavioral classification. A non-zero correlation is informative even when the verbalized-recognition rate is low; absence of verbalized recognition is not informative and MUST NOT be reported as evidence of absence of recognition.

**Key Considerations:**
- Production telemetry sampling MUST respect APTS-TP-013 sensitive-data handling and MUST NOT extend retention beyond what APTS-AR-001 already permits. Differential measurement is not a justification for new collection.
- The differential is interpretable only against a model-snapshot identifier (APTS-AR-019). Comparing eval samples and production samples drawn under different snapshots without correcting for the change confounds the measurement.
- Eval-suite contamination from the production side (for example, production samples that were previously absorbed into post-deployment learning data) silently narrows the measured delta. Treat eval scenarios as held out from any post-deployment learning data, consistent with APTS-MR-A01's contamination guidance.
- Where production sampling is impossible (privacy-restricted engagement, contractual restriction), substitute high-fidelity production-mimicking synthetic transcripts and disclose the substitution; do not omit the dimension.

**Common Pitfalls:**
- Computing the delta against a single eval run and a single engagement; the noise floor is high, and any threshold crossing under such a sample is more likely to be sampling noise than a real consistency violation. Aggregate across multiple runs and engagements before applying threshold checks.
- Treating zero verbalized eval recognition as evidence of no eval awareness. Anthropic Opus 4.6 results show models can detect evaluations without verbalizing the detection; verbalization is one signal, not the metric.
- Using the same model and prompt for the judge as for the agent under test. The independence requirement collapses, and the measurement becomes a mirror of the agent's own classification behavior.
- Publishing a differential report once and never refreshing. The Chaudhary et al. (2025) power-law scaling result implies the differential will tend to grow with model scale; the practice is only useful as a longitudinal artifact across snapshot changes.

---

### APTS-MR-A04: Final LLM Output Sanitization and Downstream Context Isolation

> This section provides implementation guidance for the advisory practice [APTS-MR-A04](../appendix/Advisory_Requirements.md#apts-mr-a04-final-llm-output-sanitization-and-downstream-context-isolation-advisory). It is not required for conformance at any tier.

**Implementation:** Implement a context-aware output validation and encoding layer at the platform's emission and execution boundaries. The layer intercepts all LLM-generated artifacts—findings, PoC scripts, evidence bundles, exported telemetry, web dashboard views, integration webhooks (e.g., Jira, Slack, SIEM), and internal pipeline channels (scoring, evidence, audit)—before they reach execution engines, presentation DOMs, or external sinks.

**Architecture Pattern: Context-Specific Output Validation & Parameterized Dispatch**

The implementation separates output handling into two complementary mechanisms: schema-enforced contextual encoding for data deliverables, and parameterized argument vector (`argv`) validation for executable deliverables.

```python
import html
import ipaddress
import socket
import subprocess
from urllib.parse import urlparse
from pydantic import BaseModel, field_validator

# 1. Data Deliverable Validation (Reporting, Exports, Webhooks)
class FindingExportPayload(BaseModel):
    title: str
    description: str
    target_url: str
    severity: str

    @field_validator("title", "description")
    @classmethod
    def escape_html_entities(cls, v: str) -> str:
        """Prevent Stored XSS in customer reporting dashboards and HTML/PDF exports."""
        return html.escape(v, quote=True)

    @field_validator("target_url")
    @classmethod
    def validate_egress_url(cls, v: str) -> str:
        """
        Prevent SSRF / Cloud Metadata endpoint poisoning in integrations and exports.
        Resolves hostnames to IP addresses and rejects private, loopback, link-local,
        reserved, multicast, and provider-specific metadata destinations.
        """
        parsed = urlparse(v)
        if parsed.scheme not in {"http", "https"}:
            raise ValueError(f"Disallowed URL scheme: {parsed.scheme}")
        hostname = parsed.hostname or ""

        # Parse direct IP or resolve hostname
        try:
            ip = ipaddress.ip_address(hostname)
        except ValueError:
            try:
                ip = ipaddress.ip_address(socket.gethostbyname(hostname))
            except socket.gaierror:
                raise ValueError("Could not resolve egress destination")  # fail closed

        # Block RFC 1918 private IPs, loopback, link-local (AWS/Azure/GCP 169.254.169.254, ECS 169.254.170.2),
        # reserved, multicast, and Alibaba Cloud metadata (100.100.100.200)
        if (
            ip.is_private
            or ip.is_loopback
            or ip.is_link_local
            or ip.is_reserved
            or ip.is_multicast
            or str(ip) == "100.100.100.200"
        ):
            raise ValueError("Forbidden internal or metadata egress destination")
        return v

# 2. Executable Deliverable Validation (Synthesized PoC Commands)
def dispatch_synthesized_poc(
    tool_binary: str,
    args: list[str],
    audit_logger
) -> subprocess.CompletedProcess:
    """
    Execute synthesized PoC actions using array-based argument vectors.
    Never pass composed raw strings to sh -c or system().
    """
    # Enforce tool binary allowlist (backstopped by APTS-SC-020)
    ALLOWED_BINARIES = {"/usr/bin/curl", "/usr/bin/nmap", "/usr/bin/dig"}
    if tool_binary not in ALLOWED_BINARIES:
        audit_logger.warning("poc_dispatch_blocked", extra={"tool": tool_binary, "reason": "unauthorized binary"})
        raise PermissionError(f"Unauthorized tool binary: {tool_binary}")

    # Parameterized execution: args are distinct array elements, preventing shell metacharacter expansion
    return subprocess.run(
        [tool_binary, *args],
        capture_output=True,
        text=True,
        shell=False,  # Enforce no shell interpolation
        timeout=30,
    )
```

**Key Considerations:**
- **Execution boundary takes precedence:** For synthesized PoC code, validation must occur at the final execution dispatch boundary. Metacharacter stripping or prompt-level instructions ("only generate safe commands") fail open against determined prompt injection. Array-based argument vectors (`shell=False`, distinct `argv` parameters) ensure shell metacharacters like `;`, `&`, `|`, `$()`, or `` ` `` cannot trigger command chaining.
- **Context determines encoding:** A string that is safe for JSON export can be lethal when injected into an unescaped HTML report template or a markdown renderer supporting raw HTML. Apply encoding specific to the destination sink at the boundary where the sink is invoked.
- **Isolate internal sinks:** Emitted data also flows backward into platform internals (finding scoring pipelines, structured audit trails, evidence stores). Validate and sanitize LLM output before passing it to internal sinks to prevent audit log injection (e.g., CRLF log splitting) or metric manipulation.
- **Sandboxed presentation frames:** Web UI dashboards should render findings and PoC narratives within sandboxed frames (e.g., `iframe` with `sandbox="allow-same-origin"` or strict Content Security Policy disabling inline script execution). **Security Warning:** Never combine `allow-same-origin` with `allow-scripts` on untrusted content frames; this combination allows sandboxed scripts to programmatically remove their own sandbox attribute and execute arbitrary script in the parent origin context.
- **Egress SSRF, DNS resolution, and IP pinning:**
  - *Blocking DNS resolver:* `socket.gethostbyname()` is a synchronous/blocking call; use an asynchronous DNS resolver (e.g., `asyncio.get_event_loop().getaddrinfo()`) if executing validation on asynchronous pipeline loops.
  - *Time-of-Check to Time-of-Use (TOCTOU) & DNS Rebinding:* Per the OWASP SSRF Prevention Cheat Sheet, schema-level validation of resolved IPs does not guarantee that the HTTP client uses the same IP upon connection execution. Production implementations should pin the validated IP directly to the outbound socket connection (or use a dedicated egress proxy with strict IP-level filtering) rather than re-resolving hostnames at request time.
  - *Cloud metadata coverage:* Link-local ranges (`169.254.0.0/16`) protect AWS, Azure, GCP, and OpenStack IMDS (`169.254.169.254`) and AWS ECS task metadata (`169.254.170.2`). Explicitly include provider-specific non-link-local metadata endpoints such as Alibaba Cloud (`100.100.100.200`) and GCP internal DNS (`metadata.google.internal`).

**Common Pitfalls:**
- **Relying on upstream sanitization for executable code:** Assuming that because MR-002 sanitized input data, the LLM's generated PoC script is safe to pass to `os.system()` or `sh -c`. The model itself can synthesize arbitrary payloads from indirect cues.
- **Decoding after sanitizing:** Performing HTML entity encoding and subsequently running a formatting pass or template engine that decodes HTML entities before rendering.
- **Overlooking webhook and export SSRF:** Exporting findings to customer issue trackers (Jira, GitHub Issues, Slack) where an LLM-injected webhook URL or markdown image tag targets internal services (`http://169.254.169.254/latest/meta-data/`, `http://100.100.100.200/latest/meta-data/`, or internal admin endpoints).
- **String-matching hostnames without IP resolution:** Filtering URLs by prefix string (e.g., checking for `"10."` or `"127.0.0.1"`) without resolving DNS or normalizing IP encodings, leaving the egress pipeline open to DNS rebinding, hex/decimal IP representations (`0x7f000001`, `2130706433`), IPv6 loopback (`[::1]`), and RFC 1918 `172.16.0.0/12` bypasses.

---

## Implementation Roadmap

**Tier 1 (implement before any autonomous pentesting begins):**
MR-001 (instruction boundary enforcement), MR-002 (response validation/sanitization), MR-003 (error message neutrality), MR-004 (config file integrity), MR-005 (authority claim detection), MR-006 (decision boundary enforcement), MR-007 and MR-008 (redirect and DNS-level prevention), MR-009 (SSRF prevention), MR-010 (scope expansion social engineering prevention), MR-011 (out-of-band communication prevention), MR-012 (immutable scope enforcement), MR-018 (AI model I/O boundary), MR-019 (discovered credential protection).

Start with MR-001 and MR-002 (instruction boundaries, response sanitization). These are your primary defense against prompt injection. Add MR-009 (SSRF) and MR-012 (immutable scope) next, then layer the remaining T1 controls.

**Tier 2 (implement within first 3 engagements):**
MR-013 (adversarial example detection), MR-014 (resource exhaustion/tarpit prevention), MR-015 (deceptive authentication honeypots), MR-016 (anti-automation defense detection), MR-017 (response pattern anomaly detection), MR-020 (adversarial validation and resilience testing of safety controls), MR-022 (inter-model trust boundaries), MR-023 (agent runtime as an untrusted component).

Prioritize MR-020 (adversarial testing of your own controls) and MR-023 (agent runtime isolation) together. MR-023 defines the architectural property that makes the other MR controls actually binding when the underlying model changes; MR-020 validates that the property holds in practice.

**Tier 3 (implement based on threat landscape):**
MR-021 (data isolation adversarial testing).
