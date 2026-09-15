# RetentionOS Block 1.1 — Engineering Review Packet v1.1

Source of truth:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md

Status:
- Block 1.1 is now formally the Follow-Up Agent only
- Block 1.2 Referral Agent is a separate lane
- Sonnet review completed and counter-reviewed
- No production Block 1 changes authorized
- No test entries authorized yet

## Accepted Sonnet Construction Findings

Accepted with revisions:
- two-scenario architecture
- control-first execution
- fail-closed transition validation
- state/outcome/flags separation
- explicit human-specified customer follow-up timing
- unique datastore claim for due-condition idempotency
- immediate destination-state readback
- internal agent nudges before autonomous customer outreach
- one fixture at a time

Required corrections already incorporated into the authoritative spec:
1. exact-6 bootstrap count is a first-migration gate only, not permanent runtime logic
2. sandbox tests must not silently change the production eligibility predicate
3. DNC does not bypass the transition table
4. DEFERRED expiry becomes an agent-due event; no invented next customer timestamp
5. claim TTL is an engineering parameter, not a guessed product constant
6. outcome-event dedupe requires a true unique event/submission identifier
7. thank-you/review/referral logic moved entirely to Block 1.2

## GitHub Copilot Assignment

Role: repository/code integrity reviewer for Block 1.1 only.

Review the authoritative Block 1.1 spec.

Focus on:
- HubSpot schema definitions
- state-transition representation
- required-field validation
- due-condition claim-key design
- unique outcome-event identity requirements
- acceptance-fixture structure
- blueprint snapshot/provenance format
- scenario diff-validator design
- Blueprint Preflight V0 integration
- deterministic serialization/hashing
- test isolation controls
- accidental production coupling

Do not:
- redefine product behavior
- add callback cadence defaults
- add referral logic
- move Make business logic into GitHub
- create new CRM fields without an explicit operational purpose

Return:
1. REPO STRUCTURE REVIEW
2. STATE / OUTCOME CONTRACT VALIDATION APPROACH
3. ACCEPTANCE FIXTURE FORMAT
4. SNAPSHOT / HASH / DIFF DESIGN
5. STATIC GUARDRAILS
6. UNIQUE EVENT-ID / IDEMPOTENCY REVIEW
7. TEST ISOLATION RISKS
8. RECOMMENDED FIRST CODED TOOL

## Microsoft Copilot Assignment

Role: Microsoft ecosystem / Outlook-side support.

Review only:
- internal Follow-Up Agent notification delivery through Microsoft 365 / Outlook
- sender/display identity possibilities
- reliable internal email delivery
- link/button handling
- mailbox/folder isolation for sandbox testing
- any transport-side constraints relevant to agent notifications

Do not define state logic.

## Gemini Assignment

Role: Block 1.1 internal communication library only.

Generate candidate wording for:
- Follow-Up Due
- Outcome Still Needed
- Deferred Follow-Up Now Due
- Re-entry Requires Attention
- Review Required

Do not create thank-you/review/referral copy here.
Those belong to Block 1.2.

## Cowork Assignment

After implementation, inspect:
- new Scenario A
- new Scenario B
- exact control-gate position
- bootstrap predicate
- due query
- claim implementation
- internal notification path
- outcome-input routing
- suppression filters
- sandbox isolation
- trigger safety
- no Block 1 modification
- no Block 1.2 logic inside Block 1.1

Preferred outputs:
- PROPOSAL COMPATIBLE
- PROPOSAL CONFLICT
- EXISTING ASSET REUSABLE
- PRODUCT DECISION REQUIRED
- RUNTIME DEFECT

## Review Rule

Proposal -> evidence -> counter-review -> founder decision.

No reviewer is authorized to promote a change.
