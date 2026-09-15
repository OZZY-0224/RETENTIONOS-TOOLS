# RetentionOS ADMIN Block 1.1 — Handoff Packet v1

## CURRENT STATE

Block 1.1 is formally the RetentionOS Follow-Up Agent.

Its responsibility is continuity pursuit of the current sales opportunity until a legitimate human-authoritative terminal outcome:
- BOUND
- LOST

Block 1.1 does not own post-bind thank-you, review, referral generation, referral lineage, or referral-form behavior. Those responsibilities are now assigned to Block 1.2 — Referral Agent.

The accepted Block 1.1 operating architecture is:

HubSpot
= operational CRM state only

Make
= RetentionOS runtime / orchestration / deterministic workflow engine

Anthropic
= minimal, event-driven intelligence layer only where useful

Outlook
= internal notification transport

Jotform
= likely V1 human outcome-capture surface, still not formally locked

GitHub
= engineering source of truth / provenance / prompt contracts / fixtures / snapshots / guardrails

ChatGPT ADMIN lane
= central architecture / product truth / coordination / counter-review / acceptance

Sonnet
= bounded construction specialist

Cowork
= read-only live-system inspector/auditor

Gemini
= secondary architecture challenger

Copilot
= code/API/repository/Microsoft implementation support

Founder
= final product authority

The fixed-prompt operating system is now part of the Block 1.1 engineering method.

## CLOSED ITEMS

Do not reopen without contradictory runtime evidence or a newer founder ruling:

- Option C is locked:
  - production Block 1 scenario 6221572 remains untouched for Block 1.1 initialization
  - Block 1.1 derives initial state from trusted Block 1 / HubSpot evidence

- Block 1.1 state model is exactly:
  - FOLLOWUP_OPEN
  - AWAITING_CUSTOMER
  - DEFERRED
  - BOUND
  - LOST

- FOLLOWUP_DUE is derived from time and is not a state

- No response does not equal LOST

- Human agent reports the sales outcome; machine validates workflow completeness and legal transition

- Machine never infers BOUND / LOST / Customer Declined / DNC from silence or elapsed time

- Human-specified customer follow-up timing is authoritative

- No arbitrary +2h / +24h / +48h callback cadence defaults

- Reminder sent does not mean follow-up complete

- A follow-up cycle is complete only when:
  - an authorized human outcome is recorded
  - or the workflow remains visibly awaiting that outcome

- Due notification idempotency uses a deterministic claim key:
  - contact_id|FOLLOWUP|next_follow_up_at

- DEFERRED expiry uses its own due-condition identity:
  - contact_id|DEFERRED_EXPIRY|deferred_until

- DEFERRED expiry transitions:
  - DEFERRED -> FOLLOWUP_OPEN
  - stage_entered_date = deferred_until
  - clear deferred_until
  - send one idempotent internal agent nudge
  - do not invent next_follow_up_at

- DNC must pass the transition table
  - if valid: LOST + suppression
  - no forced override outside legal state/outcome validation

- Exact eligible count = 6 is a one-time first-migration acceptance gate only
  - not permanent runtime logic

- Ongoing standing initialization uses the same production predicate with no permanent exact-count assertion

- Production bootstrap predicate:
  callback_state NOT_HAS_PROPERTY
  AND is_test_contact HAS_PROPERTY
  AND is_test_contact NEQ true
  AND notes_last_contacted HAS_PROPERTY

- Testing must not silently alter the production business predicate

- Block 1.2 owns:
  - post-bind thank-you
  - review request
  - referral request
  - referral token/link/QR
  - referral form
  - referral attribution
  - referral lineage
  - referral-generated new-lead loop back into Block 1

- Block 1.1 BOUND means:
  - terminal state for Follow-Up Agent
  - future handoff signal for Block 1.2
  - no closeout/referral execution inside Block 1.1

- Claim TTL is an engineering parameter, not a product constant

- No arbitrary 15-minute TTL is accepted

- Outcome-event identity must be genuinely unique

- Weak derived identity such as contact_id + outcome + rounded minute is retired

