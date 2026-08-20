# Spaces — Product Glossary

**Purpose:** One place for **product language** so owners, designers, finance, site, and support say the same thing.  
**Technical SSOT:** Field names and write rules live in [DOMAIN-ONTOLOGY.md](../DOMAIN-ONTOLOGY.md).  
**UI help:** Status / phase legend on Projects, Work, Portfolio (Studio Web).  
**JTBD review:** [JTBD-STATUS-DOCS-MONEY.md](./JTBD-STATUS-DOCS-MONEY.md) (job vs task vs docs vs money).

**Audience:** Humans using Studio Web (and desk edge cases). Avoid ERP jargon unless needed.

---

## How to read this glossary

| Column | Meaning |
|--------|---------|
| **Say this** | Preferred studio phrase in meetings, WhatsApp, and UI labels |
| **Means** | Plain definition |
| **In the system** | Field or DocType (for implementers / power users) |
| **Do not confuse with** | Nearby terms that break conversations |

---

## 1. Project identity (admin / support language)

These three answers are **different** and all valid:

| Question | Answer |
|----------|--------|
| What is the job **called**? | **Project name** |
| What do we say on the **phone**? | **Studio project code** (when set) |
| What does the **computer** store as the primary key? | **ERP project id** |

### 1.1 Job order (studio project code)

| | |
|--|--|
| **Say this** | **Job Order** · job order number · `2608-0001` |
| **Means** | Short **human** identity for the job. New numbers are creation year/month plus one global non-resetting sequence: `YYMM-NNNN`. |
| **In the system** | `Project.studio_project_code` (unique Data; UI label **Job Order**). |
| **Examples (new format)** | `2608-0001`, `2608-0002`, `2609-0003` |
| **Examples (legacy, keep as-is)** | `FS25010`, `DS26056`, `P26010`, older freeform codes |
| **Who uses it** | Principal, project leads, admin, support, phone calls ("Which 2608-0001?") |
| **Rules** | Backend assigns at create; users cannot override it; **immutable after save**; unique. Does **not** replace ERP id. |
| **Do not confuse with** | ERP project id (`PROJ-0002`), project name ("Al Reem Villa"), customer name, invoice number |

**Product rule:** If someone asks "What's the job order?" answer with `studio_project_code`. If a developer needs the record key, use **ERP project id**.

Historical Job Order Handle data remains stored only; it has no active Studio UI, API, provisioning, or effect on new codes.

**Roadmap:** [OWNER-PRODUCT-NOTES-ROADMAP.md](./OWNER-PRODUCT-NOTES-ROADMAP.md).

### 1.2 ERP project id

| | |
|--|--|
| **Say this** | System id · ERP id · `PROJ-0002` |
| **Means** | Auto-generated **system name** of the Project document. Stable forever. Used in URLs, APIs, accounting links. |
| **In the system** | `Project.name` (primary key) |
| **Examples** | `PROJ-0001`, `PROJ-0002` |
| **Who uses it** | Finance deep links, integrations, audit logs, developers |
| **Rules** | Never invent or rename casually. Not for client-facing marketing. |
| **Do not confuse with** | Studio project code, sales invoice name (`ACC-SINV-…`) |

### 1.3 Project name

| | |
|--|--|
| **Say this** | Job title · project name |
| **Means** | Full human title of the engagement (often site + type). |
| **In the system** | `Project.project_name` |
| **Examples** | `Al Reem Villa Interior`, `[DEMO] Coastal Landscape Package` |
| **Who uses it** | Everyone in UI lists and reports |
| **Do not confuse with** | Studio code (short), customer legal name |

### 1.4 Quick compare

| | Studio project code | Project name | ERP project id |
|--|---------------------|--------------|----------------|
| **Example** | `AR-02` | Al Reem Villa Interior | `PROJ-0002` |
| **Length** | Short | Long | Fixed system style |
| **Unique** | Yes (when set) | Prefer unique titles | Always unique |
| **Spoken aloud** | Yes | Sometimes | Rarely |
| **Search** | Yes (Studio Web + desk) | Yes | Yes |
| **Legal / money docs** | Optional on prints later | Often on client docs | Always in backend links |

### 1.5 Historical naming (do not implement)

The `{studio prefix}-{sequence}` ideas below are **not** the live Job Order rule. Current software assigns `{YYMM}-{NNNN}` at create (see §1.1 and [JOB-ORDER-NUMBERING.md](./JOB-ORDER-NUMBERING.md)). Do not add a user-editable prefix field.

**Legacy stored examples** (keep as-is; do not bulk-rewrite):

```text
{studio prefix}-{sequence}   # historical convention only
```

