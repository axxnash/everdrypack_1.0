# EverDryPack Operations System
## `AGENTS.md`

This file defines how AI coding agents such as Codex should work on the EverDryPack project.

The goal is to keep implementation aligned with:

1. `prd.md`
2. `design.md`
3. this `AGENTS.md`
4. the approved EverDryPack SOP
5. the existing repository structure and database

The application is an internal manufacturing operations system for Ever Drypack Malaysia Sdn. Bhd.

---

# 1. Core Product Principle

EverDryPack must not be built as a collection of unrelated digital paper forms.

It is one connected manufacturing workflow:

```text
Admin Order Entry
        ↓
Material Requirement
        ↓
Store Stock Check / Reservation
        ↓
Purchasing if Material Shortage
        ↓
Supplier Delivery
        ↓
Store Receiving / GRN
        ↓
Incoming QC
        ↓
Released Inventory
        ↓
Admin Production Scheduling
        ↓
Store Material Issue
        ↓
Production / Operator
        ↓
In-Process QC / SPC
        ↓
Finished Goods
        ↓
Store Finished-Goods Receipt
        ↓
Final QC
        ↓
Delivery
        ↓
Reports / MD Dashboard
```

The core software rule is:

> **Enter data once, reuse it everywhere, and automatically send each department only the information they need.**

---

# 2. Source of Truth

Before implementing any feature, read:

```text
prd.md
design.md
AGENTS.md
```

Priority when documents conflict:

1. Explicit instruction from the current task
2. `prd.md` for business/SOP requirements
3. `design.md` for UI, UX, frontend structure, and current technical architecture
4. `AGENTS.md` for coding and implementation rules
5. Existing schema/code

Do not invent undocumented business rules.

If a real company value is unknown, make it configurable or leave a clearly identified placeholder rather than hard-coding a planning example.

Examples of values that must come from company master data:

- product production rate,
- packets per carton,
- QC weight limits,
- QC frequency,
- QC sample size,
- BOM quantities,
- machine compatibility,
- reorder levels,
- supplier lead time,
- final QC sampling rules.

---

# 3. Approved Tech Stack

Use this stack unless the user explicitly changes it.

## Frontend

```text
Vite
React.js
JavaScript
Tailwind CSS
React Router DOM
```

Recommended frontend libraries:

```text
TanStack Query
React Hook Form
Zod
Recharts
Lucide React
```

## Backend

```text
Node.js
Express.js
JavaScript
```

## Database

```text
PostgreSQL
Neon PostgreSQL
```

## ORM

```text
Prisma
```

## Architecture

```text
Separate frontend and backend
REST API
Route → Middleware → Controller → Service → Prisma/Repository
Role-based access control
```

---

# 4. Technology Restrictions

Do **not** introduce:

- Next.js
- TypeScript
- `.ts`
- `.tsx`
- Next.js Server Actions
- Next.js API routes
- Next.js Server Components
- unnecessary microservices
- unnecessary Redis
- unnecessary message queues
- unnecessary Kubernetes
- GraphQL unless explicitly requested

Use JavaScript throughout the MVP.

Frontend files should normally use:

```text
.js
.jsx
```

Backend files should use:

```text
.js
```

---

# 5. Current User Roles

The MVP contains exactly six operational roles:

```text
MD
ADMIN
QC
OPERATOR
PURCHASING
STORE
```

Do not add roles unless explicitly requested.

---

# 6. Role Ownership

## MD

Main purpose:

- view factory performance,
- view production,
- view risks,
- view shortages,
- view QC,
- view deliveries,
- view reports.

MD is mainly read-only.

## ADMIN

Main purpose:

- key customer/production orders,
- coordinate operations,
- review material readiness,
- confirm production scheduling,
- maintain master data,
- manage users,
- view reports.

Admin owns the production schedule.

## QC

Main purpose:

- incoming inspection,
- material release/rejection,
- in-process QC,
- SPC,
- corrective-action checks,
- final inspection,
- calibration where assigned.

QC owns quality decisions.

## OPERATOR

Main purpose:

- run assigned production,
- start/pause/resume/end jobs,
- submit required production/QC data,
- record defects,
- record downtime,
- complete cartons.

Operator UI must remain extremely simple.

## PURCHASING

Main purpose:

- view material shortages,
- create purchase orders,
- manage suppliers,
- manage supplier ETA.

Purchasing does not own physical inventory.

## STORE

Main purpose:

- receive physical goods,
- create GRN,
- manage stock,
- reserve materials,
- issue materials,
- receive finished goods,
- prepare goods for delivery.

Store owns physical stock movement.

---

# 7. Mandatory SOP Rules

## 7.1 Incoming Goods

Received goods must follow:

```text
RECEIVED
→ PENDING_QC
→ RELEASED / REJECTED
```

Received goods must not become usable production stock before QC release.

## 7.2 Inventory Availability

Only released stock can count as production-available.

Inventory must distinguish at minimum:

```text
AVAILABLE
RESERVED
QUARANTINE
REJECTED
WIP
FINISHED_GOODS
```

## 7.3 Production Start

Production must not start unless:

- order is scheduled,
- machine is available,
- required material has passed QC,
- required material has been issued by Store,
- no conflicting active production run exists.

## 7.4 Scheduling

Scheduling is semi-automatic.

The backend may recommend:

- machine,
- earliest start,
- estimated runtime,
- estimated completion,
- delivery risk.

Admin confirms the schedule.

Never silently reschedule an order.

## 7.5 In-Process QC

Current silica gel workflow uses:

```text
QC interval: 20 minutes
Sample size: 4 packets
```

These values must be configurable per product/specification.

## 7.6 Finished Goods

Finished goods flow:

```text
Production Complete
→ Store Receipt
→ Final QC
→ Ready for Delivery
```

A finished carton is not automatically ready for delivery.

---

# 8. Repository Structure

Prefer a simple monorepo-style layout:

```text
everdrypack/
├─ frontend/
├─ backend/
├─ prd.md
├─ design.md
├─ AGENTS.md
└─ README.md
```

If the existing repository uses a different but clean layout, do not restructure it unnecessarily.

---

# 9. Frontend Structure

Recommended:

```text
frontend/
├─ src/
│  ├─ api/
│  ├─ assets/
│  ├─ components/
│  │  ├─ layout/
│  │  ├─ common/
│  │  ├─ orders/
│  │  ├─ schedule/
│  │  ├─ purchasing/
│  │  ├─ store/
│  │  ├─ receiving/
│  │  ├─ qc/
│  │  ├─ production/
│  │  ├─ finishedGoods/
│  │  ├─ delivery/
│  │  └─ reports/
│  ├─ pages/
│  ├─ hooks/
│  ├─ context/
│  ├─ routes/
│  ├─ utils/
│  ├─ constants/
│  ├─ App.jsx
│  └─ main.jsx
├─ public/
├─ index.html
├─ vite.config.js
└─ package.json
```

Do not create one huge `App.jsx`.

---

# 10. Backend Structure

Use the route/controller/service style requested for this project.

Recommended:

```text
backend/
├─ src/
│  ├─ config/
│  ├─ routes/
│  ├─ controllers/
│  ├─ services/
│  ├─ repositories/
│  ├─ middleware/
│  ├─ validators/
│  ├─ utils/
│  ├─ constants/
│  ├─ jobs/
│  ├─ app.js
│  └─ server.js
├─ prisma/
│  ├─ schema.prisma
│  ├─ migrations/
│  └─ seed.js
├─ .env.example
└─ package.json
```

Do not put the whole backend in `server.js`.

---

# 11. Backend Layer Responsibilities

## Routes

Routes define:

- path,
- HTTP method,
- middleware,
- controller.

Example:

```js
router.post(
  "/orders",
  authenticate,
  authorize("ADMIN"),
  validate(createOrderSchema),
  orderController.createOrder
);
```

No complex business logic in route files.

## Middleware

Use middleware for:

```text
authentication
authorization
validation
error handling
request logging
```

## Controllers

Controllers should be thin:

1. read request inputs,
2. call a service,
3. send response.

