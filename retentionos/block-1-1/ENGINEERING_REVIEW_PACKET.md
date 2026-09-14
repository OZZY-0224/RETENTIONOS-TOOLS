# RetentionOS Block 1.1 — Engineering Review Packet

Source of truth:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md

Status:
- Product behavior substantially frozen
- No production Block 1 changes authorized
- No test entries authorized yet
- Review is implementation-focused

## Claude Sonnet Assignment

Role: construction engineer.

Read the implementation spec as authoritative product contract.

Do not:
- redesign the state model
- introduce default callback cadences
- modify production Block 1
- invent new product states/outcomes
- treat BOUND as policy servicing
- merge G17 into Block 1.1

Return exactly these sections:

1. PROPOSED MAKE TOPOLOGY
   - Scenario A: Outcome Intake + State Engine
   - Scenario B: Continuity Sweep
   - module-by-module sequence
   - route/filter logic
   - exact fail-closed points

2. HUBSPOT IMPLEMENTATION MAP
   - property definitions
   - enum internal values
   - write points
   - readback verification points

3. DATASTORE / IDEMPOTENCY MAP
   - RETENTIONOS_CONTROL
   - RETENTIONOS_CONTINUITY_CLAIMS
   - optional exceptions/events stores
   - claim acquisition / confirmation / expiry behavior

4. INPUT SURFACE RECOMMENDATION
   - smallest safe V1 agent outcome-input mechanism
   - preserve rule: agent reports outcome; system derives state

5. RACE CONDITIONS / FAILURE MODES
   - concurrent sweep
   - partial execution
   - HubSpot eventual consistency
   - repeated polling
   - suppression conflicts
   - gate unreadable/missing

6. SANDBOX TEST PLAN
   - one fixture at a time
   - no production-customer impact
   - explicit expected destination state
   - exact isolation requirements

7. IMPLEMENTATION CONFLICTS
   - only report a conflict if the supplied contract cannot be implemented safely as written
   - distinguish platform constraint from product ambiguity

8. MINIMUM BUILD ORDER
   - smallest executable sequence
   - do not batch unrelated behaviors

Decision labels:
- PROPOSAL COMPATIBLE
- PROPOSAL CONFLICT
- EXISTING ASSET REUSABLE
- PRODUCT DECISION REQUIRED

## GitHub Copilot Assignment

Role: repository/code integrity reviewer.

Review the same implementation spec and validate the engineering-control-plane design.

Focus on:
- schema validity
- state-transition representation
- acceptance-fixture structure
- blueprint snapshot/provenance format
- scenario diff validation strategy
- preflight static checks
- referral-lineage schema correctness
- testability
- accidental coupling to production
- deterministic serialization/hashing where used

Do not:
- redefine product behavior
- move Make business logic into code
- invent new CRM fields
- turn repository assets into live customer storage

Return:
1. REPO STRUCTURE REVIEW
2. CONTRACT VALIDATION APPROACH
3. TEST FIXTURE FORMAT
4. SNAPSHOT / HASH / DIFF DESIGN
5. REFERRAL SCHEMA REVIEW
6. STATIC GUARDRAILS
7. RISKS / MISSING ENGINEERING CONTROLS
8. RECOMMENDED FIRST CODED TOOL

## Review Rule

Proposal -> evidence -> counter-review -> founder decision.

No reviewer is authorized to promote a change.
