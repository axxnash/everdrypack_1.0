# EverDryPack Operations System
## `AGENTS.md`

This file defines how AI coding agents should work on the EverDryPack project.

The goal is to keep implementation aligned with the approved product requirements, UI design, factory SOP, role permissions, and data integrity rules.

---

# 1. Project Context

EverDryPack is an internal manufacturing operations system for Ever Drypack Malaysia Sdn. Bhd.

The system digitises and connects:

```text
Order
→ Material Requirement
→ Purchasing
→ Receiving / GRN
→ Incoming QC
→ Store
→ Production Scheduling
→ Production Run
→ In-Process QC / SPC
→ Finished Goods
→ Final QC
→ Delivery
→ Reports
```

The system must follow the company's existing SOP while reducing manual data entry.

The core principle is:

> Enter data once, reuse it everywhere, and only show each department the information they need.

---

# 2. Source of Truth

When implementing features, use these documents in this priority order:

1. `prd.md`
2. `design.md`
3. `AGENTS.md`
4. Existing database schema
5. Existing code behavior

If existing code conflicts with `prd.md`, prefer the PRD unless the requested task explicitly says otherwise.

If UI behavior conflicts with `design.md`, prefer the design specification unless the task explicitly overrides it.

Do not invent business rules that are not documented.

If a requirement is genuinely ambiguous, choose the simplest implementation that:

- preserves the SOP,
- minimizes duplicate entry,
- protects data integrity,
- and can be extended later.

---

# 3. Current User Roles

The MVP contains six roles:

```text
MD
ADMIN
QC
OPERATOR
PURCHASING
STORE
```

Do not add extra roles unless explicitly requested.

---

# 4. Role Responsibilities

## MD

Primary purpose:

- Monitor the business.
- View production.
- View risks.
- View QC.
- View inventory.
- View deliveries.
- View reports.

MD is mostly read-only.

---

## Admin

Primary purpose:

- Key in customer orders.
- Manage scheduling.
- Manage master data.
- Monitor operational workflow.
- Manage users where permitted.

Admin is the owner of production scheduling.

---

## QC

Primary purpose:

- Incoming inspection.
- In-process QC.
- SPC monitoring.
- Final inspection.
- Quality release / rejection.

QC owns quality decisions.

---

## Operator

Primary purpose:

- Execute production.
- Start / pause / resume / stop jobs.
- Enter required QC samples.
- Record defects.
- Record downtime.
- Complete cartons.

Operator UI must remain simple.

---

## Purchasing

Primary purpose:

- Handle material shortages.
- Create purchase orders.
- Manage supplier ETA.
- Maintain suppliers.

Purchasing does not manage physical inventory.

---

## Store

Primary purpose:

- Receive materials.
- Create GRN.
- Manage physical inventory.
- Reserve materials.
- Issue materials to production.
- Receive finished goods.
- Prepare goods for delivery.

Store owns physical stock movement.

---

# 5. Business Workflow Rules

These rules are mandatory.

## 5.1 Incoming Material

Received material must not become production-usable stock immediately.

Required flow:

```text
RECEIVED
→ PENDING_QC
→ RELEASED / REJECTED
```

Only released stock is counted as available.

---

## 5.2 Production Start

Production cannot start unless:

- the order is scheduled,
- the machine is available,
- required material is released,
- required material has been issued by Store.

---

## 5.3 Scheduling

Scheduling is semi-automatic.

The system may recommend:

- machine,
- earliest start,
- run time,
- completion time,
- delivery risk.

Admin must confirm the schedule.

Do not silently reschedule production orders.

---

## 5.4 QC Sampling

For current silica gel production:

```text
Frequency: every 20 minutes
Sample size: 4 packets
```

These values must be configurable.

Do not hard-code them globally if a product-specific setting exists.

---

## 5.5 Incoming Moisture Rule

For current silica gel incoming inspection:

```text
Moisture < 2.5%  → ACCEPT
Moisture > 2.5%  → REJECT
```

Keep threshold configurable in product/material QC specification data.

---

## 5.6 Finished Goods

