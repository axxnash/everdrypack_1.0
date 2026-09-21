# EverDryPack Production & Inventory Management System
## Product Requirements Document (PRD)

**Company:** Ever Drypack Malaysia Sdn. Bhd.  
**Document:** Product Requirements Document  
**Product Name:** EverDryPack Operations System  
**Version:** 1.0  
**Status:** MVP Definition  
**Primary Use Case:** Digitise the existing production, QC, receiving, store, purchasing, finished-goods and delivery workflow while preserving the company's approved SOP.

---

# 1. Product Vision

EverDryPack Operations System is a web-based internal manufacturing operations platform for Ever Drypack Malaysia Sdn. Bhd.

The system replaces the company's paper-based forms with connected digital workflows while continuing to follow the existing Standard Operation Procedure for silica gel desiccant production.

The core design principle is:

> **Enter data once, reuse it everywhere, and automatically send each department only the information they need.**

The application must connect:

**Customer Order → Production Planning → Material Check → Purchasing → Receiving → Incoming QC → Store → Production → In-Process QC/SPC → Finished Goods → Final QC → Delivery → Reports**

The system should be simple enough for operators to use on the production floor, while still giving management complete visibility and traceability.

---

# 2. Business Objectives

The system must:

1. Digitise existing paper forms.
2. Follow the current approved SOP instead of replacing it.
3. Reduce repeated manual data entry.
4. Improve production scheduling.
5. Give every department the correct information automatically.
6. Provide real-time production visibility.
7. Track material availability before production begins.
8. Track incoming material inspection and release status.
9. Digitise the 20-minute QC/SPC production checks.
10. Track defects, rejects and downtime.
11. Track completed cartons and finished goods.
12. Improve delivery readiness visibility.
13. Provide management reports and dashboards.
14. Maintain a complete audit trail.
15. Allow the system to scale later without redesigning the full application.

---

# 3. Current SOP That Must Be Preserved

The software workflow must follow the existing Ever Drypack production flow:

1. **Receiving**
2. **Incoming Inspection**
3. **Packing of Silica Gel Desiccant / Production**
4. **Packing of Finished Goods**
5. **Finished Goods Store**
6. **Final Inspection**
7. **Delivery**

The software may simplify data entry, automate calculations, automate notifications and connect departments, but it must not bypass these process stages.

---

# 4. User Roles

The MVP has **6 user roles**.

## 4.1 Managing Director (MD)

Primary purpose: monitor and make management decisions.

MD can:

- View management dashboard.
- View live production status.
- View production schedule.
- View material shortages.
- View inventory.
- View QC status and alerts.
- View finished goods.
- View delivery readiness.
- View reports and analytics.
- View delayed / at-risk production orders.
- Review major operational issues.

MD should normally be **read-only** except for selected approvals or priority changes if enabled later.

## 4.2 Admin

Primary purpose: manage orders, planning, scheduling and system master data.

Admin can:

- Create customer / production orders.
- Enter customer PO/reference.
- Select product.
- Enter quantity.
- Enter required delivery date.
- Add special instructions.
- Review system schedule recommendation.
- Assign machine.
- Confirm or change production schedule.
- View material availability.
- Monitor all department statuses.
- Maintain customers.
- Maintain products.
- Maintain machines.
- Maintain users.
- Maintain material master data.
- Maintain product specifications.
- Maintain standard production rates.
- Maintain product-material requirements/BOM.
- Generate reports.

Admin is the **main owner of production scheduling**.

## 4.3 QC

Primary purpose: inspect and release material/product quality.

QC can:

- View incoming inspection queue.
- Inspect received raw materials.
- Record moisture test.
- Record appearance result.
- Approve or reject incoming material.
- Release accepted material to usable stock.
- Perform production QC/SPC checks.
- Enter four sample weights.
- Record appearance.
- Record dimension.
- Record sealing.
- Review automatic pass/fail result.
- Record corrective action.
- Perform final inspection.
- Approve finished goods for delivery.
- Reject/hold non-conforming goods.
- View QC history.
- View SPC chart.

## 4.4 Production / Operator

Primary purpose: execute scheduled production.

Operator can:

- View assigned production jobs.
- See machine assignment.
- See product.
- See target quantity.
- See material readiness.
- Start production.
- Pause production.
- Resume production.
- Stop production.
- Enter required QC values when prompted.
- Record defects.
- Record downtime.
- Complete cartons.
- End production.
- View own production history.

Operator UI must be the simplest interface in the system.

## 4.5 PO / Purchasing

Primary purpose: purchase shortage materials and manage supplier ETA.

Purchasing can:

- View automatic material shortage list.
- View required-by date.
- View affected production orders.
- Create purchase orders.
- Select supplier.
- Enter quantity.
- Enter expected arrival date.
- Update PO status.
- Update supplier ETA.
- View incoming purchase orders.
- View supplier records.

Purchasing should not manage physical inventory quantities directly.

## 4.6 Store

Primary purpose: control physical stock movement.

Store can:

- Receive incoming supplier goods.
- Create Goods Received Note (GRN).
- Enter received quantity.
- Enter damaged/rejected quantity.
- Submit GRN to QC.
- View inventory.
- Reserve material for production.
- Issue material to production.
- Receive finished goods from production.
- Confirm finished-goods transfer.
- Verify lot quantity.
- Prepare goods for delivery.
- Update physical stock movements.

Store is responsible for **physical stock**, while Purchasing is responsible for **supplier purchasing**.

---

# 5. Core Workflow

## 5.1 Step 1 — Admin Creates Order

Admin enters:

- Customer
- Customer PO / reference number
- Product
- Order quantity
- Unit
- Required delivery date
- Special customer requirements
- Notes

System automatically generates:

- Internal order number
- Order creation date/time
- Order creator
- Order status

Initial status:

`UNSCHEDULED`

## 5.2 Step 2 — System Calculates Requirements

After order creation, the system retrieves product master data.

Product master should include:

- Product name
- Product code
- Packet weight
- Units per carton
- Compatible machines
- Standard production rate
- QC frequency
- QC specification range
- Required materials / BOM
- Packaging material requirements
- Final inspection rules

System calculates:

- Estimated material requirement
- Estimated cartons
- Estimated run time
- Earliest possible production date
- Machine availability
- Material availability
- Delivery-date feasibility

## 5.3 Step 3 — Store Stock Check

System compares required material against released inventory.

Inventory must distinguish:

- Available
- Reserved
- Awaiting QC / Quarantine
- Rejected
- Work in Progress
- Finished Goods

Only **Available** material may be used for production planning.

Example:

| Material | Required | Available | Shortage |
|---|---:|---:|---:|
| Silica Gel | 100 kg | 150 kg | 0 |
| Packing Roll | 2 rolls | 1 roll | 1 |
| Carton | 20 pcs | 50 pcs | 0 |

If no shortage:

`MATERIAL_READY`

If shortage exists:

`WAITING_MATERIAL`

---

# 6. Purchasing Workflow

If a shortage exists, Purchasing receives an automatic task.

Purchasing sees:

- Production order
- Product
- Material
- Required quantity
- Current stock
- Shortage quantity
- Required-by date
- Production start date
- Priority

Purchasing creates a PO containing:

- Supplier
- Material
- Quantity
- Unit
- PO date
- Expected arrival date
- Remarks

Purchase statuses:

- Draft
- Ordered
- Confirmed
- In Transit
- Arriving Today
- Received
- Cancelled

Supplier ETA must feed back into the production schedule.

---

# 7. Receiving / GRN Workflow

When supplier material arrives, Store creates a GRN.

GRN fields:

- Supplier
- DO / Invoice number
- Date
- GRN number (auto-generated)
- Material / item
- Quantity received
- Quantity rejected/damaged
- Quantity accepted physically
- Remarks
- Prepared by
- Submission date/time

GRN workflow:

`DRAFT → RECEIVED → PENDING_QC → RELEASED / REJECTED`

Important rule:

> Received material must not become usable production stock until QC approves it.

---

# 8. Incoming QC Workflow

Incoming material requiring QC appears automatically in the QC queue.

For silica gel incoming inspection, the system must support the current SOP moisture criteria:

- Random sampling according to current SOP.
- Moisture content is recorded.
- **Below 2.5% = Accept**
- **Above 2.5% = Reject**

QC form should support:

- GRN number
- Supplier
- Material
- Sampling method
- Moisture content
- Appearance
- Result
- Remarks
- Inspector
- Inspection date/time

Possible results:

- PASS
- REJECT
- HOLD / MRB

If PASS:

`QUARANTINE → AVAILABLE`

If REJECT:

`QUARANTINE → REJECTED / MRB`

---

# 9. Production Scheduling

Scheduling should be **semi-automatic**.

The software recommends a schedule, but Admin confirms it.

## 9.1 Inputs

Scheduling engine uses:

- Required delivery date
- Product
- Quantity
- Compatible machine
- Machine availability
- Existing production queue
- Standard production rate
- Material readiness
- Supplier ETA
- Planned maintenance
- Changeover time
- Historical production rate later

## 9.2 Recommendation

System should display:

- Recommended machine
- Estimated run time
- Earliest possible start
- Estimated completion
- Material status
- Delivery risk
- Conflicts

