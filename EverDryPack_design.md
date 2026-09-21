# EverDryPack Operations System
## UI / UX Design Specification

**Company:** Ever Drypack Malaysia Sdn. Bhd.  
**Product:** EverDryPack Production & Inventory Management System  
**Document:** `design.md`  
**Version:** 2.0  
**Status:** MVP Design Specification

---

# 1. Purpose

This document defines the visual design, UX rules, page layouts, role-specific interfaces, reusable components, frontend structure, API interaction pattern, and responsive behavior for the EverDryPack Operations System.

This version uses the latest project stack:

```text
Frontend
- Vite
- React.js
- JavaScript
- Tailwind CSS
- React Router DOM

Backend
- Node.js
- Express.js
- JavaScript

Database
- PostgreSQL
- Neon PostgreSQL

ORM
- Prisma

Architecture
- Separate frontend and backend
- REST API
- Role-based access control
```

**Do not introduce TypeScript.**

The system must preserve the current EverDryPack SOP while making the workflow faster, clearer, and easier to trace digitally.

---

# 2. Product Design Goal

EverDryPack should feel like a professional internal manufacturing operations platform.

It should not feel like:

- a generic admin-template dashboard,
- a cartoon-style factory app,
- a collection of scanned paper forms,
- or an unnecessarily complex ERP.

The visual language should communicate:

- reliability,
- manufacturing discipline,
- quality,
- traceability,
- operational clarity,
- and professionalism.

Every screen should answer:

> **What does this user need to know and do right now?**

---

# 3. Core UX Principle

The core design rule is:

> **Enter information once, reuse it everywhere, and automatically send each department only the information they need.**

The connected workflow is:

```text
Admin Order Entry
        ↓
Material Requirement
        ↓
Store Availability Check
        ↓
Purchasing if Shortage
        ↓
Receiving / GRN
        ↓
Incoming QC
        ↓
Released Inventory
        ↓
Production Scheduling
        ↓
Material Issue
        ↓
Production
        ↓
In-Process QC / SPC
        ↓
Finished Goods
        ↓
Final QC
        ↓
Delivery
        ↓
Reports / MD Dashboard
```

The UI must represent this as one connected workflow, not as unrelated digital forms.

---

# 4. User Roles

The MVP contains six roles:

```text
MD
Admin
QC
Operator
Purchasing
Store
```

Each role uses the same application but sees different navigation, actions, and data based on permissions.

---

# 5. Role UX

## 5.1 MD

Primary goal:

> Understand the state of the factory quickly.

MD should mainly see:

- current production,
- orders at risk,
- machine status,
- material shortages,
- QC issues,
- finished goods,
- deliveries,
- reports and trends.

MD is mostly read-only.

## 5.2 Admin

Primary goal:

> Turn customer requirements into a workable production plan.

Admin should mainly use:

- orders,
- production schedule,
- customers,
- products,
- machines,
- users,
- master data,
- reports.

Admin owns production scheduling.

## 5.3 QC

Primary goal:

> See what requires inspection and make quality decisions accurately.

QC should mainly use:

- incoming QC,
- in-process QC,
- SPC,
- final QC,
- calibration,
- QC history.

## 5.4 Operator

Primary goal:

> Run the assigned production job with as few actions as possible.

Operator should mainly use:

- My Job,
- Start Production,
- QC Check,
- Record Defect,
- Pause / Downtime,
- Complete Carton,
- End Production.

Operator UI must be the simplest interface in the system.

## 5.5 Purchasing

Primary goal:

> Know what materials are missing and when they are required.

Purchasing should mainly use:

- shortages,
- purchase orders,
- suppliers,
- supplier ETA.

## 5.6 Store

Primary goal:

> Control physical stock movement accurately.

Store should mainly use:

- receiving,
- GRN,
- inventory,
- material issue,
- material requests,
- finished-goods receipt,
- delivery preparation.

---

# 6. Brand Direction

Use the official EverDryPack logo in:

- login,
- app header,
- printable forms,
- PDF reports.

Do not repeat large logos inside ordinary page content.

The application theme should be derived from the EverDryPack blue and orange branding.

---

# 7. Color System

## EverDryPack Blue

Primary use:

- navigation,
- main buttons,
- selected tabs,
- links,
- chart primary series,
- focus states.