A completed carton is not automatically ready for delivery.

Required flow:

```text
Production Complete
→ Store Receives Finished Goods
→ Final QC
→ Ready for Delivery
```

---

## 5.7 Inventory

Inventory must be transaction-based.

Do not directly overwrite stock totals without a corresponding transaction record.

Examples:

```text
GRN_RECEIPT
QC_RELEASE
QC_REJECTION
RESERVATION
MATERIAL_ISSUE
MATERIAL_RETURN
FINISHED_GOODS_RECEIPT
DELIVERY_OUT
STOCK_ADJUSTMENT
```

---

# 6. Core Order Statuses

Use clear internal enums.

Recommended:

```text
UNSCHEDULED
WAITING_MATERIAL
SCHEDULED
READY
RUNNING
PAUSED
QC_HOLD
PRODUCTION_COMPLETE
AWAITING_FINISHED_GOODS_RECEIPT
AWAITING_FINAL_QC
READY_FOR_DELIVERY
DELIVERED
CLOSED
DELAYED
AT_RISK
CANCELLED
REJECTED
```

UI labels should remain human-readable.

Example:

```text
WAITING_MATERIAL → Waiting Material
QC_HOLD          → QC Hold
READY_FOR_DELIVERY → Ready for Delivery
```

---

# 7. Preferred Tech Stack

Use the current project stack when already established.

Preferred architecture:

```text
Next.js
TypeScript
Tailwind CSS
shadcn/ui
PostgreSQL
Prisma
```

Authentication may use the implementation already present in the project.

Do not replace an existing auth solution without a strong reason.

---

# 8. General Engineering Principles

## 8.1 Keep the Architecture Simple

Do not introduce:

- microservices,
- message queues,
- event buses,
- Redis,
- complex workflow engines,
- Kubernetes,
- unnecessary background infrastructure

unless the project genuinely requires them.

The MVP should remain easy to deploy and maintain.

---

## 8.2 Prefer Server-Side Enforcement

Security and business rules must be enforced on the server.

Hiding a button is not permission enforcement.

Every mutating endpoint/server action must validate:

- authenticated user,
- user role,
- permission,
- input data,
- current record state.

---

## 8.3 Avoid Duplicate Business Logic

If the same rule is used in multiple places, move it into a shared domain/service function.

Examples:

```text
calculateMaterialRequirement()
canStartProduction()
calculateQcResult()
calculateAvailableStock()
getScheduleRecommendation()
```

---

## 8.4 Preserve Auditability

Do not silently mutate critical historical records.

Important changes should create an audit log.

---

# 9. Project Structure Guidance

Preferred structure:

```text
app/
  (auth)/
  dashboard/
  orders/
  schedule/
  purchase/
  store/
  receiving/
  qc/
  production/
  finished-goods/
  delivery/
  reports/
  admin/

components/
  layout/
  common/
  orders/
  schedule/
  purchasing/
  store/
  receiving/
  qc/
  production/
  finished-goods/
  delivery/
  reports/

lib/
  auth/
  db/
  permissions/
  validation/
  domain/
  services/
  utils/

prisma/
  schema.prisma

types/
```

Keep domain logic out of React components where practical.

---

# 10. Naming Conventions

Use clear names.

Good:

```text
productionOrder
materialRequirement
qcInspection
inventoryTransaction
finishedGoodsTransfer
```

Avoid vague names:

```text
data
item2
temp
thing
recordData
```

---

# 11. Database Naming

Prefer singular Prisma model names and descriptive fields.

Example:

```prisma
model ProductionOrder {
  id               String   @id @default(cuid())
  orderNumber      String   @unique
  customerId       String
  productId        String
  quantity         Decimal
  requiredDate     DateTime
  status           ProductionOrderStatus
  createdAt        DateTime @default(now())
  updatedAt        DateTime @updatedAt
}
```

Do not use database fields whose meaning is unclear.

---

# 12. IDs and Document Numbers

Use database IDs internally.

Use human-readable generated document numbers externally.

Examples:

```text
SO-20260921-001
PO-20260921-003
GRN-20260921-004
BATCH-20260921-01
CTN-20260921-0001
FGT-20260921-001
DO-20260921-001
```

Document numbering must be generated on the server.

Do not trust the client to generate unique business document numbers.

---

# 13. Validation

Use schema validation for every form and server action.

Preferred:

```text
Zod
```

Validate:

- required values,
- positive quantity,
- valid dates,
- supported enums,
- foreign-key existence,
- permission,
- business-state transitions.

Example:

A production quantity cannot be:

```text
0
negative
NaN
undefined
```

---

# 14. Money and Quantity Data

For important numeric values, avoid floating-point assumptions.

Use:

- `Decimal` in Prisma for weights and quantities when needed,
- integer units for exact piece counts.

Examples:

```text
pieces       → integer
cartons      → integer
kg           → Decimal
grams        → Decimal
percentage   → Decimal
```

---

# 15. Date and Time Rules

Store timestamps consistently.

Preferred:

- database: UTC
- display: application/user timezone

For Malaysia deployment, UI commonly displays:

```text
Asia/Kuala_Lumpur
```

Do not perform schedule logic using formatted date strings.

Use actual date objects/timestamps.

---

# 16. Permission Enforcement

Create reusable permission checks.

Example:

```ts
requireRole(user, ["ADMIN"])
requireRole(user, ["QC"])
requireRole(user, ["STORE", "ADMIN"])
```

Or permission-based checks:

```text
MANAGE_ORDERS
MANAGE_SCHEDULE
MANAGE_PURCHASE_ORDERS
MANAGE_GRN
MANAGE_QC
RUN_PRODUCTION
MANAGE_INVENTORY
VIEW_REPORTS
```

Do not scatter hard-coded role conditionals throughout the UI.

---

# 17. UI Design Rules

The UI must follow `design.md`.

Key visual rules:

- Professional
- Industrial
- Clean
- White background
- Blue primary
- Orange accent
- Compact cards
- Table-focused
- Minimal gradients
- Minimal shadows
- No cartoon-style UI

---

# 18. EverDryPack Theme

Primary brand:

```text
Blue
Orange
White
Neutral gray
```

Functional colors:

```text
Green = success
Amber = warning
Red = failure
Gray = inactive
```

Use colors consistently.

Do not assign random colors to modules.

---

# 19. Tables

Transactional pages should primarily use tables.

Use tables for:

- production orders,
- purchase orders,
- GRNs,
- inventory,
- QC history,
- finished goods,
- deliveries,
- users.

Tables should support:

- search,
- filters,
- pagination,
- status badge,
- action menu.

---

# 20. Forms

Forms should be compact and predictable.

Layout:

```text
Label
Input
Helper/error
```

Do not hide required field labels inside placeholders.

Buttons should follow this order when possible:

```text
Secondary actions → Primary action
```

Example:

```text
Cancel | Save Draft | Create Order
```

---

# 21. Operator UI Rules

Operator screens are special.

They must prioritize:

1. Current job
2. Material status
3. Production status
4. Next QC
5. Main actions

The normal production control buttons are:

```text
Start Production
QC Check
Record Defect
Pause / Downtime
Complete Carton
End Production
```

Do not add unrelated controls to the operator home screen.

---

# 22. Operator Button State

## READY

Enable:

```text
Start Production
```

Disable:

```text
QC Check
Record Defect
Pause
Complete Carton
End Production
```

---

## RUNNING

Enable:

```text
QC Check
Record Defect
Pause
Complete Carton
End Production
```

---

## PAUSED

Enable:

```text
Resume
End Production
```

---

## QC HOLD

Production resume must be disabled until release.

---

# 23. Order Creation Logic

When Admin creates an order:

1. Validate customer.
2. Validate product.
3. Validate quantity.
4. Validate required delivery date.
5. Create order.
6. Generate material requirement.
7. Compare requirement against available released stock.
8. Calculate shortage.
9. Generate schedule recommendation.
10. Create role-specific tasks/notifications.

This should happen through domain/services, not in UI components.

---

# 24. Material Requirement Logic