Example:

**Recommended Machine:** Machine 01  
**Estimated Run Time:** 32 hours  
**Material Ready:** 25 Sep  
**Earliest Start:** 26 Sep 08:30  
**Estimated Completion:** 27 Sep 16:30  
**Required Delivery:** 30 Sep  
**Status:** On Schedule

Admin presses:

`CONFIRM SCHEDULE`

---

# 10. Production Order Statuses

The main production lifecycle should be:

1. `UNSCHEDULED`
2. `WAITING_MATERIAL` — only if required
3. `SCHEDULED`
4. `READY`
5. `RUNNING`
6. `PAUSED` — if applicable
7. `QC_HOLD` — if applicable
8. `PRODUCTION_COMPLETE`
9. `AWAITING_FINISHED_GOODS_RECEIPT`
10. `AWAITING_FINAL_QC`
11. `READY_FOR_DELIVERY`
12. `DELIVERED`
13. `CLOSED`

Additional exception statuses:

- `DELAYED`
- `AT_RISK`
- `CANCELLED`
- `REJECTED`

---

# 11. Operator Daily Workflow

Operator workflow must be extremely simple.

Normal workflow:

`LOGIN → MY JOB → START PRODUCTION → QC CHECKS → RECORD ISSUES IF ANY → COMPLETE CARTON → END PRODUCTION`

## 11.1 Operator Home Screen

Display:

- Machine
- Current production order
- Product
- Customer
- Target quantity
- Material status
- Production progress
- Completed cartons
- Reject count
- Next QC check
- Production status

Primary buttons:

- **Start Production**
- **QC Check**
- **Record Defect**
- **Pause / Downtime**
- **Complete Carton**
- **End Production**

Only actions relevant to the current production state should be enabled.

---

# 12. Start Production

Before Start Production is enabled:

- Production order must be scheduled.
- Required material must be released.
- Store must have issued required material.
- Machine must be available.

When operator presses **Start Production**, record:

- Order
- Batch
- Machine
- Operator
- Start date/time
- Product
- Target quantity

Status becomes:

`RUNNING`

---

# 13. In-Process QC / SPC

The current SOP requires QC checks during production.

For silica gel packing:

- QC frequency: **every 20 minutes**
- Sample count: **4 packets**

When production starts, the system automatically creates QC due times.

Example:

Start: 08:37  
Checks:

- 08:57
- 09:17
- 09:37
- 09:57
- etc.

The operator/QC screen should clearly display:

`NEXT QC CHECK IN: XX MIN`

When due, a visible alert must appear.

---

# 14. QC Check Form

The production QC form should support:

- Batch number
- Product
- Machine
- Sampling time
- Sample 1 weight
- Sample 2 weight
- Sample 3 weight
- Sample 4 weight
- Appearance
- Dimension
- Sealing
- Remarks

System automatically calculates:

- Minimum
- Maximum
- Average
- Pass/Fail

The product master defines the specification.

Example for 1 g product:

- Lower limit: 0.90 g
- Upper limit: 1.20 g

The specification must be configurable per product.

---

# 15. SPC Chart

System generates a live SPC / weight chart.

Required:

- Sample average
- Center line (CL)
- Upper control limit (UCL)
- Lower control limit (LCL)
- Time or sample sequence
- Highlight out-of-spec values

The system should preserve historical SPC records.

---

# 16. QC Failure Workflow

If any required QC check fails:

1. Result becomes `FAIL`.
2. Production receives an alert.
3. QC receives an alert.
4. Admin receives an alert.
5. Order may move to `QC_HOLD`.
6. Corrective action must be recorded.
7. Production resumes only after authorised release.

Corrective-action fields:

- Problem
- Action taken
- Responsible person
- Date/time
- Re-check result

---

# 17. Defect Recording

Operator can record defects during production.

Default defect types:

- White dot
- Underweight
- Overweight
- Sealing NG
- Dimension NG
- Cutting defect
- Printing defect
- Empty sachet
- Contamination
- Other

Fields:

- Date/time
- Production order
- Machine
- Batch
- Defect type
- Quantity
- Remarks
- Recorded by

---

# 18. Downtime Recording

If production stops, operator presses **Pause / Downtime**.

Default reasons:

- Material change
- Packing roll change
- Film jam
- Machine fault
- Sensor fault
- Cleaning
- Power interruption
- No material
- Planned stop
- Other

Record:

- Start time
- End time
- Duration
- Machine
- Order
- Reason
- Remarks
- Operator

When resolved:

`PAUSED → RUNNING`

---

# 19. Carton Completion