- The following identities are separate:
  - claim_key = due-condition identity
  - outcome_capture_token = notification-to-outcome correlation token
  - submission_id = unique actual human outcome-submission event ID

- Outcome pending does not require a new callback_state

- "Notification sent, outcome not yet recorded" is represented operationally by:
  - confirmed notification claim
  - persisted outcome_capture_token
  - absence of an accepted correlated outcome event

- No missing-outcome reminder cadence is accepted yet

- Anthropic, if used in Block 1.1, is an intelligence layer only
  - it does not generate state
  - it does not generate IDs
  - it does not decide terminal outcomes
  - it should be called only after a legitimate claimed event, not on every sweep
  - outputs should be minimal and cacheable

- HubSpot should remain deliberately lightweight
  - CRM state store, not RetentionOS automation engine
  - avoid reliance on HubSpot workflow/token automation where Make can own the behavior cleanly

- GitHub is the engineering control surface
  - not the live CRM
  - not the workflow runtime
  - do not rewrite certified Make logic merely to move it into code

## OPEN ITEMS

Only genuine remaining decisions / implementation unknowns:

1. PRODUCT DECISION REQUIRED — DNC voluntary re-entry suppression clearing
   - current fail-safe direction: suppression remains true until explicit authorized human clear
   - no auto-clear accepted

2. PRODUCT DECISION REQUIRED — final outcome-input surface
   - dedicated Jotform outcome form is the leading V1 candidate
   - HubSpot-native controlled input remains possible only if it proves equally atomic/deterministic
   - not yet formally locked

3. ENGINEERING PARAMETER — claim TTL
   - select from observed normal runtime duration + safety margin
   - do not guess product behavior

4. PRODUCT / OPERATIONS POLICY REQUIRED — missing-outcome internal reminder timing
   - outcome-pending representation is accepted
   - reminder cadence is not

5. PLATFORM / ACCESS CONSTRAINT — HubSpot write/property-creation capability
   - current connected tool surface previously reported contact writes requiring reauthorization
   - property-definition creation was not exposed
   - must be reverified before schema construction

6. LIVE IMPLEMENTATION VERIFICATION — RETENTIONOS_CONTROL
   - concept accepted
   - actual datastore existence / supported construction path still requires live verification

7. LIVE IMPLEMENTATION VERIFICATION — isolated Continuity Sweep scenario
   - not yet built

## NEXT ACTION

One concrete next step:

Execute SANDBOX BUILD DIRECTIVE 001.

Bounded objective:
- fresh Make inspection
- confirm no existing Block 1.1 Continuity Sweep scenario conflict
- confirm production 6221572 untouched
- confirm 6254611 remains inactive
- create one new inactive Continuity Sweep shell only
- verify RETENTIONOS_CONTROL construction/reuse path
- no HubSpot contact writes
- no internal emails
- no Jotform submissions
- no claims writes
- no customer-facing side effects
- stop and return observed evidence

Do not proceed beyond this build boundary without a new accepted prompt.

## DO NOT TOUCH

- Make 6221572 — active production Block 1
- Make 5061111 — historical/original Block 1
- Make 6147956 — contaminated forensic sandbox
- BRIDGE_CONTROL / BRIDGE datastores — pattern reference only, never RetentionOS runtime dependency
- Block 1 certified identity resolution
- Block 1 C-13 fail-closed behavior
- Block 1 lifecycle synchronization
- Block 1 retry:false protections
- Block 1 vCard behavior
- Block 1 timeline logging
- call_disposition remains retired / untouched
- appointment display strings must not be used as V1 continuity timing source
- 6254611 must remain inactive until explicit trigger isolation is proven

## ASSET MAP

### HubSpot
Account:
- 242512230

Known useful existing fields:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Planned Block 1.1 fields:
- callback_state
- stage_entered_date
- next_follow_up_at
- callback_outcome
- deferred_until
- retention_contact_suppressed
- retention_contact_suppression_reason

Held unless proven necessary:
- review_required
- last_follow_up_at
- follow_up_owner