Do not place scheduling, inventory, or QC calculations in controllers.

## Services

Services contain authoritative business logic.

Examples:

```text
authService
orderService
scheduleService
materialRequirementService
purchaseService
receivingService
inventoryService
qcService
productionService
finishedGoodsService
deliveryService
notificationService
auditService
reportService
```

## Repositories

Repositories may contain reusable Prisma queries.

Use them when they improve reuse and readability; do not add boilerplate only for architecture's sake.

---

# 12. REST API Principles

Base API path:

```text
/api
```

Examples:

```text
GET    /api/orders
POST   /api/orders
GET    /api/orders/:id
PATCH  /api/orders/:id

POST   /api/orders/:id/schedule
GET    /api/schedule

GET    /api/material-shortages

GET    /api/purchase-orders
POST   /api/purchase-orders

GET    /api/grns
POST   /api/grns
POST   /api/grns/:id/submit-qc

GET    /api/qc/incoming
POST   /api/qc/incoming/:grnId

GET    /api/inventory
POST   /api/material-issues

POST   /api/production-runs/:id/start
POST   /api/production-runs/:id/pause
POST   /api/production-runs/:id/resume
POST   /api/production-runs/:id/end

POST   /api/production-runs/:id/defects
POST   /api/production-runs/:id/cartons

POST   /api/qc/production-checks

POST   /api/finished-goods/transfers
POST   /api/finished-goods/transfers/:id/receive

POST   /api/final-qc/:batchId

GET    /api/deliveries
POST   /api/deliveries/:id/dispatch
POST   /api/deliveries/:id/complete
```

---

# 13. API Response Format

Success:

```json
{
  "success": true,
  "data": {}
}
```

Error:

```json
{
  "success": false,
  "message": "Material has not been released by QC."
}
```

Validation error:

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {
    "quantity": "Quantity must be greater than 0."
  }
}
```

Do not expose raw Prisma/database errors to users.

---

# 14. Frontend API Access

Use one centralized API client:

```text
frontend/src/api/apiClient.js
```

It should handle:

- API base URL,
- credentials,
- JSON parsing,
- common error transformation,
- authentication behavior.

Domain API files may include:

```text
authApi.js
ordersApi.js
scheduleApi.js
purchaseApi.js
inventoryApi.js
receivingApi.js
qcApi.js
productionApi.js
finishedGoodsApi.js
deliveryApi.js
reportsApi.js
```

Do not scatter hard-coded backend URLs through components.

---

# 15. React State Rules

Use local React state for:

- modal state,
- local form UI,
- selected item,
- temporary UI filters.

Recommended for server state:

```text
TanStack Query
```

Use it for:

- fetching,
- mutations,
- caching,
- query invalidation,
- loading/error states.

Do not copy all backend data into global React context.

---

# 16. Authentication

Authentication must be enforced by Express.

Requirements:

- secure login,
- password hashing,
- authenticated user endpoint,
- secure cookie/session or well-designed token approach,
- backend role checks,
- logout.

Never store plain-text passwords.

Never depend solely on frontend route guards.

---

# 17. Authorization

Frontend may hide irrelevant navigation/actions for UX.

Backend always enforces role/permission.

Example:

```text
Operator calls POST /api/purchase-orders
→ HTTP 403
```

even if the request is manually sent.

---

# 18. Permission Helpers

Centralize authorization.

Prefer middleware/helpers such as:

```js
authorize("ADMIN")
authorizeAny(["ADMIN", "STORE"])
```

Do not duplicate role checks throughout the codebase.

---

# 19. Database / Prisma Rules

Use PostgreSQL through Prisma.

Neon may host PostgreSQL.

Use:

```text
DATABASE_URL
```

from environment configuration.

Never commit production credentials.

---

# 20. Main Data Entities

Expected domain entities include:

```text
User
Customer
Supplier
Product
ProductSpec
Material
ProductMaterial
Machine
Order
ProductionSchedule
ProductionRun
Batch
Carton

PurchaseOrder
PurchaseOrderItem

GoodsReceived
GoodsReceivedItem

