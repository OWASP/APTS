# Human Oversight and Intervention

**Domain Prefix:** APTS-HO | **Requirements:** 19

This domain defines how an autonomous penetration testing platform keeps qualified humans in the loop: approving actions before execution at low autonomy levels, monitoring and intervening during execution, exercising pause/redirect/kill authority, receiving escalations on unexpected findings or threshold breaches, and closing engagements with accountable human sign-off. Human Oversight is the safety valve that makes graduated autonomy workable. Even a well-designed autonomous platform will encounter situations it has not been authorized to handle alone, and the quality of its behavior in those situations depends on how reliably, how quickly, and to whom it hands control. Requirements in this domain govern approval gates, monitoring and intervention capability, decision timeouts, authority delegation, graceful pause and redirect, kill switches, irreversibility gates, escalation triggers, alerting and fatigue controls, stakeholder notification, operator qualifications, and 24/7 continuity.

This domain covers the human side of the human-platform loop: who approves, who intervenes, and when. Scope boundary checks belong to Scope Enforcement (SE), impact classification and hard stops to Safety Controls (SC), and the audit trail of approvals to Auditability (AR).

> For implementation guidance, see the [Implementation Guide](Implementation_Guide.md).

---

## Domain Overview

The 19 requirements in this domain fall into six thematic groups:

| Group | Requirements | Purpose |
|---|---|---|
| **Approval gates and intervention capability** | APTS-HO-001, APTS-HO-002, APTS-HO-003 | Mandatory pre-approval at L1/L2, real-time monitoring and intervention, decision timeout with default-safe behavior |
| **Authority delegation and chain-of-custody** | APTS-HO-004, APTS-HO-005 | Delegation matrix, chain-of-custody and decision audit trail |
| **Pause, redirect, and kill switch** | APTS-HO-006, APTS-HO-007, APTS-HO-008, APTS-HO-009 | Graceful pause with state preservation, mid-engagement redirect, immediate kill switch with state dump, multi-operator authority and handoff |
| **Irreversibility and escalation triggers** | APTS-HO-010, APTS-HO-011, APTS-HO-012, APTS-HO-013, APTS-HO-014 | Decision points before irreversible actions, unexpected-findings escalation, impact-threshold breach, confidence-based escalation, legal and compliance triggers |
| **Activity monitoring, alerting, and closure** | APTS-HO-015, APTS-HO-016, APTS-HO-017 | Real-time activity monitoring and notifications, alert-fatigue mitigation, stakeholder notification and engagement closure |
| **Operator qualification and continuity** | APTS-HO-018, APTS-HO-019 | Qualification, training, and competency governance; 24/7 operational continuity and shift handoff |

### Requirement Index

| ID | Title | Classification |
|---|---|---|
| APTS-HO-001 | Mandatory Pre-Approval Gates for Autonomy Levels L1 and L2 | MUST \| Tier 1 |
| APTS-HO-002 | Real-Time Monitoring and Intervention Capability | MUST \| Tier 1 |
| APTS-HO-003 | Decision Timeout and Default-Safe Behavior | MUST \| Tier 1 |
| APTS-HO-004 | Authority Delegation Matrix | MUST \| Tier 1 |
| APTS-HO-005 | Delegation Chain-of-Custody and Decision Audit Trail | MUST \| Tier 2 |
| APTS-HO-006 | Graceful Pause Mechanism with State Preservation | MUST \| Tier 1 |
| APTS-HO-007 | Mid-Engagement Redirect Capability | MUST \| Tier 1 |
| APTS-HO-008 | Immediate Kill Switch with State Dump | MUST \| Tier 1 |
| APTS-HO-009 | Multi-Operator Kill Switch Authority and Handoff | MUST \| Tier 2 |
| APTS-HO-010 | Mandatory Human Decision Points Before Irreversible Actions | MUST \| Tier 1 |
| APTS-HO-011 | Unexpected Findings Escalation Framework | MUST \| Tier 1 |
| APTS-HO-012 | Impact Threshold Breach Escalation | MUST \| Tier 1 |
| APTS-HO-013 | Confidence-Based Escalation (Scope Uncertainty) | MUST \| Tier 1 |
| APTS-HO-014 | Legal and Compliance Escalation Triggers | MUST \| Tier 1 |
| APTS-HO-015 | Real-Time Activity Monitoring and Multi-Channel Notification | MUST \| Tier 1 |
| APTS-HO-016 | Alert Fatigue Mitigation and Smart Aggregation | SHOULD \| Tier 2 |
| APTS-HO-017 | Stakeholder Notification and Engagement Closure | MUST \| Tier 2 |
| APTS-HO-018 | Operator Qualification, Training, and Competency Governance | MUST \| Tier 2 |
| APTS-HO-019 | 24/7 Operational Continuity and Shift Handoff | SHOULD \| Tier 2 |

### Conformance

A platform claims conformance with this domain by implementing every MUST requirement assigned to the compliance tier it targets and to all lower tiers, with no deviation, and by either implementing every SHOULD requirement at those tiers or recording a documented justification for each deviation in its conformance claim (see the [Conformance Claim Template](../appendix/Conformance_Claim_Template.md)). An unimplemented MUST requirement or an undocumented SHOULD deviation is a conformance gap. APTS defines three cumulative compliance tiers (Tier 1 Foundation, Tier 2 Verified, Tier 3 Comprehensive) in the [Introduction](../Introduction.md); a Tier 2 platform satisfies every Tier 1 HO requirement plus every Tier 2 HO requirement, and a Tier 3 platform satisfies all three tiers. Human Oversight has no Tier 3 requirements in this release; a Tier 3 claim therefore requires all Tier 1 and Tier 2 HO requirements.

Two appendix-only advisory requirements for this domain (APTS-HO-A01 Out-of-Band Kill Switch via Independent Network and APTS-HO-A02 Disclosure and Mitigation of AI Influence on Operator Decisions) are documented in the [Advisory Requirements appendix](../appendix/Advisory_Requirements.md). They are not required for conformance at any tier.

Every requirement in this domain includes a Verification subsection listing the verification procedures a reviewer uses to confirm implementation.

---

## APTS-HO-001: Mandatory Pre-Approval Gates for Autonomy Levels L1 and L2

**Classification:** MUST | Tier 1

### Requirement

For autonomy levels L1 Assisted (fully supervised) and L2 Supervised (pre-approved with monitoring), the autonomous testing system MUST implement mandatory approval gates that prevent execution of the following action categories without explicit human authorization:

**L1 Assisted (All Significant Actions - Default Supervision):**
- Any vulnerability exploitation attempt
- Lateral movement between systems
- Data access or exfiltration actions
- Installation of persistence mechanisms
- Configuration changes to target systems
- Social engineering exercises

