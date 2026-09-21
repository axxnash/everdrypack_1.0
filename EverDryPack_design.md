# EverDryPack Operations System
## UI / UX Design Specification (`design.md`)

**Company:** Ever Drypack Malaysia Sdn. Bhd.  
**Product:** EverDryPack Production & Inventory Management System  
**Document Type:** UI / UX Design Specification  
**Version:** 1.0  
**Purpose:** Define the visual system, navigation, page layouts, component behavior, role-specific interfaces, interaction patterns, responsive rules, and implementation guidance for the EverDryPack web application.

---

# 1. Design Goal

The EverDryPack system should feel like a professional industrial operations platform, not a generic admin dashboard and not a cartoon-style application.

The design must make it easy for every department to answer one question quickly:

> **What do I need to do right now?**

The visual design should communicate:

- Reliability
- Manufacturing discipline
- Traceability
- Quality control
- Operational clarity
- Professional customer-facing standards

The software must remain simple enough for shop-floor users while still being detailed enough for Admin, QC, Store, Purchasing, and MD.

---

# 2. Design Principles

## 2.1 Data First

Important production information should always be more visually prominent than decoration.

Prioritize:

- Current status
- Quantity
- Due date
- Machine
- Material readiness
- QC result
- Production progress
- Alerts
- Required action

Avoid unnecessary decorative graphics.

---

## 2.2 Simple by Role

Each role should only see the pages and actions relevant to their job.

The same system is used by all users, but navigation and permissions are role-based.

Examples:

- Operator should not see Purchasing screens.
- Purchasing should not see operator controls.
- MD should see high-level operational status, not data-entry forms.
- QC should quickly access pending inspections and SPC.

---

## 2.3 One Clear Primary Action

Every workflow page should have one obvious next action.

Examples:

- Admin: **Create Order**
- Purchasing: **Create PO**
- Store: **Issue Materials**
- QC: **Submit Inspection**
- Operator: **Start Production**
- Finished Goods: **Confirm Receipt**

Secondary actions should be visually quieter.

---

## 2.4 Professional, Not Cartoonish

Avoid:

- Oversized illustrations
- Cartoon machines
- Excessive iconography
- Heavy gradients
- Decorative mascots
- Bright multi-color panels
- Large emoji-style icons
- Too many floating cards

Prefer:

- Flat professional icons
- Compact cards
- Clean tables
- Thin borders
- Neutral backgrounds
- Consistent spacing
- Strong typography hierarchy
- Small status chips

---

## 2.5 Progressive Disclosure

Do not show every field and option at once.

Show only what is needed for the current task.

Example:

When an operator is waiting to start:

- Show job details
- Show material ready/not ready
- Show Start button

Only after production starts should QC, defect, downtime, carton, and stop controls become active.

---

# 3. Brand Direction

Use the official EverDryPack logo as the primary brand reference.

The design theme should be based on the logo colors:

## Primary Colors

### EverDryPack Blue

Use for:

- Main navigation
- Primary buttons
- Links
- Selected tabs
- Main chart series
- Focus states

Suggested palette:

```css
--blue-900: #0B2F66;
--blue-800: #0D4FA3;
--blue-700: #1264C7;
--blue-600: #1677E8;
--blue-100: #EAF3FF;
--blue-050: #F5F9FF;
```

### EverDryPack Orange

Use for:

- Secondary accent
- Important warnings
- Purchase / material actions
- Highlighted callouts
- Attention states

Suggested palette:

```css
--orange-700: #D85A00;
--orange-600: #F36C0D;
--orange-500: #FF7A18;
--orange-100: #FFF1E7;
--orange-050: #FFF8F3;
```

---

# 4. Functional Status Colors

Status colors should communicate meaning consistently.

## Green — Success / Healthy

Use for:

- Running
- Accepted
- Passed
- Released
- Ready
- Delivered
- Completed

```css
--green-700: #15803D;
--green-600: #16A34A;
--green-100: #DCFCE7;
```

## Red — Critical / Failure