```css
--edp-blue-950: #08244A;
--edp-blue-900: #0B2F66;
--edp-blue-800: #0D4FA3;
--edp-blue-700: #1264C7;
--edp-blue-600: #1677E8;
--edp-blue-100: #EAF3FF;
--edp-blue-50: #F5F9FF;
```

## EverDryPack Orange

Secondary accent:

- purchasing emphasis,
- warning highlights,
- branded secondary actions.

```css
--edp-orange-700: #D85A00;
--edp-orange-600: #F36C0D;
--edp-orange-500: #FF7A18;
--edp-orange-100: #FFF1E7;
--edp-orange-50: #FFF8F3;
```

## Functional Colors

Green:

```text
Running
Ready
Pass
Accepted
Released
Completed
Delivered
```

Red:

```text
Rejected
Failed QC
Critical
Stopped
Overdue
```

Amber:

```text
Pending
Waiting Material
At Risk
Pending QC
Due Soon
```

Gray:

```text
Draft
Idle
Disabled
Cancelled
Not Applicable
```

---

# 8. Typography

Recommended:

```text
Inter
or system-ui
```

Suggested scale:

```text
Page Title          28–30px / 700
Section Title       18–20px / 600
Card Title          14–16px / 600
Body                14px / 400
Table Text          13–14px / 400
Secondary Text      12–13px / 400
Metric Value        28–34px / 700
Status Badge        12px / 600
```

Avoid oversized headings.

---

# 9. App Shell

Desktop layout:

```text
┌──────────────────────────────────────────────────────────────┐
│ Logo / Brand      Search       Alerts             User      │
├──────────────┬───────────────────────────────────────────────┤
│              │                                               │
│ Sidebar      │               Page Content                    │
│              │                                               │
│              │                                               │
└──────────────┴───────────────────────────────────────────────┘
```

Recommended:

```text
Top bar: 64–72px
Sidebar: 210–224px
Max content width: 1440px
Page padding: 24–32px
Card gap: 16px
Section gap: 24px
```

---

# 10. Top Navigation

Contains:

- EverDryPack logo,
- company name,
- global search,
- notifications,
- date,
- user name,
- role,
- profile menu.

Search placeholder:

```text
Search order, batch, material, supplier...
```

Search may support:

- order number,
- batch,
- GRN,
- material,
- product,
- supplier,
- customer,
- carton number.

---

# 11. Sidebar Navigation

Use a clean light sidebar with small professional icons.

## Admin

```text
Dashboard
Orders
Schedule
Purchase
Store
Receiving
QC
Production
Finished Goods
Delivery
Reports

Administration
Users
Master Data
Settings
```

## MD

```text
Dashboard
Orders
Schedule
Inventory
QC
Live Production
Finished Goods
Delivery
Reports
```

## QC

```text
Dashboard
Incoming QC
Production QC
SPC
Final QC
Calibration
QC History
```

## Operator

```text
My Job
Production
QC Check
Defects
Downtime
Cartons
History
```

## Purchasing

```text
Dashboard
Material Shortages
Purchase Orders
Suppliers
Supplier ETA
```

## Store

```text
Dashboard
Receiving
Inventory
Material Issue
Material Requests
Finished Goods
Delivery
```

---

# 12. General Page Pattern

Each page should follow:

```text
Page Title
Short explanation                               Primary Action
```

Example:

```text
Orders
Create and manage production requirements.       + New Order
```

Avoid large hero sections.

---

# 13. Cards

Use cards only for meaningful grouping.

```css
background: #FFFFFF;
border: 1px solid #E5E7EB;
border-radius: 8px;
box-shadow: 0 1px 2px rgba(0,0,0,0.03);
```

Avoid heavy gradients, large shadows, and overly rounded cards.

---

# 14. KPI Cards

Example:

```text
QC Pass Rate
98.6%
↑ 0.8% vs last week
```

Use:

- small icon,
- short label,
- large value,
- small supporting trend/status.

No paragraphs inside KPI cards.

---

# 15. Status Badge

Use compact pills:

```text
● Running
● Scheduled
● Waiting Material
● Pending QC
● Passed
● Rejected
● Ready
● Delivered
```

Status must include text and not depend on color alone.

---

# 16. Tables

Transactional records should be table-first.

Use tables for:

