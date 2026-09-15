# RetentionOS — Block 1.1 Follow-Up Agent Implementation Specification v1.1

Status: AUTHORITATIVE IMPLEMENTATION BASELINE — BLOCK 1.2 SPLIT APPLIED
Branch: block-1-1-v1-spec
Product authority: Founder
Architecture coordinator: ChatGPT
Implementation review: Claude Sonnet
Repository/code review: GitHub Copilot
Microsoft ecosystem support: Microsoft Copilot
Communications support: Gemini
Live deployment audit: Claude Cowork

## 1. Product Mission

Block 1.1 is the RetentionOS Follow-Up Agent.

Its sole mission is:
- preserve continuity of an active sales pursuit
- surface the next required agent action at the human-specified time
- require the human agent to report the call outcome
- validate the reported outcome
- derive the next continuity state
- repeat until the pursuit reaches BOUND or LOST

Block 1.1 is not:
- referral generation
- thank-you/review/referral closeout
- policy servicing
- CSR workflow
- claims workflow
- renewal management
- active-policy retention
- G17 market re-exposure

Those boundaries are now explicit.

## 2. Block Architecture

### Block 1
Transactional intake / quote / trusted evidence engine.

### Block 1.1
Follow-Up Agent.
Continuity pursuit until a legitimate terminal sales outcome.

### Block 1.2
Referral Agent.
New separate discovery/build lane.

Block 1.2 owns:
- post-bind thank-you
- experience/review request
- referral request
- referral links / QR
- referral attribution
- referral lineage
- routing new referral leads back into Block 1

Block 1.1 must not implement those behaviors.

### Block 2
CSR / bound-policy servicing lane.

## 3. Locked Option C Architecture

Production Block 1 scenario 6221572 is not modified to initialize Block 1.1.

Block 1 owns trusted facts.
Block 1.1 owns continuity state and derives its initial state from trusted Block 1 / HubSpot evidence.

Do not reopen Block 1 initialization writes unless the founder explicitly changes Option C.

## 4. Authoritative Asset Roles

- Jotform 262458040681154
  - production callback intake

- Jotform 260774321233047
  - retired callback tracker

- Make 6221572
  - active production Block 1
  - do not modify for Block 1.1 initialization

- Make 6254611
  - engineering sandbox
  - inactive
  - not Block 1.1
  - must be isolated before fresh end-to-end Block 1 -> Block 1.1 testing

- Make 5061111
  - historical original Block 1
  - do not modify

- Make 6147956
  - forensic-only contaminated sandbox
  - never implementation base

- Make 6189260 / BRIDGE_CONTROL 145954
  - pattern reference only
  - never wire RetentionOS to BRIDGE_CONTROL

## 5. Closed Block 1 Contracts

Do not reopen without contradictory runtime evidence:
- deterministic 3/3 identity resolution
- R1 / R2 / R3 / R0
- C-13 fail-closed validation
- retry:false
- lifecycle synchronization
- callback_type preservation
- call_disposition non-use
- PIF
- SR22 / FR44 / None
- appointment display contract
- vCard behavior
- HubSpot timeline logging

Block 1.1 consumes trusted output and does not re-perform those responsibilities.

## 6. Accepted State Model

Exactly five states:
- FOLLOWUP_OPEN
- AWAITING_CUSTOMER
- DEFERRED
- BOUND
- LOST

FOLLOWUP_DUE is not a state.
It is derived from:
current_time >= next_follow_up_at

review_required is not a state.
It is a cross-cutting flag.

### FOLLOWUP_OPEN
An agent action is required.

### AWAITING_CUSTOMER
The agent has completed the current action and the prospect owes the next response/action.

### DEFERRED
The prospect explicitly requested a pause until a specified future time.

### BOUND
Human-authoritative successful terminal state for Block 1.1.

When BOUND is recorded:
- Block 1.1 stops pursuing the lead
- Block 1.1 does not perform post-bind closeout
- BOUND becomes a future handoff event for Block 1.2

### LOST
Human-authoritative unsuccessful terminal state.

Permanent rule:
No response != Lost.

## 7. State / Outcome / Flags Separation

callback_state = where the pursuit is

callback_outcome = what happened in the human interaction

