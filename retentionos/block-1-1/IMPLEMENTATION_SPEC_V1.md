# RetentionOS — Block 1.1 Implementation Specification v1

Status: DRAFT FOR ENGINEERING REVIEW
Branch: block-1-1-v1-spec
Product authority: Founder
Architecture coordinator: ChatGPT
Implementation review: Claude Sonnet
Code/repository review: GitHub Copilot
Microsoft ecosystem support: Microsoft Copilot
Communications-library support: Gemini
Live inspection / deployment audit: Claude Cowork

## 1. Product Mission

Block 1.1 is RetentionOS's AI-assisted sales continuity layer.

Block 1 answers:
- what happened during the callback / quote interaction?

Block 1.1 answers:
- what needs to happen next?
- who owns it?
- when is it supposed to happen?
- what happened when that action occurred?
- how is progress preserved until the sales pursuit reaches a legitimate outcome?

Block 1.1 is not:
- policy servicing
- CSR workflow
- claims workflow
- renewal management
- active-policy retention

The only approved post-bind extension in Block 1.1 is:
- thank-you
- customer-experience / review request
- referral-generation closeout

Anything involving ongoing bound-policy servicing belongs to Block 2.

## 2. Locked Architecture

### Option C
Production Block 1 scenario 6221572 is not modified to initialize Block 1.1.

Block 1 owns trusted evidence.
Block 1.1 owns continuity state and derives initial state from trusted Block 1 / HubSpot evidence.

### Existing asset roles

- Jotform 262458040681154
  - current role: production callback intake
  - historical SANDBOX naming is irrelevant

- Jotform 260774321233047
  - current role: retired callback tracker

- Make 6221572
  - current role: active production Block 1
  - do not modify for Block 1.1 initialization

- Make 6254611
  - current role: engineering sandbox
  - inactive
  - not a Block 1.1 scenario
  - must be isolated before fresh end-to-end Block 1 -> Block 1.1 testing

- Make 5061111
  - historical original Block 1
  - do not modify

- Make 6147956
  - forensic-only contaminated sandbox
  - never use as implementation base

- Make 6189260 / BRIDGE_CONTROL 145954
  - governance-pattern reference only
  - never connect RetentionOS directly to BRIDGE_CONTROL

## 3. Closed Block 1 Contracts

Do not reopen without contradictory runtime evidence:

- deterministic 3/3 identity resolution
- R1 MATCH / R2 COLLISION / R3 NEW / R0 halt
- C-13 fail-closed validation
- retry:false
- lifecycle synchronization
- callback_type preservation
- call_disposition non-use
- PIF behavior
- SR22 / FR44 / None behavior
- appointment display contract
- vCard semantics
- HubSpot timeline logging

Block 1.1 consumes trusted output and does not re-perform those responsibilities.

## 4. Accepted V1 State Model

Exactly five states:

- FOLLOWUP_OPEN
- AWAITING_CUSTOMER
- DEFERRED
- BOUND
- LOST

### FOLLOWUP_OPEN
Active sales pursuit where another agent action is required.

### AWAITING_CUSTOMER
Current agent action is complete and the prospect owes the next response/action.

### DEFERRED
The prospect explicitly requested a legitimate delay until a specified future time.

### BOUND
Human-authoritative successful terminal sales state.

Meaning inside Block 1.1:
- sale written
- stop sales chasing
- permit one-time sales closeout
- do not begin policy servicing

### LOST
Human-authoritative unsuccessful terminal sales state.

Permanent rule:
No response != Lost.

## 5. State / Outcome / Flags Separation

### State
callback_state
Answers:
- where is the pursuit operationally?

### Outcome
callback_outcome
Answers:
- what happened in the human interaction?

### Flags
Represent cross-cutting conditions, not workflow position.

Examples:
- review_required
- retention_contact_suppressed
- testing indicators

Do not create state-machine explosion.

## 6. Accepted Outcome Family

Current canonical outcomes:

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
-> preserve outcome = Customer Declined

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
Depends on subordinate truth:
- customer owes something and agent has no active scheduled action -> AWAITING_CUSTOMER
- agent owes action at explicit timestamp -> FOLLOWUP_OPEN + next_follow_up_at
- customer explicitly asks pursuit be paused until a date/time -> DEFERRED + deferred_until

### Rate Changed / Follow Up
Remains active.
State is derived from who owes the next action and whether an explicit follow-up time exists.

If a current-state / outcome combination is not explicitly valid:
- fail closed
- emit ILLEGAL TRANSITION
- do not guess a plausible state

## 7. Follow-Up Timing Contract

