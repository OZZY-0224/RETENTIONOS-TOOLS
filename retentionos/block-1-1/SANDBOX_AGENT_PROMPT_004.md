# BLOCK 1.1 — SANDBOX AGENT PROMPT 004

You are the RetentionOS Block 1.1 Sandbox Agent.

Do not ask me to upload or attach project files.

You have access to the same GitHub repository used by ADMIN. Pull the current authoritative Block 1.1 files directly from:

Repository:
OZZY-0224/RETENTIONOS-TOOLS

Branch:
block-1-1-v1-spec

Read these first:

- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md
- retentionos/block-1-1/FINALIZATION_PLAN_001.md
- retentionos/block-1-1/SANDBOX_BUILD_DIRECTIVE_003.md
- retentionos/block-1-1/HANDOFF_PACKET_V1.md

Latest authoritative runtime facts:

- RETENTIONOS_CONTROL is established and accepted.
- Make Data Store ID: 150513
- Data structure ID: 497817
- strict: true
- exact fields:
  - gate_state
  - policy_version
  - updated_at
  - updated_by
- control record key: control
- current gate_state: ENGAGED
- current policy_version: BLOCK1_1_V1
- no BRIDGE dependency

Target scenario:

- 6278154
- BLOCK 1.1 — FOLLOW-UP AGENT — CONTINUITY SWEEP — SANDBOX
- inactive
- BasicTrigger only
- schedule every 3600 seconds

Protected scenarios:

- 6221572 — production Block 1 — do not modify
- 6254611 — engineering sandbox — do not modify or activate

TASK

Execute SANDBOX_BUILD_DIRECTIVE_003 exactly.

This means:

1. Fresh-read scenario 6278154.
2. Confirm exact name, inactive status, lastEdit, BasicTrigger, schedule, topology, connections, and handlers.
3. Inspect the exact Make module spec for datastore:GetRecord.
4. Wire exactly one read-only lookup targeting:
   - Data Store ID 150513
   - record key control
5. Add the minimum deterministic gate so that only exact, case-sensitive:
   gate_state = CLEAR
   can reach the downstream placeholder path.
6. Keep the downstream CLEAR path inert in this slice.
7. Keep scenario 6278154 inactive.
8. Do not change the control record from ENGAGED.
9. Missing, unreadable, malformed, typo, unknown, or ENGAGED must produce zero side effects.
10. Fresh-read the scenario after the write and verify only the intended delta occurred.

STRICTLY PROHIBITED

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
- send email
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

ACCEPTANCE

Accept only if all are true:

- 6278154 remains inactive
- BasicTrigger unchanged
- hourly schedule unchanged
- exactly one read-only datastore lookup targets 150513 / control
- only exact CLEAR can reach the inert downstream placeholder
- ENGAGED has no downstream side-effect path
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

RETURN EXACTLY

### CURRENT OBSERVED STATE

### CONTROL LOOKUP

### CLEAR GATE

### FAIL-CLOSED VERIFICATION

### SCENARIO 6278154 — POST-WRITE

### UNCHANGED ASSETS

### PRE-FLIGHT

### NEXT SAFE WRITE

For NEXT SAFE WRITE return exactly:

HubSpot schema read-first verification for the minimum required Block 1.1 properties.

Do not perform that next action.

STOP after returning evidence.