- orders,
- purchase orders,
- GRNs,
- inventory,
- QC records,
- production runs,
- finished goods,
- deliveries,
- users.

Features:

- search,
- filtering,
- pagination,
- sortable important columns,
- status badge,
- row action menu.

Recommended row height:

```text
44–48px
```

---

# 17. Forms

Pattern:

```text
Label *
[ Input ]
Helper / error text
```

Do not use placeholder text as the only label.

Footer pattern:

```text
Cancel     Save Draft     Primary Action
```

---

# 18. Buttons

## Primary

Blue:

```text
Create Order
Submit Inspection
Issue Materials
Generate Report
```

## Success

Green where appropriate:

```text
Start Production
Accept
Release
```

## Warning

Orange:

```text
Pause Production
```

## Danger

Red:

```text
Reject
Stop Production
Cancel Order
```

## Secondary

White with border:

```text
View Details
Cancel
Clear
Print
```

---

# 19. Dashboard

Dashboard should answer:

> What is happening today?

Top metrics:

```text
Orders Today
Orders At Risk
Machines Running
QC Pass Rate
Finished Cartons
Materials Low
Deliveries Due
```

Use maximum 6–7 primary KPI cards.

Main panels:

### Today's Schedule

```text
Time
Order
Product
Machine
Quantity
Status
```

### Machine Status

```text
Machine
Product
Operator
Status
Progress
```

### Inventory Risk

```text
Material
Available
Required
Status
```

### Alerts / Tasks

```text
Type
Message
Time
Status
```

### Production Trend

Simple 7-day chart.

---

# 20. MD Dashboard

Focus on management decisions.

Top:

```text
Production Today
Orders At Risk
Machines Running
Critical Shortages
QC Failures
Deliveries Due
```

Main panels:

```text
Production vs Plan
Delayed Orders
Material Risks
QC Alerts
Delivery Performance
```

No operational entry forms.

---

# 21. Orders Page

Admin main table:

```text
Order No.
Customer
Product
Quantity
Required Date
Material Status
Schedule Status
Production Status
Delivery Status
Actions
```

Header controls:

```text
Search
Status
Date
+ New Order
```

---

# 22. New Order

Fields:

```text
Customer *
Customer PO / Reference *
Product *
Quantity *
Unit *
Required Delivery Date *
Special Requirements
Notes
```

After selecting product, show compact product information:

```text
Pack Weight
Pcs / Carton
Compatible Machines
Standard Rate
QC Frequency
```

Buttons:

```text
Cancel
Save Draft
Create Order
```

---

# 23. Order Detail

Header:

```text
SO-20260921-001
ABC Sdn. Bhd.
Silica Gel 1g
```

Status flow:

```text
Order
→ Materials
→ Schedule
→ Production
→ Final QC
→ Delivery
```

Tabs:

```text
Overview
Materials
Schedule
Production
QC
Finished Goods
Delivery
Audit
```

---

# 24. Schedule Page

Top metrics:

```text
Unscheduled
Scheduled
Waiting Material
At Risk
```

The system recommends a schedule.
Admin confirms it.

---

# 25. Schedule Recommendation

Example:

```text
Recommended Machine       Machine 01
Estimated Runtime         8h 30m
Material Ready            Yes
Earliest Start            22 Sep 08:30
Estimated Completion      22 Sep 17:00
Required Delivery         25 Sep
Delivery Risk             Low
```

Actions:

```text
Change Machine
Change Start
Confirm Schedule
```

Never silently reschedule an order.

---

# 26. Machine Calendar

Weekly schedule:

```text
             Mon    Tue    Wed    Thu    Fri
Machine 01   [job]  [job]  ...
Machine 02   [job]  ...
```

Production block:

```text
SO-260921-01
Silica Gel 1g
08:30–17:00
```

Colors:

```text
Blue   Scheduled
Green  Running / Complete
Amber  Waiting / At Risk
Gray   Maintenance
```

For MVP, Edit Schedule forms are sufficient; drag-and-drop is optional later.

---

# 27. Purchasing Dashboard

KPIs:

```text
Open POs
Material Shortages
Arriving Today
Delayed Supplier Orders
```

Shortage table:

```text
Material
Required
Available
Shortage
Needed By
Affected Order
Priority
```

Primary action:

```text
Create Purchase Order
```

---

# 28. Purchase Order Form