IncomingQcInspection
ProductionQcCheck
ProductionQcSample
FinalQcInspection

Defect
DowntimeRecord
CorrectiveAction

MaterialRequest
MaterialIssue

InventoryTransaction
InventoryReservation

FinishedGoodsTransfer
FinishedGoodsTransferItem

Delivery
DeliveryItem

Equipment
Calibration

Notification
AuditLog
```

Exact model names may vary if the existing schema already uses clear names.

---

# 21. Human-Readable Document Numbers

Use database IDs internally and readable business numbers externally.

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

Generate these on the server.

---

# 22. Validation

Every mutating endpoint must validate:

- required fields,
- strings/enums,
- quantity > 0,
- valid dates,
- IDs,
- permissions,
- current workflow status.

Client validation improves UX.

Backend validation is authoritative.

Recommended:

```text
Zod
```

or another consistent validation library.

---

# 23. Numbers and Units

Use integers for exact counts such as:

```text
packet count
carton count
sample count
```

Use Prisma Decimal for values requiring precision:

```text
weight
kg
grams
percentage
material quantity
```

---

# 24. Dates and Time

Store timestamps consistently, preferably UTC.

Display Malaysia time in the UI:

```text
Asia/Kuala_Lumpur
```

Do not perform schedule calculations using formatted date strings.

---

# 25. Order Creation Workflow

When Admin creates an order:

1. validate user/input,
2. create order,
3. generate internal order number,
4. load product master,
5. calculate material requirement,
6. compare against released inventory,
7. identify shortage,
8. create shortage task if required,
9. calculate schedule recommendation,
10. create role-specific notifications,
11. create audit log.

Do not make Admin re-enter known product/system values.

---

# 26. BOM / Material Requirements

Material requirements come from Product BOM/master data.

Concept:

```text
Product BOM
×
Order Quantity
=
Material Requirements
```

Do not manually duplicate materials in every order.

Preserve historical requirement values if master data can later change.

---

# 27. Material Availability

Concept:

```text
AVAILABLE =
released on-hand
- reserved
- blocked
```

Do not count:

```text
quarantine
rejected
unreleased GRN stock
```

as production-available.

---

# 28. Material Shortage Workflow

If stock cannot cover requirements:

```text
WAITING_MATERIAL
```

Create a shortage/task record with:

```text
productionOrderId
materialId
requiredQty
availableQty
shortageQty
requiredBy
priority
status
```

Purchasing should receive this automatically.

---

# 29. Purchase Order Workflow

When creating a PO from a shortage, prefill where possible:

- material,
- shortage quantity,
- required-by date,
- linked order.

Supplier ETA must feed back into material/schedule readiness.

Purchasing does not perform QC release or physical stock adjustment.

---

# 30. GRN / Receiving

Store creates the GRN.

Physical receipt may create stock in:

```text
QUARANTINE / PENDING_QC
```

not `AVAILABLE`.

Flow:

```text
DRAFT
→ RECEIVED
→ PENDING_QC
→ RELEASED / REJECTED
```

---

# 31. Incoming QC

QC receives GRN/material information automatically and enters only inspection-specific data.

For current configured silica gel rule:

```text
Moisture below 2.5% = Accept
Moisture above 2.5% = Reject
```

Do not invent the equality-at-2.5 behavior if the company has not confirmed it.

Keep QC rules configurable.

---

# 32. Critical QC Transactions

QC release should be atomic where practical:

1. save inspection result,
2. update inventory state,
3. create inventory transaction,
4. update linked production readiness,
5. create notification,
6. create audit log.

Use Prisma transactions for multi-write critical actions.

---

# 33. Production Scheduling

Backend owns scheduling calculations.

Inputs may include:

```text
required delivery date
product
quantity
compatible machines
machine availability
current schedule
production rate
material readiness
supplier ETA
maintenance
changeover
```

Output:

```text
recommendedMachine
estimatedRuntime
earliestStart
estimatedCompletion
deliveryRisk
conflicts
```

Admin confirms.

---

# 34. Schedule Conflicts

Backend must prevent:

- two jobs using the same machine at conflicting times,
- start on an unavailable machine,
- invalid start/end dates.

Frontend checks are helpful but not sufficient.

---

# 35. Operator UI

Normal primary actions:

```text
Start Production
QC Check
Record Defect
Pause / Downtime
Complete Carton
End Production
```

Do not add unrelated controls.

---

# 36. Production Start

Before starting, server validates:

```text
scheduled order
material released
material issued
machine available
no conflicting run
authorized operator
```

Store:

```text
productionRun
machine
operator
batch
product
targetQty
actualStartTime
```

---

# 37. QC Reminder Logic

Do not rely only on browser timers.

Backend should expose authoritative timing such as:

```text
lastQcAt
nextQcDueAt
qcFrequencyMinutes
```

Frontend may display a countdown.

Current default planning rule:

```text
20 minutes
4 samples
```

but configurable product rules are authoritative.

---

# 38. Production QC

Persist raw sample values:

```text
w1
w2
w3
w4
```

Backend calculates:

```text
minimum
maximum
average
pass/fail
```

Never store only the average.

---

# 39. QC Limits

Use product/specification master values.

Do not hard-code mockup example values.

If configured limits are inclusive, test exact boundaries.

---

# 40. SPC

SPC graphs use persisted QC data.

Do not generate fake production QC points.

Keep separate where appropriate:

```text
CL
UCL
LCL
acceptance minimum
acceptance maximum
```

---

# 41. QC Failure

A failed required QC check may trigger:

```text
FAIL
→ Operator alert
→ QC alert
→ Admin alert
→ QC_HOLD
→ corrective action
→ authorized release
```

Never automatically resume production after a failed QC.

---

# 42. Defects

Defect records should link to:

```text
productionRun
order
batch
machine
operator
timestamp
defect type
quantity
remarks
```

Use defect master data where available.

---

# 43. Downtime

Downtime records include:

```text
productionRun
machine
start
end
duration
reason
remarks
operator
```

Calculate duration from timestamps where possible.

---

# 44. Estimated vs Confirmed Output

Keep distinct:

```text
estimatedOutput
confirmedFinishedQuantity
```

Estimated output may use running time × machine rate.

Confirmed output comes from accepted carton/finished-good records.

Never display estimated output as exact accepted output.

---

# 45. Carton Completion

Carton records may include:

```text
cartonNumber
batchId
productionRunId
productId
quantity
weightConfirmed
appearancePassed
labelPassed
status
completedBy
completedAt
```

Auto-fill known context.

---

# 46. Finished Goods Transfer

Generate transfer details from existing production records.

Production should not re-enter:

- product,
- machine,
- operator,
- batch,
- cartons,
- known timestamps.

Store enters receipt-specific information.

---

# 47. Final QC

Finished goods cannot become Ready for Delivery if required final QC is not accepted.

Possible results:

```text
ACCEPT
REJECT
HOLD / MRB
```

---

# 48. Delivery

Delivery references:

```text
order
customer
approved finished goods
batch
cartons
quantity
```

Validate final QC acceptance before dispatch.

---

# 49. Inventory Model

Every stock movement requires a transaction.

Examples:

```text
GRN_RECEIPT
QC_RELEASE
QC_REJECTION
RESERVATION
MATERIAL_ISSUE
MATERIAL_RETURN
PRODUCTION_WIP
FINISHED_GOODS_RECEIPT
DELIVERY_OUT
STOCK_ADJUSTMENT
```

Do not directly overwrite stock totals without an auditable transaction.

---

# 50. Audit Trail

Audit at minimum:

```text
order create/update/cancel
schedule confirm/change
PO create/update
GRN create/update
QC decision
stock adjustment
material issue
production start/pause/resume/end
QC sample edit
defect edit
carton completion/edit
finished-goods receipt
final QC
delivery dispatch/complete
user/role change
```

Do not log secrets or passwords.

---

# 51. Notifications

Role-specific examples:

## Admin
- shortage,
- schedule conflict,
- QC failure,
- production delay,
- order at risk.

## QC
- incoming inspection,
- QC overdue,
- final QC pending.

## Purchasing
- material shortage,
- supplier/ETA issue.

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
- critical QC issue,
- production delay,
- delivery risk.

Avoid duplicate notifications on refresh.

---

# 52. Frontend Design Rules

Follow `design.md`.

The interface must be:

```text
professional
clean
corporate
industrial
minimal
data-first
easy to scan
```

Brand:

```text
EverDryPack Blue
EverDryPack Orange
White
Neutral Gray
```

Functional colors:

```text
Green = success
Amber = warning
Red = failure
Gray = inactive
```

---

# 53. UI Restrictions

Do not create:

- cartoon UI,
- emoji navigation,
- giant colorful cards,
- excessive gradients,
- unnecessary illustrations,
- random colors for each module,
- text-heavy operator screens.

Prefer:

- compact tables,
- clear forms,
- subtle cards,
- professional icons,
- clear status badges,
- simple charts,
- clear primary buttons.

---

# 54. React Component Rules

Keep components focused.

Good examples:

```text
OrderForm
OrderStatusTimeline
ScheduleRecommendation
InventoryTable
GrnForm
QcSampleForm
SpcChart
OperatorJobCard
ProductionControls
CompleteCartonModal
```

Do not build huge components that contain fetching, authorization, business logic, and many unrelated forms.

---

# 55. React Router

Use React Router DOM.

Do not build application navigation with a giant conditional switch in `App.jsx`.

Use protected and role-aware routes for UX.

Backend authorization remains authoritative.

---

# 56. Forms

Recommended:

```text
React Hook Form
Zod
```

Client validation is for UX.

Backend validation is mandatory.

---

# 57. Tables

Operational list pages should usually be table-based.

Support:

- pagination,
- search,
- filtering,
- status,
- row actions.

Do not retrieve unlimited records.

---

# 58. Charts

Recommended:

```text
Recharts
```

Appropriate for:

- production trend,
- SPC,
- reject breakdown,
- machine utilization,
- delivery trend.

Do not chart data that is clearer in a table.

---

# 59. Async UI

Every data screen should handle:

```text
loading
success
empty
error
```

Mutations should:

1. disable submit,
2. prevent duplicate calls,
3. display result,
4. invalidate/refetch relevant queries.

---

# 60. Express Error Handling

Use centralized error middleware.

Typical mapping:

```text
400 bad input/business validation
401 unauthenticated
403 unauthorized
404 not found
409 conflict/state conflict
500 unexpected server error
```

Do not return server stack traces to normal users.

---

# 61. Prisma Transactions

Use `$transaction` for critical multi-step changes such as:

- QC release + inventory movement,
- material issue + stock transaction,
- carton completion + confirmed finished quantity,
- FG receipt + stock increase,
- delivery + finished-goods deduction.

---

# 62. Historical Integrity

Avoid hard deleting:

```text
orders
production runs
QC records
inventory transactions
GRNs
finished goods
deliveries
audit logs
```

Use inactive/archive behavior for master data where needed.

---

# 63. Pagination / Performance

List APIs should support parameters such as:

```text
page
limit
search
status
dateFrom
dateTo
```

Avoid large unnecessary Prisma `include` trees.

---

# 64. Security

Never expose:

- database credentials,
- auth secrets,
- password hashes,
- server environment variables.

Browser-safe frontend variables may use:

```text
VITE_API_URL
```

Do not expose secrets via `VITE_*`.

---

# 65. CORS

Frontend and backend are separate applications.

Configure production CORS to allow the actual frontend origin.

If using auth cookies, configure:

- credentials,
- secure,
- sameSite,
- allowed origin

correctly.

Do not leave unrestricted production CORS.

---

# 66. Environment Examples

Frontend:

```text
frontend/.env.example

