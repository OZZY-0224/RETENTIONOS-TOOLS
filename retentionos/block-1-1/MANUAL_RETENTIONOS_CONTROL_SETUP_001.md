# RetentionOS Block 1.1 — Manual Make Data Store Setup 001

Status:
REQUIRED PLATFORM STEP

Purpose:
Create or positively verify the isolated Make Data Store required for the Block 1.1 fail-closed control gate.

Target asset name:
RETENTIONOS_CONTROL

## Required Data Store

Create exactly one isolated RetentionOS Data Store named:

RETENTIONOS_CONTROL

Do not reuse:
- BRIDGE_CONTROL
- BRIDGE_IDEMPOTENCY
- BRIDGE_EXECUTIONS
- BRIDGE_WORK_ITEMS
- BRIDGE_ARTIFACTS
- ROS API Probe
- any unrelated existing datastore

## Recommended V1 Schema

Use a single control-record design.

Fields:
- gate_state — text
- policy_version — text
- updated_at — date/datetime
- updated_by — text

Control record key:
- control

Initial record values:
- gate_state = ENGAGED
- policy_version = BLOCK1_1_V1
- updated_at = current timestamp
- updated_by = founder/admin

Why ENGAGED first:
The gate must fail closed until wiring and acceptance are complete.
Do not initialize the control to CLEAR before the gate is verified.

## Required Evidence To Return

After creation/verification, return:

RETENTIONOS_CONTROL
- datastore ID
- exact asset name
- exact field schema
- control record key
- current record values
- screenshot or direct Make evidence if available

Do not wire scenario 6278154 yet unless a new sandbox directive authorizes it.

## Next Gate

After ADMIN receives the datastore ID/schema, issue the next bounded sandbox directive:

- fresh-read scenario 6278154
- add read-only datastore lookup for key control
- fail closed unless gate_state exactly equals CLEAR
- keep scenario inactive
- no HubSpot writes
- no bootstrap writes
- no claims
- no email
- no Anthropic
- no Jotform
- stop for review
