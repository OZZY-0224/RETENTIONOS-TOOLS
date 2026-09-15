# CLAUDE COWORK — HUBSPOT CALLBACK_STATE SETUP V1

Version:
COWORK_HUBSPOT_CALLBACK_STATE_SETUP_V1

Mode:
BOUNDED LIVE-SYSTEM IMPLEMENTATION + VERIFICATION

## INPUT

HubSpot account:
242512230

Current accepted Block 1.1 schema evidence:
- callback_state does not exist
- property-definition creation is not exposed through the current connected ChatGPT HubSpot surface
- no reusable existing property satisfies the RetentionOS callback_state contract
- hs_lead_status is not equivalent
- call_disposition is retired/untouched

Protected systems:
- Make 6221572 — production Block 1 — DO NOT MODIFY
- Make 6254611 — engineering sandbox — DO NOT MODIFY OR ACTIVATE
- Make 6278154 — Block 1.1 Continuity Sweep sandbox — inactive — DO NOT MODIFY
- RETENTIONOS_CONTROL — Data Store 150513 — DO NOT MODIFY
- No BRIDGE assets may be used or modified

## AUTHORITY

Founder / ADMIN authorizes exactly one HubSpot property-definition write:

Create one CONTACT property:

Internal name:
callback_state

Label:
Callback State

Type:
Enumeration / single select

Allowed internal values exactly:
- followup_open
- awaiting_customer
- deferred
- bound
- lost

Recommended display labels:
- Follow-Up Open
- Awaiting Customer
- Deferred
- Bound
- Lost

No other property creation is authorized.

## TASK

1. Open HubSpot property management for CONTACT properties in account 242512230.
2. Search for an existing property with internal name exactly:
   callback_state
3. If it exists:
   - inspect it
   - do not overwrite it
   - verify exact schema/options
   - if it differs from the approved contract, return PROPOSAL CONFLICT and stop
4. If it does not exist:
   - create exactly one CONTACT property
   - label: Callback State
   - internal name: callback_state
   - field type: single-select enumeration
   - internal option values exactly:
     - followup_open
     - awaiting_customer
     - deferred
     - bound
     - lost
5. Do not add extra options.
6. Read the property back after creation.
7. Capture property ID/metadata if HubSpot exposes it.

## PROHIBITIONS

Do not:
- create stage_entered_date
- create next_follow_up_at
- create callback_outcome
- create deferred_until
- create retention_contact_suppressed
- create retention_contact_suppression_reason
- create review_required
- create last_follow_up_at
- create follow_up_owner
- modify existing HubSpot properties
- write any HubSpot contact
- run bootstrap initialization
- run migration
- modify any Make scenario
- activate any scenario
- modify RETENTIONOS_CONTROL
- use BRIDGE
- send email
- call Anthropic
- submit Jotform
- create claims
- perform unrelated cleanup

## OUTPUT

Return exactly:

### RESULT

One of:
- CREATED AND VERIFIED
- EXISTING ASSET REUSABLE
- PROPOSAL CONFLICT
- PLATFORM / ACCESS CONSTRAINT

### CALLBACK_STATE

- HubSpot object: CONTACT
- property ID if exposed
- internal name
- label
- type
- field type
- full option list with display label + internal value
- creation timestamp if exposed

### VERIFICATION

Confirm:
- internal name exactly callback_state
- single-select enumeration
- exactly five options
- no extra values
- no other property created or modified
- no contact writes

### UNCHANGED ASSETS

Confirm:
- 6221572 untouched
- 6254611 untouched/inactive
- 6278154 untouched/inactive
- RETENTIONOS_CONTROL unchanged
- no BRIDGE
- no email
- no Anthropic
- no Jotform
- no claims
- no initialization

### NEXT GATE

Return only:

Hand callback_state creation/readback evidence back to ADMIN for bootstrap constructibility verification and the next bounded schema slice.

## STOP RULE

Stop after callback_state creation and readback verification.