When a carton is completed, operator presses **Complete Carton**.

System auto-generates carton number.

Carton record:

- Carton number
- Batch
- Order
- Product
- Machine
- Operator
- Completion time
- Quantity
- Confirmation method
- Appearance result
- Weight confirmation
- Label confirmation
- Status

For the existing process, confirmed finished quantity may rely on carton/weight confirmation instead of continuous packet-by-packet counting.

The system must clearly distinguish:

- Estimated output
- Confirmed finished quantity

---

# 20. Packing of Finished Goods

The digital workflow must preserve the SOP checks for finished cartons.

Inspection points include:

- Appearance
- Total weight
- Carton condition
- Tightness
- Label

Reject/rectification examples:

- Torn carton
- Untight carton
- Missing label
- Incorrect packaging

Possible carton states:

- Completed
- Requires Rectification
- Accepted
- Awaiting Transfer

---

# 21. Finished Goods Transfer

Production-completed cartons are sent to Store.

The system generates a digital Finished Goods Transfer Note automatically from existing production data.

Transfer includes:

- Date
- Time
- Machine number
- Operator
- Product
- Total quantity
- Carton count
- Remarks
- Production confirmation
- Store receipt confirmation

Workflow:

`PRODUCTION COMPLETE → TRANSFER PENDING → RECEIVED BY STORE`

After Store confirms:

Finished Goods inventory increases.

---

# 22. Final Inspection

After Store receives finished goods, QC receives a final inspection task.

Final inspection parameters include:

- Appearance
- Weight
- Dimension
- Sealing
- Customer requirement
- Sampling plan

Sampling may follow:

- ANSI standard
- Customer-specific requirement

The software must support configurable Major / Minor criteria.

Results:

- ACCEPT
- REJECT
- HOLD / MRB

If accepted:

`READY_FOR_DELIVERY`

If rejected:

`MRB / REJECTED`

---

# 23. Delivery

Delivery workflow should support:

- Delivery order
- Customer
- Product
- Batch
- Cartons
- Quantity
- Scheduled delivery date
- Actual delivery date
- Vehicle / transporter if required later
- Remarks
- Delivery status

Before dispatch, confirm:

- Carton condition
- Correct label
- Correct quantity
- Final QC accepted

Statuses:

- Preparing
- Ready
- Scheduled
- Out for Delivery
- Delivered
- On Hold

---

# 24. Inventory Model

Inventory should be transaction-based.

Do not simply overwrite stock balance.

Every stock change creates an inventory transaction.

Transaction types:

- GRN_RECEIPT
- QC_RELEASE
- QC_REJECTION
- RESERVATION
- MATERIAL_ISSUE
- MATERIAL_RETURN
- PRODUCTION_WIP
- FINISHED_GOODS_RECEIPT
- DELIVERY_OUT
- STOCK_ADJUSTMENT

Inventory categories:

- Raw Material
- Packaging Material
- Consumable
- Work in Progress
- Finished Goods
- Quarantine
- Rejected

---

# 25. Material Request / Issue

Production may request additional material.

Workflow:

`REQUESTED → ACKNOWLEDGED → ISSUED → RECEIVED`

Material request includes:

- Production order
- Material
- Requested quantity
- Requested by
- Request date/time
- Required by
- Issued quantity
- Store issuer
- Production receiver
- Remarks

---

# 26. Equipment Calibration

The system should digitise the Calibration Schedule.

Equipment fields:

- Equipment name
- Serial number
- Location
- Calibration date
- Next due date
- Allowable error
- Calibration frequency
- Status
- Certificate / attachment later

Statuses:

- Active
- Due Soon
- Overdue
- Out of Service

System should provide reminders.

---

# 27. Digital Forms

The system must replace or automatically generate the following existing records:

1. Goods Received Note (GRN)
2. Quality Check List — Silica Gel / Power Dry
3. Quality Check List — Activated Clay Desiccant
4. Production Performance Monitoring Form
5. Finished Goods Transfer Note
6. SPC Checklist
7. Material Request Sheet
8. Calibration Schedule
9. Production Schedule for the Month
10. Daily Ever Drypack Operation Activity
11. Incoming QC Record
12. Final Inspection Record
13. Delivery record / delivery order as required

Where practical, forms should be generated automatically from system transactions rather than manually filled from zero.

---

# 28. Dashboard

The management/admin dashboard should show:

- Orders today
- Orders at risk
- Machines running
- QC pass rate
- Finished cartons
- Low-stock materials
- Deliveries due
- Today's production schedule
- Machine status
- Inventory summary
- Production trend
- Alerts/tasks

The dashboard must remain clean and professional.

