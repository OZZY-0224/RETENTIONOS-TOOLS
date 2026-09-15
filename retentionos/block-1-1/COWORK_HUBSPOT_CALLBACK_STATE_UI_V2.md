# CLAUDE COWORK — HUBSPOT CALLBACK_STATE UI SETUP V2

Version:
COWORK_HUBSPOT_CALLBACK_STATE_UI_V2

Mode:
BOUNDED LIVE-SYSTEM IMPLEMENTATION + VERIFICATION

## INPUT

HubSpot account:
242512230

Accepted evidence:
- callback_state is absent
- no exact or near-match conflict exists
- current connector property surface is read-only
- a Make API-call route exists but would require an additional Make scenario write
- ADMIN selects the direct HubSpot UI path instead for this slice

Existing related property:
- callback_type

Protected systems:
- Make 6221572 — do not modify
- Make 6254611 — do not modify or activate
- Make 6278154 — do not modify or activate
- RETENTIONOS_CONTROL 150513 — do not modify
- no BRIDGE assets
- no contact writes
- no initialization

## AUTHORITY

Founder / ADMIN authorizes exactly one HubSpot CONTACT property-definition write through the HubSpot UI:

Label:
Callback State

Internal name:
callback_state

Type:
enumeration

Field type:
single-select dropdown/select

Options exactly:
- Follow-Up Open -> followup_open
- Awaiting Customer -> awaiting_customer
- Deferred -> deferred
- Bound -> bound
- Lost -> lost

No other property creation is authorized.

## TASK

1. Open HubSpot CONTACT property settings for account 242512230.
2. Fresh-search for callback_state.
3. If it exists unexpectedly:
   - inspect only
   - do not overwrite
   - return PROPOSAL CONFLICT if schema differs
4. Inspect callback_type and capture its property group / groupName.
5. Create callback_state in the SAME property group as callback_type.
6. Use a single-select dropdown/select field behavior.
7. Create exactly the five approved options, with unique display order.
8. Read back the property after creation and verify:
   - internal name
   - label
   - group
   - type
   - field type
   - complete option list
9. Add Callback State to the normal human-visible contact record card/view where callback_type is surfaced, if HubSpot treats card placement separately from property creation.
10. Open a contact record only to verify the field is visibly present on the intended card/view.
11. Do NOT change any contact value.

## PROHIBITIONS

Do not:
- create any other property
- modify callback_type
- modify call_disposition
- write any contact field value
- run initialization
- run migration
- modify Make scenarios
- create a throwaway Make scenario
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
- property ID if exposed
- internal name
- label
- property group / groupName
- type
- field type
- full option list
- creation timestamp if exposed

### CARD PLACEMENT
- whether property creation automatically surfaced it
- exact card/view where it is now visible
- whether any contact value was changed

### VERIFICATION
Confirm:
- same property group as callback_type
- internal name exactly callback_state
- single-select dropdown/select
- exactly five options
- no extra options
- no other property created or modified
- no contact value changed

### UNCHANGED ASSETS
Confirm:
- 6221572 untouched
- 6254611 untouched/inactive
- 6278154 untouched/inactive
- RETENTIONOS_CONTROL unchanged
- no Make scenario created
- no BRIDGE
- no email
- no Anthropic
- no Jotform
- no claims
- no initialization

### GOTCHA
Record:
- connected HubSpot property-definition tools are read-only
- property creation must use HubSpot UI or another explicitly authorized write path
- property creation and contact-card placement are separate concerns and both require verification

### NEXT GATE
Return only:

Hand callback_state creation/readback/card-placement evidence back to ADMIN for bootstrap constructibility verification.

## STOP RULE

Stop after callback_state creation, readback, and card-placement verification.