Use for:

- Rejected
- Failed QC
- Critical shortage
- Overdue
- Stopped
- Invalid

```css
--red-700: #B91C1C;
--red-600: #DC2626;
--red-100: #FEE2E2;
```

## Amber — Warning / Waiting

Use for:

- Pending
- Due soon
- Waiting material
- Waiting QC
- At risk

```css
--amber-700: #B45309;
--amber-500: #F59E0B;
--amber-100: #FEF3C7;
```

## Gray — Neutral / Inactive

Use for:

- Draft
- Idle
- Cancelled
- Disabled
- Not applicable

```css
--gray-900: #111827;
--gray-700: #374151;
--gray-500: #6B7280;
--gray-300: #D1D5DB;
--gray-200: #E5E7EB;
--gray-100: #F3F4F6;
--gray-050: #F9FAFB;
```

---

# 5. Typography

Recommended:

- **Inter**
- **Geist**
- **Arial / system-ui fallback**

Use a clean sans-serif typeface.

Suggested scale:

```text
Page title:          28–32px / 700
Section title:       18–20px / 600
Card title:          14–16px / 600
Body:                14px / 400
Table text:          13–14px / 400
Secondary text:      12–13px / 400
Metric value:        28–36px / 700
Status badge:        12px / 600
```

Avoid oversized headings.

---

# 6. Application Shell

Desktop layout:

```text
┌──────────────────────────────────────────────────────────────┐
│ Logo / Company        Search               Alerts   User     │
├──────────────┬───────────────────────────────────────────────┤
│              │                                               │
│   Sidebar    │                 Page Content                  │
│              │                                               │
│              │                                               │
└──────────────┴───────────────────────────────────────────────┘
```

---

# 7. Top Navigation Bar

Height:

- 64–72px

Contains:

- EverDryPack logo
- Company name
- Global search
- Notification icon
- Current date
- User avatar / name
- Role
- Account dropdown

Global search placeholder:

> Search order, batch, material, supplier...

Search should support later:

- Order number
- Batch number
- GRN
- Material
- Product
- Supplier
- Customer
- Carton

---

# 8. Sidebar Navigation

Desktop width:

- 200–224px

The sidebar should use a white or very light background in the professional theme.

Selected page:

- Blue background or blue left indicator
- White or dark blue text depending on treatment

Suggested Admin menu:

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
Users
Settings
```

The role should determine which items appear.

---

# 9. Role-Based Navigation

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

Mostly read-only.

---

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
Users
Settings
```

---

## QC

```text
Dashboard
Incoming QC
Production QC / SPC
Final QC
Calibration
QC History
```

---

## Production / Operator

Keep navigation minimal:

```text
My Job
Production
QC Check
Defects
Downtime
Cartons
History
```

---

## Purchasing

```text
Dashboard
Material Shortages
Purchase Orders
Suppliers
Supplier ETA
```

---

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

# 10. Layout System

Use a 12-column desktop grid.

Recommended content widths:

- Max content width: 1440px
- Horizontal page padding: 24–32px
- Card gap: 16–20px
- Section gap: 24px

Cards:

```css
border-radius: 8px;
border: 1px solid #E5E7EB;
background: #FFFFFF;
box-shadow: 0 1px 2px rgba(0,0,0,0.03);
```

Avoid large floating shadows.

---

# 11. Core Components

## 11.1 Metric Card

Use for high-level numbers.

Structure:

```text
Label
BIG VALUE
Small supporting text
```

Example:

```text
QC Pass Rate
98.6%
↑ 0.8% vs last week
```

Metric cards should not contain too much text.

---

## 11.2 Status Badge

Examples:

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

Use compact pills.

---

## 11.3 Data Table

Default table style:

- White background
- Light gray header
- 44–48px row height
- Minimal borders
- Sticky header where useful
- Row hover
- Search and filter above table
- Pagination
- Sortable columns if needed

Use tables for operational data instead of converting every record into cards.

---

## 11.4 Form Field

Recommended structure:

```text
Label *
[ Input / Select ]
Helper text if required
```

Required fields use small red `*`.

Do not use placeholder text as the only field label.

---

## 11.5 Primary Button

Blue by default.

Examples:

- Create Order
- Submit Inspection
- Issue Materials
- Generate Report

Operator Start can be green because of its operational meaning.

---

## 11.6 Secondary Button

White background with border.

Examples:

- Clear
- Cancel
- View Details
- Print

---

## 11.7 Danger Button

Red.

Examples:

- Reject
- Stop Production
- Delete only where allowed

---

# 12. Dashboard Design

## Purpose

Provide a quick understanding of operations.

## Layout

Top:

```text
Dashboard
Operations overview for EverDryPack Malaysia Sdn. Bhd.
```

Summary cards:

- Orders Today
- Machines Running
- QC Pass Rate
- Finished Cartons
- Materials Low
- Deliveries Due

Main content:

### Today's Schedule

Columns:

- Time
- Order
- Product
- Quantity
- Status

### Machine Status

Columns:

- Machine
- Process
- Status
- Current Output
- Utilization / OEE later

### Inventory Summary

Show only critical / important materials.

### Alerts / Tasks

Show:

- Type
- Message
- Time
- Status

### Production Trend

7-day bar chart.

MD version should emphasize:

- Production
- QC
- Delivery
- Delays
- Material shortage
- Risk

---

# 13. Orders Page

## Purpose

Admin keys customer demand into the system.

### Header

```text
Orders
Create and manage customer production requirements.
```

### New Order Button

Primary action:

`+ New Order`

---

# 14. New Order Form

Fields:

```text
Customer *
Customer PO / Reference *
Product *
Quantity *
Unit *
Required Delivery Date *
Special Requirement
Notes
```

After product selection, show a small information panel:

```text
Product: Silica Gel 1g
Standard Pack: 5,000 pcs / carton
Compatible Machines: M-01, M-02
Standard Rate: 52 packs/min
QC Frequency: Every 20 min
```

Buttons:

- Cancel
- Save Draft
- Create Order

---

# 15. Order Detail Page

Header:

```text
SO-20260921-001
ABC Sdn. Bhd.
Silica Gel 1g
```

Status timeline:

```text
Order Created
   ↓
Materials
   ↓
Schedule
   ↓
Production
   ↓
Final QC
   ↓
Delivery
```

Display:

- Customer
- Customer PO
- Product
- Quantity
- Delivery date
- Current status
- Machine
- Material readiness
- Production progress
- QC status
- Delivery status

Tabs:

```text
Overview
Materials
Production
QC
Finished Goods
Delivery
Audit Log
```

---

# 16. Schedule Page

## Purpose

Allow Admin to schedule production smoothly after order creation.

Top metrics:

- Unscheduled
- Scheduled
- Waiting Material
- At Risk

Main content:

### Schedule Recommendation

Professional compact panel:

```text
Recommended Machine     M-01
Estimated Run Time      8h 30m
Earliest Start          29 Apr 08:00
Estimated Finish        29 Apr 16:30
Material Status         Available
Delivery Risk           Low
```

Buttons:

- Review
- Confirm Schedule

Do not automatically schedule without Admin confirmation.

---

# 17. Weekly Machine Schedule

Use a timeline/calendar.

Rows:

```text
Machine 01
Machine 02
```

Columns:

```text
Mon
Tue
Wed
Thu
Fri
Sat
Sun
```

Blocks should show:

```text
SO-260921-01
Silica Gel 1g
08:00–16:00
```

Colors:

- Blue = Scheduled
- Green = Running/Completed
- Amber = Waiting Material / At Risk
- Gray = Maintenance

Admin may later drag jobs, but MVP can use Edit Schedule modal.

---

# 18. Purchase Planning Page

Header:

```text
Purchase Planning
Manage supplier orders and material shortages.
```

Metrics:

- Open POs
- Material Shortages
- Arriving Today
- Suppliers

### Material Shortage Table

Columns:

```text
Material
Required
Available
Shortage
Needed By
Priority
```

Priorities:

- Critical
- High
- Medium
- Low

---

# 19. Create Purchase Order Form

Fields:

```text
Supplier *
Material *
Quantity *
Unit *
Expected Arrival *
Remarks
```

When opened from a shortage record, prefill:

- Material
- Shortage quantity
- Needed-by date

Primary button:

`Create PO`

---

# 20. Supplier ETA Table

Columns:

```text
PO No.
Supplier
Material
Quantity
ETA
Status
```

Statuses:

- Confirmed
- In Transit
- Arriving Today
- Received
- Delayed

---

# 21. Store & Inventory Page

Header:

```text
Store & Inventory
Control stock and production material movement.
```

Metrics:

- Available Stock
- Reserved
- WIP
- Low Stock

---

# 22. Current Stock Table

Columns:

```text
Material / Item
Category
On Hand
Reserved
Available
Unit
Location
Status
```

Available is calculated:

```text
Available = On Hand - Reserved - Quarantine
```

Use status:

- Healthy
- Low Stock
- Critical

---

# 23. Issue to Production Panel

Select:

`Production Order`

Automatically show:

```text
Product
Planned Quantity
Start Date
Material Requirements
```

Material issue table:

```text
Material
Required
On Hand
Reserved
Issue Qty
Unit
```

Primary action:

`Issue to Production`

After issue:

- Inventory transaction created
- Production material status updated
- Operator sees Material Ready

---

# 24. Receiving / GRN Page

Header:

```text
Goods Receiving Note (GRN)
Record incoming supplier materials.
```

Top form:

```text
Supplier *
DO / Invoice No. *
Date *
GRN No. [auto]
```

Progress:

```text
1. Store
Received

→

2. QC
Pending

→

3. Released
Pending
```

---

# 25. Received Items Table

Columns:

```text
Material
Qty In
Damaged / Rejected
Accepted Physically
Unit
Remark
```

Buttons:

- Save Draft
- Submit QC
- Print GRN

Store should not perform final QC acceptance.

---

# 26. Incoming QC Page

Metrics:

- Pending Inspection
- Passed Today
- Rejected Today
- Released Stock

Main form:

```text
GRN Number *
Material *
Supplier
Sample Method *
Moisture Content (%) *
Appearance *
Decision
Remarks
```

For silica gel show SOP criteria clearly:

```text
Below 2.5%  → ACCEPT
Above 2.5%  → REJECT
```

The decision should be automatically suggested from moisture reading but confirmed by QC.

---

# 27. Pending Inspection Queue

Table:

```text
GRN No.
Date Received
Material
Supplier
Qty
Status
Action
```

Action:

`Inspect`

---

# 28. Operator / Production UI

This screen must be simpler than every other module.

Operator should not see dense management tables.

## Operator Start Screen

Header:

```text
My Production
Machine 01
Operator Name
```

Primary job panel:

```text
READY

Production Order
SO-260921-01

Product
Silica Gel 1g

Target
50,000 pcs

Material
READY

Start Time
Not started
```

Primary button:

`START PRODUCTION`

---

# 29. Operator Running Screen

After Start:

```text
RUNNING

Machine 01
Silica Gel 1g
Order SO-260921-01

Progress
13,200 / 50,000

Completed Cartons
2 / 10

Defects
128 pcs

Next QC
12 min
```

Primary action row:

```text
QC CHECK
RECORD DEFECT
PAUSE / DOWNTIME
COMPLETE CARTON
END PRODUCTION
```

Avoid extra widgets.

---

# 30. Operator State-Based Controls

## READY

Enabled:

- Start Production

Disabled:

- QC
- Defect
- Downtime
- Complete Carton
- End Production

---

## RUNNING

Enabled:

- QC Check
- Record Defect
- Pause
- Complete Carton
- End Production

---

## PAUSED

Enabled:

- Resume
- Add Downtime Remark
- End Production

---

## QC HOLD

Enabled:

- View QC Issue