Avoid excessive cards, decorative graphics and cartoon-style UI.

---

# 29. Orders & Schedule UI

Admin screen must include:

## New Order

- Customer
- Customer PO/reference
- Product
- Quantity
- Unit
- Delivery date
- Notes

## Schedule Recommendation

- Recommended machine
- Estimated run time
- Earliest start
- Estimated completion
- Material status
- Delivery risk

## Production Schedule

Table:

- Order number
- Customer
- Product
- Machine
- Start date
- End date
- Quantity
- Status
- Actions

## Weekly Machine Schedule

Calendar/timeline for Machine 01 and Machine 02.

---

# 30. Purchase Planning UI

Purchasing dashboard:

Summary:

- Open POs
- Material shortages
- Arriving today
- Active suppliers

Material shortage table:

- Material
- Required
- Available
- Shortage
- Needed by
- Priority

Create Purchase Order form:

- Supplier
- Material
- Quantity
- Unit
- Expected arrival
- Remarks

Supplier ETA table.

Linked production orders.

---

# 31. Store & Inventory UI

Store dashboard:

Summary:

- Available stock
- Reserved
- WIP
- Low-stock items

Current stock table:

- Material
- Category
- On hand
- Reserved
- Unit
- Status

Issue to Production:

- Production order
- Product
- Required material
- Required quantity
- Issue quantity

Pending material requests.

Stock trend.

---

# 32. Receiving / GRN UI

Receiving screen:

- Supplier
- DO / Invoice
- Date
- Auto GRN number
- Received items table
- Qty in
- Rejected
- Accepted
- Remarks

Process indicator:

`STORE → QC → RELEASED`

Buttons:

- Save Draft
- Submit QC
- Print GRN

Release/reject should normally be controlled by QC workflow, not Store.

---

# 33. Incoming QC UI

Incoming QC dashboard:

Summary:

- Pending inspections
- Passed today
- Rejected today
- Released stock

Inspection form:

- GRN
- Material
- Supplier
- Sample method
- Moisture %
- Appearance
- Decision
- Remarks

Display SOP criteria prominently:

- Below 2.5% → Accept
- Above 2.5% → Reject

Pending inspection queue.

---

# 34. Live Production UI

Production / management live screen should show:

Machine status:

- Machine 01
- Machine 02
- Running / Idle / Paused / Stopped
- Product
- Speed
- Uptime

Active job:

- Product
- Order
- Operator
- Target
- Confirmed output
- Progress
- Material ready
- Next QC check

Operator controls:

- Start
- Pause
- Stop
- Log Defect
- Complete Carton

Recent defects and downtime.

For operator accounts, non-essential management information should be hidden.

---

# 35. QC / SPC UI

QC screen:

New QC Check:

- Batch
- Product
- Time
- W1
- W2
- W3
- W4

Result:

- Spec range
- Minimum
- Maximum
- Average
- Pass/Fail

SPC chart:

- UCL
- CL
- LCL
- Sample result trend

Recent QC checks.

Out-of-spec alert.

---

# 36. Finished Goods & Delivery UI

Summary:

- Finished goods
- Ready for delivery
- Awaiting final QC
- Delivered today

Completed cartons table:

- Batch
- Product
- Cartons
- Quantity
- Final QC status
- Delivery status
- Completion date

Actions:

- Generate Transfer
- Confirm Receipt
- Ready for Delivery
- Print Slip

Delivery queue.

---

# 37. Reports & Analytics

Reports must support filters:

- Date range
- Product
- Machine
- Customer later
- Export format

Management metrics:

- Total production
- QC pass rate
- Total rejects
- On-time delivery
- Machine utilisation
- Material consumption
- Inventory movement

Charts:

- Daily production output
- QC pass-rate trend
- Reject breakdown
- Delivery trend
- Machine utilisation

Standard reports:

- Production Summary
- QC Summary
- Inventory Summary
- Delivery Summary
- Calibration Report
- Material Consumption Report
- Finished Goods Report

Export:

- PDF
- Excel/CSV
- Print

---

# 38. Notifications & Alerts

System alerts include:

- Material shortage
- Material arrival due
- Incoming QC pending
- QC check due
- QC overdue
- QC failure
- Machine downtime
- Schedule conflict
- Production delay
- Order at risk
- Low inventory
- Calibration due
- Calibration overdue
- Finished goods waiting for transfer
- Final QC pending
- Delivery due

Notifications should be role-specific.

---

# 39. Department Information Distribution

After Admin creates an order, each department receives only what it needs.