```text
Supplier *
Material *
Quantity *
Unit *
Expected Arrival *
Remarks
```

When opened from a shortage, prefill material, shortage quantity, required-by date, and linked order.

---

# 29. Supplier ETA

```text
PO No.
Supplier
Material
Quantity
ETA
Status
Affected Production
```

Statuses:

```text
Draft
Ordered
Confirmed
In Transit
Arriving Today
Received
Delayed
Cancelled
```

---

# 30. Store Dashboard

KPIs:

```text
Available Stock
Reserved
Quarantine
WIP
Low Stock
```

Main panels:

```text
Current Inventory
Material Requests
Materials to Issue
Finished Goods to Receive
```

---

# 31. Inventory

Table:

```text
Code
Material
Category
On Hand
Reserved
Quarantine
Available
Unit
Location
Status
```

Only released stock counts as available for production.

---

# 32. Issue Materials

Store selects a production order.

Auto-load:

```text
Product
Planned Quantity
Machine
Production Date
Required Materials
```

Table:

```text
Material
Required
Reserved
On Hand
Issue Qty
Unit
```

Action:

```text
Issue Materials
```

---

# 33. Receiving / GRN

Header:

```text
Goods Received Note (GRN)
Record incoming supplier materials.
```

Fields:

```text
Supplier *
DO / Invoice No. *
Date *
GRN No. [automatic]
```

Workflow indicator:

```text
Store Received
→ Pending QC
→ Released
```

---

# 34. GRN Items

```text
Material
Qty Received
Damaged / Rejected
Physically Accepted
Unit
Remark
```

Actions:

```text
Save Draft
Submit to QC
Print GRN
```

Store does not perform final QC release.

---

# 35. Incoming QC

KPIs:

```text
Pending Inspection
Passed Today
Rejected Today
Released Stock
```

Inspection queue should be visually dominant.

---

# 36. Incoming QC Form

Auto-load:

```text
GRN
Supplier
Material
Quantity
```

QC enters:

```text
Sample Method
Moisture %
Appearance
Remarks
```

Display the configured specification prominently.

Decision:

```text
PASS
REJECT
HOLD / MRB
```

---

# 37. Operator Home

This screen must be simpler than every management screen.

No purchase information.
No dense tables.
No unrelated reports.

Header:

```text
My Production
Operator: Ahmad
Machine 01
```

Job panel:

```text
READY

SO-260921-01
Silica Gel 1g

Target
50,000 pcs

Materials
READY

Scheduled Start
08:30
```

Primary action:

```text
START PRODUCTION
```

---

# 38. Operator Running Screen

```text
RUNNING

Machine 01
Silica Gel 1g
SO-260921-01

Confirmed Output
13,200 / 50,000

Completed Cartons
2 / 10

Recorded Defects
128 pcs

Next QC
12 min
```

Primary actions:

```text
QC CHECK
RECORD DEFECT
PAUSE / DOWNTIME
COMPLETE CARTON
END PRODUCTION
```

Buttons should be large and touch-friendly.

---

# 39. Operator State Behavior

## Ready

Only Start Production is enabled.

## Running

Enable:

```text
QC Check
Record Defect
Pause
Complete Carton
End Production
```

## Paused

Show:

```text
Production Paused
Duration
Reason
```

Actions:

```text
Resume Production
End Production
```

## QC Hold

Show clear red/amber hold banner.
Resume is disabled until release.

## Complete

Show:

```text
Production Complete
Finished cartons awaiting Store receipt.
```

---

# 40. Production QC / SPC

QC form:

```text
Batch
Product
Machine
Time

W1
W2
W3
W4

Appearance
Dimension
Sealing
Remarks
```

Action:

```text
Submit Check
```

---

# 41. QC Result

Display automatic calculations:

```text
Specification      0.90 – 1.20 g
Minimum            0.98
Maximum            1.05
Average            1.01
Result             PASS
```

Users should not manually calculate these values.

---

# 42. SPC Chart

Use a clean line chart:

```text
Blue solid line     Sample / Average
Green dashed line   CL
Red dashed line     UCL / LCL
```

No decorative backgrounds.

---

# 43. QC Reminder

Operator should always see:

```text
Next QC Check
12 minutes
Due 10:40 AM
```

When due:

```text
QC CHECK DUE
```

When overdue, escalate visual urgency.

---

# 44. QC Failure