flags = additional conditions such as:
- review_required
- retention_contact_suppressed
- testing indicators

Do not create state-machine explosion.

## 8. Canonical Outcomes

Current approved outcome family:
- Successful Bind
- Rate Changed / Follow Up
- Still Need Time / Credentials / Payment
- No Answer / No Contact
- Priced Out
- Customer Declined
- Do Not Contact

Closed mappings:

### Successful Bind
-> BOUND

### Customer Declined
-> LOST
-> preserve outcome

### Priced Out
-> LOST

### Do Not Contact
-> LOST
-> retention_contact_suppressed = true
-> suppression reason = do_not_contact

### No Answer / No Contact
-> remains active
-> never auto-LOST

### Still Need Time / Credentials / Payment
Depends on subordinate human facts:
- customer owes something and no active agent action is scheduled -> AWAITING_CUSTOMER
- agent owes action at explicit next time -> FOLLOWUP_OPEN + next_follow_up_at
- customer explicitly asks pursuit be paused -> DEFERRED + deferred_until

### Rate Changed / Follow Up
Remains active.
State depends on who owes next action and the explicit timing supplied.

Any current-state/outcome combination not explicitly allowed:
- fail closed
- ILLEGAL TRANSITION
- no guessed destination state

## 9. Follow-Up Timing Contract

All customer follow-up timing is human-specified.

Do not invent:
- +2 hours
- +24 hours
- +48 hours
- callback-type cadence
- fallback windows

If an outcome requires another follow-up:
- next_follow_up_at must be explicitly supplied

If required timing is missing:
- fail validation
- do not synthesize a fallback

If no further follow-up is required:
- next_follow_up_at is not required

## 10. Follow-Up Agent Identity

The AI workflow component is formally named:

RetentionOS Follow-Up Agent

The human sales agent remains the authoritative HubSpot owner.

The Follow-Up Agent is not the owner of the lead.
It is the sales-continuity assistant assigned to push the human owner to complete the next action and report the outcome.

## 11. Internal Email Notification Contract

When a FOLLOWUP_OPEN condition becomes due, the Follow-Up Agent should present itself to the assigned human agent through an internal email notification.

The source of truth is not AI memory.
The source of truth is:
- callback_state
- next_follow_up_at
- assigned HubSpot owner
- idempotent due-event claim

Conceptual email:

From/display identity:
RetentionOS Follow-Up Agent

Subject:
Follow-Up Due — <Contact> — <Callback Type>

Email should provide:
- contact identity
- callback type / reason
- current continuity state
- last known customer interaction context where available
- explicit intended follow-up time
- expected next action
- link/button to LOG CALL OUTCOME
- optional link to open the contact in HubSpot

The internal notification is an agent-workflow action, not customer outreach.

## 12. Due Notification Idempotency

For a due follow-up:

claim_key =
contact_id|FOLLOWUP|next_follow_up_at

Flow:
1. due condition found
2. attempt unique claim
3. only successful claimant may send agent email
4. send internal agent notification
5. confirm claim

If the same record is seen again for the same due timestamp:
- existing valid claim prevents another notification

A later legitimate next_follow_up_at creates a new due condition and therefore a new claim key.

This is how the Follow-Up Agent can present itself at every intended follow-up time without duplicate sends for the same due condition.

## 13. Outcome Completion Doctrine

A follow-up is not complete when the reminder email is sent.

A follow-up cycle is complete only when:
- the human agent records an authorized outcome
or
- the workflow remains visibly awaiting that outcome

Do not confuse:
notification delivered
with:
sales action completed

This distinction is authoritative.

## 14. Outcome Verification Responsibility

The human agent reports what happened.

The machine does not infer:
- BOUND
- LOST
- Customer Declined
- DNC
from silence, elapsed time, repeated failed calls, or task age.

The Follow-Up Agent / State Engine verifies:
- outcome is canonical
- current transition is legal
- required subordinate facts are present
- explicit next timing exists where required
- suppression rules are respected

The machine verifies workflow completeness and contract validity, not the truthfulness of the human's account of the conversation.

## 15. Outcome Input Surface

The V1 input surface remains an implementation decision.