Human-specified timing is authoritative.

Do not invent:
- +2 hours
- +24 hours
- +48 hours
- callback-type cadence defaults
- arbitrary follow-up windows

If an outcome requires another follow-up:
- a valid explicit next_follow_up_at is required

If required timing is missing:
- fail validation
- do not create a fallback time

If no further follow-up is required:
- next_follow_up_at is not required

Due is derived:
current_time >= next_follow_up_at

FOLLOWUP_DUE is not a persistent state.

## 8. Deferred Contract

DEFERRED means customer-directed intentional suspension.

Required:
- callback_state = DEFERRED
- deferred_until = explicit timestamp/date-time

If customer re-enters before deferred_until:
- customer action supersedes old deferment
- DEFERRED -> FOLLOWUP_OPEN according to new valid context
- old deferred date no longer governs current pursuit

## 9. Re-Entry Contract

### Active + same callback type
- same pursuit
- do not create parallel cycle by default
- refresh context / timing according to approved outcome

### Active + changed callback type
- same current sales pursuit
- latest valid callback_type becomes operational intent

### LOST + new valid intake
- legitimate re-engagement
- LOST -> FOLLOWUP_OPEN
- preserve prior historical LOST outcome/evidence
- new active cycle becomes current continuity cycle

### BOUND + new intake
- never auto-reopen
- may represent another policy, vehicle, household member, transaction, or need
- if contact-level data cannot distinguish opportunity identity:
  CONSTRAINT / GAP DISCOVERED — PRODUCT DECISION REQUIRED

## 10. Option C Bootstrap Contract

Initialization is idempotent and initialization-only.

Known strong evidence fields already present in HubSpot:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Hardened initialization predicate to verify before write:

- callback_state NOT_HAS_PROPERTY
- is_test_contact HAS_PROPERTY
- is_test_contact NEQ true
- notes_last_contacted HAS_PROPERTY

On successful initialization:
- callback_state = awaiting_customer
- stage_entered_date = notes_last_contacted

Do not use:
- now
- createdate
- inferred appointment timing

Bootstrap must never overwrite an already initialized record.

Previously measured expected first-run eligible population:
- 6 contacts

Controlled deployment gate:
- before writes, final eligibility query must equal exactly 6
- if count differs, halt
- after successful initialization, same predicate should return 0

No production bootstrap write is authorized merely by this specification.

## 11. HubSpot Schema

Observed existing useful properties:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Observed Block 1.1 properties not yet present:
- callback_state
- stage_entered_date
- next_follow_up_at
- callback_outcome
- last_follow_up_at
- deferred_until
- review_required
- retention_contact_suppressed
- retention_contact_suppression_reason

Current intended field contract:

### callback_state
single enumeration
internal values:
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
single enumeration
internal values proposed:
- successful_bind
- rate_changed_follow_up
- still_need_time_credentials_payment
- no_answer_no_contact
- priced_out
- customer_declined
- do_not_contact

### last_follow_up_at
hold until a precise event definition is confirmed during implementation.
Do not create just because it sounds useful.

### deferred_until
datetime

### review_required
boolean
cross-cutting flag

### retention_contact_suppressed
boolean

### retention_contact_suppression_reason
enumeration
initial V1 value:
- do_not_contact

Do not create follow_up_owner unless existing HubSpot ownership proves insufficient.

## 12. Runtime Architecture

Block 1.1 should use two new Make scenarios.

### Scenario A — Outcome Intake + State Engine

Human/event-driven.

Responsibilities:
- receive approved agent-authoritative outcome input
- read control gate first
- fetch current contact state
- validate current state
- validate canonical outcome
- validate required subordinate facts
- derive state deterministically
- set timestamps
- set deferment where applicable
- set suppression where applicable
- prevent machine-authored BOUND / LOST
- fail visibly on illegal transitions
- preserve historical event evidence where implemented

Agent reports what happened.
The state engine determines what that means.

Agents should not directly set arbitrary callback_state values.

### Scenario B — Continuity Sweep

Scheduled/time-driven.

V1 responsibilities:
1. initialization
2. FOLLOWUP_OPEN due processing
3. DEFERRED expiration processing

At start:
- read RETENTIONOS_CONTROL
- only exact CLEAR permits side effects

Initialization branch:
- search eligible uninitialized records
- first controlled deployment count gate = exact expected count
- write awaiting_customer + stage_entered_date from notes_last_contacted
- never overwrite initialized state

FOLLOWUP_OPEN branch:
- callback_state = followup_open
- next_follow_up_at <= now
- not test
- not suppressed
- acquire unique claim
- create one agent-facing nudge
- confirm claim
- avoid duplicate side effect