| Role | Information |
|---|---|
| MD | Order progress, delivery risk, production progress, shortages, QC alerts, reports |
| Admin | Full order, schedule, department status, material readiness |
| Store | Required materials, reservations, GRN, issues, finished goods receipt |
| Purchasing | Shortages, required-by dates, suppliers, purchase ETA |
| Production | Machine, product, target, material-ready status, job sequence, QC timer |
| QC | Incoming inspections, 20-minute checks, failed checks, final inspection |

The system must avoid duplicate manual data entry.

---

# 40. Permissions

## MD

Read access:

- Dashboard
- Orders
- Schedule
- Purchase summary
- Store/inventory summary
- Receiving summary
- QC
- Production
- Finished goods
- Delivery
- Reports

## Admin

Full operational access:

- Orders
- Schedule
- Master data
- Users
- Products
- Machines
- Customers
- Reports
- Operational monitoring

## QC

Write:

- Incoming QC
- Production QC
- Final QC
- Corrective actions
- Calibration if assigned

Read:

- Relevant GRN
- Relevant production order
- Product specifications

## Production / Operator

Write:

- Start/pause/resume/end production
- QC sample entry if allowed
- Defects
- Downtime
- Carton completion
- Material requests

Read:

- Assigned jobs
- Material readiness
- Relevant specifications

## Purchasing

Write:

- Purchase orders
- Supplier ETA
- Supplier information

Read:

- Material shortages
- Required-by dates
- Linked production orders

## Store

Write:

- GRN
- Physical receipts
- Material issue
- Inventory movement
- Finished-goods receipt
- Delivery preparation

Read:

- Production material requirements
- QC release status

---

# 41. Audit Trail

Every important change must record:

- User
- Role
- Action
- Record type
- Record ID
- Previous value
- New value
- Date/time

Examples:

- QC value edited
- Order rescheduled
- Stock adjusted
- Material released
- Production ended
- Carton quantity changed
- Delivery status changed

Historical production/QC records must not be silently overwritten.

---

# 42. Data Model

Recommended main tables:

```text
users
roles
permissions

customers
suppliers

products
product_specs
materials
product_materials

machines
equipment
calibrations

orders
production_orders
production_schedule
production_runs

batches
cartons

goods_received
goods_received_items

incoming_qc_inspections
production_qc_checks
production_qc_samples
final_qc_inspections

defects
downtime_records
corrective_actions

material_requests
material_request_items
material_issues

purchase_orders
purchase_order_items

inventory_items
inventory_balances
inventory_transactions
inventory_reservations

finished_goods_transfers
finished_goods_transfer_items

deliveries
delivery_items

notifications
approvals
audit_logs
```

---

# 43. Important Business Rules

1. Material cannot be used for production before QC release.
2. Only released stock counts as available.
3. Production cannot start if mandatory material is unavailable.
4. Admin owns production scheduling.
5. Purchasing manages supplier procurement, not physical inventory.
6. Store manages physical inventory.
7. QC owns product/material quality decisions.
8. Operator should not access purchasing/admin functions.
9. Production QC reminder is generated automatically after production starts.
10. QC frequency must be configurable, defaulting to the current SOP requirement.
11. Four sample weights are required for the current silica gel QC check.
12. Failed QC must generate an alert.
13. Completed cartons require confirmation before entering finished goods.
14. Final QC must pass before goods become ready for delivery.
15. All critical state changes require an audit log.
16. Quantity calculations must distinguish estimated production from confirmed finished quantity.
17. Schedule recommendations must never silently reschedule orders without Admin confirmation.

---

# 44. UI / Visual Design Requirements

The application should use the actual EverDryPack branding.

## Brand Direction

Use the company logo.(logo.png)

Primary visual colours should be inspired by the logo:

- EverDryPack Blue — primary actions / navigation
- EverDryPack Orange — highlights / warnings / secondary accent
- White — main application background
- Light neutral grey — panels / tables
- Green — success / accepted / running
- Red — rejected / failure / critical alerts

## Professional Style

The design must be:

- Corporate
- Clean
- Modern
- Industrial / manufacturing oriented
- Data-first
- Minimal
- Easy to scan

Avoid:

- Cartoon illustrations
- Oversized decorative icons
- Excessive gradients
- Too many colours
- Excessive text
- Decorative dashboards with no operational purpose

Use:

- Compact tables
- Clear typography
- Subtle shadows/borders
- Consistent spacing
- Small professional status chips
- Clear primary action buttons

---

# 45. Responsive Design

Primary usage:

- Desktop PCs
- Office laptops
- Production-floor tablets

Minimum supported width should support tablet usage.

Operator screens should be especially touch-friendly.

Operator primary buttons should be large enough for quick use:

- Start
- QC Check
- Defect
- Downtime
- Complete Carton
- End Production