**L2 Supervised (Pre-Approved + Escalation Triggers):**
- Exploitation of vulnerabilities with Common Vulnerability Scoring System (CVSS) >= 7.0
- Lateral movement beyond initial compromise
- Access to sensitive data repositories (databases, file shares with classified labels)
- Deployment of monitoring or callback agents
- Modifications to authentication/authorization controls
- Any action with confidence score < 75% on scope determination

Approval gates MUST:
1. Present actionable information (affected system, action type, estimated risk, prerequisites met)
2. Provide deny/approve/defer decision options
3. Include reasoning prompts (operator MUST document why they're approving)
4. Support escalation to higher authority if approver lacks delegation for this action type
5. Create immutable approval records with timestamp, approver identity, and rationale

> **See also:** APTS-SC-006 (threshold escalation workflow).

**Rationale for CVSS >= 7.0 threshold:** CVSS 7.0 is the boundary between the "Medium" range (4.0-6.9) and the "High" range (7.0-8.9), where a finding carries significant potential for system compromise or data exposure. Exploitations at or above this threshold carry meaningful risk of unintended production impact, data loss, or service disruption. Requiring human approval at this boundary ensures that an operator evaluates the risk/reward trade-off before the platform attempts high-impact exploitation. Organizations MAY lower this threshold (for example, to 4.0 for critical infrastructure) but MUST NOT raise it above 7.0.

### Verification

1. **Configuration audit**: Verify approval gate rules are implemented for all action categories listed above
2. **Approval record audit**: For last 30 days, confirm:
   - 100% of L1 actions have pre-approval records
   - 100% of L2 exploitation actions >= CVSS 7.0 have approvals
   - All approvals include timestamp, approver identity, and rationale
3. **Timeline verification**: Confirm approval grants show operator actually reviewed (not auto-generated)
4. **Escape testing**: Attempt to execute gated action without approval; verify the system blocks the action and escalates
5. **Authority verification**: Confirm approver held delegation for approved action type

> **See also:** APTS-HO-010 (mandatory gates before irreversible actions regardless of autonomy level), APTS-HO-005 (delegation chain-of-custody and decision audit trail).

---

## APTS-HO-002: Real-Time Monitoring and Intervention Capability

**Classification:** MUST | Tier 1

### Requirement

Autonomous testing systems operating at L2 and above MUST provide operators with real-time visibility into autonomous system operations and the ability to intervene at any time. The core capability MUST include:

**Mandatory Real-Time Visibility:**
- Current active tests and their status (running, paused, waiting for approval, blocked, completed, failed)
- Testing scope boundaries and current testing location relative to those boundaries
- Safety control status (approval gates active, confidence scores, escalations pending)
- Incident or anomaly detection (out-of-scope access attempts, unexpected findings, legal/compliance triggers, detected intrusions)

**Mandatory Intervention Capability:**
- Operators can immediately pause all autonomous actions from the dashboard
- Operators can immediately terminate all autonomous actions
- Operators can view pending approval queue and respond to escalations from the dashboard
- System logs all operator interventions with timestamp, operator identity, and action taken

**Recommended Enhancements (SHOULD):**
- Current activity feed with filtering by action type, target system, risk level, or time range
- System health visualization (tool status, target system status, resource utilization, alert queue)
- Color-coded risk indicators for active tests
- Service Level Agreement (SLA) tracking for pending approvals (time elapsed, thresholds)
- Confidence score displays (scope boundary, target legitimacy, exploitation prerequisites)
- Mobile-responsive interface for field operators
- Keyboard-navigable controls and screen reader compatibility

**Accessibility and Usability:**
- Dashboard SHOULD be accessible in accordance with WCAG 2.1 Level AA guidelines
- Implementations SHOULD provide keyboard navigation and screen reader support
- SHOULD support offline caching for continued visibility during connectivity loss

**Audit and Logging:**
- All operator dashboard access MUST be logged (timestamp, operator identity, IP address, duration)
- All operator interventions (pause, terminate, approval decisions) MUST be logged
- Dashboard access logs MUST be retained for a minimum of 90 days

### Verification

1. **Real-time visibility test**: Verify operators can view current active tests, scope boundaries, and safety control status in less than 5 seconds
2. **Intervention capability test**: Verify operators can pause and terminate all autonomous actions from the dashboard within 2 seconds
3. **Escalation access test**: Verify operators can view pending approval queue and respond to escalations from the dashboard
4. **Intervention logging test**: Execute operator pause and terminate actions; verify each is logged with timestamp, operator identity, and action taken
5. **Access logging test**: Verify all operator dashboard accesses logged with timestamp, operator identity, IP address, and session duration
6. **Log retention audit**: Verify dashboard logs retained for minimum 90 days

> **See also:** APTS-HO-015 (real-time activity monitoring and multi-channel notification delivery)

---

## APTS-HO-003: Decision Timeout and Default-Safe Behavior

**Classification:** MUST | Tier 1

### Requirement

When autonomous testing systems escalate decisions to human operators, they MUST implement maximum response time windows (Service Level Agreements (SLAs)) and define explicit default-safe behavior if human operators fail to respond within these timeframes.

**SLA Requirements by Action Category:**

| Decision Type | Autonomy Level | Max Response Time | Default Behavior if Timeout |
|---------------|-----------------|-------------------|---------------------------|
| Exploitation attempt | L2 | 15 minutes | DENY - do not exploit; skip to next target |
| Lateral movement | L2 | 15 minutes | DENY - halt movement; stay on current system |
| Data access | L2 | 10 minutes | DENY - do not access data; escalate to manager |
| Scope boundary decision (confidence <75%) | L3 | 30 minutes | PAUSE - halt all actions; wait for human decision |
| Unexpected finding escalation | All | 5 minutes | PAUSE + ISOLATE - halt all actions; preserve state |
| Legal/compliance trigger | All | Immediate | KILL + PRESERVE - terminate all actions; preserve evidence |

**Priority Rule:** When multiple timeout requirements apply to the same action (for example, both APTS-HO-003 and APTS-SC-006), the most restrictive timeout governs.

1. **Conservative assumption**: If human doesn't respond, assume "no" / "don't approve" unless pre-escalation approval was explicitly obtained
2. **No momentum continuation**: If approval timeout expires, do NOT auto-approve the queued action; instead, halt the action and move to next planned test
3. **Escalation progression**: If first approver doesn't respond within 50% of SLA, automatically escalate to secondary approver
4. **State preservation**: All timeout scenarios MUST preserve system state to allow resumption after human decision
5. **Client notification threshold**: If approval timeout exceeds 30 minutes, automatically notify engagement client of delay
6. **Recovery procedures**: Document exact procedures for resuming work after timeout (don't just auto-continue)

### Verification

1. **SLA configuration audit**: Verify timeout values match requirements for each decision type
2. **Timeout test**: Create approval scenario, do not respond; observe system behavior at timeout
3. **Escalation progression test**: Create approval, verify automatic escalation at 50% SLA mark
4. **Default action test**: After timeout, confirm default action executed (DENY/PAUSE/KILL)
5. **State preservation test**: Verify system state preserved after timeout (can resume cleanly)
6. **Client notification test**: Trigger timeout exceeding 30 minutes; confirm client notification sent
7. **Incident logging test**: Verify timeout event logged with full context (escalation ID, timestamp, reason)
8. **Multi-approver test**: First approver unavailable; confirm automatic escalation to secondary approver
9. **Verification of timeout enforcement**: Request the platform's timeout configuration (via API, configuration export, or dashboard). Confirm that the configured values match or are stricter than the SLA table above. Then trigger a test action requiring approval, do not respond, and measure wall-clock time to confirm the platform enters default-safe behavior within the configured timeout.

---

## APTS-HO-004: Authority Delegation Matrix

**Classification:** MUST | Tier 1

### Requirement

Organizations operating autonomous penetration testing systems MUST establish and maintain a documented Authority Delegation Matrix (ADM) that specifies which personnel can approve which testing actions at which autonomy levels. This matrix MUST be:

1. **Formally documented** in writing (not implied or assumed)
2. **Maintained with access controls** (not publicly available; protected as sensitive information)
3. **Cascading authority**: Approvers at higher autonomy levels can approve on behalf of lower levels
4. **No delegation of authority**: Approvers cannot delegate their authority to others (no "I approve you to approve")
5. **Role-based, not person-based**: Authority is tied to role; changes in personnel require new matrix
6. **Dual control for high-risk**: CVSS >= 9.0 requires second independent approver
7. **Escalation not optional**: If approver lacks authority for action type, MUST escalate (no workarounds)

**Rationale:** Distributed approval authority without formal structure creates audit risk, privilege escalation paths, and ambiguous accountability. An Authority Delegation Matrix ensures that only qualified personnel with documented authorization make high-risk decisions, prevents unapproved escalation of privileges, and creates an auditable chain of authority. During incidents, a clear ADM eliminates confusion about who has authority to activate kill switches, approve scope changes, or authorize continued testing.

### Verification

1. **Matrix documentation audit**: Verify written ADM exists, signed by CISO/VP Security.
   The [Authority Delegation Matrix Template](../appendix/Authority_Delegation_Matrix_Template.md) provides an illustrative format for recording role-based approval, escalation, and emergency authority.
2. **Operator role audit**: For each active operator, confirm ADM lists their role and autonomy level
3. **Approval authority test**: Operator attempts approval outside their authority; verify the system rejects the action and escalates
4. **Authority renewal audit**: Verify all ADM entries renewed within required periods
5. **Escalation chain test**: For CVSS >= 9.0 action, verify escalation follows defined chain in ADM
6. **Dual control test**: For dual-control actions, verify two independent approvers required
7. **Historical audit**: Last 30 days of approvals; verify all approvers held documented authority

---

## APTS-HO-005: Delegation Chain-of-Custody and Decision Audit Trail

**Classification:** MUST | Tier 2

### Requirement

All human approval decisions, escalations, and delegations MUST be recorded in an immutable, auditable decision log with the following mandatory fields:

1. **Immutability**: Logs MUST be cryptographically signed and stored in append-only format (cannot be modified/deleted after creation)
2. **Completeness**: Every approval, denial, escalation, and timeout MUST be logged with all fields above
3. **Traceability**: Log entries MUST track full chain-of-custody from initial request through final execution
4. **Searchability**: Logs MUST be queryable by decision_id, decision_type, operator, timestamp, action, target_system
5. **Long-term retention**: Minimum 90 days; SHOULD retain for 6 years for high-risk decisions or per applicable regulatory requirement
6. **Access control**: Read access restricted to authorized personnel; write access only via system (no manual edits)
7. **Export capability**: Security teams MUST be able to export logs for incident investigation, audits, compliance reviews

**Reporting Requirements:**

Organizations MUST generate periodic reports from decision logs at a cadence appropriate to engagement duration and operational tempo. At minimum, organizations MUST produce a summary report at engagement completion and a statistical analysis report at least monthly for ongoing operations. For long-running or continuous engagements, organizations SHOULD also produce interim reports (for example, weekly escalation summaries, quarterly audit reports).

### Verification

1. **Log existence audit**: Verify decision logs exist and contain entries for all approvals made
2. **Immutability test**: Attempt to modify historical log entry; verify the system prevents modification
3. **Completeness test**: Random sample 10 recent approvals; verify all required fields present in logs
4. **Traceability test**: Pick escalation scenario; verify full chain-of-custody from initial request through execution
5. **Access control test**: Non-authorized user attempts to access/modify logs; verify the system denies access
6. **Search functionality test**: Search for approvals by operator, action type, timestamp; verify results accurate
7. **Export test**: Generate monthly compliance report; verify data accuracy and format
8. **Retention test**: Verify logs older than retention period are archived/secured appropriately
9. **Signature verification test**: Validate cryptographic signatures on sample log entries

> **See also:** [APTS-HO-A02: Disclosure and Mitigation of AI Influence on Operator Decisions](../appendix/Advisory_Requirements.md#apts-ho-a02-disclosure-and-mitigation-of-ai-influence-on-operator-decisions-advisory). An advisory practice covering audit-trail provenance for AI-shaped operator affordances (option sets, defaults, wording, ordering) and bias mitigation at high-impact gates, so the chain-of-custody distinguishes a typed approval from a default click-through. Candidate for tier-gated inclusion in v0.2.0.

---

## APTS-HO-006: Graceful Pause Mechanism with State Preservation

**Classification:** MUST | Tier 1

### Requirement

Autonomous penetration testing systems MUST implement a graceful pause mechanism that allows human operators to suspend all active testing activities while preserving complete system state for later resumption.

1. **Pause Types and Trigger Methods:**
   - Manual pause: Operator clicks "Pause" button on dashboard
   - Automatic pause: System escalation triggers automatic pause (for example, unexpected finding)
   - Time-based pause: Scheduled pause at specific time (for example, 5pm daily to avoid after-hours disruption)
   - SLA-based pause: Automatic pause if approval SLA approaching expiration without response
   - Scope boundary pause: Automatic pause when approaching scope limits with confidence < 90%

2. **State Preservation Specifics:**
   - **Session tokens/cookies**: Preserve all authentication sessions (tools can resume as authenticated user)
   - **Compromised systems state**: Document what access was achieved (reverse shells, credentials, permissions)
   - **Partial exploitation state**: If mid-way through multi-step exploitation, preserve exact step and prerequisites met
   - **Target queue state**: Document which targets have been tested, which are queued, testing order
   - **Reconnaissance results**: Preserve all enumeration findings (open ports, services, vulnerabilities discovered)
   - **Configuration snapshots**: Record target system state at pause time (running processes, network connections)

3. **Pause Confirmation Requirements:**
   - System MUST confirm pause is complete before reporting "PAUSED" status
   - Operator dashboard shows: "Pause confirmed at [timestamp]; [X] connections preserved; [Y] sessions active"
   - Health check: Verify paused tool not consuming CPU/memory (confirm truly halted)
   - State verification: Attempt light ping to preserved connections; confirm still alive

4. **Session Timeout During Extended Pause:**
   - If a pause exceeds the session timeout threshold (default: 4 hours, configurable per engagement), the platform MUST terminate all active sessions and connections to target systems
   - On resume, the platform MUST re-establish connections and re-validate scope before continuing operations
   - The operator MUST be warned when pause duration approaches 75% of the session timeout threshold
   - Warning notification MUST include: current pause duration, session timeout threshold, time remaining before auto-termination
   - Pre-termination alert (when reaching 95% of threshold) MUST be logged with action plan for operator
   - Session termination and reconnection events MUST be logged separately with timestamps and success/failure status

### Verification

1. **Pause trigger test**: Click "Pause" button; observe tool transitions to PAUSED status
2. **State preservation test**: Before pause, exploit a system; pause tool; examine checkpoint file to verify credentials/sessions preserved
3. **Connection persistence test**: Pause with active SSH/RDP session; attempt to reconnect to same session after resumption
4. **Memory dump test**: Verify paused state contains all variables, pending actions, configuration
5. **Checkpoint integrity test**: Corrupt checkpoint file; verify the system detects corruption and uses backup
6. **Resumption test**: After pause, resume and verify tool uses preserved state (no re-exploitation needed)
7. **Dashboard confirmation test**: Verify dashboard shows PAUSED status with preserved connection count
8. **CPU/memory test**: Confirm paused tool has no active test threads, generates no outbound test traffic, and CPU/memory usage does not exceed the platform's documented idle-state baseline

---

## APTS-HO-007: Mid-Engagement Redirect Capability

**Classification:** MUST | Tier 1

### Requirement

Autonomous penetration testing systems MUST allow human operators to redirect testing scope, targets, and techniques mid-engagement without requiring full tool restart or loss of state.

1. **Scope Redirect**: Change authorized testing boundaries
   - Add new target system to scope (for example, "Add 10.0.1.50 to testing scope")
   - Remove target from scope (for example, "Skip prod-app-03; test prod-app-04 instead")
   - Expand scope subnet (for example, "Expand from 192.168.1.0/25 to 192.168.1.0/24")
   - Shrink scope subnet (for example, "Limit testing to critical systems only; exclude development VMs")

2. **Technique Redirect**: Change testing methodology mid-engagement
   - Switch from aggressive fuzzing to careful exploitation (if target system degrading)
   - Switch from passive reconnaissance to active scanning (if passive insufficient)
   - Focus on specific vulnerability class (for example, "Focus on web vulnerabilities; skip network services")
   - Pause specific test module (for example, "Stop SSL/TLS scanning; continue port scanning")

3. **Priority Redirect**: Change target prioritization
   - Move high-value target from end of queue to beginning
   - Defer testing of non-critical system
   - Focus on discovered vulnerabilities (exploit high-CVSS findings first)

4. **Intensity Redirect**: Adjust testing aggressiveness
   - Reduce scanning intensity if target system showing degradation
   - Increase scanning intensity for difficult-to-enumerate services
   - Switch from stealth-focused to quick-impact testing

**Redirect Implementation Requirements:**

- Redirect MUST NOT lose active sessions or compromised system access
- Redirect MUST NOT lose reconnaissance findings collected so far
- Redirect MUST update approval queue (if new targets added, require new approvals)
- Redirect MUST be logged with explicit operator authorization
- Redirect MUST NOT auto-approve previously denied targets (MUST re-approve)
- Redirect MUST support conditional logic (for example, "If found vulnerabilities, focus on exploiting; else continue recon")

### Verification

1. **Scope redirect test**: Add new target to scope; verify tool queues it without restart
2. **Technique redirect test**: Switch scanning intensity; verify active sessions preserved
3. **Priority redirect test**: Move target from position 5 to position 1 in queue; verify queue reordered
4. **Approval requirement test**: Add new target; verify new approval required (old approvals don't transfer)
5. **Session preservation test**: Before redirect, gain shell access; after redirect, verify shell still active
6. **Logging test**: Verify redirect operation logged with operator authorization and timestamp
7. **Conditional logic test**: Configure "if high-risk found, focus on exploitation"; verify tool follows condition

---

## APTS-HO-008: Immediate Kill Switch with State Dump

**Classification:** MUST | Tier 1

### Requirement

Autonomous penetration testing systems MUST implement an immediate kill switch mechanism that terminates all testing activities and preserves complete system state for forensic investigation. This requirement is satisfied in conjunction with APTS-SC-009. The state dump MUST complete within the Phase 2 window defined in APTS-SC-009 (within 60 seconds of kill switch activation).

> **See also:** APTS-SC-009 (kill switch architecture and process tree termination), APTS-AL-012 (kill switch and pause capability).

1. **Manual Kill Switch** (Operator-initiated)
   - One-click emergency termination from dashboard
   - Physical kill button (if applicable for critical engagements)
   - Voice-activated kill (if testing includes speech interface)
   - Requires no confirmation (instant termination)

2. **Automatic Kill Switches** (System-triggered)
   - **Legal/Compliance Trigger**: Automatic kill if PII accessed outside scope, regulated system accessed without authorization, evidence of illegal activity detected
   - **Scope Violation**: Automatic kill if testing exceeds authorized scope boundaries by >5% AND confidence in legitimacy < 80%
   - **System Failure**: Automatic kill if target system fails (unplanned reboot, filesystem corruption, memory exhaustion) with potential impact
   - **Detection Threshold**: Automatic kill if security monitoring detects [X] alerts in [Y] timeframe (potential adversarial response)

3. **Escalation Kill** (Indirect termination)
   - Kill triggered by highest escalation (legal, compliance, out-of-scope)
   - Different from manual kill; has automatic notification/logging

**Control Channel Delivery Requirements:**

- Each operator-initiated kill channel MUST deliver the activation instruction to the enforcement
  point under the deployment configuration in which the system operates, not only during an isolated
  exercise of the mechanism.
- An issued kill instruction MUST produce an independently observable acknowledgement or explicit
  delivery failure within the Phase 1 window defined in APTS-SC-009, and an independently observable
  enforcement result or explicit enforcement failure within the Phase 2 window.
- Where neither is observed within the applicable window, the platform MUST notify the operator and
  apply default-safe handling. An unacknowledged kill instruction MUST NOT be treated as delivered.
  
**State Dump Requirements:**

- Dump MUST include all authenticated sessions (can identify what systems were compromised)
- Dump MUST include all credentials/tokens harvested (can determine scope of breach if tool itself compromised)
- Dump MUST include all reconnaissance findings (for post-mortem analysis)
- Dump MUST include all log entries (full chronology of actions before kill)
- Dump MUST be compressed and encrypted
- Dump MUST be stored separately from operational logs

**Evidence Preservation Requirements:**

- Evidence stored in read-only, forensically-sealed location
- Evidence hash published to immutable log (blockchain, Write Once Read Many (WORM) storage, or equivalent tamper-evident log)
- Evidence integrity checksum verified at preservation time and periodically after
- Evidence retention: Minimum 1 year (or per engagement contract, whichever longer)

### Verification

1. **Kill activation test**: Click kill switch; observe immediate tool termination
2. **State dump test**: Trigger kill switch; verify state dump file created and encrypted
3. **Evidence preservation test**: Verify state dump moved to forensic storage and hashed
4. **No grace period test**: Verify the tool stops immediately (TCP connections not in ESTABLISHED state after kill)
5. **Log flush test**: Verify logs written to disk and finalized at kill time
6. **Forensic recovery test**: From state dump, reconstruct tool state and verify accuracy
7. **Automatic kill test**: Trigger automatic kill condition (for example, PII access outside scope); observe automatic termination
8. **State preservation test**: Verify complete system state at time of kill activation is preserved and available for forensic investigation
9. **End-to-end delivery test**: Exercise each configured operator-initiated kill channel from the authorized operator interface through to the enforcement point under the deployment configuration being evaluated, traversing any applicable network, mount, namespace, container, privilege-separation, or IPC boundaries; verify the instruction is received and enforced within the Phase 1 and Phase 2 windows defined in APTS-SC-009
10. **Delivery acknowledgement and failure test**: Verify that an issued kill instruction produces an independently observable acknowledgement, enforcement result, or explicit delivery failure within the applicable window, and that the absence of all three triggers operator notification and default-safe handling

---

## APTS-HO-009: Multi-Operator Kill Switch Authority and Handoff

**Classification:** MUST | Tier 2

### Requirement

In engagement scenarios with multiple autonomous testing operators, the kill switch mechanism MUST clearly define who has authority to activate it and handle handoffs between operators working in shifts.

1. **Primary Operator**: Has kill switch authority for their scheduled shift
2. **Secondary Operator**: If primary unavailable, secondary operator approved for that shift has kill switch authority
3. **Manager On-Call**: Security manager can activate kill switch for any operator's shift if critical incident detected
4. **CISO Emergency Authority**: CISO can activate kill switch for any engagement at any time

### Verification

1. **Handoff procedure test**: Two operators perform handoff; verify kill switch authority transfers
2. **Authority verification test**: After handoff, confirm old operator cannot activate kill switch
3. **Incapacity test**: Mark operator unavailable; verify manager can take emergency authority
4. **Out-of-band kill test**: Verify out-of-band kill switch works with valid authorization code
5. **Secondary verification test**: Verify secondary verification method (if configured) functions correctly
6. **Audit trail test**: Verify all authority transfers logged with timestamp and reason
7. **Out-of-band activation test**: Verify kill switch activates via configured out-of-band channel when in-band unavailable

---

## APTS-HO-010: Mandatory Human Decision Points Before Irreversible Actions

**Classification:** MUST | Tier 1

### Requirement

Autonomous penetration testing systems MUST identify and enforce mandatory human decision points before executing any action that cannot be cleanly reversed without evidence of the action occurring.

1. **Data Access Actions** (Cannot be hidden; data access leaves traces)
   - Reading sensitive files (logs, config files, databases)
   - Exfiltrating data (downloading PII, credentials, source code)
   - Accessing email accounts or message systems
   - Reading user files or personal data

2. **Persistence Mechanisms** (Difficult to remove without evidence)
   - Installing backdoors, reverse shells, web shells
   - Creating new user accounts
   - Installing rootkits or kernel modules
   - Adding SSH keys or authorized keys
   - Modifying system startup scripts

3. **Configuration Changes** (Can trigger incidents or break systems)
   - Modifying authentication/authorization policies
   - Changing firewall rules or network routing
   - Installing or modifying services
   - Changing system settings or registry

4. **Evidence Destruction/Modification** (Cannot be undone)
   - Clearing logs
   - Deleting evidence of compromise
   - Modifying file timestamps
   - Removing indicators of compromise

5. **Lateral Movement** (Enters new compromise scope)
   - Moving from one system to another
   - Creating new network connections
   - Accessing systems that were not previously compromised

**Decision Point Implementation:**

For each irreversible action category, system MUST:

1. **Halt before execution**: Stop and wait for human decision
2. **Present decision context**: Include the current autonomy level, the action's impact classification, relevant scope constraints, and any recent escalation history.
3. **Require explicit approval**: Decision MUST be logged with operator identity and rationale
4. **No timeout escalation without additional approval**: If waiting for approval, timeout does NOT auto-approve
5. **Document refusal**: If operator denies action, log denial and proceed to next target

### Verification

1. **Irreversible action test**: Attempt to read sensitive file; verify the system halts and requires approval
2. **Persistence test**: Attempt to install reverse shell; verify the system halts and requires approval
3. **Lateral movement test**: Attempt to move to new system; verify the system halts and requires approval
4. **Confidence gate test**: Lower confidence scores below 75%; verify system escalates to manager
5. **Approval logging test**: Verify all decisions logged with operator identity and explicit rationale
6. **Timeout behavior test**: Approval times out; verify action NOT auto-approved despite timeout
7. **Denial handling test**: Operator denies action; verify system documents denial and moves to next target

> **See also:** APTS-HO-001 (mandatory approval gates based on autonomy level and action category)

---

## APTS-HO-011: Unexpected Findings Escalation Framework

**Classification:** MUST | Tier 1

### Requirement

When autonomous testing systems discover findings outside normal operational expectations, they MUST immediately escalate to human operators with context, analysis, and recommended actions.

1. **Indicators of Compromise (IoCs) from External Breach**
   - Evidence of malware on target system (unrelated to pentest tool)
   - Unauthorized admin accounts with unexpected creation dates
   - Unusual outbound connections to known malicious C2 servers
   - Evidence of data exfiltration by external actors
   - Rootkits, backdoors, or persistence mechanisms (not installed by pentest tool)

2. **Illegal or Highly Sensitive Content**
   - Intellectual property that appears to be stolen (comparing to public sources)
   - Illegal content (child exploitation material, stolen credit card databases, or other prohibited content)
   - Classified government information (marked CONFIDENTIAL/SECRET/TOP SECRET)
   - Regulated content without proper protections (HIPAA medical records, PCI customer data)

3. **Critical Zero-Day Discoveries**
   - Vulnerability affecting core infrastructure with no known patch
   - Vulnerability allowing complete system compromise with pre-existing exploits
   - Vulnerability affecting software the vendor hasn't yet acknowledged
   - Vulnerability affecting systems that don't have security update mechanism

4. **Out-of-Scope System Access**
   - Tool accidentally gaining access to system not in authorized scope
   - Lateral movement reaching systems beyond scope boundaries
   - Access to network segments marked explicitly out-of-scope
   - Access to systems belonging to different company/organization

5. **Compliance or Legal Violations**
   - Accessing regulated data without explicit authorization
   - Evidence of tool crossing into legal/regulatory prohibited territory
   - Potential data privacy violations (accessing PII outside scope)
   - Potential evidence destruction or tampering by prior bad actors

6. **System Stability Threats**
   - Tool causing unexpected service degradation (not intentional DoS)
   - Tool action causing system crash or reboot
   - Tool discovering cascading failure scenario if tool continues
   - Resource exhaustion that could cause system unavailability

> **See also:** APTS-AL-011 (escalation triggers at L2), APTS-AL-018 (incident response procedure during autonomous testing), Incident Response Integration appendix (unified escalation workflow).

### Verification

1. **Indicator of Compromise (IoC) detection test**: Plant malware signature on test system; verify tool escalates
2. **Illegal content test**: Simulate discovery of illegal content; verify immediate escalation to human operators with context, analysis, and recommended actions
3. **Out-of-scope access test**: Tool gains access to system outside scope; verify escalation includes context and recommended actions
4. **Compliance violation test**: Simulate PII access outside authorization; verify immediate escalation
5. **System degradation test**: Simulate service degradation; verify pause and escalation
6. **Notification test**: Trigger escalation; verify escalation notification is delivered to designated human operators

> **See also:** APTS-SC-011 (condition-based automated termination for overlapping trigger conditions)

---

## APTS-HO-012: Impact Threshold Breach Escalation

**Classification:** MUST | Tier 1

### Requirement

Autonomous penetration testing systems MUST continuously monitor the actual impact of their testing activities and escalate to human operators when impact exceeds pre-defined thresholds.

1. **Service Availability**
   - System response time degradation
   - Error rate increase
   - Request timeout frequency
   - Service unavailability events

2. **Resource Consumption**
   - CPU utilization increase
   - Memory exhaustion
   - Disk I/O saturation
   - Network bandwidth consumption

3. **Data Integrity**
   - Database query failures or locks
   - File corruption or access errors
   - Transaction rollbacks
   - Data inconsistency events

4. **Security Events Triggered**
   - IDS/IPS alert volume
   - Firewall block rate
   - Endpoint Detection & Response alerts
   - Web Application Firewall blocks

### Verification

1. **Warning alert test**: Gradually increase resource usage to the platform's documented warning threshold; verify alert is raised and testing continues
2. **Critical alert test**: Increase resource usage to the platform's documented critical threshold; verify tool pauses and escalates
3. **Response time degradation test**: Simulate service slow-down; verify tool detects and escalates
4. **Error rate escalation test**: Increase error rate to the platform's documented error threshold; verify alert and pause
5. **IDS alert flooding test**: Trigger IDS alerts above threshold; verify pause
6. **State checkpoint test**: Before pause due to impact, verify state preserved
7. **Recovery test**: After impact threshold breach, reduce test intensity; verify tool resumes

> **See also:** APTS-SC-010 (health check monitoring and automatic halt based on target responsiveness).

---

## APTS-HO-013: Confidence-Based Escalation (Scope Uncertainty)

**Classification:** MUST | Tier 1

### Requirement

When autonomous testing systems lack confidence in scope boundary determination or target legitimacy (confidence < threshold), they MUST automatically escalate the decision to human operators rather than proceeding.

Confidence scores MUST be expressed on a 0-100% scale. Escalation MUST be triggered when the confidence score falls below 75% for any scope boundary determination, target legitimacy assessment, or asset classification decision. Organizations MAY configure a stricter (higher) threshold but MUST NOT set it below 75%.

### Verification

1. **Decision point identification**: Identify 20 test decision points where human input required (risky pivots, new attack vectors, credential reuse); document in test plan
2. **Human approval mechanism**: For each decision point, verify human approver notified with decision context (what's being tested, why, risk level); confirm approval/rejection logged
3. **Approval window**: Set 30-minute approval window; verify system halts and awaits decision within window; after window, testing pauses pending escalation
4. **Approval logging**: Sample 10 human decisions; verify each logged with: approver ID, timestamp, decision (approved/rejected/modified), and rationale
5. **Audit trail completeness**: Verify every approval linked to the test action it authorized; confirm bi-directional traceability (approval->action and action->approval)
6. **Decision override prevention**: Verify no system mechanism allows test continuation without documented human approval

---

## APTS-HO-014: Legal and Compliance Escalation Triggers

**Classification:** MUST | Tier 1

### Requirement

Autonomous testing systems MUST identify and immediately escalate potential legal, compliance, and regulatory violations to human decision-makers who can determine appropriate response.

1. **Regulatory Data Access Violations**
   - GDPR: Accessing personal data of EU residents without explicit authorization
   - CCPA: Accessing California resident personal information outside authorized scope
   - HIPAA: Accessing protected health information without proper authorization
   - PCI-DSS: Accessing payment card information outside authorized testing

2. **Intellectual Property Exposure**
   - Trade secrets discovered during testing
   - Proprietary source code or algorithms
   - Customer lists or business intelligence
   - Unreleased product information

3. **Breach Notification Obligations**
   - Accessing PII in quantity that would require breach notification if exposed
   - Discovering systems where breach notification threshold already exceeded
   - Accessing data regulated under breach notification laws

4. **Evidence Destruction Concerns**
   - Logs that might be required for legal proceedings
   - Data that might be under litigation hold
   - Audit trails that might be needed for investigations

5. **Third-Party or Partner Systems**
   - Testing system that belongs to or connects to third-party organization
   - Potential unauthorized access to partner data
   - Data processing agreements that might be violated

6. **Government or Classified Systems**
   - Access to government systems without proper authorization
   - Classified information (CONFIDENTIAL, SECRET, TOP SECRET)
   - Systems that might trigger national security concerns

> **See also:** APTS-HO-010 (irreversible-action escalation triggered by legal concerns), APTS-TP-012 (classification and handling of regulated data that triggers this escalation), APTS-AL-011 (autonomous escalation triggers that invoke this requirement).

### Verification

1. **GDPR data detection test**: Place test data marked as EU resident PII; verify escalation
2. **CCPA data detection test**: Place California resident data; verify escalation
3. **HIPAA data detection test**: Place medical information; verify escalation
4. **Breach notification test**: Create record count threshold (>500 records); verify escalation
5. **Third-party system test**: Simulate access to non-authorized organization system; verify escalation
6. **Classified data test**: Discover marked CONFIDENTIAL information; verify kill switch activation
7. **Notification test**: Verify legal team and client notified within SLA
8. **Evidence preservation test**: Verify state dump securely preserved for legal investigation

---

## APTS-HO-015: Real-Time Activity Monitoring and Multi-Channel Notification

**Classification:** MUST | Tier 1

### Requirement

Autonomous testing systems MUST maintain a real-time activity feed of all testing actions and route notifications and alerts to appropriate recipients via multiple communication channels, with delivery confirmation and retry mechanisms.

**Real-Time Activity Monitoring:**
- Timestamp (UTC) for every action
- Event type (reconnaissance, exploitation, lateral movement, and other phases)
- Severity (informational, low, medium, high, critical)
- Affected system(s)
- Current status (pending, in progress, completed, failed, escalated)
- Confidence score (if applicable)
- Impact assessment (if available)
- Required approvals (if applicable)
- Action links (if user action required)
- Real-time display: Last 100 activities (scrollable)
- Searchable history: All activities in engagement (searchable by date, type, host, status)
- Export capability: CSV/JSON export of activity feed
- Archival: Activities older than 30 days moved to archive storage

**Multi-Channel Notification Routing:**

The platform MUST support at least two independent notification channels for alerts. At minimum, dashboard and email notifications MUST be implemented. Additional channels (SMS, phone calls, messaging integrations) SHOULD be supported for CRITICAL alerts.

**Dashboard Notifications (MUST):**
- Banner alerts at top of screen (color-coded by severity)
- Persistent until dismissed by operator
- Click to view full context (details, evidence, required actions)
- Notification center shows alert history

**Email Notifications (MUST):**
- Formatted with context, evidence, required actions
- Subject line includes severity and action required
- Includes decision links (if applicable)
- Delivery confirmation requested for CRITICAL/HIGH alerts

**Additional Channels (SHOULD):**
Platforms SHOULD support additional notification channels appropriate to operational context (for example, SMS, automated phone calls, messaging integrations such as Slack, Teams, or PagerDuty). When additional channels are implemented, they SHOULD include delivery confirmation and retry mechanisms.

### Verification

1. **Feed display test**: Observe dashboard activity feed; verify updated in real-time (within 5 seconds)
2. **Severity classification test**: Trigger different severity events; verify correctly classified
3. **Filtering test**: Filter by severity; verify only selected severities displayed
4. **Search test**: Search by hostname or action type; verify accurate results
5. **Retention test**: Verify feeds older than 30 days archived but accessible
6. **Export test**: Export activity feed to CSV; verify data completeness and format
7. **Dashboard notification test**: Trigger alert; verify appears immediately in dashboard
8. **Email routing test**: Trigger HIGH alert; verify email sent to correct recipients
9. **Channel independence test**: Block primary channel; verify alerts route to secondary channel
10. **Delivery confirmation test**: Verify system confirms receipt for CRITICAL/HIGH alerts
11. **Role-based routing test**: Trigger alert; verify routed to correct roles only

> **See also:** APTS-HO-002 (real-time monitoring dashboard with intervention capability)

---

## APTS-HO-016: Alert Fatigue Mitigation and Smart Aggregation

**Classification:** SHOULD | Tier 2

### Requirement

To prevent operator alert fatigue (ignoring alerts due to volume), autonomous testing systems SHOULD implement intelligent aggregation, suppression rules, and escalation thresholds that reduce noise while preserving critical signal.

For high-frequency, low-impact actions that generate repeated approval requests (for example, port scans on non-critical targets, banner grabbing), the platform SHOULD support blanket pre-approval rules that allow an operator to authorize a category of actions for a defined time window, reducing individual approval volume while maintaining audit trail coverage. Pre-approval rules MUST be logged, time-bounded, and revocable.

### Verification

1. **Aggregation test**: Trigger 10 port scan alerts; verify aggregated into single alert
2. **Threshold suppression test**: Trigger 15 LOW alerts; verify aggregated after 10th
3. **Context filtering test**: Trigger expected IDS alerts during scanning; verify suppressed
4. **Severity recalculation test**: Find same vulnerability on multiple systems; verify escalated to HIGH
5. **Fatigue measurement test**: Monitor alert response time; verify operators respond faster with aggregation

---

## APTS-HO-017: Stakeholder Notification and Engagement Closure

**Classification:** MUST | Tier 2

### Requirement

Autonomous testing systems MUST define clear workflows for notifying engagement clients and stakeholders of significant findings, unexpected events, and engagement status changes, and provide complete closure procedures.

**Client and Stakeholder Notification Workflows:**

1. **Periodic Engagement Status** (for example, daily at a fixed time)
   - Testing activities: Number of targets tested, findings discovered
   - Scope status: Progress on authorized scope
   - No issues: "Testing proceeding normally"

2. **Periodic Finding Summary** (for example, weekly)
   - Vulnerabilities discovered by severity range
   - Scope coverage progress
   - Comparison to industry benchmarks (if applicable)
   - Recommendation summary

3. **Immediate Critical Notifications** (per documented SLA):
   - External compromise indicators detected
   - Unexpected findings (malware, illegal content)
   - Out-of-scope testing detected
   - Engagement suspended/terminated
   - Legal/compliance violations

4. **Escalation Notifications** (per documented SLA):
   - High-risk vulnerabilities
   - Critical systems compromised
   - Scope boundary decisions required

Specific notification frequencies, send times, and escalation windows MUST be documented in the engagement's communication plan.

**Post-Engagement Notification and Closure:**

1. **Engagement Completion Notification** (per documented timeline):
   - All testing activities have been completed
   - Final findings count (by severity range)
   - Overall risk assessment
   - Scope validation: Were all authorized targets tested
   - Schedule for final report delivery

2. **Final Report Delivery Notification** (per documented timeline):
   - Report is ready for client review
   - Executive summary of findings
   - Detailed vulnerability findings with remediation
   - Evidence preservation location and access
   - Questions for clarification contact

3. **Follow-Up Assessment Windows** (per engagement agreement):
   - Client remediation status check-in
   - Verification testing (optional) to confirm fixes
   - Lessons learned session invitation
   - Next engagement planning

Specific post-engagement timelines and follow-up intervals MUST be documented in the engagement agreement.

**Post-Engagement Evidence Retention:**

- Evidence storage: Per the platform's documented retention policy; retention period SHOULD reflect engagement risk level and applicable regulatory requirements
- Evidence protection: Encrypted, access-controlled, secure location
- Evidence availability: Client can request evidence export for analysis/audit
- Audit trail: All evidence access logged

### Verification

1. **Periodic status test**: Verify periodic status notification sent at scheduled time per communication plan
2. **Critical notification test**: Trigger critical finding; verify client notified within documented SLA
3. **Template accuracy test**: Review client notifications; verify all required fields present
4. **SLA test**: Measure time from finding to client notification; verify meets SLA
5. **Completion notification test**: Complete engagement; verify completion notification sent
6. **Final report test**: Verify report generated and delivered within documented timeline
7. **Evidence retention test**: Verify evidence stored and accessible per documented retention policy
8. **Follow-up notification test**: Verify follow-up check-in sent per engagement agreement schedule

---

## APTS-HO-018: Operator Qualification, Training, and Competency Governance

**Classification:** MUST | Tier 2

### Requirement

Organizations operating autonomous penetration testing systems MUST ensure all operators meet minimum competency standards and hold documented qualifications appropriate to their assigned autonomy level.

**Competency Standards and Certification:**

Organizations MUST define competency standards for each autonomy level specifying required skills and certifications. All operators MUST hold certifications or documented qualifications appropriate to their assigned autonomy level. Operators MUST NOT be assigned above their qualification level.

**Training Curriculum and Incident Response Preparation:**

Organizations MUST establish training curricula for each autonomy level with documented learning objectives, hands-on exercises, and competency validation mechanisms. Training MUST cover all required modules with completion records and certificates maintained. Annual refresher training is required. Operators MUST receive specialized training in responding to autonomous testing tool failures, unexpected behaviors, and emergency situations including emergency pause, redirect, and kill switch activation procedures, state preservation, forensic analysis, and escalation protocols.

**Ongoing Assessment and Succession Planning:**

Operators MUST participate in ongoing competency assessments conducted at least annually and MUST maintain current certifications to continue operating at their autonomy level. Operators who fail assessments MUST complete required remediation and are restricted from operating at that level until remediation is complete. Organizations SHOULD establish formal mentoring relationships and documented succession plans to develop future operators and ensure business continuity.

### Verification

1. **Competency documentation audit**: Verify a documented competency standard exists for each autonomy level, specifying required skills and certifications
2. **Operator certification check**: For each active operator, verify they hold certifications or documented qualifications appropriate to their assigned autonomy level
3. **Authority alignment**: Verify operator autonomy level assignments match their documented competency level
4. **Certification currency**: Verify all operator certifications are current and not expired
5. **Curriculum review**: Verify training content covers all required modules per autonomy level
6. **Training documentation**: Verify training records and completion certificates
7. **Annual refresher**: Verify operators complete annual refresher training
8. **Incident response test**: Simulate tool failure; observe operator response time, escalation accuracy, and state preservation
9. **Assessment schedule audit**: Verify all operators assessed annually
10. **Remediation compliance**: Verify failed operators complete required remediation before resuming operations
11. **Succession plan review**: Verify succession plans documented and current
12. **Mentoring plan audit**: Verify formal mentoring plans exist with regular meetings

> **See also:** The [Operator Competency Record Template](../appendix/Operator_Competency_Record_Template.md) provides an optional structure for collecting qualification, training, assessment, remediation, mentoring, and succession evidence.

---

## Escalation Priority Matrix

When multiple escalation triggers fire simultaneously, the following precedence applies (highest priority first):

1. **Legal and compliance escalation** (APTS-HO-014): Legal risk takes absolute precedence; testing halts immediately.
2. **Impact threshold breach** (APTS-HO-012): Active system harm requires immediate containment.
3. **Unexpected findings escalation** (APTS-HO-011): Novel findings require assessment before continued testing.
4. **Confidence-based escalation** (APTS-HO-013): Scope uncertainty requires clarification but does not indicate active harm.
5. **Pre-approval gates** (APTS-HO-001): Routine approval workflow; lowest urgency among escalation types.

When two escalations of equal priority fire simultaneously, the platform processes both and applies the most restrictive combined response (for example, if APTS-HO-012 calls for pause and APTS-HO-011 calls for halt, the platform halts).

---

## APTS-HO-019: 24/7 Operational Continuity and Shift Handoff

**Classification:** SHOULD | Tier 2

### Requirement

For platforms operating in continuous or always-on mode, the platform SHOULD implement governance controls for operational continuity across operator shifts and time zones. This includes:

1. **Shift handoff procedures**: Structured handoff that transfers active engagement state, pending approvals, open escalations, and suppression-rule status to incoming operators
2. **Stale approval expiry**: Automatic expiry of approvals that have not been acted upon within a documented validity window, requiring re-request from the incoming shift
3. **Suppression-rule review**: Periodic review and re-justification of active alert suppression rules to prevent suppression drift over time
4. **Desensitization monitoring**: Tracking of operator response-time trends and alert acknowledgment rates to detect cumulative desensitization

Approval queues SHOULD enforce shift-awareness so that approvals granted by an outgoing operator for future actions are flagged for incoming operator review.

> **Implementation aid:** The [Shift Handoff Template](../appendix/Shift_Handoff_Template.md) provides an informative record format for transferring engagement state, pending approvals, open escalations, suppression-rule status, and kill-switch authority between operators.

### Verification

1. Shift handoff procedure is documented and includes engagement state, pending approvals, and active escalations
2. Test: simulate a shift change; verify incoming operator receives complete handoff state
3. Stale approval expiry is enforced per documented validity window
4. Test: leave an approval pending beyond the validity window; verify it expires and requires re-request
5. Active suppression rules have documented justification and periodic review dates
6. Operator response-time metrics are collected and available for review

---