Disabled:

- Resume until QC/Admin release

---

## COMPLETE

Display:

```text
Production Complete
Transfer cartons to Store
```

---

# 31. Production QC / SPC Page

Header:

```text
QC / SPC
In-process quality control.
```

Top layout:

Left:

### New QC Check

```text
Batch No.
Product
Time

W1
W2
W3
W4
```

Primary:

`Submit Check`

Right:

### Result

```text
Specification
0.900 – 1.200 g

Min
Max
Average

PASS
```

---

# 32. SPC Chart

Use a professional simple line chart.

Display:

- Sample result
- UCL
- CL
- LCL

Use:

- Blue solid line = sample
- Green dashed = CL
- Red dashed = UCL/LCL

Do not fill the chart with gradients.

---

# 33. QC Failure Alert

Use a clear inline red banner:

```text
Out-of-Spec Sample Detected

Sample 24
Weight 0.87g
Lower Limit 0.90g

[View Details]
```

Do not use modal popups for every warning.

---

# 34. Defect Entry

Modal or side panel.

Fields:

```text
Defect Type *
Quantity *
Remark
```

Defect type dropdown:

- White Dot
- Underweight
- Overweight
- Sealing NG
- Dimension NG
- Cutting
- Printing
- Empty Sachet
- Contamination
- Other

Button:

`Save Defect`

---

# 35. Downtime Entry

Modal:

```text
Reason *
Start Time [auto]
Remark
```

Reasons:

- Material Change
- Film Jam
- Machine Fault
- Sensor Fault
- Cleaning
- Power Interruption
- No Material
- Planned Stop
- Other

When paused show:

```text
Machine Paused
Duration 00:12:34

[Resume Production]
```

---

# 36. Complete Carton

Modal:

```text
Carton No. [auto]
Product
Batch
Quantity
Weight Confirmation
Appearance
Label
Remark
```

Primary:

`Complete Carton`

When accepted:

- Carton status = Awaiting Transfer
- Finished quantity increases

---

# 37. Finished Goods Page

Metrics:

- Finished Goods
- Awaiting Final QC
- Ready for Delivery
- Delivered Today

### Completed Cartons Table

```text
Batch
Product
Cartons
Quantity
Final QC
Delivery Status
Completed Date
```

---

# 38. Finished Goods Transfer

Action panel:

- Generate Transfer
- Confirm Receipt
- Ready for Delivery
- Print Transfer Slip

Transfer flow:

```text
Production
   ↓
Awaiting Store
   ↓
Store Received
   ↓
Awaiting Final QC
```

---

# 39. Final QC Page

Queue:

```text
Batch
Product
Quantity
Store Receipt Date
Status
Action
```

Final inspection form:

```text
Appearance
Weight
Dimension
Sealing
Sample Size
Major Defects
Minor Defects
Decision
Remarks
```

Decision:

- Accept
- Reject
- Hold

Accepted batch moves to:

`READY FOR DELIVERY`

---

# 40. Delivery Page

Metrics:

- Ready
- Scheduled
- Out for Delivery
- Delivered Today

Delivery queue:

```text
Order
Customer
Product
Cartons
Quantity
Scheduled Date
Status
Action
```

Delivery detail:

```text
Customer
Address
Order
Batch
Cartons
Quantity
Final QC
Scheduled Date
Actual Date
Remark
```

Actions:

- Prepare
- Mark Ready
- Dispatch
- Mark Delivered
- Print Delivery Slip

---

# 41. Reports Page

Filters:

```text
Date Range
Product
Machine
Customer
Export Format
```

Primary:

`Generate Report`

Top KPIs:

- Total Production
- QC Pass Rate
- Rejects
- On-Time Delivery
- Machine Utilization

Charts:

- Daily Production Output
- QC Pass Rate
- Reject Breakdown
- Delivery Trend
- Machine Utilization
- Material Consumption

Report templates:

- Production
- QC
- Inventory
- Delivery
- Calibration
- Material Consumption
- Finished Goods

---