VITE_API_URL=
```

Backend:

```text
backend/.env.example

DATABASE_URL=
PORT=
FRONTEND_URL=
AUTH_SECRET=
```

Never commit real `.env` values.

---

# 67. Seed Data

Development seed data may include the six roles and two machines.

Clearly mark sample values as development data.

Do not let example product rates or QC limits silently become production defaults.

---

# 68. Testing Requirements

At minimum cover:

## Permissions

```text
Operator cannot create PO
Purchasing cannot release QC
Store cannot make QC decisions
QC cannot manage users
```

## Inventory

```text
quarantine excluded
released stock available
reservation reduces availability
material issue creates transaction
delivery decreases FG
```

## Production

```text
cannot start without issued material
machine conflicts prevented
valid pause/resume
```

## QC

```text
four required samples where configured
min/max/average
pass/fail
out-of-spec workflow
```

## Scheduling

```text
machine conflicts
material-ready constraint
Admin confirmation
```

---

# 69. Coding Style

Use modern readable JavaScript:

```text
const
async/await
small functions
descriptive names
clear error handling
```

Avoid clever one-liners when readability suffers.

Use project ESLint/Prettier configuration if available.

---

# 70. JavaScript Safety

Because TypeScript is intentionally not used:

- validate API inputs,
- validate important environment variables,
- never trust browser object shapes,
- use schemas for complex payloads,
- add tests around business logic,
- use JSDoc where it meaningfully improves clarity.

Do not use the absence of TypeScript as a reason to weaken validation.

---

# 71. Naming Convention

React:

```text
OrderForm.jsx
StatusBadge.jsx
OperatorJobCard.jsx
```

Hooks:

```text
useOrders.js
useInventory.js
```

Backend example:

```text
order.routes.js
order.controller.js
order.service.js
order.repository.js
order.validator.js
```

Use one convention consistently.

---

# 72. Avoid Duplicate Logic

Shared business functions may include:

```text
calculateMaterialRequirements()
calculateAvailableStock()
calculateQcResult()
canStartProduction()
getScheduleRecommendation()
generateDocumentNumber()
```

Authoritative versions belong on the backend.

---

# 73. State Transitions

Do not allow arbitrary status assignment from request bodies.

Validate allowed transitions.

Example:

```text
UNSCHEDULED
→ WAITING_MATERIAL / SCHEDULED