Use an inline alert:

```text
OUT OF SPECIFICATION

Sample W2 = 0.87 g
Lower Limit = 0.90 g

Production has been placed on QC Hold.

View Details
```

Avoid unnecessary modal interruptions.

---

# 45. Defect Entry

Short modal/side panel:

```text
Defect Type *
Quantity *
Remarks
```

Action:

```text
Save Defect
```

Defect types come from master data.

---

# 46. Downtime Entry

When operator pauses:

```text
Reason *
Start Time [automatic]
Remarks
```

After save:

```text
PAUSED
00:07:25
Film Jam
```

Action:

```text
Resume Production
```

---

# 47. Complete Carton

Auto-load:

```text
Carton No.
Batch
Product
Machine
Operator
```

Confirm:

```text
Quantity
Weight
Appearance
Label
Remarks
```

Action:

```text
Complete Carton
```

---

# 48. Finished Goods

KPIs:

```text
Finished Goods
Awaiting Store Receipt
Awaiting Final QC
Ready for Delivery
Delivered Today
```

Table:

```text
Batch
Product
Cartons
Confirmed Qty
Store Status
Final QC
Delivery Status
Completed At
```

---

# 49. Finished Goods Transfer

Generate transfer data automatically from production records.

Production should not retype:

- machine,
- operator,
- product,
- batch,
- quantity,
- completion date/time.

Store confirms:

```text
Received Cartons
Received Quantity
Remarks
```

Action:

```text
Confirm Receipt
```

---

# 50. Final QC

Queue:

```text
Batch
Product
Quantity
Store Receipt Date
Status
Action
```

Inspection:

```text
Appearance
Weight
Dimension
Sealing
Sampling Plan
Major Defects
Minor Defects
Remarks
```

Decision:

```text
Accept
Reject
Hold / MRB
```

Only accepted finished goods become Ready for Delivery.

---

# 51. Delivery

KPIs:

```text
Ready
Scheduled
Out for Delivery
Delivered Today
```

Queue:

```text
Order
Customer
Product
Cartons
Quantity
Required Date
Delivery Date
Status
Actions
```

Actions:

```text
Prepare
Mark Ready
Dispatch
Mark Delivered
Print Slip
```

---

# 52. Reports

Filters:

```text
Date Range
Product
Machine
Customer
Report Type
```

Main metrics:

```text
Total Production
QC Pass Rate
Total Rejects
On-Time Delivery
Machine Utilization
```

Charts:

```text
Daily Production
QC Trend
Reject Breakdown
Delivery Trend
Machine Utilization
Material Consumption
```

Exports:

```text
PDF
Excel / CSV
Print
```

---

# 53. Calibration

Table:

```text
Equipment
Serial No.
Location
Last Calibration
Next Due
Allowable Error
Frequency
Status
```

Statuses:

```text
Active
Due Soon
Overdue
Out of Service
```

---

# 54. Notifications

Group notifications by importance:

```text
Critical
Warning
Information
```

Examples:

```text
QC failure on Machine 01
Packing Roll below reorder level
GRN-260921-03 released by QC
Final QC pending for Batch B-260921-01
```

Avoid duplicate notifications on every refresh.

---

# 55. Empty, Loading, and Error States

## Empty

```text
No pending inspections.
All received materials have been inspected.
```

## Loading

Use skeletons and button loading states.

```text
Create Order → Creating...
```

## Error

```text
Could not save this record.
Your changes were not submitted.

Try Again
```

Never silently fail.

---

# 56. Confirmation Dialogs

Use confirmation only for high-impact actions:

- reject material,
- cancel order,
- end production,
- stock adjustment,
- delete draft.

Do not ask for confirmation for routine navigation.

---

# 57. Responsive Design

## Desktop

Primary office environment.

- sidebar visible,
- full data tables,
- multi-column layout.

## Tablet

Important for production.

- collapsible sidebar,
- larger operator buttons,
- two-column layouts where suitable,
- local horizontal table scrolling.

## Mobile

Secondary.

- navigation drawer,
- stacked cards,
- simplified tables where possible.

Operator controls must remain usable.

---

# 58. Accessibility

Minimum requirements:

- semantic HTML,
- keyboard navigation,
- visible focus,
- properly labeled inputs,
- sufficient contrast,
- status text in addition to color,
- touch targets around 44px,
- accessible modal focus management.