# 42. MD Dashboard Variation

MD dashboard should prioritize:

```text
Production Today
Orders at Risk
Material Shortages
QC Failures
Machines Running
Deliveries Due
```

Main panels:

- Production vs Plan
- Delayed Orders
- Major QC Alerts
- Inventory Risks
- Delivery Performance

MD should not see operational data-entry controls.

---

# 43. Notifications

Notification dropdown groups by priority.

Example:

```text
Critical
QC failure – Machine 01

Warning
Silica Gel stock below reorder level

Info
GRN-260921-03 released by QC
```

Role-specific notification rules apply.

---

# 44. Empty States

Do not show blank tables.

Example:

```text
No pending inspections

All received materials have been inspected.
```

Action where applicable:

`View History`

---

# 45. Loading States

Use skeleton rows/cards.

Avoid full-screen spinners.

Example:

```text
[ skeleton table rows ]
```

---

# 46. Error States

Use inline form errors.

Example:

```text
Quantity must be greater than 0.
```

System errors:

```text
Could not save this record.
Your changes were not submitted.

[Try Again]
```

Never silently fail.

---

# 47. Confirmation Patterns

Use confirmation only for high-impact actions:

- Reject material
- End production
- Delete draft
- Stock adjustment
- Cancel order

Do not confirm routine actions like opening a record.

---

# 48. Modals

Use modals only for short focused actions:

- Record defect
- Record downtime
- Complete carton
- Confirm stock adjustment
- Quick status change

Long forms should be full pages or side panels.

---

# 49. Responsive Design

## Desktop

Primary supported layout.

- Sidebar visible
- Tables full width
- Multi-column cards

## Tablet

- Sidebar collapsible
- 2-column metrics
- Tables horizontally scroll if needed
- Operator actions remain large

## Mobile

Not primary for MVP.

If supported:

- Sidebar becomes menu drawer
- Tables convert to list/stack where practical
- Operator screen must remain usable

---

# 50. Accessibility

Minimum requirements:

- Keyboard accessible
- Visible focus states
- Do not communicate status by color alone
- Icon + text for critical state
- Sufficient contrast
- Labels attached to inputs
- Buttons minimum 40–44px touch height
- Status badges include text

---

# 51. Recommended Component Library

Preferred:

- Tailwind CSS
- shadcn/ui
- Radix primitives
- Lucide icons

Charts:

- Recharts
- Chart.js

Do not use heavy visual libraries unless necessary.

---

# 52. Suggested App Routes

```text
/login

/dashboard

/orders
/orders/new
/orders/[id]

/schedule

/purchase
/purchase/orders
/purchase/orders/[id]
/purchase/suppliers

/store
/store/inventory
/store/material-requests
/store/material-issue

/receiving
/receiving/grn/new
/receiving/grn/[id]

/qc/incoming
/qc/incoming/[id]
/qc/production
/qc/spc
/qc/final
/qc/calibration

/production
/production/my-job
/production/runs/[id]
/production/history

/finished-goods
/finished-goods/transfers
/finished-goods/[batchId]

/delivery
/delivery/[id]

/reports

/admin/users
/admin/customers
/admin/products
/admin/materials
/admin/machines
/admin/settings
```

---

# 53. Suggested Component Structure

```text
components/
  layout/
    app-shell.tsx
    sidebar.tsx
    topbar.tsx
    page-header.tsx

  common/
    data-table.tsx
    status-badge.tsx
    metric-card.tsx
    empty-state.tsx
    alert-banner.tsx
    confirmation-dialog.tsx
    search-filter-bar.tsx

  orders/
    order-form.tsx
    order-summary.tsx
    order-status-timeline.tsx

  schedule/
    schedule-recommendation.tsx
    machine-calendar.tsx
    production-order-table.tsx

  purchasing/
    material-shortage-table.tsx
    purchase-order-form.tsx
    supplier-eta-table.tsx

  store/
    inventory-table.tsx
    material-issue-panel.tsx
    material-request-table.tsx

  receiving/
    grn-form.tsx
    grn-items-table.tsx
    grn-status-flow.tsx

  qc/
    incoming-inspection-form.tsx
    qc-sample-form.tsx
    qc-result-card.tsx
    spc-chart.tsx
    final-qc-form.tsx

  production/
    operator-job-card.tsx
    production-controls.tsx
    production-progress.tsx
    defect-modal.tsx
    downtime-modal.tsx
    carton-modal.tsx

  finished-goods/
    finished-goods-table.tsx
    transfer-panel.tsx

  delivery/
    delivery-queue.tsx
    delivery-detail.tsx

  reports/
    report-filters.tsx
    production-chart.tsx
    qc-trend-chart.tsx
    reject-chart.tsx
```