DEFERRED branch:
- callback_state = deferred
- deferred_until <= now
- not test
- not suppressed
- transition to FOLLOWUP_OPEN according to approved rules
- clear old deferment
- preserve actual transition boundary timestamp
- do not invent next follow-up time

## 13. RetentionOS Control

Create isolated RetentionOS-side control, not BRIDGE_CONTROL.

Concept:
RETENTIONOS_CONTROL

Fields:
- gate_state
- policy_version
- updated_at
- updated_by

Allowed V1 values:
- CLEAR
- ENGAGED

Exact behavior:
- CLEAR -> side effects may proceed
- ENGAGED -> zero side effects
- missing -> zero side effects
- typo -> zero side effects
- malformed -> zero side effects
- unreadable -> zero side effects
- unknown -> zero side effects

No PAUSED / DRAIN in V1 unless demonstrated need appears.

## 14. Continuity Claim / Idempotency Contract

Scheduled processing must tolerate repeated polling.

Recommended isolated datastore:
RETENTIONOS_CONTINUITY_CLAIMS

Conceptual fields:
- claim_key
- contact_id
- condition_type
- due_at
- claim_state
- claimed_at
- claim_expires_at
- confirmed_at
- scenario_execution_id

Unique key:
contact_id|condition_type|due_timestamp

Claim lifecycle:
- CLAIM
- ACT
- CONFIRM
- EXPIRE if abandoned

A valid existing claim for the same due condition:
- skip duplicate action

Expired unconfirmed claim:
- eligible for controlled recovery

## 15. Exception Visibility

Recommended lightweight store:
RETENTIONOS_EXCEPTIONS

Purpose:
- refused operation must remain visible

Candidate exception types:
- INITIALIZATION_EVIDENCE_MISSING
- INVALID_STATE
- INVALID_OUTCOME
- ILLEGAL_TRANSITION
- CONTROL_INVALID
- CLAIM_STUCK
- MISSING_REQUIRED_TIMESTAMP
- SUPPRESSION_CONFLICT

Doctrine:
bad or incomplete truth -> no side effect -> visible exception

## 16. Continuity Event History

Recommended lightweight store:
RETENTIONOS_CONTINUITY_EVENTS

Purpose:
- preserve sales history through re-entry and state changes

Candidate event types:
- INITIALIZED
- FOLLOWUP_DUE
- NUDGE_CREATED
- CUSTOMER_REPLIED
- DEFERRED
- DEFERMENT_RELEASED
- REENTRY
- BOUND
- LOST
- DNC
- BOUND_CLOSEOUT_SENT
- REFERRAL_CREATED

History is not a replacement for current HubSpot state.

## 17. Agent-Facing Nudge Contract

V1 should primarily create internal sales-assistant prompts, not autonomous customer follow-up.

A useful nudge should tell the agent:
- who
- why
- what happened last
- what is due
- when it is due
- what next action is expected

The AI assistant is intended to push workflow, protect conversion, and reduce neglected opportunities.

## 18. Bound Sales Closeout

BOUND stops the active sales chase.

One final sales-side closeout is permitted:

BOUND
-> stop continuity timers
-> one-time closeout
-> thank-you email
-> rate-your-experience CTA
-> referral CTA

This is not policy servicing.

A one-time closeout idempotency mechanism is required.
Candidate concept:
- bound_closeout_sent
or equivalent event/claim

Do not create a new workflow state such as THANK_YOU_SENT.

## 19. Referral Agent

Referral Agent is a sales-originated shared service, initially owned by Block 1.1.

Responsibilities:

### Closeout activation
- thank-you
- review request
- referral request
- unique referral identity / link / QR

### Referral attribution
- identify immediate referrer
- identify root referrer
- preserve agent ownership
- preserve referral generation
- set lead source = Client Referral

### Referral lineage
Track relationship chain across generations.

Conceptual record:
- referral_id
- referrer_contact_id
- referred_contact_id
- root_contact_id
- parent_referral_id
- referral_agent_id
- generation
- referral_token
- status
- created_at
- submitted_at
- bound_at

Do not store live customer data in GitHub.
GitHub stores schema, code, contracts, fixtures, validators and static assets only.

## 20. Referral Flow

Preferred consent-safe flow:

BOUND customer
-> receives unique referral link / QR
-> shares link voluntarily
-> referred person submits their own information
-> lead_source = Client Referral
-> agent attribution preserved
-> new lead enters normal Block 1 path
-> Block 1.1 follows it
-> if BOUND, Referral Agent can start next generation