Product BOM must drive material requirement.

Do not manually duplicate materials inside every order.

Example:

```text
Product
  ↓
ProductMaterial / BOM
  ↓
Order Quantity
  ↓
Required Materials
```

If product specification changes, historical production orders must preserve the material requirements actually used at the time.

Consider snapshotting requirement values when the order is confirmed.

---

# 25. Schedule Recommendation Logic

Initial MVP recommendation can use:

```text
order quantity
÷ standard production rate
+ setup/changeover allowance
```

Also consider:

- compatible machines,
- existing scheduled jobs,
- material-ready date,
- maintenance blocks,
- delivery date.

Output:

```text
recommended machine
earliest start
estimated finish
estimated duration
material status
risk
```

Do not claim prediction accuracy beyond available data.

---

# 26. Material Availability

Available stock should be calculated from released inventory.

Conceptually:

```text
available =
on_hand
- reserved
- quarantined
- blocked
```

Avoid treating incoming GRN stock as available before release.

---

# 27. Purchasing Task Generation

When shortage exists:

Create a material shortage record or task.

It should contain:

```text
materialId
productionOrderId
requiredQty
availableQty
shortageQty
requiredBy
priority
status
```

Purchasing should see this immediately.

---

# 28. GRN Logic

Store creates GRN.

On initial save:

- do not update usable stock.

On physical receipt:

- create stock in quarantine/received state.

After QC release:

- create release transaction.

After rejection:

- move to rejected state.

---

# 29. Incoming QC Logic

For each incoming inspection:

Store:

```text
inspectedBy
inspectionTime
material
GRN
sampleMethod
moisture
appearance
decision
remarks
```

Decision should be server-validated against active specification rules where applicable.

---

# 30. Production Start Logic

Before start, server must verify:

```text
order.status == READY or SCHEDULED as allowed
machine available
required material issued
no active conflicting production run
user has RUN_PRODUCTION permission
```

Then create `ProductionRun`.

Record:

```text
machine
operator
startTime
order
batch
targetQty
```

---

# 31. QC Reminder Logic

After production starts:

Generate QC due times from:

```text
production start
+
qc frequency
```

For MVP, reminders can be calculated dynamically rather than requiring a background job for every interval.

The UI can derive:

```text
nextQcDueAt
```

from last QC or production start.

If notification persistence is required, use scheduled records/tasks.

---

# 32. QC Calculation

Given four weights:

```text
w1
w2
w3
w4
```

Calculate:

```text
min
max
average
```

Validate each required sample against configured spec.

Return:

```text
PASS
FAIL
```

Store the raw values.

Never store only the average.

---

# 33. SPC Data

SPC chart should use persisted QC samples/check results.

Do not generate fake points.

Return:

```text
sample sequence
timestamp
value / average
UCL
CL
LCL
```

If control limits differ from product acceptance limits, keep them as separate fields.

---

# 34. Defect Logic

Defects must link to:

```text
productionRun
order
batch
machine
operator
time
type
quantity
remark
```

Do not subtract defects automatically from confirmed carton quantity unless business logic explicitly requires it.

The company currently relies on confirmed finished/carton quantity rather than exact automatic packet count.

---

# 35. Estimated vs Confirmed Output

Keep both.

Example:

```text
estimatedOutput
confirmedFinishedQty
```

Estimated output may be based on:

```text
run time × machine rate
```

Confirmed output is based on:

- carton completion,
- verified quantity,
- production records.

Do not present estimated output as exact accepted production.

---

# 36. Carton Completion

On complete carton:

Create a carton record.

Fields should include:

```text
cartonNumber
productionRunId
batchId
productId
quantity
weightConfirmed
appearancePassed
labelPassed
completedBy
completedAt
status
```

Finished quantity should derive from accepted carton records.

---

# 37. Finished Goods Transfer

Do not ask users to retype data already known.

Generate transfer data from:

- batch,
- carton,
- machine,
- operator,
- product,
- timestamps.

Store only new information:

- transfer confirmation,
- Store receiver,
- receipt time,
- remarks.

---