SCHEDULED
→ READY

READY
→ RUNNING

RUNNING
→ PAUSED / QC_HOLD / PRODUCTION_COMPLETE
```

---

# 74. Main Production Statuses

Recommended internal statuses:

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

UI uses readable labels.

---

# 75. Legacy Paper Forms

Digital workflow is primary.

Generate legacy forms from connected records where practical.

Examples:

- GRN,
- Production Performance,
- QC/SPC checklist,
- Material Request,
- Finished Goods Transfer,
- Final Inspection,
- Delivery record.

Do not create an isolated database model for every old paper sheet if the data already exists in connected entities.

---

# 76. Synology NAS

The NAS may later support:

- PDF archive,
- backup export,
- document storage,
- internal archival.

Do not use Synology as the primary application database for MVP.

---

# 77. Export

Official PDF/Excel exports should use authoritative system records.

Do not rely solely on scraping visible browser tables.

---

# 78. Accessibility

Maintain:

- labels,
- keyboard access,
- visible focus,
- contrast,
- readable statuses,
- accessible dialogs,
- tablet-friendly operator controls.

---

# 79. Responsive Priority

Primary:

```text
desktop
office laptop
production tablet
```

Mobile is secondary.

Do not shrink operator actions into tiny buttons.

---

# 80. Implementation Phases

## Phase 1 — Foundation

```text
Vite frontend
Express backend
Prisma + Neon
Authentication
6-role RBAC
Application shell
Users
Customers
Suppliers
Products
Materials
Machines
```

## Phase 2 — Orders & Scheduling

```text
Admin order entry
BOM calculation
Stock check
Shortage logic
Schedule recommendation
Admin confirmation
Machine schedule
```

## Phase 3 — Purchasing / Store / Receiving

```text
Material shortages
PO
Supplier ETA
GRN
Inventory transactions
Reservations
Material issue
```

## Phase 4 — QC

```text
Incoming QC
Release/rejection
Production QC
QC reminders
Sample entry
Calculations
SPC
Final QC
```

## Phase 5 — Production

```text
Operator My Job
Start/Pause/Resume/End
Defects
Downtime
Cartons
Progress
```

## Phase 6 — Finished Goods / Delivery

```text
FG transfer
Store receipt
FG inventory
Delivery workflow
```

## Phase 7 — Management

```text
MD dashboard
Reports
Alerts
Audit trail
PDF/Excel
UI polish
```

---

# 81. Do Not Build Everything at Once

Implement clean vertical slices.

Do not create every page/table/route in one giant untested change.

Prefer incremental phases that can be manually tested.

---

# 82. Agent Workflow Before Coding

For each significant task:

1. Read relevant PRD sections.
2. Read relevant `design.md`.
3. Read `AGENTS.md`.
4. Inspect existing frontend.
5. Inspect existing backend.
6. Inspect Prisma schema.
7. Inspect current routes/services.
8. Identify already-completed work.
9. Create a concise implementation plan.
10. Implement the smallest complete solution.

---

# 83. Implementation Sequence

Where appropriate:

```text
Prisma schema/migration
→ validator
→ service
→ repository/query
→ controller
→ route
→ authorization
→ frontend API client
→ React page/component
→ loading/error/empty state
→ tests
```

Business logic should not be buried in React.

---

# 84. Verification

Before finishing a change, inspect the actual scripts in `package.json`.

Run appropriate existing checks such as:

Frontend:

```bash
npm run lint
npm run build
npm test
```

Backend:

```bash
npm run lint
npm test
```

Prisma:

```bash
npx prisma validate
```

Do not claim checks passed unless they were actually run.

---

# 85. Definition of Done

A feature is complete when:

1. PRD business rules are followed.
2. `design.md` UX is followed.
3. backend permission enforcement exists.
4. backend input validation exists.
5. relevant writes are safe/transactional.
6. audit logging exists where required.
7. UI includes loading/error/empty states.
8. happy path works.
9. important failure path works.
10. configured lint/build/tests pass.
11. no TypeScript/Next.js was introduced.

---

# 86. End-of-Task Summary

After implementing, summarize:

```text
What changed
Files changed
API routes added/updated
Prisma migration/schema changes
Permissions added
Business rules implemented
Manual testing steps
Missing real company data
Known limitations
```

---

# 87. Unknown Company Values

Never guess real production values.

If unknown:

- create a configurable master-data field,
- use clearly marked development examples only if needed,
- note the required company confirmation.

Examples:

```text
machine rate
BOM quantity
carton quantity
weight limits
sample frequency
supplier lead time
```

---

# 88. Prohibited Behaviors

Do not:

- bypass incoming QC,
- release quarantine stock automatically,
- start production without issued materials,
- let Purchasing directly modify physical stock,
- let Store make QC quality decisions,
- give Operator Admin functions,
- silently reschedule production,
- treat estimated output as confirmed output,
- overwrite QC history silently,
- delete inventory transaction history,
- hard-code unknown company values,
- duplicate data unnecessarily,
- generate fake production data in live workflows,
- add TypeScript,
- move the app to Next.js,
- make the UI cartoonish.

---

# 89. Preferred Decision Rule

When two implementation choices are possible, prefer the one that:

1. follows the SOP,
2. reduces duplicate entry,
3. gives the correct role ownership,
4. preserves traceability,
5. keeps Operator use simple,
6. keeps authoritative business logic on the Express backend,
7. is easy to maintain,
8. can scale later.

---

# 90. Final Agent Principle

For every field ask:

> **Does the system already know this?**

If yes:

```text
Auto-fill it.
```

For every department handoff ask:

> **Does another role need to act on this?**

If yes:

```text
Create the status/task/notification automatically.
```

For every sensitive action ask:

> **Which role owns this decision?**

Enforce that ownership in Express middleware/services.

For every paper form ask:

> **Can the form be generated from existing connected records?**

If yes:

```text
Generate it instead of asking users to type it again.
```

The objective is to build a connected manufacturing workflow using:

```text
Vite + React.js + JavaScript
Node.js + Express.js + JavaScript
Prisma + PostgreSQL / Neon
```

while preserving the EverDryPack SOP and keeping the system simple for daily factory use.