---

# 59. Frontend Stack

Use:

```text
Vite
React.js
JavaScript
Tailwind CSS
React Router DOM
TanStack Query
React Hook Form
Zod (optional but recommended)
Recharts
Lucide React
```

No TypeScript files should be introduced unless the project decision changes explicitly.

---

# 60. Frontend Folder Structure

```text
frontend/
├─ src/
│  ├─ api/
│  │  ├─ apiClient.js
│  │  ├─ authApi.js
│  │  ├─ ordersApi.js
│  │  ├─ purchaseApi.js
│  │  ├─ inventoryApi.js
│  │  ├─ receivingApi.js
│  │  ├─ qcApi.js
│  │  ├─ productionApi.js
│  │  ├─ finishedGoodsApi.js
│  │  └─ deliveryApi.js
│  │
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
│  │
│  ├─ pages/
│  ├─ hooks/
│  ├─ context/
│  ├─ constants/
│  ├─ utils/
│  ├─ routes/
│  ├─ App.jsx
│  └─ main.jsx
│
├─ public/
└─ package.json
```

---

# 61. Reusable React Components

Common:

```text
Button.jsx
Input.jsx
Select.jsx
Textarea.jsx
Modal.jsx
ConfirmDialog.jsx
DataTable.jsx
StatusBadge.jsx
MetricCard.jsx
EmptyState.jsx
AlertBanner.jsx
SearchBar.jsx
FilterBar.jsx
Pagination.jsx
PageHeader.jsx
```

Domain components:

```text
orders/
  OrderForm.jsx
  OrderSummary.jsx
  OrderStatusTimeline.jsx

schedule/
  ScheduleRecommendation.jsx
  MachineSchedule.jsx

store/
  InventoryTable.jsx
  MaterialIssuePanel.jsx

receiving/
  GrnForm.jsx
  GrnItemsTable.jsx
  GrnWorkflow.jsx

qc/
  IncomingInspectionForm.jsx
  QcSampleForm.jsx
  QcResultPanel.jsx
  SpcChart.jsx
  FinalQcForm.jsx

production/
  OperatorJobCard.jsx
  ProductionControls.jsx
  ProductionProgress.jsx
  DefectModal.jsx
  DowntimeModal.jsx
  CompleteCartonModal.jsx
```

---

# 62. React Router Structure

```text
/login
/dashboard

/orders
/orders/new
/orders/:id

/schedule

/purchase
/purchase/orders
/purchase/orders/:id
/purchase/suppliers

/store
/store/inventory
/store/material-requests
/store/material-issue

/receiving
/receiving/grn/new
/receiving/grn/:id

/qc/incoming
/qc/incoming/:id
/qc/production
/qc/spc
/qc/final
/qc/calibration

/production
/production/my-job
/production/runs/:id
/production/history

/finished-goods
/finished-goods/transfers
/finished-goods/:batchId

/delivery
/delivery/:id

/reports

/admin/users
/admin/customers
/admin/products
/admin/materials
/admin/machines
/admin/settings
```

---

# 63. API Client Design

Centralize HTTP calls in:

```text
src/api/apiClient.js
```

Responsibilities:

- API base URL,
- credentials/cookies,
- JSON headers,
- shared error handling,
- authentication handling.

Do not call raw backend URLs throughout components.

---

# 64. Server State

Recommended:

```text
TanStack Query
```

Use for:

- list fetching,
- detail fetching,
- caching,
- mutation status,
- query invalidation,
- refetch after changes.

Examples:

```text
useOrders()
useOrder(id)
useInventory()
usePendingQc()
useProductionRun(id)
```

Do not duplicate server data unnecessarily in React Context.

---

# 65. Local State

Use normal React state for:

- modal open/close,
- selected table rows,
- temporary filters,
- form interaction state,
- operator countdown display.

Context may hold:

- logged-in user,
- user role,
- lightweight UI preferences.

---

# 66. Backend Stack

```text
Node.js
Express.js
JavaScript
Prisma ORM
PostgreSQL / Neon
```

Backend owns authoritative:

- authentication,
- authorization,
- validation,
- business logic,
- scheduling logic,
- inventory transactions,
- QC rules,
- audit logs.

Do not place authoritative business rules only in React.

---

# 67. Backend Folder Structure