---

# 46. Recommended Tech Stack

Initial implementation:

Frontend / Application
React.js
Vite
JavaScript
React Router
Tailwind CSS
shadcn/ui or an equivalent professional component system
TanStack Query for API data management
Recharts for production, inventory and QC charts

TypeScript will not be used.

Backend
Node.js
Express.js
JavaScript
REST API architecture
Zod for request validation
Centralised error handling
Role-based access-control middleware

Use the following backend flow:

Route → Controller → Service → Prisma → PostgreSQL

Responsibilities:

Routes: Define API endpoints
Controllers: Handle requests and responses
Services: Contain production and business rules
Prisma: Perform database operations
Middleware: Handle authentication, authorisation and validation

Suggested modules:

Authentication
Users
Machines
Production runs
Quality control
Rejections
Cartons
Inventory
Orders
Reports
Forecasting
Audit logs
Database

Recommended:

Supabase PostgreSQL

Supabase is suitable for EverDryPack because it provides:

Managed PostgreSQL database
Authentication
Realtime database updates
File storage
Database backups
Future notification capabilities

Alternative:

Neon PostgreSQL

Neon is suitable if the application only requires a managed PostgreSQL database without Supabase Auth, Storage or Realtime features.

ORM

Preferred:

Prisma ORM
Prisma Migrate
Prisma Studio

Prisma will connect the Express backend to the Supabase PostgreSQL database.

Prisma can be used normally with JavaScript. TypeScript is not required.

Authentication

Recommended:

Supabase Auth
JWT verification in the Express backend
Role-based access control

Initial user roles:

Administrator
Management
Supervisor
Operator
Quality Control
Viewer or Client

All sensitive permissions must be verified by the Express backend, not only controlled through the frontend interface.

Realtime Updates

Recommended:

Supabase Realtime

Realtime functionality may be used for:

Current machine status
Active operator
Current production run
Production quantity
Carton completion progress
QC status and alerts
Rejection updates
Daily target progress

Because no external machine hardware will be connected during the MVP, realtime information will initially depend on operator-entered records.

Socket.IO may be added later if the system requires more advanced custom realtime events.

Reports and Documents

Recommended libraries:

PDFKit or Puppeteer for PDF generation
ExcelJS for Excel exports
Supabase Storage for initial document storage

The system should support:

Daily production reports
QC reports
Rejection reports
Inventory reports
Carton records
Operator-performance records
PDF exports
Excel exports
Hosting

Recommended:

Vercel for the React frontend
Render for the Node.js and Express backend
Supabase for PostgreSQL, Authentication, Storage and Realtime

The frontend and backend should be deployed separately.

Suggested deployment structure:

React Frontend
      ↓
Express REST API
      ↓
Prisma ORM
      ↓
Supabase PostgreSQL
Project Structure

A monorepo structure may be used:

everdrypack/
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── layouts/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── utils/
│   │   └── routes/
│   └── package.json
│
├── server/
│   ├── src/
│   │   ├── config/
│   │   ├── modules/
│   │   ├── middleware/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── app.js
│   │   └── server.js
│   ├── prisma/
│   │   ├── schema.prisma
│   │   ├── migrations/
│   │   └── seed.js
│   └── package.json
│
├── package.json
└── README.md

Example backend module:

production/
├── production.routes.js
├── production.controller.js
├── production.service.js
└── production.validation.js
Synology NAS

The Synology NAS may later be used for:

Scheduled report backups
PDF archives
Purchase and sales documents
Database backup exports
Internal company document storage
Long-term production-record archives

The NAS should not be used as the primary application database for the MVP.

Final Recommended Stack
Frontend:
React.js + Vite + JavaScript + Tailwind CSS

Backend:
Node.js + Express.js + JavaScript

Backend Architecture:
Route → Controller → Service → Prisma

Database:
Supabase PostgreSQL

ORM:
Prisma

Authentication:
Supabase Auth with role-based access control

Realtime:
Supabase Realtime

Hosting:
Vercel + Render + Supabase

Future Archival:
Synology NAS

# 47. MVP Scope

## Phase 1 — Foundation

- Authentication
- 6 roles
- User management
- Customer master
- Supplier master
- Product master
- Material master
- Machine master

## Phase 2 — Order & Schedule

- Admin order entry
- Material requirement calculation
- Material availability check
- Schedule recommendation
- Admin schedule confirmation
- Machine calendar

## Phase 3 — Purchase / Store / Receiving

- Material shortage list
- Purchase orders
- Supplier ETA
- GRN
- Inventory
- Material reservation
- Material issue

## Phase 4 — QC

