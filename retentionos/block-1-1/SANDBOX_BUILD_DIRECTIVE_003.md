# ChatGPT Sandbox Agent — Block 1.1 Build Directive 003

Version:
SANDBOX_BUILD_DIRECTIVE_003

Classification:
TYPE B — IMPLEMENTATION
TYPE C — VERIFICATION

## INPUT

Authoritative control asset:

- Make Data Store ID: 150513
- Exact name: RETENTIONOS_CONTROL
- Data structure ID: 497817
- strict: true
- Exact fields:
  - gate_state
  - policy_version
  - updated_at
  - updated_by
- Control record key: control
- Current gate_state: ENGAGED
- Current policy_version: BLOCK1_1_V1
- No BRIDGE dependency

Accepted inactive Continuity Sweep shell:

- Scenario ID: 6278154
- Exact name: BLOCK 1.1 — FOLLOW-UP AGENT — CONTINUITY SWEEP — SANDBOX
- Current structure: one util:BasicTrigger
- Schedule: every 3600 seconds
- Status: inactive

Protected scenarios:
- 6221572 — production Block 1 — DO NOT MODIFY
- 6254611 — engineering sandbox — DO NOT MODIFY OR ACTIVATE

## AUTHORITY

Founder / ADMIN has authorized exactly one bounded Make change:

Wire the read-only RetentionOS control-gate path into inactive scenario 6278154.

No bootstrap module or side-effecting branch is authorized in this slice.

## TASK

Fresh-read scenario 6278154.

Then add only the minimum control-gate topology required to prove:

1. A read-only `datastore:GetRecord` lookup targets:
   - Data Store ID: 150513
   - Record key: control

2. Only an exact, case-sensitive deterministic match:
   - gate_state = CLEAR

   may reach the downstream placeholder path.

3. The downstream CLEAR path must remain inert in this slice.

4. Any other condition must produce zero side effects:
   - ENGAGED
   - missing record
   - unreadable record
   - malformed value
   - typo
   - unknown value

5. Scenario 6278154 must remain inactive.

## BUILD DISCIPLINE

Before writing:

- fresh-read 6278154
- confirm exact scenario ID/name
- confirm inactive
- capture current lastEdit
- confirm BasicTrigger and hourly schedule
- confirm no handlers/connections
- inspect exact module spec for datastore:GetRecord
- use datastore ID 150513 only
- use record key control only
- define the exact intended delta
- use expectedLastEdit if supported

Do not guess module names or parameters.

## STRICT PROHIBITIONS

Do not:

- modify 6221572
- modify 6254611
- activate 6254611
- activate 6278154
- modify RETENTIONOS_CONTROL schema
- change strict:true
- change control record values
- change ENGAGED to CLEAR
- reference BRIDGE_CONTROL
- reference any BRIDGE datastore
- add HubSpot modules
- create HubSpot properties
- write HubSpot contacts
- add bootstrap initialization
- add claims
- send Outlook/email
- call Anthropic
- create Jotform submissions
- build Scenario A
- implement FOLLOWUP_OPEN
- implement DEFERRED
- implement DNC
- implement re-entry
- add Block 1.2 behavior
- use retry:true
- use literal and(
- use literal or(
- perform unrelated cleanup

## GATE CONTRACT

The gate comparison must be exact and deterministic.

Accepted pass condition:

`gate_state == "CLEAR"`

The current live record is ENGAGED, therefore a safe verification of the current configuration should not reach the downstream placeholder path.

Do not change the live control record to CLEAR in this directive.

Missing/unreadable control must not fall through as CLEAR.

If the Make module/filter semantics cannot guarantee fail-closed behavior for missing/unreadable records without an additional explicit structure, stop and report:

CONSTRAINT / GAP DISCOVERED — PRODUCT DECISION REQUIRED

Do not improvise a side-effecting fallback.

## ACCEPTANCE

The change is accepted only if all are true:

- 6278154 remains inactive
- BasicTrigger unchanged
- hourly schedule unchanged
- exactly one read-only control lookup targets Data Store 150513 / key control
- only exact CLEAR can reach the inert downstream placeholder path
- ENGAGED reaches no downstream side-effect path
- missing/unreadable cannot fall through to CLEAR
- no BRIDGE references
- no retry:true
- no literal and(
- no literal or(
- no HubSpot writes
- no email
- no Anthropic
- no Jotform
- no claims
- no initialization
- fresh post-write read proves intended topology only

A successful Make save or run alone is not acceptance.

## REQUIRED RETURN FORMAT

### CURRENT OBSERVED STATE

Include:
- 6278154 status
- before lastEdit
- BasicTrigger
- schedule
- connections
- handlers

### CONTROL LOOKUP

Include:
- module ID
- exact module type
- datastore ID
- record key
- connection requirements if any

### CLEAR GATE

Include:
- exact comparison/filter structure
- confirmation that matching is exact and case-sensitive
- exact downstream placeholder topology
- confirmation placeholder is inert

### FAIL-CLOSED VERIFICATION

Include:
- ENGAGED behavior
- missing record behavior
- unreadable/error behavior
- malformed/unknown value behavior
- whether any case can fall through to CLEAR

### SCENARIO 6278154 — POST-WRITE

Include:
- after lastEdit
- inactive status
- exact topology
- exact schedule
- connections
- handlers

### UNCHANGED ASSETS

Confirm:
- 6221572 untouched
- 6254611 untouched/inactive
- RETENTIONOS_CONTROL schema unchanged and strict:true
- control record remains ENGAGED / BLOCK1_1_V1
- no BRIDGE dependency
- no HubSpot writes
- no email
- no Anthropic
- no Jotform
- no claims
- no initialization

### PRE-FLIGHT

Report:
- retry:true
- literal and(
- literal or(
- trigger drift
- schedule drift
- handler drift
- connection drift
- structural issues

### NEXT SAFE WRITE

Return exactly one next action only:

HubSpot schema read-first verification for the minimum required Block 1.1 properties.

Do not perform that next action.

## NEXT GATE

STOP after returning evidence.

Do not continue into HubSpot schema work in this directive.
