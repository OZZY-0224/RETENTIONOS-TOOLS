# Claude Sonnet — Block 1.1 Follow-Up Agent Update

Status:
- Block 1.1 has been narrowed and finalized as the Follow-Up Agent.
- Block 1.2 Referral Agent is now a separate architecture lane.
- Your prior implementation review was accepted with revisions and incorporated into the authoritative spec.

Authoritative source:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md
- branch: block-1-1-v1-spec

## Architecture Split

### Block 1
Intake / quote / trusted-evidence engine.

### Block 1.1
Follow-Up Agent.
Continuity pursuit until BOUND or LOST.

### Block 1.2
Referral Agent.
Post-bind thank-you / review / referral generation / referral lineage / new-lead loop back into Block 1.

Do not put Block 1.2 logic into Block 1.1.

## Accepted Revisions To Your Prior Review

1. The exact count of 6 is a one-time migration gate only.
   - First controlled bootstrap: expected count = 6.
   - After migration: same predicate should return 0.
   - Ongoing production: future eligible contacts should initialize normally.
   - A later non-zero count is not automatically an exception.

2. Sandbox testing must not silently replace the production bootstrap predicate with a different business rule.
   - is_test_contact=true is excluded from the real production predicate.
   - Testing must preserve the same business logic and isolate the environment/transport deliberately.

3. DNC does not bypass transition validation.
   - Do Not Contact may result in LOST + suppression only through an allowed transition.
   - Invalid state/outcome combinations remain ILLEGAL TRANSITION.

4. DEFERRED expiry does not require a fabricated next customer timestamp.
   - At deferred_until, the pause ends.
   - Agent action becomes due.
   - Transition to FOLLOWUP_OPEN.
   - stage_entered_date should reflect the deferment boundary.
   - clear deferred_until.
   - create one idempotent internal agent nudge for that expiry condition.

5. Claim TTL is an engineering parameter.
   - Do not hardcode 15 minutes as product behavior.
   - Select after observing runtime duration and overlap risk.

6. Outcome-intake deduplication requires a real unique event/submission ID.
   - Do not use contact_id + outcome + rounded minute as a fallback identity.

7. Block 1.2 split:
   - Remove thank-you
   - remove review request
   - remove referral CTA
   - remove referral token/lineage
   - remove referral form
   - remove referral closeout
   from Block 1.1.

## Follow-Up Agent Identity

Formal internal component name:

RetentionOS Follow-Up Agent

The HubSpot owner remains the human sales agent.

The Follow-Up Agent:
- watches continuity state
- detects when an agent action is due
- sends an internal notification at the intended human-specified time
- directs the human agent to record the call outcome
- validates the outcome contract
- derives the next state
- repeats until BOUND or LOST

It is not a customer-service agent and is not the lead owner.

## Internal Email Contract

For a due FOLLOWUP_OPEN condition:

1. Scheduled Continuity Sweep detects due condition.
2. Acquire unique claim:
   contact_id|FOLLOWUP|next_follow_up_at
3. Only the successful claimant may send the internal email.
4. Email presents as:
   RetentionOS Follow-Up Agent
5. Email should include:
   - contact
   - callback type / reason
   - continuity state
   - relevant last interaction context
   - intended follow-up time
   - expected next action
   - LOG CALL OUTCOME link/button
   - optional HubSpot contact link
6. Confirm the claim after successful notification.
7. Repeated sweep of the same due condition must not produce a duplicate email.
8. A new explicit next_follow_up_at creates a new legitimate due event.

## Completion Doctrine

Authoritative rule:

A reminder being sent does not mean the follow-up is complete.

A follow-up cycle is complete only when:
- the authorized human outcome is recorded
or
- the workflow remains visibly awaiting that outcome.

The machine must never treat notification delivery as sales-work completion.

## Outcome Verification Responsibility

Human:
- reports what happened.

Machine:
- validates canonical outcome
- validates legal current-state transition
- validates subordinate facts
- validates required next timestamp
- validates suppression rules
- writes state only after validation

The machine does not infer BOUND, LOST, Declined, or DNC from silence or elapsed time.

## Outcome Input Surface

Still open as an implementation decision.

Preferred requirements:
- atomic event
- unique event/submission ID
- contact_id
- agent identity
- canonical callback_outcome
- subordinate facts
- explicit next_follow_up_at when required
- deferred_until when required
- event timestamp

A dedicated Jotform agent-outcome form is currently a strong candidate.
A HubSpot-native controlled action remains acceptable only if it can provide equally atomic deterministic input without exposing callback_state to direct editing.

## Current Product Gap

DNC + later voluntary re-entry:
- suppression-clear behavior is not yet accepted.
- current fail-safe direction: suppression remains true until explicit authorized human clear.

Do not invent an auto-clear rule.

## Your Updated Assignment

Review the revised authoritative Block 1.1 spec and return only implementation deltas created by the new Follow-Up Agent definition and Block 1.2 split.

Return exactly:

1. TOPOLOGY DELTAS
   - what changes from your previous proposed Scenario A / Scenario B design

2. INTERNAL EMAIL DELIVERY DESIGN
   - safest Make / Outlook topology for one notification per due condition
   - where claim happens
   - where send happens
   - where confirmation happens
   - how send failure is represented

3. OUTCOME-CAPTURE LINK DESIGN
   - how to carry stable contact/event identity from email to outcome intake
   - no weak rounded-minute dedupe

4. OUTCOME-PENDING MODEL
   - how to represent "notification sent, outcome not yet recorded"
   - without inventing a new workflow state unless operationally necessary

5. DEFERRED EXPIRY IMPLEMENTATION
   - exact modules/filters/write order
   - one idempotent nudge
   - no invented customer timestamp

6. BLOCK 1.2 REMOVAL CHECK
   - identify any remaining referral/closeout coupling that should be removed from Block 1.1

7. UPDATED MINIMUM BUILD ORDER
   - smallest safe construction order from current state

8. DECISION
   - PROPOSAL COMPATIBLE
   - PROPOSAL CONFLICT
   - PRODUCT DECISION REQUIRED
   - PLATFORM CONSTRAINT

Do not redesign the state model.
Do not introduce callback cadences.
Do not modify production Block 1.
Do not add Block 1.2 behavior.
Do not promote anything.

## Immediate Build Boundary

The sandbox agent's first directive remains:

- new inactive Block 1.1 Continuity Sweep shell
- verify control-gate path
- add read-only Option C bootstrap path only after control path is understood
- no contact writes
- no internal emails
- no test submissions
- stop for review
