# RetentionOS ADMIN Block 1.1 — Fixed Prompt Operating System Augmentation

## 71. FIXED PROMPTS ARE PART OF THE OPERATING SYSTEM

RetentionOS has repeatedly benefited from using fixed prompts instead of improvising new instructions every time.

Treat fixed prompts as reusable engineering controls.

They are not casual templates.

They exist to preserve:

- scope
- role clarity
- evidence discipline
- acceptance rules
- build constraints
- tool-specific behavior
- consistent handoff structure

The ADMIN Block 1.1 lane should actively reuse them from end to end.

---

## 72. FIXED PROMPT PRINCIPLE

Use:

fixed prompt → bounded task → structured output → review → acceptance

instead of:

open-ended AI conversation → interpretation drift → accidental redesign

Every major AI/platform handoff should use a known prompt structure.

---

## 73. END-TO-END FIXED PROMPT CHAIN

The preferred sequence is:

### 1. ADMIN / PRODUCT PROMPT

Purpose:
define exactly what is being built and what is already settled.

Must include:

- objective
- accepted behavior
- do-not-touch boundaries
- current architecture
- unresolved decisions
- expected output
- no-write rule if task is read-only

This is the source prompt for the task.

### 2. SONNET CONSTRUCTION PROMPT

Use when the product behavior is already decided.

Sonnet should receive a bounded instruction such as:

CONSTRUCT — BLOCK 1.1 IMPLEMENTATION TASK

Given the approved RetentionOS product contract below, determine the smallest safe technical implementation.

Do not redesign product behavior.
Do not reopen Block 1.
Do not invent defaults.
Do not add fields unless required.

Return:

- exact schema
- exact Make architecture
- transition logic
- filters
- formulas/mappings
- tool/code artifacts where useful
- implementation sequence
- acceptance cases

Any genuine ambiguity must be labeled:
CONSTRAINT / GAP DISCOVERED — PRODUCT DECISION REQUIRED

Sonnet should not be given a vague prompt like “what would you do?”

---

## 74. COWORK INSPECTION PROMPT

Use Cowork for concrete live-system inspection.

Preferred structure:

READ-ONLY INSPECTION — DO NOT MODIFY ANYTHING

Inspect the current live/sandbox systems against the supplied proposal.

Verify only:

- exact asset IDs
- current scenario state
- property existence
- module compatibility
- trigger configuration
- mappings
- sandbox isolation
- implementation conflicts

Return findings only under:

- PROPOSAL COMPATIBLE
- PROPOSAL CONFLICT
- EXISTING ASSET REUSABLE
- PRODUCT DECISION REQUIRED

Do not redesign RetentionOS.
Do not recommend unrelated improvements.
Do not modify anything.

This prevents Cowork from turning inspection into architecture.

---

## 75. CHATGPT COUNTER-REVIEW PROMPT

When importing Sonnet/Cowork/Gemini/Copilot output back into ADMIN Block 1.1, evaluate it with this fixed question set:

1. Does it preserve accepted RetentionOS behavior?
2. Did it invent a product decision?
3. Is there a simpler construction?
4. Does it duplicate existing logic?
5. Does it weaken fail-closed behavior?
6. Does it create unnecessary fields/states/scenarios?
7. Does it conflict with current live evidence?
8. Is it testable?
9. Is it reversible?
10. What exact founder decision, if any, remains?

Do not accept another model’s output merely because it is detailed.

---

## 76. GEMINI ARCHITECTURE CHALLENGE PROMPT

Use Gemini only when a second independent architecture perspective is valuable.

Prompt pattern:

ARCHITECTURE CHALLENGE — DO NOT REDESIGN THE PRODUCT

Review the supplied RetentionOS Block 1.1 architecture.

Assume the product behavior and boundaries are fixed.

Challenge only:

- implementation complexity
- failure modes
- state-model consistency
- platform fit
- scalability
- maintainability

Do not introduce new product features.

Return:

- strongest parts
- implementation risks
- simpler alternatives
- hidden assumptions
- only genuine product gaps

Gemini is a challenger, not the source of truth.

---

## 77. COPILOT IMPLEMENTATION PROMPT

Use Copilot for code/API/repository work.

Prompt pattern:

IMPLEMENTATION SUPPORT — PRODUCT LOGIC IS FIXED

Build/review the requested code or API artifact against the supplied RetentionOS contract.

Do not alter business behavior.

Focus on:

- correctness
- API/schema validity
- deterministic behavior
- readable implementation
- testability
- failure handling

Return:

- implementation
- assumptions
- test cases
- known limitations

Use this for:

- GitHub tooling
- API payloads
- repository automation
- scripts
- diff/provenance utilities

---

## 78. MAKE BUILD PROMPT

Before any Make change, use a fixed construction prompt.

Structure:

MAKE CHANGE — BOUNDED IMPLEMENTATION

Target scenario:
<scenario ID>

Allowed change:
<exact modules/fields>

Required behavior:
<product contract>

Must remain unchanged:
<modules/topology/handlers/trigger/connections>

Preflight:

- fresh scenario read
- confirm lastEdit
- reject stale base
- reject and(
- reject or(
- reject retry:true

After change:

- fresh read
- confirm target delta
- confirm untouched topology
- confirm handlers
- confirm trigger
- confirm connections

No unrelated cleanup.

This is the operational equivalent of a change ticket.

---

## 79. HUBSPOT SCHEMA PROMPT

Before adding or changing properties:

HUBSPOT SCHEMA CHECK — READ FIRST

Verify whether each proposed property already exists.

For each field return:

- existing internal name
- type
- allowed values
- current usage
- conflict risk
- create/reuse recommendation

Do not create anything yet.

Only after this read should a write prompt be issued.

---

## 80. GITHUB TOOLING PROMPT

Use a fixed prompt for engineering controls:

RETENTIONOS TOOLING TASK

Build a tool that improves:

- provenance
- repeatability
- inspection
- testing
- deployment safety

Do not move certified business logic out of Make unless explicitly authorized.

Tool must:

- fail visibly
- produce deterministic output
- be testable
- avoid modifying production
- document limitations

This preserves GitHub’s proper role.

---

## 81. ACCEPTANCE PROMPT

After a feature is built:

ACCEPTANCE TEST — DO NOT MODIFY DESIGN

Validate the feature only against the approved contract.

For each test return:

- input
- expected state
- expected side effect
- actual result
- pass/fail
- evidence

Runtime success alone is not acceptance.

Verify destination state and human-visible behavior.

This keeps QA separate from construction.

---

## 82. PROMOTION PROMPT

Before moving sandbox behavior into production:

PROMOTION REVIEW

Confirm:

- accepted behavior passed
- no unresolved product gaps
- sandbox state verified
- production target freshly read
- exact intended delta known
- preflight passed
- no trigger drift
- no handler drift
- no connection drift
- rollback/reference state preserved

If any item is unknown:
do not promote.

Promotion is a distinct stage, not the last step of building.

---

## 83. HANDOFF PROMPT

Whenever a chat/session becomes too heavy, generate a fixed handoff.

Required structure:

CURRENT STATE

What is true now.

CLOSED ITEMS

What must not be reopened.

OPEN ITEMS

Only genuine remaining work.

NEXT ACTION

One concrete next step.

DO NOT TOUCH

Certified behavior/assets/boundaries.

ASSET MAP

Current IDs and roles.

LATEST FOUNDER RULINGS

Any decision that supersedes prior design.

TOOL / AI ROLE

Who should handle the next task.

This is how continuity survives Memory being off.

---

## 84. FIXED PROMPTS SHOULD BE COMPOSED, NOT REWRITTEN FROM SCRATCH

If a task needs multiple controls, combine the relevant prompt blocks.

Example:

For a Cowork feasibility check:

- ADMIN product context
- Cowork read-only inspection prompt
- exact asset list
- expected output format

For a Sonnet implementation:

- ADMIN product context
- Sonnet construction prompt
- accepted state/outcome contract
- do-not-touch list

For promotion:

- Make build discipline
- acceptance results
- promotion prompt
- provenance data

Do not improvise a completely new workflow every time.

---

## 85. PROMPT VERSIONING

When a fixed prompt materially changes, version it.

Example:

- BLOCK1_1_ADMIN_MASTER_V1
- COWORK_INSPECTION_V1
- SONNET_CONSTRUCTION_V2
- MAKE_PROMOTION_V1

If a newer founder ruling supersedes an old prompt:

latest founder ruling wins.

Do not let an older prompt silently override a newer product decision.

---

## 86. PROMPT INPUT / OUTPUT CONTRACT

Every fixed prompt should make these explicit:

INPUT
What evidence/context is being supplied?

AUTHORITY
Which parts are product truth vs proposals?

TASK
What exactly should the model/tool do?

PROHIBITIONS
What must it not change/reopen?

OUTPUT
What structure must come back?

NEXT GATE
Who reviews/accepts the output?

This is the same deterministic philosophy used in Block 1 itself.

---

## 87. FULL END-TO-END AI WORKFLOW

For a typical Block 1.1 feature:

Step 1 — ADMIN

Define/freeze behavior.

↓

Step 2 — Sonnet

Construct implementation.

↓

Step 3 — ChatGPT

Counter-review.

↓

Step 4 — Cowork

Inspect compatibility with live systems.

↓

Step 5 — ADMIN

Resolve any actual product gaps.

↓

Step 6 — Build

Implement smallest bounded change.

↓

Step 7 — Acceptance

Run fixed test cases.

↓

Step 8 — Cowork / direct inspection

Verify actual destination state.

↓

Step 9 — ADMIN

Accept or reject.

↓

Step 10 — Promotion

Use fixed promotion prompt.

↓

Step 11 — Handoff

Update continuity packet.

That is the full reusable operating loop.

---

## 88. FINAL FIXED-PROMPT RULE

The purpose of the prompt system is not bureaucracy.

It is to prevent:

- model drift
- stale assumptions
- accidental redesign
- scope creep
- unverifiable implementation
- lost context between chats
- one AI overriding another without evidence

The rule is:

Prompt the role, constrain the task, demand the output, verify the result, centralize the decision.

That is how fixed prompts should be used from end to end throughout Block 1.1.