Do not create a separate referral sales engine.

## 21. Jotform Role

Possible new future assets:
- customer-experience / rating form
- referral lead form
- agent outcome intake only if HubSpot UI is not sufficiently clean

Do not repurpose retired Callback Tracker by default.

No form should be created or published until its exact field contract, routing, and acceptance case are defined.

## 22. Appointment Timing

Do not rely on display-only appointment strings for Block 1.1 V1 due logic.

Do not infer missing year.

APPOINTMENT_DUE should not be introduced until a structured reliable appointment timestamp exists.

## 23. Contract Halt Blind Spot

R0 invalid contract creates no contact.
Block 1.1 is structurally blind to those halted intakes.

Accepted V1 blind spot.
Do not modify Block 1 merely to solve it.

## 24. R2 Collision

Existing R2 collision review task remains the human review mechanism.

Do not reopen Block 1 merely to seed review_required.

If later runtime evidence proves Block 1.1 needs explicit R2 visibility, raise a bounded implementation decision.

## 25. AI Team Responsibilities

### Founder
- product authority
- final behavior rulings
- acceptance

### ChatGPT
- authoritative project context
- architecture
- build sequencing
- direct backend work where available
- counter-review
- acceptance
- promotion logic

### ChatGPT Sandbox Agent
- hands-on Make / HubSpot / Jotform construction
- route scenarios to correct forms
- controlled trial setup
- one acceptance fixture at a time

### Claude Sonnet
Receives this contract.
Task:
- propose smallest safe technical construction
- Make topology
- schemas
- race conditions
- idempotency
- failure handling
- test plan
- do not invent product behavior

### GitHub Copilot
Repository / code engineering:
- schema validation
- fixture validation
- state-table validation
- referral-lineage code review
- preflight tooling
- snapshots / hashes
- diff validation
- developer tests

### Microsoft Copilot
Microsoft ecosystem support:
- Outlook / Microsoft 365 workflow review
- communication transport-side operational support
- Microsoft-side implementation checks where useful

### Gemini
Communications library:
- bound thank-you variants
- review-request variants
- referral-request variants
- referral reminders
- referral thank-you
- agent nudge language

Gemini does not define state logic.
Accepted copy should be versioned and reviewed before live use.

### Claude Cowork
Read-only live-system inspector:
- actual Make topology
- mappings
- filters
- HubSpot properties
- connections
- sandbox isolation
- control gate
- claims
- suppression
- referral mappings
- final deployment audit

## 26. Test Discipline

Never create a test entry before defining:

- test case name
- input
- expected route
- expected state
- expected timestamp
- expected task/nudge
- expected email behavior
- expected datastore behavior
- expected suppression
- acceptance readback
- rollback/stop condition

One fixture at a time.

No bulk synthetic entries.

No production-customer testing.

is_test_contact alone is not sufficient isolation.

Before customer-facing testing verify:
- sandbox trigger cannot consume production messages
- real customer email cannot be sent
- test identities are synthetic
- no production task pollution
- no unintended production contact mutation

## 27. Make Change Discipline

Before any Make modification:
1. fresh scenario read
2. verify scenario ID
3. verify asset role
4. capture lastEdit
5. inspect target modules
6. inspect handlers and topology
7. define exactly what may change