---

# 54. Role Guard Design

Navigation and actions must be controlled by server-side permissions.

Example:

```text
MD
READ_ALL_OPERATIONAL

ADMIN
MANAGE_ORDERS
MANAGE_SCHEDULE
MANAGE_MASTER_DATA

QC
MANAGE_QC
RELEASE_MATERIAL
FINAL_QC

OPERATOR
RUN_PRODUCTION
RECORD_DEFECT
RECORD_DOWNTIME
COMPLETE_CARTON

PURCHASING
MANAGE_PURCHASE_ORDERS
MANAGE_SUPPLIERS

STORE
MANAGE_GRN
MANAGE_INVENTORY
ISSUE_MATERIAL
RECEIVE_FINISHED_GOODS
```

Do not rely only on hiding buttons in the UI.

---

# 55. Audit Display

Where useful, show:

```text
Created by
Created at
Last updated by
Last updated at
```

Order detail should include an Audit Log tab.

Audit table:

```text
Date & Time
User
Action
Record
Old Value
New Value
```

---

# 56. Form Auto-Fill Rules

Reduce user entry.

Examples:

### Admin selects Product

Auto-load:

- Compatible machines
- Standard rate
- QC range
- Materials
- Pack quantity

### Store selects Production Order

Auto-load:

- Required material list
- Required quantity
- Reserved quantity

### QC selects GRN

Auto-load:

- Supplier
- Material
- Received quantity

### Operator opens My Job

Auto-load:

- Machine
- Product
- Batch
- Target
- Materials
- QC frequency

---

# 57. Status Naming Rules

Use simple human-readable labels.

Prefer:

```text
Waiting Material
Pending QC
Ready
Running
Paused
Completed
At Risk
Rejected
Delivered
```

Avoid technical status codes in the interface.

Database can use:

```text
WAITING_MATERIAL
PENDING_QC
READY
RUNNING
```

---

# 58. Professional UI Tone

Preferred labels:

- `Create Order`
- `Submit to QC`
- `Issue Materials`
- `Start Production`
- `Complete Carton`
- `Confirm Receipt`
- `Ready for Delivery`

Avoid:

- Fun / casual phrases
- Gamified text
- Emoji
- Cartoon-style labels

---

# 59. Dashboard Density

Target:

- 4–6 KPI cards maximum per row
- 2 major panels per row
- Tables instead of too many small widgets
- Avoid more than 3 chart types on the same screen unless it is the dedicated Reports page

---

# 60. Animation

Use minimal animation.

Allowed:

- Short progress transition
- Button loading state
- Collapse/expand
- Notification appearance

Avoid:

- Bouncing icons
- Constant motion
- Decorative animation
- Animated backgrounds

---

# 61. Print / PDF Design

Digital records that reproduce legacy forms should have a print-friendly layout.

Print output should contain:

- Company logo
- Company name
- Form title
- Record number
- Date
- Data
- Approval / user records
- Page number
- Revision metadata where required

Do not print the sidebar or web UI controls.

---

# 62. Product Master Design

Product master fields should include:

```text
Product Code
Product Name
Type
Packet Weight
Unit
Packets / Carton
Compatible Machines
Standard Rate
QC Frequency
Weight Lower Limit
Weight Upper Limit
Dimension Spec
Sealing Requirement
Active Status
```