Preferred qualities:
- atomic event
- unique event/submission ID
- contact_id
- agent identity
- canonical callback_outcome
- subordinate facts
- explicit next_follow_up_at when required
- deferred_until when required
- submission timestamp

A small dedicated Jotform agent-outcome form is currently a strong candidate because it naturally provides a unique submission event and conditional required fields.

A HubSpot-native controlled action remains a valid alternative if live inspection proves it can provide equally atomic, deterministic input without exposing callback_state for direct editing.

Do not accept weak deduplication such as timestamp-rounded-to-minute event identity.

## 16. Missing Outcome Follow-Up

The Follow-Up Agent must distinguish:
- due notification sent
from:
- outcome recorded

If a due notification has been sent but no authorized outcome is recorded, the lead must remain visibly unresolved.

A future internal agent reminder may state:
Outcome still needed — <Contact>

This reminder must not alter customer state automatically.

The exact reminder timing for missing agent outcomes is not yet part of the customer follow-up cadence contract and should not be invented during the first build slice.

## 17. Deferred Contract

DEFERRED means legitimate customer-directed suspension.

Required:
- callback_state = DEFERRED
- deferred_until = explicit timestamp

When deferred_until is reached:
- the deferment ends
- the agent's action becomes due
- transition to FOLLOWUP_OPEN
- stage_entered_date should reflect the deferment boundary
- clear deferred_until
- create one idempotent agent-facing nudge for the expiry condition
- do not invent a new customer follow-up timestamp

If the customer re-enters before deferred_until:
- customer action supersedes the deferment
- DEFERRED -> FOLLOWUP_OPEN according to current valid context

## 18. Re-Entry Contract

### Active + same callback_type
- same pursuit
- refresh current context
- no parallel cycle by default

### Active + changed callback_type
- same active pursuit
- latest valid callback_type becomes operational intent

### LOST + valid new intake
- legitimate re-engagement
- LOST -> FOLLOWUP_OPEN
- preserve prior LOST history

### BOUND + new intake
- never auto-reopen
- may represent a new opportunity
- if contact-level data cannot distinguish the new transaction:
  CONSTRAINT / GAP DISCOVERED — PRODUCT DECISION REQUIRED

### DNC + later voluntary re-entry
Unresolved product rule:
- whether suppression is automatically cleared is not yet accepted

Fail-safe current direction:
- pursuit may be recognized as re-engagement only under an approved transition
- retention_contact_suppressed remains true unless explicitly cleared by an authorized human action

This remains a product decision before automated outreach from such a re-entry.

## 19. Option C Bootstrap

Initialization is idempotent and initialization-only.

Hardened predicate:

callback_state NOT_HAS_PROPERTY
AND
is_test_contact HAS_PROPERTY
AND
is_test_contact NEQ true
AND
notes_last_contacted HAS_PROPERTY

On successful initialization:
- callback_state = awaiting_customer
- stage_entered_date = notes_last_contacted

Do not use:
- now
- createdate
- appointment inference

Bootstrap never overwrites an initialized record.

### First controlled migration gate
Previously measured expected eligible count:
6

Before the first production initialization write:
- re-run the final predicate
- expected count must equal exactly 6
- otherwise halt and investigate

After that controlled migration:
- same predicate should return 0

### Ongoing production bootstrap
The exact-6 rule is not permanent.

After rollout, newly completed legitimate Block 1 contacts satisfying the same predicate must initialize normally.

A future non-zero eligible count is expected normal intake, not automatically an exception.

## 20. HubSpot Schema

Observed existing fields:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Block 1.1 fields currently not present at last verification:
- callback_state
- stage_entered_date
- next_follow_up_at
- callback_outcome
- last_follow_up_at
- deferred_until
- review_required
- retention_contact_suppressed
- retention_contact_suppression_reason

Intended V1:

### callback_state
enumeration:
- followup_open
- awaiting_customer
- deferred
- bound
- lost

### stage_entered_date
datetime

### next_follow_up_at
datetime

### callback_outcome
enumeration:
- successful_bind
- rate_changed_follow_up
- still_need_time_credentials_payment
- no_answer_no_contact
- priced_out
- customer_declined
- do_not_contact

### deferred_until
datetime

### retention_contact_suppressed
boolean