### Make
- 6221572 — current production Block 1
- 6254611 — engineering sandbox, inactive, not Block 1.1
- 5061111 — historical original Block 1
- 6147956 — forensic contaminated sandbox
- 6189260 — BRIDGE killswitch reference only

Team:
- 883164

Organization:
- 3954046

Zone:
- us2.make.com

### Jotform
- 262458040681154 — production callback intake
- 260774321233047 — retired callback tracker
- 262454423943055 — historical appointment-input sandbox, read-only

### GitHub
Repository:
- OZZY-0224/RETENTIONOS-TOOLS

Branch:
- block-1-1-v1-spec

Authoritative / current files:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md
- retentionos/block-1-1/ENGINEERING_REVIEW_PACKET.md
- retentionos/block-1-1/SANDBOX_BUILD_DIRECTIVE_001.md
- retentionos/block-1-1/SONNET_UPDATE_001.md
- retentionos/block-1-1/MASTER_PROMPT_AI_STACK_SECTIONS_58_70.md
- retentionos/block-1-1/MASTER_PROMPT_FIXED_PROMPTS_SECTIONS_71_88.md

## LATEST FOUNDER RULINGS

Newest founder rulings override older AI proposals.

Current authoritative rulings:

- Block 1.1 = Follow-Up Agent
- Block 1.2 = Referral Agent
- Block 1.1 ends at BOUND or LOST
- post-bind referral/thank-you/review logic belongs to Block 1.2
- retain Option C; do not modify production Block 1 just to seed Block 1.1
- RetentionOS value should live primarily in Make AI-agent architecture / engineering
- avoid becoming dependent on HubSpot automation/token usage
- HubSpot should remain a lightweight state store
- AI calls should be simple, minimal, low-cost, and event-driven
- Anthropic may enrich internal Follow-Up Agent context but cannot own authoritative state or identifiers
- fixed prompts are part of the operating system
- specialized AI/tool roles are settled and should not be rediscovered
- all newly created/updated project documents should also be attached in the ADMIN chat

## RETROACTIVE REVIEW NORMALIZATION

The prior Sonnet implementation reviews should be filed under the fixed ADMIN counter-review vocabulary.

Accepted implementation findings:
- PROPOSAL COMPATIBLE — two-scenario Block 1.1 architecture
- PROPOSAL COMPATIBLE — claim -> send -> confirm notification flow
- PROPOSAL COMPATIBLE — outcome-pending represented outside callback_state
- PROPOSAL COMPATIBLE — DEFERRED expiry uses its own direct due nudge
- PROPOSAL COMPATIBLE — full removal of Block 1.2 closeout/referral execution from Block 1.1

Corrections already applied:
- first migration exact-count gate is one-time only
- stage_entered_date on DEFERRED expiry = deferred_until, not sweep now
- DNC must use legal transition validation
- test method must not alter production bootstrap predicate
- claim TTL is not fixed at 15 minutes
- claim_key / outcome_capture_token / submission_id are distinct identities
- no arbitrary N-day missing-outcome rule

Open normalized labels:
- PRODUCT DECISION REQUIRED — DNC suppression clear on voluntary re-entry
- PRODUCT DECISION REQUIRED — final outcome-capture surface
- PRODUCT / OPERATIONS POLICY REQUIRED — missing-outcome reminder timing
- ENGINEERING PARAMETER — claim TTL
- PLATFORM / ACCESS CONSTRAINT — HubSpot write/property creation access

## TOOL / AI ROLE FOR NEXT TASK

Next task owner:
ChatGPT Sandbox Agent

Task type:
TYPE B — IMPLEMENTATION
TYPE C — VERIFICATION

Prompt to use:
SANDBOX_BUILD_DIRECTIVE_001.md

ADMIN lane responsibilities after return:
- counter-review against fixed 10-question checklist
- accept/reject observed live state
- authorize only the next smallest safe write

Cowork is not needed yet unless the sandbox agent cannot verify the live implementation path or a compatibility conflict appears.

Sonnet should not receive another broad construction prompt until the first live shell/control-path evidence is returned.
