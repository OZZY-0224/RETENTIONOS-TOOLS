# ChatGPT Sandbox Agent — Block 1.1 Build Directive 004

Version:
SANDBOX_BUILD_DIRECTIVE_004

Classification:
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
- retentionos/block-1-1/SANDBOX_BUILD_DIRECTIVE_003.md

Accepted runtime facts:
- Scenario 6278154 exists and remains inactive.
- RETENTIONOS_CONTROL is authoritative:
  - Data Store 150513
  - Data structure 497817
  - strict:true
  - record key control
  - gate_state ENGAGED
  - policy_version BLOCK1_1_V1
- Make ChatGPT App scenario editing cannot currently bind datastore:GetRecord to the verified Data Store ID.
- SANDBOX_BUILD_DIRECTIVE_003 therefore closed as:
  PLATFORM / ACCESS CONSTRAINT
- No control gate was written.
- No scenario drift occurred.
- Step 2 implementation remains blocked on Make UI / alternate supported Make surface.
- This directive authorizes only read-first HubSpot schema verification.

HubSpot account:
242512230

Known existing relevant CONTACT properties:
- callback_type
- lifecyclestage
- notes_last_contacted
- vcard_sent
- hubspot_owner_id
- is_test_contact
- call_notes

Previously absent Block 1.1 properties:
- callback_state
- stage_entered_date
- next_follow_up_at
- callback_outcome
- deferred_until
- retention_contact_suppressed
- retention_contact_suppression_reason

Held unless implementation evidence requires them:
- review_required
- last_follow_up_at
- follow_up_owner

## AUTHORITY

Founder / ADMIN authorizes a read-only HubSpot schema inspection only.

No property creation.
No contact writes.
No Make changes.
No Jotform.
No email.
No Anthropic.
No claims.

## TASK

For each proposed Block 1.1 CONTACT property below, verify:

1. whether it exists
2. exact internal name
3. exact type
4. exact field type / enumeration behavior if applicable
5. current options/values if it exists
6. current usage/conflict risk
7. whether an existing reusable property already satisfies the same contract
8. whether creation is required

Inspect exactly these proposed V1 properties:

### callback_state
Desired contract:
enumeration
- followup_open
- awaiting_customer
- deferred
- bound
- lost

### stage_entered_date
Desired contract:
datetime

### next_follow_up_at
Desired contract:
datetime

### callback_outcome
Desired contract:
enumeration
- successful_bind
- rate_changed_follow_up
- still_need_time_credentials_payment
- no_answer_no_contact
- priced_out
- customer_declined
- do_not_contact

### deferred_until
Desired contract:
datetime

### retention_contact_suppressed
Desired contract:
boolean

### retention_contact_suppression_reason
Desired contract:
enumeration
- do_not_contact

Also verify the held fields only for collision/reuse evidence:
- review_required
- last_follow_up_at
- follow_up_owner

Do not create any property in this directive.

## BOOTSTRAP DEPENDENCY CHECK

Explicitly verify whether callback_state exists.

If callback_state is absent:
- final production bootstrap predicate remains not constructible
- do not fake it
- do not create it
- do not run initialization

If callback_state exists unexpectedly:
- report exact schema and current usage
- do not alter it

## PROHIBITIONS

Do not:
- create or modify HubSpot properties
- write HubSpot contacts
- modify Make scenario 6278154
- modify 6221572
- modify 6254611
- activate any scenario
- change RETENTIONOS_CONTROL
- reference BRIDGE
- create Jotform submissions
- send email
- call Anthropic
- create claims
- run initialization
- resolve product decisions
- invent replacement field names

## REQUIRED RETURN FORMAT

### HUBSPOT SCHEMA OBSERVED STATE

Include:
- account
- read/write capability observed
- property-definition capability observed

### REQUIRED V1 PROPERTY MATRIX

For each required property return:
- proposed name
- exists: yes/no
- observed internal name
- observed type
- observed field type/options
- reusable existing property: yes/no
- conflict/current usage
- create required: yes/no

### HELD PROPERTY COLLISION CHECK

For:
- review_required
- last_follow_up_at
- follow_up_owner

Return only:
- exists
- collision/reuse risk
- recommendation: HOLD / REUSE EXISTING / PRODUCT DECISION REQUIRED

### BOOTSTRAP CONSTRUCTIBILITY

Return:
- callback_state exists: yes/no
- full production bootstrap predicate constructible: yes/no
- exact blocker if no

### UNCHANGED ASSETS

Confirm:
- no HubSpot writes
- no property creation
- no contact writes
- 6278154 untouched/inactive
- 6221572 untouched
- 6254611 untouched/inactive
- RETENTIONOS_CONTROL unchanged
- no BRIDGE
- no email
- no Anthropic
- no Jotform
- no claims

### NEXT SAFE WRITE

Return exactly one smallest next action based on evidence.

Do not perform it.

## STOP RULE

Stop after read-first verification.
