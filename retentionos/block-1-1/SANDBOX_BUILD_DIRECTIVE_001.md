# ChatGPT Sandbox Agent — Block 1.1 Build Directive 001

Classification:
TYPE B — IMPLEMENTATION
TYPE C — VERIFICATION

Objective:
Create the first inactive Block 1.1 Continuity Sweep shell and verify the control-gate / bootstrap-read construction path without writing to HubSpot contacts, sending emails, or creating test submissions.

Authoritative source:
retentionos/block-1-1/IMPLEMENTATION_SPEC_V1.md

## Scope

Build only:
- a new Make scenario for Block 1.1 Continuity Sweep
- inactive status
- scheduled/time-driven shell
- RetentionOS control-gate path if an isolated RETENTIONOS_CONTROL asset is already verified available
- otherwise stop and report that the control asset must be created through the supported Make UI/path
- read-only HubSpot bootstrap search path after the gate design is proven

Do not build yet:
- contact state writes
- Outcome/State Engine
- FOLLOWUP_OPEN nudge sending
- Outlook email notifications
- claims datastore writes
- DEFERRED transition
- DNC
- re-entry
- test contacts
- Jotform submissions
- Block 1.2 referral logic

## Mandatory Pre-Change Read

Before creating anything:
1. read Make environment
2. confirm team 883164 or report current actual team
3. confirm no existing Block 1.1 Continuity Sweep already exists
4. read production 6221572 only for reference; do not modify
5. read 6254611 and confirm inactive; do not activate
6. confirm current connections needed for HubSpot read operations
7. report whether RETENTIONOS_CONTROL can be verified from available tools

## New Scenario Naming

Recommended:
BLOCK 1.1 — FOLLOW-UP AGENT — CONTINUITY SWEEP — SANDBOX

If a naming conflict exists:
stop and report it rather than silently selecting a different asset.

## Required Safety

- scenario remains inactive
- no customer-facing modules
- no production contact writes
- no email sends
- no test submissions
- no production trigger consumption
- no retry:true
- no literal and(
- no literal or(
- no BRIDGE_CONTROL connection

## Bootstrap Read Contract

Do not execute a production write.

When the HubSpot property callback_state exists, the production predicate is:

callback_state NOT_HAS_PROPERTY
AND
is_test_contact HAS_PROPERTY
AND
is_test_contact NEQ true
AND
notes_last_contacted HAS_PROPERTY

Until callback_state exists:
do not fake the final predicate.
Report the dependency explicitly.

If a read-only preflight query using the existing three evidence filters is useful, it may be used only as verification evidence and must be labeled as pre-schema verification, not the final bootstrap query.

## Expected Return

Return:

CURRENT OBSERVED STATE

NEW SCENARIO
- scenario ID
- name
- status
- lastEdit
- trigger kind
- schedule
- connections
- modules/topology

CONTROL GATE STATUS
- verified reusable asset
or
- unavailable/unverified and exact manual dependency

BOOTSTRAP READ STATUS
- final predicate available / blocked by missing callback_state
- any read-only evidence counts observed

UNCHANGED ASSETS
- confirm 6221572 untouched
- confirm 6254611 untouched/inactive

PRE-FLIGHT
- retry:true findings
- and(/or( findings
- other structural issues

NEXT SAFE WRITE
- exact smallest next action only

Stop after reporting.
Do not continue into state-writing behavior without new authorization.