| Studio style (historical) | Example still stored |
|---------------------------|----------------------|
| Site / client initials | `AR-02` (Al Reem #2) |
| Discipline prefix | `INT-014`, `EXT-003`, `LND-008` |
| Year + sequence | `26-041` |

**Avoid:** Reusing codes after cancel; inventing a second code field; treating this section as a create-time override.

---

## 1.6 Project category

| | |
|--|--|
| **Say this** | Project category · type of project |
| **Means** | What kind of job this is (design studio work, fit-out, print, construction, or a custom type the studio adds). |
| **In the system** | `Project.studio_discipline` (Select; UI label **Category**) |
| **Defaults** | Design, Fitout, Printing, Construction |
| **Extensible** | User can add other types from New project ("Add other type…"); options append via `append_select_option` |
| **Do not confuse with** | Job status (where the job is in delivery), job scale (capacity complexity only) |

Legacy values (Interior / Exterior / Landscape / Mixed) may still exist on older rows until remapped.

## 1.7 Project people (Lead and Support)

| | |
|--|--|
| **Say this** | People · Lead · Support |
| **Means** | Who is on the job. **Lead** owns delivery. **Support** helps. Several people allowed. |
| **In the system** | `Project Team Member` via `set_project_people`. Lead also writes `Project.lead_architect`. Full write rules: [ASSIGNMENT.md](../02-architecture/ASSIGNMENT.md). |
| **Do not confuse with** | Task people (same words, different writer). Retired communication-owner data. |

## 1.7b Task people (Lead and Support)

| | |
|--|--|
| **Say this** | People · Lead · Support |
| **Means** | Who does the task. **Lead** owns it. **Support** helps. |
| **In the system** | `Task._assign` JSON via `set_task_people`. First id is Lead. |
| **Do not confuse with** | Project people; ERP single ToDo assignee |

## 1.8 Location

| | |
|--|--|
| **Say this** | Location · site location |
| **Means** | Where the job is (site / plot / city text). |
| **In the system** | `Project.site_address` |
| **Later** | Multi-site child table if one job has several sites |

## 1.9 Project value

| | |
|--|--|
| **Say this** | Project value |
| **Means** | Commercial envelope / target value of the job (not legal invoices). |
| **In the system** | `Project.overall_budget` (label Project value) |
| **Rules** | Do **not** show on Project Overview primary strip; Money tab owns value + VAT (VAT fields Wave 3). |
| **Do not confuse with** | SI outstanding, PE cash, burn from PIs |

## 2. Job progress (prefer one status)

**Product direction (2026-08):** Prefer a **single job status** on Projects. Legacy design phase remains in the DB for older data but is not the primary Projects list control.

### 2.0 Job status (canonical list)

| Say this | System value (`project_lifecycle_status`) |
|----------|-------------------------------------------|
| Moodboard prep | `Moodboard prep` |
| Under design | `Under design` |
| Shop drawings preparation | `Shop drawings preparation` |
| Construction | `Construction` |
| On hold | `On hold` |
| Completed | `Completed` |

Legacy Kickoff / Design / Procurement / Execution / Handover / On Hold are remapped on ensure. See `phase_gates.py` and owner roadmap.

| Say this | Means | System field | Not the same as |
|----------|-------|--------------|-----------------|
| **Job stage** | Commercial / ops stage of the job | `project_lifecycle_status` | Design phase, ERP open/closed |
| **Design phase** | Design progress (drawings / deliverables) | `design_phase` | Job stage |
| **ERP open / closed** | Books flag: job still open or finished/cancelled | `Project.status` | Job stage |

**One-liner for training:**  
*Job stage = where the studio is on the job. Design phase = where design is. ERP open = is the job still open in the books.*

### Job stage values

See **2.0 Job status (canonical list)** above. Do not use the leftover Kickoff / Design / Procurement list. JTBD review: [JTBD-STATUS-DOCS-MONEY.md](./JTBD-STATUS-DOCS-MONEY.md).

### Design phase values

Brief · Concept · Schematic · Design Development · Construction · Handover

### ERP project status values

Open · Completed · Cancelled

---

## 3. Delivery deadlines

| Say this | Means | System | Notes |
|----------|-------|--------|-------|
| **Hard delivery date** | Must-hit job end (client / contract / expected end) | `expected_end_date` | Drives **urgent** jobs when overdue or within 7 days |
| **Soft delivery date** | Internal target | `delivery_soft_deadline` | Planning only; does not rewrite invoices |
| **Weekly project** | Soft or hard due this calendar week, or overdue open job | Derived | Home + Projects focus chip |
| **Urgent delivery** | Hard overdue or hard within 7 days (open) | Derived | Home + Projects focus chip |
| **Soft task deadline** | Planning due date on a task | `exp_end_date` + `deadline_kind=Soft` | Snooze may move the date |
| **Hard task deadline** | Client package / must-hit task due | `exp_end_date` + `deadline_kind=Hard` | Same date field; kind is the severity label |
| **Overdue task** | Open task with due date before today | `exp_end_date` + status not complete | Overdue SSOT for Home cards |

---

## 4. Pipeline (enquiry → job)

| Say this | Means | System |
|----------|-------|--------|
| **Enquiry / lead** | Unqualified inbound interest | `Lead` |
| **Opportunity** | Qualified pursuit / fee conversation | `Opportunity` |
| **Converted / won job** | Opportunity won → creates **Project** | Opportunity status → convert automation |
| **Project type (pipeline)** | Interior / Exterior / Landscape / Mixed on enquiry | `Lead.project_type`, `Opportunity.project_type` |
| **Studio discipline** | Same four values on the live job | `Project.studio_discipline` |

---

## 5. Money language

| Say this | Means | System | Not the same as |
|----------|-------|--------|-----------------|
| **Fee plan / client milestone** | Planned bill event | `project_client_milestones` | Legal Sales Invoice |
| **Sales invoice (SI)** | Legal tax invoice to client | `Sales Invoice` | Fee plan row |
| **Payment (PE)** | Cash movement against invoice(s) | `Payment Entry` | "Paid" typed by hand on a plan row |
| **Still to collect / AR** | Client unpaid balances | AR / SI outstanding | Fee plan status alone |
| **Supplier pay plan** | Planned pay-outs | `project_supplier_payments` | Purchase Invoice |
| **Purchase invoice (PI)** | Legal supplier bill | `Purchase Invoice` | Material cost plan line |
| **Burn** | Spent vs job budget | `budget_spent`, `budget_burn_pct` | Soft delivery risk |

**Product rule:** **Paid** on a fee milestone means cash is real (Payment Entry path), not a checkbox wish.

---

## 6. People

| Say this | Means | System |
|----------|-------|--------|
| **Client / customer** | Billing party | `Customer` (UI may say Client) |
| **Legacy communication owner** | Historical follow-up-owner data; not used by Studio product logic | `communication_owner` |
| **Lead architect** | Design lead on the job | `lead_architect` |
| **Subcontractor** | Studio word for paid execution supplier | Often `Supplier` + project partner/pay rows |

---

## 6b. Capacity and engagement

| Say this | Means | System / default |
|----------|-------|------------------|
| **Engagement (per project)** | How committed a person is on **one** job: Full Time, Part Time, or Advisory | `Project Team Member.engagement` |
| **Full Time (on a job)** | Primary focus on that project | Weight **1.0** load unit |
| **Part Time (on a job)** | Shared across jobs | Weight **0.5** |
| **Advisory** | Light review / oversight only | Weight **0.25** |
| **Allocation %** | Optional exact % override of engagement | `allocation_pct` (100 = full) |
| **Base load** | Sum of engagement weights across open projects | Capacity board |
| **Follow-up / change buffer** | Standard allowance for RFIs, revisions, client changes, overdue chase | Default **20%** |
| **Effective load** | Base load × (1 + buffer%) | e.g. 1.0 FT + 0.5 PT = 1.5 base → **1.8** effective |
| **Capacity band** | Light / OK / Heavy from effective load | Default: light ≤ 1.8, ok ≤ 3.5, else heavy |
| **Project count** | Number of open jobs the person is on | Capacity board |
| **Changes (capacity)** | Pending / draft variations on their jobs | `Project Studio Variation` |
| **Follow-ups (capacity)** | Overdue open tasks on their jobs | Task overdue SSOT |

**Product rule:** Capacity is **not** timesheet hours. It is planned engagement load plus a **fixed 20%** for follow-ups and changes so architects are not planned at 100% on pure delivery.

**Configure:** Set engagement on each Project → Team row (desk). Lead architect alone (no team row) counts as Full Time on that job. Buffer and weights live in `spaces_studio.setup.capacity` (site conf `spaces_capacity` can override).

---

## 7. Support / admin cheat sheet

When a call or ticket says…

| They say | You look up | Field / place |
|----------|-------------|----------------|
| "AR-02" | Studio project code | Projects list **Code** column; search box |
| "PROJ-0002" | ERP project id | Subtitle under project name; desk URL |
| "Al Reem villa" | Project name | Projects list title |
| "Is the job still open?" | ERP open/closed **or** job stage | Prefer job stage for studio; ERP for books |
| "Where is design?" | Design phase | Projects column / filters |
| "Why is it urgent?" | Hard delivery date | Hard due column; Home urgent card |
| "Invoice paid?" | Payment Entry / SI outstanding | Finance; not fee plan alone |

---

## 8. UI labels (Studio Web SSOT)

Prefer these labels in product copy:

| UI label | Glossary term |
|----------|---------------|
| **Code** | Studio project code |
| **Project** | Project name (with ERP id as secondary text) |
| **Job stage** | Lifecycle / job stage |
| **Design phase** | Design phase |
| **ERP open** | ERP project status |
| **Soft due** | Soft delivery date (project) |
| **Hard due** | Hard delivery date (project) |
| **Task status** | Task work status |
| **Deadline** (Soft/Hard on Work) | Task deadline kind |

---

## 9. Related docs

| Doc | Use when |
|-----|----------|
| [DOMAIN-ONTOLOGY.md](../DOMAIN-ONTOLOGY.md) | Engineering SSOT, effect graph, write rules |
| [USER-GUIDE.md](../USER-GUIDE.md) | Logins and day-to-day links |
| [OWNER-DASHBOARD.md](../OWNER-DASHBOARD.md) | Owner command center pitch |
| Studio Web **Legend** button | Status / phase confusion on Projects & Work |

---

## Document control

| Version | Date | Notes |
|---------|------|-------|
| 1.0 | 2026-07-19 | Product glossary; studio project code admin/support definitions |
