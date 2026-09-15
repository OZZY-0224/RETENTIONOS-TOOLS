# CLAUDE COWORK — RETENTIONOS_CONTROL SETUP PROMPT 001

Version:
COWORK_RETENTIONOS_CONTROL_SETUP_V1

Mode:
BOUNDED LIVE-SYSTEM IMPLEMENTATION + VERIFICATION

## INPUT

Current RetentionOS Block 1.1 state:

- Make organization: 3954046 — My Organization
- Make team: 883164 — My Team
- Make zone: us2.make.com
- Accepted sandbox scenario:
  - 6278154
  - BLOCK 1.1 — FOLLOW-UP AGENT — CONTINUITY SWEEP — SANDBOX
  - inactive
  - one BasicTrigger only
  - no handlers
  - no connections
  - no side effects

Protected scenarios:
- 6221572 — production Block 1 — active — DO NOT MODIFY
- 6254611 — engineering sandbox — inactive — DO NOT MODIFY OR ACTIVATE

The Make ChatGPT App surface can create/edit scenarios but cannot create or enumerate Make Data Stores.

This task exists only to establish the isolated control Data Store needed for Block 1.1.

## AUTHORITY

Founder / ADMIN has authorized creation or positive verification of exactly one Make Data Store:

RETENTIONOS_CONTROL

Accepted control design:

Data Store name:
RETENTIONOS_CONTROL

Data structure fields:
- gate_state — Text
- policy_version — Text
- updated_at — Date/DateTime
- updated_by — Text

Control record key:
control

Initial values:
- gate_state = ENGAGED
- policy_version = BLOCK1_1_V1
- updated_at = current timestamp
- updated_by = founder/admin

Fail-closed rule:
- only exact CLEAR may permit future side effects
- ENGAGED, missing, malformed, unreadable, typo, or unknown must produce zero side effects

Initial state must be ENGAGED.

## TASK

1. Open the Make Data Store management UI for team 883164.
2. Search for an existing Data Store named exactly:
   RETENTIONOS_CONTROL

3. If it already exists:
   - inspect it
   - verify its exact schema
   - verify whether it is isolated to RetentionOS
   - verify current record/key convention
   - do not alter it unless it exactly matches the accepted design and only the initial control record is missing

4. If it does not exist:
   - create exactly one Data Store named:
     RETENTIONOS_CONTROL
   - create the exact approved data structure only
   - create the single control record with key:
     control
   - initialize:
     gate_state = ENGAGED
     policy_version = BLOCK1_1_V1
     updated_at = current timestamp
     updated_by = founder/admin

5. Capture the Data Store ID after creation/verification.

6. Read back the final schema and control record.

## PROHIBITIONS

Do not:
- modify scenario 6221572
- modify scenario 6254611
- modify scenario 6278154
- activate any scenario
- wire the Data Store into any scenario yet
- create any other Data Store
- use or modify BRIDGE_CONTROL
- use or modify any BRIDGE datastore
- create HubSpot properties
- write HubSpot contacts
- send emails
- call Anthropic
- submit Jotform
- create claims
- create test contacts
- redesign the control schema
- add extra fields
- change ENGAGED to CLEAR
- perform unrelated Make cleanup

If an existing RETENTIONOS_CONTROL conflicts with this approved design, do not overwrite or repair it.

Return:
PROPOSAL CONFLICT

If Make prevents creation or inspection, return:
PLATFORM / ACCESS CONSTRAINT

## OUTPUT

Return exactly these sections:

### RESULT

One of:
- EXISTING ASSET REUSABLE
- CREATED AND VERIFIED
- PROPOSAL CONFLICT
- PLATFORM / ACCESS CONSTRAINT

### RETENTIONOS_CONTROL

- Data Store ID
- exact name
- team/workspace
- exact schema
- record key convention
- current control record values

### VERIFICATION

Confirm:
- gate_state = ENGAGED
- policy_version = BLOCK1_1_V1
- updated_at populated
- updated_by populated
- no extra fields
- no BRIDGE dependency

### UNCHANGED ASSETS

Confirm:
- 6221572 untouched
- 6254611 untouched/inactive
- 6278154 untouched/inactive
- no HubSpot writes
- no email
- no Anthropic
- no Jotform
- no claims
- no other Data Store created

### NEXT GATE

Return only:

Hand Data Store ID/schema back to ADMIN for the next bounded sandbox directive to wire the read-only control gate into inactive scenario 6278154.

## NEXT GATE RULE

Do not continue into scenario wiring.

Stop after the Data Store is created/verified and evidence is returned.