Tabs:

```text
General
Materials / BOM
QC Specification
Machine Settings
History
```

---

# 63. Material Master Design

Fields:

```text
Material Code
Material Name
Category
Unit
Supplier(s)
Reorder Level
Storage Location
QC Required?
Specification
Active
```

---

# 64. Machine Master Design

Fields:

```text
Machine Code
Machine Name
Location
Supported Products
Default Speed
Status
Calibration / Maintenance Reference
```

Statuses:

- Available
- Running
- Maintenance
- Out of Service

---

# 65. User Management Design

Admin user list:

```text
Name
Username / Email
Role
Status
Last Login
Action
```

Create User:

```text
Name
Email / Username
Role
Password / Invite
Active
```

Roles:

- MD
- Admin
- QC
- Operator
- Purchasing
- Store

---

# 66. Settings

Settings sections:

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

Examples:

### QC Settings

```text
Default Sampling Frequency: 20 min
Default Sample Size: 4
```

### Numbering

```text
Order Prefix: SO
GRN Prefix: GRN
PO Prefix: PO
Batch Prefix: BATCH
Carton Prefix: CTN
```

---

# 67. Document Numbering UX

IDs should be generated automatically.

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

Users should not manually create IDs unless Admin override is enabled.

---

# 68. Scheduling UX Rules

When Admin creates an order:

System immediately shows:

```text
Checking materials...
Checking machine capacity...
Calculating estimated run time...
```

Then display:

```text
Schedule Recommendation

Machine              M-01
Material             Ready
Earliest Start       22 Sep, 08:30
Est. Completion      23 Sep, 16:30
Delivery             25 Sep
Risk                  Low
```

Admin actions:

```text
Confirm
Change Machine
Change Start Date
```

If shortage exists:

```text
Material Shortage

Packing Roll
Required: 2
Available: 1
Shortage: 1

Purchasing notified automatically.
```

---

# 69. Cross-Department Handoff Design

Every handoff should be visible as status, not dependent on verbal communication.

Examples:

```text
Order Created
→ Store Material Check

Shortage Detected
→ Purchasing Task

Material Received
→ QC Inspection

QC Released
→ Store Available Stock

Schedule Confirmed
→ Operator Job

Production Complete
→ Store FG Receipt

Store Received
→ Final QC

Final QC Passed
→ Delivery Ready
```

Each handoff creates:

- New task
- Notification
- Status update
- Audit event

---

# 70. Design Acceptance Criteria

The UI is considered acceptable when:

1. A new user can identify the main action on each page in under 5 seconds.
2. Operator can run a normal shift without accessing Admin pages.
3. Admin can create and schedule an order without duplicate data entry.
4. Store can identify material shortages and issue quantities clearly.
5. QC can see all pending inspections immediately.
6. Purchasing can see exactly which shortages affect production.
7. MD can identify delayed orders and production risks without opening multiple screens.
8. Status colors are consistent across the entire system.
9. No page looks visually overloaded.
10. UI follows the EverDryPack blue/orange brand while remaining professional.
11. Charts are used only where they improve decisions.
12. Tables remain the main presentation for transactional records.
13. Every important action produces immediate visual feedback.
14. Mobile/tablet layout does not break.
15. The final interface does not feel cartoonish.

---

# 71. Final Design Summary

The EverDryPack interface should feel like a compact manufacturing operations platform.

The design language is:

> **White space + dark/navy text + EverDryPack blue + controlled orange accents + compact industrial tables + clear status indicators.**

The application should prioritize:

**Status → Action → Data → History**

The system must not ask employees to reproduce paper forms field-by-field unless absolutely necessary.

Instead:

- Admin enters the order once.
- System distributes information.
- Store confirms physical stock.
- Purchasing handles shortages.
- QC controls acceptance.
- Operator runs production through a few clear buttons.
- Finished goods flow to Store and QC.
- MD sees the result.

This design is intended to support a clean, professional, scalable implementation of the EverDryPack PRD.