### retention_contact_suppression_reason
enumeration:
- do_not_contact

### review_required
hold unless implementation evidence requires explicit V1 use

### last_follow_up_at
hold until a precise event definition is proven necessary

Do not create follow_up_owner unless HubSpot ownership proves insufficient.

## 21. Runtime Architecture

Two new Make scenarios.

### Scenario A — Outcome Intake + State Engine
Event-driven.

Responsibilities:
- receive atomic agent outcome event
- read RETENTIONOS_CONTROL first
- fetch current contact
- validate state
- validate canonical outcome
- validate subordinate facts
- validate transition
- validate required timestamps
- derive next state
- write HubSpot
- read back destination state
- record exception/event evidence where implemented
- do not perform Block 1.2 behavior

DNC must still pass through the transition contract.
Do not force LOST regardless of current state.

### Scenario B — Continuity Sweep
Scheduled.

Responsibilities:
1. initialize eligible uninitialized contacts
2. process due FOLLOWUP_OPEN conditions
3. process DEFERRED expiry
4. support claim recovery/idempotency

Top-level control gate must fail closed.

## 22. RETENTIONOS_CONTROL

Separate RetentionOS control mechanism.

Do not connect to BRIDGE_CONTROL.

Fields:
- gate_state
- policy_version
- updated_at
- updated_by

V1:
- CLEAR
- ENGAGED

Only exact CLEAR permits side effects.

Anything else:
zero side effects.

## 23. RETENTIONOS_CONTINUITY_CLAIMS

Recommended isolated datastore.

Fields:
- claim_key
- contact_id
- condition_type
- due_at
- claim_state
- claimed_at
- claim_expires_at
- confirmed_at
- scenario_execution_id

Lifecycle:
CLAIMED -> CONFIRMED
or
CLAIMED -> EXPIRED / eligible for controlled recovery

Claim TTL is an engineering parameter.
Do not hardcode an arbitrary product value before observing actual runtime duration and selecting a safety margin.

## 24. Exceptions and Event History

Recommended lightweight support:

RETENTIONOS_EXCEPTIONS
- visible fail-closed evidence

RETENTIONOS_CONTINUITY_EVENTS
- immutable transition/history evidence

These are support systems and must not delay proving the core initialization slice.

## 25. Test Discipline

Never create a test record before defining:
- test case
- input
- expected route
- expected state
- expected timestamp
- expected agent email behavior
- expected claim behavior
- expected suppression
- readback acceptance
- stop/rollback condition

One fixture at a time.

No production-customer tests.

is_test_contact alone is not isolation.

Do not use a different production predicate merely to make a test pass.
Testing must preserve production business logic while the environment/transport is isolated deliberately.

## 26. Make Change Discipline

Before every Make modification:
1. fresh scenario read
2. verify ID
3. verify asset role
4. capture lastEdit
5. inspect exact modules in scope
6. inspect topology/handlers
7. define allowed change