Prohibit:
- retry:true on fail-closed/non-idempotent paths
- unsupported and(
- unsupported or(

After modification:
1. fresh read
2. verify intended change
3. verify untouched modules
4. verify topology
5. verify handlers
6. verify trigger
7. verify connections
8. verify no unintended drift

Use expectedLastEdit where supported.

## 28. GitHub Engineering Control Plane

Repository:
OZZY-0224/RETENTIONOS-TOOLS

Recommended structure:

retentionos/block-1-1/
- README.md
- architecture/
- contracts/
- schemas/
- fixtures/
- tests/
- referral/
- communications/
- preflight/
- snapshots/
- diff-validator/

GitHub holds:
- code
- schemas
- contracts
- fixtures
- validators
- static communication assets
- provenance

GitHub does not hold the live customer referral database.

## 29. Acceptance Targets

Minimum final proving set:

### Initialization
- valid Block 1 evidence initializes
- initialization happens once
- initialized records are not overwritten
- stage timestamp derives from evidence
- test contacts excluded
- pre-Gen3 back-book excluded
- missing required completion evidence fails safely

### Callback continuation
- Callback Scheduled continuity
- Same-Day Loading Payment continuity
- Rate Change Follow Up continuity
- explicit human-specified next follow-up
- missing required follow-up timestamp fails visibly

### Continuity Sweep
- FOLLOWUP_OPEN before due -> no action
- FOLLOWUP_OPEN due -> one claim / one nudge
- duplicate concurrent sweep -> no duplicate nudge
- abandoned claim can expire/recover
- DEFERRED before due -> unchanged
- DEFERRED expiry -> approved active transition

### Outcome Engine
- Successful Bind -> BOUND
- Priced Out -> LOST
- Customer Declined -> LOST
- DNC -> LOST + suppression
- No Answer does not auto-Lost
- malformed outcome -> no write
- malformed state -> fail closed
- illegal transition -> no write

### Re-entry
- early DEFERRED customer return -> active
- active same type -> same pursuit
- active changed type -> latest valid intent
- LOST new valid intake -> reopen
- BOUND new intake -> no automatic reopen

### Governance
- CLEAR permits
- ENGAGED -> zero side effects
- missing gate -> zero
- malformed gate -> zero

### Bound closeout / referral
- bound closeout occurs once
- thank-you sends once
- review CTA correct
- referral CTA/token correct
- referral submission attributed to immediate referrer
- root attribution preserved
- agent ownership preserved
- new referral enters normal Block 1 / Block 1.1 flow

## 30. Construction Sequence

One business objective at a time.

1. Freeze this specification after peer review
2. Sonnet implementation review
3. ChatGPT counter-review
4. Copilot repository/schema/fixture review
5. Verify HubSpot write capability
6. Create minimum HubSpot state fields
7. Verify RetentionOS control datastore existence or create via supported Make UI/workflow
8. Create inactive Continuity Sweep shell
9. Add control gate
10. Add bootstrap dry-run only
11. Verify expected population
12. Add controlled bootstrap write
13. Verify readback and second-pass zero
14. Create inactive Outcome / State Engine shell
15. Implement terminal outcome mappings
16. Implement active outcome mappings
17. Implement explicit follow-up timestamp validation
18. Implement FOLLOWUP_OPEN due branch
19. Implement claims/idempotency
20. Implement DEFERRED branch
21. Implement DNC suppression
22. Implement re-entry
23. Isolate 6254611 before fresh end-to-end Block 1 testing
24. Run one end-to-end fixture at a time
25. Add bound closeout
26. Freeze Gemini communication library
27. Add referral form and attribution
28. Add referral lineage
29. Copilot final code/tooling audit
30. Cowork live-system deployment audit
31. Founder acceptance
32. Promote / activate under RetentionOS control gate

## 31. Current Observed Live State

Fresh observations before this spec was written:

### HubSpot
Present:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Not found:
- callback_state
- stage_entered_date
- next_follow_up_at
- callback_outcome
- last_follow_up_at
- deferred_until
- review_required
- retention_contact_suppressed
- retention_contact_suppression_reason

### Make 6221572
- active production
- lastEdit observed: 2026-09-13T04:27:13.752Z
- 3 incomplete executions
- not waiting on incomplete executions
- certified topology present
- connections currently reported healthy

### Make 6254611
- inactive
- lastEdit observed: 2026-09-12T23:54:47.744Z
- 0 incomplete executions
- structurally mirrors Block 1
- must remain inactive until trigger isolation is proven

### Existing Block 1.1 scenarios
- none found by current Make scenario search

### RetentionOS control datastore
- current Make tool surface cannot enumerate/create datastores
- existence remains UNVERIFIED
- do not assume absent or present

### HubSpot write/tool limitation
Current connected HubSpot tool reported contact writes requiring reauthorization.
Property-definition creation is not exposed through the current HubSpot connector surface.
This is an implementation-access constraint, not a product gap.

## 32. True Product Gaps

At this checkpoint, no product decision blocks initial Option C bootstrap construction.

Future possible gap:
- BOUND re-entry that cannot be distinguished at contact level may require opportunity/quote identity.
Do not solve unless V1 is blocked.

## 33. Immediate Next Engineering Step

TYPE B / TYPE C boundary:

Before live construction:
1. send this spec to Sonnet for implementation review
2. review Sonnet response against the product contract
3. let GitHub Copilot validate repo contracts / fixtures / schemas
4. do not create test entries yet
5. do not modify production
6. prepare the sandbox agent's first bounded directive:
   - create inactive Continuity Sweep shell
   - control gate
   - bootstrap dry-run only
   - no contact writes

## 34. Operating Principle

Preserve accepted behavior.
Inspect before writing.
Build the smallest correct thing.
Fail closed when uncertain.
Verify destination, not just execution.
Do not invent product behavior.
Close accepted work and move forward.