# 38. Final QC

Final QC must reference finished goods/batch.

Do not allow delivery-ready state without final QC acceptance when the product requires final QC.

---

# 39. Delivery

Delivery records should reference the original order and finished goods.

Do not manually duplicate:

- product,
- customer,
- order quantity

if they can be derived.

Allow actual shipped quantity and cartons to differ only through an explicit recorded adjustment.

---

# 40. Audit Logging

Audit these actions at minimum:

```text
order create/update/cancel
schedule confirm/change
purchase order create/update
GRN create/update
QC accept/reject
stock adjustment
material issue
production start/pause/resume/end
QC sample edit
defect edit
carton completion/edit
finished goods transfer
final QC
delivery dispatch/complete
user/role changes
```

Audit log should record:

```text
actor
action
entity
entityId
timestamp
before
after
```

Avoid putting sensitive passwords/tokens in audit data.

---

# 41. Notifications

Notifications should be targeted.

Examples:

## Admin

- schedule risk,
- material shortage,
- QC failure,
- production delay.

## QC

- incoming inspection pending,
- QC check overdue,
- final QC pending.

## Purchasing

- shortage,
- supplier ETA delayed.

## Store

- incoming delivery,
- material request,
- finished goods awaiting receipt.

## Operator

- job ready,
- QC due,
- QC hold.

## MD

- critical shortage,
- production at risk,
- QC critical issue,
- late delivery risk.

---

# 42. Avoid Notification Spam

Do not generate repeated identical alerts every page refresh.

Notifications should have:

```text
type
entityId
dedupeKey
createdAt
readAt
resolvedAt
```

---

# 43. Error Handling

Every mutation should return a structured result.

Example:

```ts
{
  success: false,
  error: "Material has not been released by QC."
}
```

Do not expose raw database errors to the user.

Log technical details server-side.

---

# 44. Loading States

For actions:

```text
Create Order
Submitting...
```

Disable button during request.

Prevent double submissions.

---

# 45. Empty States

Implement useful empty states.

Example:

```text
No material shortages.
All scheduled production currently has sufficient released stock.
```

Avoid empty white panels.

---

# 46. Accessibility

Minimum:

- semantic HTML,
- keyboard navigation,
- visible focus,
- labeled inputs,
- adequate contrast,
- status text in addition to color,
- accessible dialogs.

---

# 47. Responsive Behavior

The primary experience is desktop/tablet.

At narrower widths:

- collapse sidebar,
- allow tables to scroll locally,
- stack metric cards,
- keep critical action buttons visible.

Do not redesign operator controls into tiny mobile buttons.

---

# 48. Testing Requirements

Add tests for critical business logic.

At minimum:

## Material

- released stock counted,
- quarantine stock excluded,
- reservations reduce availability.

## Production

- cannot start without issued material,
- cannot run two jobs on same machine simultaneously.

## QC

- pass/fail calculations,
- four required samples,
- spec boundary behavior.

## Inventory

- transactions change balances correctly,
- delivery decreases finished goods,
- material issue decreases available raw material.

## Permissions

- operator cannot create PO,
- purchasing cannot approve QC,
- QC cannot manage users,
- MD cannot mutate restricted operational data unless allowed.

---

# 49. Boundary Tests

Test exact limits.

Examples:

If specification is:

```text
0.90 to 1.20
```

Define explicitly whether boundary values pass.

Recommended:

```text
>= lower
<= upper
```

So:

```text
0.90 PASS
1.20 PASS
```

For moisture:

Clarify the exact equality rule in code.

If SOP only says:

```text
Below 2.5 = Accept
Above 2.5 = Reject
```

do not silently guess equality. Store the policy in a configurable specification and document the chosen behavior.

---

# 50. Seed Data

Development seed data should include:

Users:

```text
MD User
Admin User
QC User
Operator 1
Purchasing User
Store User
```

Machines:

```text
M-01
M-02
```

Example products:

```text
Silica Gel 1g
Silica Gel 2g
Silica Gel 5g
Activated Clay Desiccant
```

Example materials:

```text
Silica Gel
Activated Clay
Packing Roll
Tyvek / Non-woven Paper
Carton Box
Label
Ink
```

Do not use seed data as production assumptions.

---

# 51. Logging

Use structured logs for:

- server errors,
- critical workflow failures,
- permission denials,
- integration failures.

Avoid excessive console logging in production.

---

# 52. Performance

Avoid:

- loading all records without pagination,
- unnecessary repeated queries,
- fetching full object graphs for list pages.

Use:

- pagination,
- selective columns,
- indexed searchable fields,
- server-side filtering.

Likely indexes:

```text
orderNumber
status
requiredDate
machineId
materialId
supplierId
grnNumber
batchNumber
cartonNumber
createdAt
```

---

# 53. Database Transactions

Use database transactions for multi-step critical mutations.

Examples:

## QC Release

```text
update inspection
create inventory transaction
update inventory balance
update material status
update linked production readiness
create audit log
```

These should succeed or fail together where practical.

---

# 54. Soft Deletion

Avoid deleting historical:

- orders,
- QC records,
- inventory transactions,
- production runs,
- GRNs,
- deliveries.

For master data use:

```text
isActive
archivedAt
```

where appropriate.

---

# 55. API / Server Action Naming

Use domain verbs.

Examples:

```text
createProductionOrder
confirmProductionSchedule
createPurchaseOrder
receiveGoods
submitIncomingQc
releaseMaterial
issueMaterials
startProduction
pauseProduction
resumeProduction
recordDefect
completeCarton
submitFinalQc
dispatchDelivery
```

Avoid generic actions like:

```text
saveData
updateThing
processItem
```

---

# 56. Form Autosave

Do not add autosave everywhere.

Use explicit save for important manufacturing records.

Draft support is appropriate for:

- order,
- purchase order,
- GRN.

QC and production actions should generally be explicit submissions.

---

# 57. Printing

Print views must be separate from application chrome.

Do not print:

- sidebar,
- top bar,
- notification buttons,
- action buttons.

Print should show:

- logo,
- company,
- document title,
- number,
- data,
- approval metadata,
- page/revision where needed.

---

# 58. Existing Paper Forms

Where the company expects the old paper format:

- preserve required information,
- generate the record from system data,
- do not force users to fill the same form manually if the system already has the values.

Digital workflow is primary.

Printable form is output.

---

# 59. Charts

Use charts only where meaningful.

Appropriate:

- production trend,
- SPC,
- reject breakdown,
- delivery trend,
- machine utilization.

Avoid charts for simple 3-row lists.

Use tables instead.

---

# 60. UI Copy

Use short operational wording.

Good:

```text
Material Ready
Pending QC
Issue Materials
Start Production
Complete Carton
Ready for Delivery
```

Avoid:

```text
Click here to proceed with the next phase of your workflow
```

---

# 61. Confirmation Rules

Require confirmation for destructive/high-impact actions:

- reject QC,
- cancel order,
- end production,
- stock adjustment,
- delete draft if permanent.

Do not add confirmation to every normal button.

---

# 62. Code Quality

All TypeScript should use strict types.

Avoid:

```ts
any
```

unless integration constraints genuinely require it.

Prefer:

```ts
unknown
```

with validation.

Do not suppress TypeScript errors without explanation.

---

# 63. React / Next.js Guidance

Prefer Server Components where suitable.

Use Client Components only where interaction requires them.

Good Client Component candidates:

- tables with client filters,
- dialogs,
- production controls,
- chart interactions,
- timers.

Do not make the whole app `"use client"`.

---

# 64. State Management

Do not introduce global state libraries by default.

Prefer:

- URL/search params,
- server data,
- local component state,
- form state.

Add a global state tool only when there is a proven need.

---

# 65. Data Fetching

Prefer framework-native server data loading.

Keep business queries in reusable service/repository functions.

Avoid fetching the same data separately in multiple nested components.

---

# 66. Security

Never expose:

- database credentials,
- API secrets,
- server tokens,
- password hashes.

Use environment variables.

Validate uploads if file attachment features are added.

---

