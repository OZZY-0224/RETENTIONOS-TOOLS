# ChatGPT Sandbox Agent — Block 1.1 Build Directive 002

Classification:
TYPE B — IMPLEMENTATION
TYPE C — VERIFICATION

Objective:
Advance Block 1.1 finalization by establishing the isolated RetentionOS control asset and wiring the read-only control-gate path into the accepted inactive Continuity Sweep shell.

Authoritative source:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md
- retentionos/block-1-1/HANDOFF_PACKET_V1.md
- accepted result of SANDBOX_BUILD_DIRECTIVE_001

Current accepted shell:
- Scenario 6278154
- BLOCK 1.1 — FOLLOW-UP AGENT — CONTINUITY SWEEP — SANDBOX
- inactive
- one BasicTrigger only
- no handlers
- no connections
- no side effects

## Allowed Objective

1. Positively verify or create the isolated Make Data Store:
   RETENTIONOS_CONTROL

2. Capture:
   - datastore ID
   - exact schema
   - exact control record/key convention

3. Minimum accepted V1 fields:
   - gate_state
   - policy_version
   - updated_at
   - updated_by

4. Accepted gate values:
   - CLEAR
   - ENGAGED

5. Wire a read-only control lookup/check into scenario 6278154 only after the datastore identity is positively known.

6. Keep scenario 6278154 inactive.

## Fail-Closed Contract

Only exact CLEAR may permit future side-effecting branches.

For this run, no side-effecting branch is authorized.

ENGAGED, missing, malformed, unreadable, typo, or unknown:
- route to zero side effects
- remain visible in topology/evidence

Do not use BRIDGE_CONTROL or any BRIDGE datastore.

## Strictly Prohibited

Do not:
- modify 6221572
- modify or activate 6254611
- activate 6278154
- create HubSpot contact writes
- create HubSpot properties
- build bootstrap initialization writes
- create claims
- send email
- call Anthropic
- create Jotform submissions
- build Scenario A
- implement FOLLOWUP_OPEN notifications
- implement DEFERRED transitions
- implement DNC
- implement re-entry
- add Block 1.2 behavior
- use retry:true
- use literal and(
- use literal or(
- perform unrelated cleanup

## Pre-Change Read

Before changing 6278154:
- fresh read 6278154
- confirm exact name
- confirm inactive
- capture lastEdit
- confirm one-module shell
- confirm no handlers/connections
- fresh read 6221572 and 6254611 for non-drift evidence
- verify RETENTIONOS_CONTROL identity before wiring

If datastore identity cannot be positively verified/created through the available tool surface:
STOP and report PLATFORM / ACCESS CONSTRAINT.
Do not modify 6278154 merely to simulate the gate.

## Post-Change Verification

If the gate is wired:
- fresh read 6278154
- confirm inactive
- confirm intended gate delta only
- confirm no HubSpot module
- confirm no email module
- confirm no Anthropic module
- confirm no Jotform module
- confirm no claim write
- confirm no handler
- confirm no retry:true
- confirm no literal and(
- confirm no literal or(
- confirm 6221572 unchanged
- confirm 6254611 unchanged/inactive

## Required Return

### CURRENT OBSERVED STATE

### RETENTIONOS_CONTROL
Return:
- datastore ID
- schema
- key/record convention
- initial record state if created
- creation/verification evidence

### SCENARIO 6278154
Return:
- before lastEdit
- after lastEdit
- inactive status
- exact topology
- gate module configuration
- connections
- handlers

### FAIL-CLOSED VERIFICATION
Return evidence for:
- CLEAR path
- ENGAGED path
- missing/malformed/unreadable behavior where safely inspectable without side effects

### UNCHANGED ASSETS
Confirm:
- 6221572 untouched
- 6254611 untouched/inactive
- no HubSpot writes
- no email
- no Anthropic
- no Jotform
- no claims
- no BRIDGE dependency

### PRE-FLIGHT
Report:
- retry:true
- literal and(
- literal or(
- trigger drift
- handler drift
- connection drift
- structural issues

### NEXT SAFE WRITE
Return one smallest next action only.

Stop after reporting.