- Incoming QC
- QC release/rejection
- Production QC
- 20-minute reminders
- Four-sample weight entry
- Automatic calculations
- SPC graph
- Final QC

## Phase 5 — Production

- Operator job screen
- Start/Pause/Resume/End
- Defects
- Downtime
- Carton completion
- Production progress

## Phase 6 — Finished Goods / Delivery

- Finished goods transfer
- Store receipt
- Finished goods inventory
- Delivery queue
- Delivery status

## Phase 7 — Management

- Dashboard
- Reports
- Alerts
- Audit log
- PDF/Excel export

---

# 48. Future Enhancements

Not required for MVP but system architecture should allow:

- Barcode / QR code scanning
- Tablet kiosk mode
- Digital signatures
- Customer portal
- Supplier portal
- Automated email notifications
- WhatsApp notifications
- Machine hardware integration
- IoT sensors
- Automated temperature/humidity collection
- Automated machine output counters
- Advanced forecasting
- OEE calculation
- Maintenance module
- Predictive maintenance
- QR-coded carton traceability
- Full batch genealogy
- Electronic document approval
- ISO/audit report packs

---

# 49. Non-Functional Requirements

## Performance

- Standard screens should load quickly on local office internet.
- Main dashboard should respond within a few seconds.
- Large tables require pagination.

## Reliability

- Transactions must not duplicate when users double-click.
- Important actions require server-side validation.
- Inventory transactions must be atomic.

## Security

- Authentication required.
- Role-based access control.
- Passwords never stored in plain text.
- Server-side permission checks.
- Audit logging for sensitive actions.

## Data Integrity

- Use unique IDs.
- Avoid deleting production/QC records.
- Prefer soft delete / inactive status for master data.
- Prevent invalid status transitions.

## Backup

- Automated PostgreSQL backups.
- Optional scheduled export to Synology NAS later.

---

# 50. Success Criteria

MVP is successful when:

1. Admin can enter an order once.
2. Material requirements are calculated automatically.
3. Store immediately sees what material is required.
4. Purchasing automatically sees shortages.
5. Incoming material cannot be used before QC release.
6. Admin can schedule production without machine conflicts.
7. Operator can perform daily work with only a small number of buttons.
8. QC reminders occur automatically according to the SOP.
9. QC results are calculated automatically.
10. Production defects and downtime are traceable.
11. Completed cartons transfer digitally to Store.
12. Final QC is required before delivery.
13. MD can see current operational status without asking each department.
14. Existing paper records can be reproduced digitally.
15. Reports can be exported for management or audit use.

---

# 51. Core User Journey Summary

```text
ADMIN
Creates Order
     ↓
SYSTEM
Calculates Materials + Time + Machine Availability
     ↓
STORE
Checks / Reserves Available Stock
     ↓
SHORTAGE?
 ┌───────┴────────┐
 Yes              No
 ↓                ↓
PURCHASING       MATERIAL READY
Creates PO          │
 ↓                  │
Supplier Delivery   │
 ↓                  │
STORE RECEIVING     │
Creates GRN         │
 ↓                  │
QC INCOMING         │
Pass / Reject       │
 ↓                  │
Released Stock ─────┘
     ↓
ADMIN
Confirms Production Schedule
     ↓
STORE
Issues Material
     ↓
PRODUCTION
Operator Starts Job
     ↓
QC / SPC
Every 20 Minutes
4 Samples
     ↓
PRODUCTION
Defects / Downtime / Cartons
     ↓
Production Complete
     ↓
STORE
Receives Finished Goods
     ↓
QC
Final Inspection
     ↓
PASS?
 ┌──────┴───────┐
 Yes            No
 ↓              ↓
READY           HOLD / MRB
FOR DELIVERY
 ↓
STORE / ADMIN
Delivery
 ↓
DELIVERED
 ↓
REPORTS + AUDIT TRAIL
 ↓
MD DASHBOARD
```

---

# 52. Development Principle

Do not build the software as a collection of unrelated digital copies of paper forms.

Build the system around connected business entities:

- Order
- Material
- Inventory
- Purchase
- GRN
- QC
- Production
- Batch
- Carton
- Finished Goods
- Delivery

The software should generate the traditional forms from this connected data.

This prevents repeated data entry and creates a complete end-to-end traceable workflow.

---

# 53. Final Product Principle

The EverDryPack system should feel different depending on the role.

**MD:** see what is happening.  
**Admin:** plan and coordinate.  
**Purchasing:** obtain missing materials.  
**Store:** control physical stock.  
**QC:** control quality.  
**Production:** run the job.

All six roles work from the same underlying order and database.

That is the core of the EverDryPack digital workflow.