Prohibit:
- unsupported and(
- unsupported or(
- retry:true on fail-closed/non-idempotent action paths

After every modification:
1. fresh read
2. verify intended change
3. verify untouched modules
4. verify topology
5. verify handlers
6. verify trigger
7. verify connections
8. verify no unintended drift

Use expectedLastEdit where supported.

## 27. Blueprint Preflight

Blueprint Preflight Gate V0 remains a static guardrail.

It checks known bad patterns such as:
- retry:true
- literal and(
- literal or(

Passing V0 does not certify a scenario.
Human/system verification remains required before promotion.

## 28. AI Team Roles

### Founder
Product authority and final acceptance.

### ChatGPT
Central architecture, implementation coordination, counter-review, acceptance and promotion logic.

### ChatGPT Sandbox Agent
Hands-on Make / HubSpot / Jotform sandbox construction and controlled trials.

### Claude Sonnet
Construction engineer and implementation challenger.

### GitHub Copilot
Repository/code integrity:
- schemas
- fixtures
- validators
- snapshots
- diff tooling
- static tests
- provenance

Referral-lineage work is now Block 1.2, not Block 1.1.

### Microsoft Copilot
Microsoft 365 / Outlook-side workflow review and communication transport support.

### Gemini
For Block 1.1:
- internal agent email wording
- outcome-reminder wording
- sales-assistant communication variants

Thank-you/review/referral copy moves to Block 1.2.

### Claude Cowork
Read-only live-system inspection and final deployment audit.

## 29. Block 1.1 Acceptance Targets

### Initialization
- valid Block 1 evidence initializes
- initialization happens once
- initialized record is never overwritten by bootstrap
- stage timestamp derives from notes_last_contacted
- test contacts excluded
- pre-Gen3 back-book excluded
- first migration gate matches expected population
- ongoing future eligible contacts initialize normally

### Follow-Up Agent
- FOLLOWUP_OPEN before due -> no email
- FOLLOWUP_OPEN at/after due -> one claim / one internal agent email
- repeated sweep -> no duplicate email for same due condition
- new valid next_follow_up_at -> later new email
- internal email identifies RetentionOS Follow-Up Agent
- email contains outcome-capture action
- reminder sent does not mark workflow complete
- outcome remains visibly pending until recorded

### Outcome Engine
- canonical outcome accepted
- invalid outcome rejected
- invalid state rejected
- illegal transition rejected
- missing required timestamp rejected
- Successful Bind -> BOUND
- Customer Declined -> LOST
- Priced Out -> LOST
- DNC -> allowed LOST transition + suppression
- No Answer never auto-LOST
- readback verifies destination state

### Deferred
- before deferred_until -> no agent action
- at deferred_until -> FOLLOWUP_OPEN + one idempotent agent nudge
- early customer return supersedes deferment

### Re-entry
- active same type -> same pursuit
- active changed type -> latest valid intent
- LOST valid re-entry -> active again with history preserved
- BOUND re-entry -> no automatic reopen
- DNC suppression persistence enforced until explicitly cleared under approved rule

### Governance
- CLEAR permits
- ENGAGED blocks
- missing gate blocks
- malformed gate blocks
- claim collision prevents duplicate nudge
- expired abandoned claim recoverable

## 30. Construction Order

1. Freeze this revised Block 1.1-only specification
2. GitHub Copilot review of schemas / fixtures / guardrails
3. Verify HubSpot write capability
4. Create minimum required HubSpot fields
5. Verify/create RETENTIONOS_CONTROL
6. Create inactive Continuity Sweep shell
7. Add control gate only
8. Add read-only Option C bootstrap query
9. Re-verify first migration count
10. Define isolated bootstrap trial method
11. Authorize controlled initialization write
12. Verify destination state and second-pass zero
13. Create inactive Outcome Intake + State Engine shell
14. finalize controlled atomic outcome-input surface
15. implement terminal mappings
16. implement active mappings
17. implement required timestamp validation
18. implement FOLLOWUP_OPEN due branch
19. implement claims/idempotency
20. implement internal Follow-Up Agent email
21. implement outcome-capture link
22. verify reminder != completion
23. implement DEFERRED expiry
24. implement DNC suppression
25. implement re-entry rules
26. isolate 6254611 before fresh end-to-end Block 1 testing
27. run one fixture at a time
28. Gemini wording review for internal Follow-Up Agent messages
29. Copilot final code/tooling audit
30. Cowork live-system deployment audit
31. founder acceptance
32. promotion/activation under RETENTIONOS_CONTROL

## 31. Explicitly Out of Scope — Block 1.2

Do not build these inside Block 1.1:
- thank-you email
- rate-your-experience request
- referral request
- referral QR/link
- referral forms
- referral tokens
- referral lineage
- referral attribution
- referral-generation follow-up
- post-bind organic lead loop

These belong to the new Block 1.2 Referral Agent lane.

## 32. Current Next Action

The first sandbox build directive is now:
- create new inactive Block 1.1 Continuity Sweep shell
- no production modification
- no contact writes
- no emails
- no test submissions
- verify control-gate construction path
- add read-only Option C bootstrap query only after control path is understood
- stop for review before any state-writing behavior

## 33. Operating Principle

Preserve accepted behavior.
Inspect before writing.
Build the smallest correct thing.
Fail closed when uncertain.
Verify destination, not just execution.
Do not invent product behavior.
A reminder is not a completed follow-up.
Close accepted work and move forward.