```text
backend/
├─ src/
│  ├─ config/
│  ├─ controllers/
│  ├─ routes/
│  ├─ services/
│  ├─ repositories/
│  ├─ middleware/
│  ├─ validators/
│  ├─ constants/
│  ├─ utils/
│  ├─ jobs/
│  ├─ app.js
│  └─ server.js
│
├─ prisma/
│  ├─ schema.prisma
│  ├─ migrations/
│  └─ seed.js
│
└─ package.json
```

---

# 68. Backend Responsibility Pattern

## Route

Defines:

- URL,
- HTTP method,
- auth middleware,
- permission middleware,
- validator,
- controller.

Example concept:

```js
router.post(
  "/orders",
  authenticate,
  authorize("ADMIN"),
  validate(createOrderSchema),
  orderController.create
);
```

## Controller

Responsible for:

- reading request,
- calling service,
- returning response.

Do not place complex workflow logic in controllers.

## Service

Contains domain/business behavior.

Examples:

```text
orderService
scheduleService
inventoryService
purchaseService
qcService
productionService
finishedGoodsService
deliveryService
auditService
notificationService
```

## Repository

Use for reusable Prisma query logic where helpful.

Do not add abstraction purely for ceremony.

---

# 69. REST API Design

Base:

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

GET    /api/reports/production
```

---

# 70. API Response Pattern

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

Validation:

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {
    "quantity": "Quantity must be greater than 0."
  }
}
```

---

# 71. Authentication UX

Login screen:

- EverDryPack logo,
- username/email,
- password,
- sign-in button,
- minimal company branding.

Recommended security pattern:

- Express authenticates,
- secure HttpOnly cookie/session or secure token strategy,
- current user and role returned to frontend,
- Express middleware enforces authorization.

Frontend route hiding is UX only, not security.

---

# 72. Frontend Route Guard

Use components such as:

```text
ProtectedRoute
RoleRoute
```

Example:

```text
Operator opens /purchase
→ redirect to /production/my-job
```

But backend must still reject unauthorized API calls with `403`.

---

# 73. Database / Prisma Model Direction

Main entities:

```text
User
Role
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
GoodsReceived
IncomingQcInspection
ProductionQcCheck
ProductionQcSample
FinalQcInspection
Defect
DowntimeRecord
MaterialRequest
MaterialIssue
InventoryTransaction
InventoryReservation
FinishedGoodsTransfer
Delivery
Notification
AuditLog
Calibration
```

---

# 74. Inventory Design Rule

Inventory is transaction-based.

Frontend displays current balance.
Backend records every movement.

Important states:

```text
Available
Reserved
Quarantine
Rejected
WIP
Finished Goods
```

Received material must not become Available until QC release.

---

# 75. Scheduling Design Rule

Backend calculates recommendation using:

```text
Required Delivery Date
Product
Quantity
Compatible Machines
Machine Availability
Standard Rate
Material Readiness
Supplier ETA
Maintenance
Changeover Time
```

Frontend only displays and allows Admin to confirm/change the recommendation.

---

# 76. QC Timer Design

Backend should provide:

```text
lastQcAt
nextQcDueAt
qcFrequencyMinutes
```

Frontend displays a live countdown.

The browser timer is visual only.
Backend timestamps remain authoritative.

---

# 77. Data Formatting

Use consistent units:

```text
50,000 pcs
100.5 kg
1.02 g
12 cartons
98.6%
```

Avoid unnecessary decimal places.

Dates:

```text
21 Sep 2026
08:30 AM
21 Sep 2026, 08:30 AM
```

---

# 78. Forms and API Mutations

For every submit action:

1. validate client-side,
2. disable button,
3. show loading text,
4. send request,
5. handle API validation,
6. show success/error feedback,
7. invalidate/refetch affected queries.

Example:

```text
Issue Materials
→ Issuing...
→ Materials Issued
```

---

# 79. Validation

Recommended frontend:

```text
React Hook Form
Zod
```

Backend validation is mandatory.

Do not trust client validation for:

- permissions,
- status transitions,
- inventory quantities,
- QC result rules,
- schedule conflicts.

---

# 80. Audit UI

Record details should show:

```text
Created By
Created At
Last Updated By
Last Updated At
```

Audit table:

```text
Date & Time
User
Role
Action
Old Value
New Value
```