# 67. File Uploads

Future calibration certificates or documents should store:

- metadata in DB,
- object/file path externally.

Validate:

- type,
- size,
- ownership,
- access permission.

Do not store huge binaries directly in standard relational rows unless required.

---

# 68. Environment Variables

Document required environment variables in:

```text
.env.example
```

Never commit real credentials.

---

# 69. Migration Rules

For Prisma:

- make schema changes deliberately,
- create migration,
- verify existing data,
- avoid destructive migrations unless explicitly approved.

For required fields added to existing populated tables, provide migration/backfill logic.

---

# 70. Implementation Order

When building from scratch, follow:

## Phase 1

```text
Auth
Roles
Users
Master Data
App Shell
```

## Phase 2

```text
Orders
Material Requirement
Scheduling
```

## Phase 3

```text
Purchasing
Receiving
Store
Inventory
```

## Phase 4

```text
Incoming QC
Production QC
SPC
Final QC
```

## Phase 5

```text
Operator Production
Defects
Downtime
Cartons
```

## Phase 6

```text
Finished Goods
Delivery
```

## Phase 7

```text
Dashboard
Reports
Audit
Polish
```

Do not build advanced forecasting before the core workflow works.

---

# 71. Definition of Done

A feature is not complete until:

1. UI is implemented.
2. Server-side validation exists.
3. Permission checks exist.
4. Database changes are correct.
5. Audit logging is added when relevant.
6. Error/loading/empty states exist.
7. Main happy path works.
8. Important failure path works.
9. TypeScript passes.
10. Lint/tests pass where configured.
11. UI follows `design.md`.
12. Workflow follows `prd.md`.

---

# 72. Agent Workflow

When asked to implement a feature:

## Step 1

Read relevant sections of:

```text
prd.md
design.md
AGENTS.md
```

## Step 2

Inspect existing:

```text
schema
routes
components
auth
permissions
services
```

## Step 3

Plan the smallest complete change.

## Step 4

Implement backend/domain logic first where the feature has business rules.

## Step 5

Implement UI.

## Step 6

Add permission and validation checks.

## Step 7

Run:

```text
typecheck
lint
tests
build
```

as applicable.

## Step 8

Fix errors introduced by the change.

## Step 9

Summarize:

- what changed,
- files changed,
- migrations,
- assumptions,
- remaining limitations.

---

# 73. Do Not Do These Things

Do not:

- redesign the workflow without instruction,
- remove required SOP stages,
- make incoming stock immediately available,
- allow Operator to access admin functions,
- allow Purchasing to directly manipulate physical inventory,
- allow Store to make QC decisions,
- automatically mark estimated production as confirmed output,
- auto-reschedule jobs without Admin confirmation,
- silently overwrite QC history,
- delete inventory transactions,
- build cartoon-style interfaces,
- add unnecessary infrastructure,
- create duplicate copies of order information in every module,
- hard-code product-specific values if they belong in master data,
- generate fake production/QC data in real workflows.

---

# 74. Preferred Decision Rule

When unsure between two implementations, prefer the one that:

1. follows the SOP,
2. reduces operator effort,
3. keeps data traceable,
4. avoids duplicate entry,
5. gives the responsible role ownership,
6. is easy to maintain,
7. can be extended later.

---

# 75. Core Product Mental Model

The system is not a collection of unrelated forms.

It is one connected manufacturing workflow.

The central entities are:

```text
Order
Material
Purchase
GRN
Inspection
Inventory
Schedule
Production Run
QC Check
Batch
Carton
Finished Goods
Delivery
```

Paper forms are generated views of these records.

Do not build each paper form as an isolated database island.

---

# 76. Final Agent Rule

The best EverDryPack implementation should make the factory workflow feel simpler than the paper process.

For each feature ask:

> Can the system already know this value?

If yes:

**Auto-fill it.**

> Does another department need this information?

If yes:

**Send it through workflow/status/notification.**

> Does the user really need another form?

If no:

**Do not create one.**

> Is this action part of the approved SOP?

If yes:

**Preserve it and make it easier to perform digitally.**
