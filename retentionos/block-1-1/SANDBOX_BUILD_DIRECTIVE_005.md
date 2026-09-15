# ChatGPT Sandbox Agent — Block 1.1 Build Directive 005

Version:
SANDBOX_BUILD_DIRECTIVE_005

Classification:
TYPE B — IMPLEMENTATION
TYPE C — VERIFICATION

## INPUT

Repository:
OZZY-0224/RETENTIONOS-TOOLS

Branch:
block-1-1-v1-spec

Read first:
- retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md
- retentionos/block-1-1/FINALIZATION_PLAN_001.md
- retentionos/block-1-1/HANDOFF_PACKET_V1.md
- retentionos/block-1-1/SANDBOX_BUILD_DIRECTIVE_004.md

Accepted runtime facts:
- HubSpot account: 242512230
- CONTACT read: AVAILABLE
- CONTACT write: REQUIRES_REAUTHORIZATION
- Property-definition read: AVAILABLE
- Property-definition creation: not exposed by current connected HubSpot surface
- Required V1 properties do not currently exist
- callback_state is absent
- final production bootstrap predicate is not constructible until callback_state exists
- held fields review_required and last_follow_up_at remain HOLD
- follow_up_owner should not be created; reuse hubspot_owner_id
- Make scenario 6278154 remains inactive and unchanged
- RETENTIONOS_CONTROL remains authoritative:
  - Data Store 150513
  - Data structure 497817
  - strict:true
  - record key control
  - gate_state ENGAGED
  - policy_version BLOCK1_1_V1
- control-gate wiring remains blocked on Make UI / alternate supported Make surface

## AUTHORITY

Founder / ADMIN authorizes one smallest schema write only:

Create exactly one HubSpot CONTACT property:

callback_state

No other property creation is authorized in this directive.

## REQUIRED PROPERTY CONTRACT

Object:
CONTACT

Label:
Callback State

Internal name:
callback_state

Type:
enumeration

Field behavior:
single-select enumeration

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

Do not add any other options.

Do not reuse hs_lead_status.

Do not modify call_disposition.

## TASK

1. Fresh-read the exact HubSpot property namespace for CONTACT.
2. Confirm callback_state is still absent immediately before creation.
3. If an exact-name property has appeared since the prior read:
   - do not overwrite it
   - inspect and report
   - stop
4. Create exactly callback_state with the approved five-value enum contract.
5. Read the property back after creation.
6. Verify:
   - exact internal name
   - exact type
   - exact option set
   - no extra values
   - no hidden replacement/reuse of another property

## STRICT PROHIBITIONS

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
- write HubSpot contacts
- run bootstrap initialization
- run the full migration write
- modify scenario 6278154
- modify 6221572
- modify 6254611
- activate any scenario
- change RETENTIONOS_CONTROL
- reference BRIDGE
- send email
- call Anthropic
- submit Jotform
- create claims
- resolve product decisions
- perform unrelated cleanup

## ACCEPTANCE

Accept only if:
- callback_state exists after the write
- internal name exactly callback_state
- type is enumeration
- exactly five values exist:
  - followup_open
  - awaiting_customer
  - deferred
  - bound
  - lost
- no extra values
- no other HubSpot property changed or created
- no contact writes occurred
- protected Make assets unchanged
- RETENTIONOS_CONTROL unchanged

## REQUIRED RETURN FORMAT

### PRE-CREATION READ

### CALLBACK_STATE PROPERTY

Return:
- property ID if exposed
- internal name
- label
- type
- field type
- full option list
- creation timestamp if exposed

### POST-CREATION VERIFICATION

### UNCHANGED ASSETS

### BOOTSTRAP CONSTRUCTIBILITY UPDATE

Return:
- callback_state exists: yes/no
- full production bootstrap predicate constructible: yes/no
- exact blocker if still no

### NEXT SAFE WRITE

Return exactly one smallest next action only.

Do not perform it.

## STOP RULE

Stop after callback_state creation and readback verification.