---

# 81. Print / PDF UI

Printable records should include:

- EverDryPack logo,
- company name,
- document title,
- document number,
- date,
- record data,
- approval metadata,
- revision/page metadata if required.

Do not print app navigation or action buttons.

Digital workflow remains the source of truth; printable paper-style forms are outputs.

---

# 82. UI Libraries

Preferred:

```text
Tailwind CSS
shadcn/ui configured for JavaScript
or Radix UI primitives
Lucide React
Recharts
```

Do not introduce TypeScript just because a component library defaults to it.

---

# 83. Master Data Design

Admin Master Data can use tabs:

```text
Products
Materials
Machines
Customers
Suppliers
```

Each uses:

```text
Search
Filters
Add New
Table
```

---

# 84. Product Master

Fields:

```text
Product Code
Product Name
Product Type
Packet Weight
Unit
Pcs / Carton
Compatible Machines
Standard Rate
QC Frequency
Active
```

Detail tabs:

```text
General
Materials / BOM
QC Specification
Machine Settings
History
```

---

# 85. Material Master

```text
Material Code
Material Name
Category
Unit
Storage Location
Reorder Level
QC Required
Primary Supplier
Active
```

---

# 86. Machine Master

```text
Machine Code
Machine Name
Location
Compatible Products
Standard Rate
Status
```

Statuses:

```text
Available
Running
Maintenance
Out of Service
```

---

# 87. User Management

Table:

```text
Name
Username / Email
Role
Status
Last Login
Actions
```

Roles:

```text
MD
Admin
QC
Operator
Purchasing
Store
```

---

# 88. Settings

Recommended sections:

```text
Company
Production
QC
Inventory
Notifications
Document Numbering
Users & Roles
Backup
```

Company-specific values must be configurable rather than scattered through code.

---

# 89. Professional UI Rules

Always:

- prioritize operational data,
- use compact spacing,
- use professional icons,
- use consistent statuses,
- align forms cleanly,
- keep tables readable,
- make the primary action obvious.

Never:

- use emoji as interface icons,
- use cartoon illustrations,
- use rainbow module colors,
- fill screens with decorative cards,
- use excessive gradients,
- put long paragraphs in operational screens.

---

# 90. Design Acceptance Criteria

The interface is acceptable when:

1. Operator can identify the next action in under five seconds.
2. Admin can create and schedule an order without repeated data entry.
3. Store can identify material to receive or issue quickly.
4. Purchasing can identify shortages and due dates immediately.
5. QC can see pending inspections immediately.
6. MD can identify major risks from one dashboard.
7. Status colors are consistent across modules.
8. Tables are readable on normal office laptops.
9. Operator controls are usable on production tablets.
10. The UI looks corporate and professional, not cartoonish.

---

# 91. Implementation Priority

## Phase 1 — Foundation

```text
Vite React frontend
Express backend
Neon + Prisma
Authentication
RBAC
App shell
Users
Master data
```

## Phase 2 — Orders & Scheduling

```text
Order entry
BOM requirement
Material check
Schedule recommendation
Admin confirmation
Machine calendar
```

## Phase 3 — Purchasing / Store / Receiving

```text
Shortages
Purchase Orders
Supplier ETA
GRN
Inventory
Reservations
Material Issue
```

## Phase 4 — QC

```text
Incoming QC
QC Release
Production QC
QC Reminders
SPC
Final QC
```

## Phase 5 — Production

```text
Operator Job
Start/Pause/Resume/End
Defects
Downtime
Cartons
Progress
```

## Phase 6 — Finished Goods / Delivery

```text
FG Transfer
Store Receipt
Finished Goods Inventory
Delivery Queue
Delivery Status
```

## Phase 7 — Management

```text
MD Dashboard
Reports
Audit Log
PDF/Excel Export
Final UI Polish
```

---

# 92. Final Design Rule

Every page should prioritize:

```text
STATUS
→ REQUIRED ACTION
→ IMPORTANT DATA
→ HISTORY
```

Before adding a field, button, card, or page, ask:

> Does this role actually need this information to perform the task?

If no:

**Do not show it.**

Before asking a user to type a value, ask:

> Does the system already know this value from another connected record?

If yes:

**Auto-fill it.**

The EverDryPack application should make the real factory workflow simpler than the paper process while preserving all required SOP controls.
