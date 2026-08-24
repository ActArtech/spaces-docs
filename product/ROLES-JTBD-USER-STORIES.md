# Spaces: Roles, Jobs-to-be-Done, User Stories, Config Validation, Gaps

**Audience:** Product, owner review, QA, agents implementing RBAC  
**Live:** https://spaces.keyteller.com/studio/ (primary) · https://spaces.keyteller.com/desk (ERP edge / infrastructure)  
**Reviewed:** 2026-07-24 (JTBD matrix vs Wave 0 live + Wave L local). Alignment pass 2026-08-17.

**Product UI policy:** **Studio Web only** for all roles. Desk is retired as UX. See [DESK-RETIREMENT.md](./DESK-RETIREMENT.md).

**This file is the role-jobs SSOT** (Frappe profiles stay here). It is not the WR register, not live-vs-local, and not build-next.

| Need | Doc |
|------|-----|
| Alignment / how to resolve stale lines | [ONTOLOGY-JTBD-ALIGNMENT.md](./ONTOLOGY-JTBD-ALIGNMENT.md) |
| Product words | [PRODUCT-GLOSSARY.md](./PRODUCT-GLOSSARY.md) + [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md) |
| WR ids | [GAPS.md](../04-studio-web/GAPS.md) |
| Dated live-vs-local | [GAP-ANALYSIS-2026-08-24.md](./GAP-ANALYSIS-2026-08-24.md) |
| Build next | [DEVELOPMENT-PRIORITIES.md](./DEVELOPMENT-PRIORITIES.md) |
| Actor overlay (not new Frappe roles) | [ACTOR-JOBS-AGENTIC-STAGE.md](./ACTOR-JOBS-AGENTIC-STAGE.md) |

**Do not implement from this file:** desk Settings as product UX; SPOC / `communication_owner` as a live job; Hours tab as daily Work; retired project tabs `Overview, Variations, Finance, Docs`. Current project tabs: Overview, People, Activity, Money, Payments, Docs, Variations.

**Related:** [USER-GUIDE.md](../06-guides/USER-GUIDE.md) · [JOURNEY-MATRIX.md](../02-architecture/JOURNEY-MATRIX.md) · [FEATURE-GAPS.md](./FEATURE-GAPS.md) (historical 2026-07 Desk inventory) · [OWNER-SHARE-READINESS.md](./OWNER-SHARE-READINESS.md)

### Status legend (this file)

| Mark | Meaning |
|------|---------|
| **Live** | Verified on VPS Studio Web (Wave 0 ship HEAD ~2578575 / d71f2a6) |
| **Local** | Coded in tree; needs commit + ship + hard refresh |
| **Partial** | Works with seed / caveats |
| **Gap** | Missing or not owner-usable without desk / support |

---

## 0. Role system SSOT

### 0.1 Role profiles (assign these to users)

| Role profile | Frappe role granted | Demo login (`SpacesDemo2026!`) |
|--------------|---------------------|--------------------------------|
| **Studio Principal Profile** | `Studio Principal` | `owner@spaces-demo.local` |
| **Studio Owner Lite Profile** | `Studio Owner Lite` | `ownerlite@spaces-demo.local` |
| **Studio Design Lead Profile** | `Studio Design Lead` | `lead.arch@`, `architect1@` / `architect2@` / `architect3@` / `designer@spaces-demo.local` |
| **Studio Site Profile** | `Studio Site and Contracts` | `site@spaces-demo.local` |
| **Studio Finance Profile** | `Studio Finance` | `finance@spaces-demo.local` |

**Rule:** Assign a **Role Profile**, not raw ERPNext roles. Run `normalize_studio_user` after manual role edits.

### 0.2 Access matrix (Studio Web vs desk)

| Route / workspace | Principal | Owner Lite | Design Lead | Site | Finance | Viewer |
|-------------------|:---------:|:----------:|:-----------:|:----:|:-------:|:------:|
| Home | yes | yes | yes | yes | yes | yes |
| Portfolio | yes | yes | yes | yes | yes | yes |
| Pipeline / Opportunities | yes | yes | yes | no | no | yes |
| Projects | yes | yes | yes | yes | no | yes |
| Work (tasks) | yes | yes | yes | yes | no | yes |
| Capacity | yes | yes | yes | no | no | yes |
| Site | yes | yes | no | yes | no | yes |
| Finance | yes | yes | no | no | yes | yes |
| Admin (`/studio/admin`) | yes | no | no | no | no | no |
| Settings (desk Studio Settings) | yes | no | no | no | no | no |
| Settings nav (web Appearance/Profile shell) | yes | no | no | no | no | no |
| Appearance via profile menu | yes | yes | yes | yes | yes | yes |
| Personal `/settings/*` paths | yes | yes | yes | yes | yes | yes |

**Home layout keys** (`home_layout.resolve_home_layout`):

| Role | Layout | Focus of cards |
|------|--------|----------------|
| Principal, Owner Lite, Admin | `owner` | Money, delivery risk, decisions |
| Design Lead | `design` | Overdue/week tasks, lead-architect jobs, variations (no AR). SPOC / `communication_owner` is leftover language, not product logic |
| Finance | `finance` | Balance due, payments, draft SI, key clients |
| Site | `site` | Issues, deliver jobs, week tasks (no supplier payables card) |

**Default Studio Web landing** (`defaultStudioLandingPath`):

| Role | Lands on |
|------|----------|
| Principal / Owner Lite | `/studio/` |
| Design Lead | `/studio/work` |
| Site | `/studio/site` |
| Finance | `/studio/finance` |

### 0.3 Capability gates (not only nav)

| Capability | Who |
|------------|-----|
| Approve / reject variations; Approve and invoice | Principal, Owner Lite, Administrator |
| Set lead architect | Principal, Owner Lite, Administrator |
| Convert Opportunity (job won) | Principal (profile intent); needs write on Opportunity |
| Create system users / company / theme | Principal (`/studio/admin`) |
| Create/submit SI, PI, PE, JE | Principal + Finance (DocType perms) |
| Create/submit MR, PO, site Issue | Principal + Site |
| Create Lead / Opportunity | Principal, Design Lead, Owner Lite |

---

## 1. Studio Principal (owner / managing director)

**Persona:** Runs the studio commercially and operationally. Needs one screen for money and decisions, full control of team and branding.

**Demo:** `owner@spaces-demo.local`  
**Surfaces:** All Studio Web routes. Desk Settings is infrastructure, not a product journey. Daily Admin is `/studio/admin`.

### 1.1 Jobs to be done (10)

| ID | Job to be done | Outcome |
|----|----------------|---------|
| P-J01 | See what needs my decision today without opening Excel | Home attention + variation cards |
| P-J02 | Know how much clients owe and which jobs | Money hero + balance-due jobs |
| P-J03 | Approve a change order and bill the client | Approve / Approve and invoice |
| P-J04 | Win a job from opportunity to project | Opportunity Converted → Project |
| P-J05 | Assign who owns delivery | Lead architect / project Lead on the team. SPOC / `communication_owner` is legacy stored data, not a Studio product job |
| P-J06 | Track burn and phase health across portfolio | Portfolio + burn table |
| P-J07 | Raise or oversee client invoices on a project | Project Money (SI/PI) + Payments (PE). Company Finance is the same documents, not a second ledger |
| P-J08 | Onboard a new team member with the right access | Admin → Add user + role profile |
| P-J09 | Keep company identity and currency correct | Admin → Edit company |
| P-J10 | Share a safe status pack with a client | Client pack link from project |

### 1.2 User stories (10)

| ID | Story | Acceptance (happy path) |
|----|-------|-------------------------|
| P-US01 | As Principal, I open Studio Home so I see overdue tasks, fees due, and variations needing action. | Cards load; empty states when sparse |
| P-US02 | As Principal, I approve a pending variation so the client milestone and draft SI path run. | Status approved; optional draft SI |
| P-US03 | As Principal, I approve and invoice in one action so finance is not a second hop. | Draft SI created / linked per automation |
| P-US04 | As Principal, I convert a won opportunity so a Project and starter tasks appear. | Project linked; tasks from template |
| P-US05 | As Principal, I set lead architect on a job so capacity and ownership are clear. | Field saved; portfolio can reassign |
| P-US06 | As Principal, I open Portfolio burn so I see spent vs budget per job. | Burn % for PROJ-0002 demo |
| P-US07 | As Principal, I open Work to complete or snooze overdue tasks. | Complete / Snooze works |
| P-US08 | As Principal, I invite a designer with Design Lead profile. | User appears in Admin team |
| P-US09 | As Principal, I change company currency / TRN for invoices. | Company form saves |
| P-US10 | As Principal, I open Admin theme and set brand colors. | Theme Settings updated |

### 1.3 Step-by-step config validation (Principal)

| Step | Action | Expected | Pass? |
|------|--------|----------|-------|
| 1 | Login `owner@spaces-demo.local` → https://spaces.keyteller.com/studio/ | Home owner layout; nav shows Home, Portfolio, Pipeline, Projects, Work, Capacity, Finance, Site, Admin | |
| 2 | Confirm role profile on User = Studio Principal Profile | Roles include `Studio Principal` | |
| 3 | Open `/studio/admin` | Company card + team + role profiles + Add user | |
| 4 | Open `/studio/work` | Task list loads (including unassigned) | |
| 5 | Open project PROJ-0002 command sheet | Overview, People, Activity, Money, Payments, Docs, Variations | |
| 6 | Variations: Principal sees Approve / Reject | `lead.arch@` may also if Principal; Design Lead must not | |
| 7 | Desk: Studio Settings workspace (infrastructure only; not product UX) | Users/company/theme exist for support. Daily product Admin is `/studio/admin` | |
| 8 | Module profile Studio Desk | No Stock/Manufacturing noise in modules | |
| 9 | `bash scripts/agent-gate.sh` or contract_audit | Green including role contracts | |
| 10 | Hard refresh Ctrl+Shift+R after deploy | Assets match ship | |

---

## 2. Studio Owner Lite (co-owner / light menu)

**Persona:** Same commercial decisions as owner, without Settings/Admin clutter or (on desk) Site menu noise. On Studio Web, Site remains reachable by product choice.

**Demo:** `ownerlite@spaces-demo.local`  
**Surfaces:** Home, Portfolio, Pipeline, Projects, Work, Capacity, Finance, Site (web). **No** Admin. **No** desk Studio Settings.

### 2.1 Jobs to be done (10)

| ID | Job to be done | Outcome |
|----|----------------|---------|
| OL-J01 | Run daily owner decisions without ERP settings | Owner home layout |
| OL-J02 | Approve variations like Principal | Same variation APIs |
| OL-J03 | See AR and jobs owed money | Finance + money cards |
| OL-J04 | Work pipeline, projects, and site without Admin clutter | Pipeline + Projects + Site |
| OL-J05 | Complete tasks and review portfolio | Work + Portfolio |
| OL-J06 | Create invoices when needed | Finance write path |
| OL-J07 | Avoid accidental company/user admin | No Admin route |
| OL-J08 | Use capacity for staffing view | Capacity board |
| OL-J09 | Follow client pack feedback | Home feedback card |
| OL-J10 | Stay out of Principal Admin | No Admin route |

### 2.2 User stories (10)

| ID | Story | Acceptance |
|----|-------|------------|
| OL-US01 | As Owner Lite, I log in and land on Studio Home owner layout. | Layout key owner |
| OL-US02 | As Owner Lite, I do not see Admin in the sidebar. | `canSeeStudioRoute(..., 'admin')` false |
| OL-US03 | As Owner Lite, I approve a variation on Home. | Approve succeeds |
| OL-US04 | As Owner Lite, I open Finance and list sales invoices. | SI list loads |
| OL-US05 | As Owner Lite, I open Pipeline and create a lead. | Lead created |
| OL-US06 | As Owner Lite, I open Projects and edit phase inline. | Phase saves |
| OL-US07 | As Owner Lite, I open Work and see my open / overdue views. | Saved views work |
| OL-US08 | As Owner Lite, if desk is opened for support I do not see Studio Settings. | Infrastructure check only; product UX stays Studio Web |
| OL-US09 | As Owner Lite, I open Site on Studio Web. | `/studio/site` allowed |
| OL-US10 | As Owner Lite, I cannot invite users via Studio Admin. | Admin 403 / no nav |

### 2.3 Step-by-step config validation (Owner Lite)

| Step | Action | Expected | Pass? |
|------|--------|----------|-------|
| 1 | Login `ownerlite@spaces-demo.local` | Profile = Studio Owner Lite Profile | |
| 2 | Studio Web nav | No Admin; has Finance, Pipeline, Projects, Work, Site | |
| 3 | Desk (if opened; infrastructure only) | No Studio Settings; Site present for parity (product is still web) | |
| 4 | Home variation approve | Buttons present for owner-class roles | |
| 5 | Direct URL `/studio/admin` | Permission error or redirect | |
| 6 | Finance create SI (if perms allow) | Create context flags true or explicit deny | |
| 7 | Capacity board | Loads engagement data | |
| 8 | Convert opportunity | Allowed if write perms; document if blocked | |
| 9 | Theme / company edit | Only via desk edge or Principal | |
| 10 | Re-login as owner | Admin returns | |

---

## 3. Studio Design Lead (architect / designer / PM)

**Persona:** Delivers drawings and tasks, manages pipeline intake, does not run GL or procurement.

**Demo:** `designer@spaces-demo.local` or `architect1@spaces-demo.local`  
**Demo note:** `lead.arch@spaces-demo.local` is **Design Lead Profile** (G-ROLE-01 fixed 2026-07-23).

**Surfaces:** Home (design layout), Portfolio, Capacity, Pipeline, Opportunities, Projects, Work. Landing: **Work**. No Finance, no Site, no Admin.

### 3.1 Jobs to be done (10)

| ID | Job to be done | Outcome |
|----|----------------|---------|
| D-J01 | See my delivery work first thing | Land on Work |
| D-J02 | Update task status (list / kanban / calendar) | Status persists |
| D-J03 | Complete or snooze a task | Due date / status update |
| D-J04 | Capture a new enquiry as Lead | Lead in Pipeline |
| D-J05 | Move opportunity through pipeline (not convert if restricted) | Status edits |
| D-J06 | Edit project design phase and docs | Project command sheet |
| D-J07 | Log time on a job | Support/API timesheet only. Hours tab is not daily Work. Timesheet v2 is WR-01 (P3), not a build from this row |
| D-J08 | See jobs where I am lead architect | Home / portfolio cards |
| D-J09 | Avoid seeing studio AR clutter | Design home cards only |
| D-J10 | Cannot approve commercial variations | No approve buttons |

### 3.2 User stories (10)

| ID | Story | Acceptance |
|----|-------|------------|
| D-US01 | As Design Lead, I log in and land on `/studio/work`. | default landing work |
| D-US02 | As Design Lead, I do not see Finance or Site or Admin. | Nav filtered |
| D-US03 | As Design Lead, I drag a kanban card to Working. | Status saved |
| D-US04 | As Design Lead, I complete an assigned task. | Status Completed |
| D-US05 | As Design Lead, I create a Lead with project type Interior. | Lead listed |
| D-US06 | As Design Lead, I open Opportunities and change status. | Inline or form save |
| D-US07 | As Design Lead, I open a project Docs tab and add a link. | Docs table updates |
| D-US08 | As Design Lead, Home shows delivery cards not AR totals. | design layout |
| D-US09 | As Design Lead, I open Capacity and see engagement load. | Board loads |
| D-US10 | As Design Lead, variation rows have no Approve. | Owner-only gate |

### 3.3 Step-by-step config validation (Design Lead)

| Step | Action | Expected | Pass? |
|------|--------|----------|-------|
| 1 | Login `designer@spaces-demo.local` | Role profile Design Lead | |
| 2 | Landing | `/studio/work` | |
| 3 | Nav keys | home, portfolio, capacity, pipeline, opportunities, projects, work (+ settings link if shown) | |
| 4 | `/studio/finance` deep link | Blocked or empty / not in nav | |
| 5 | `/studio/site` deep link | Blocked | |
| 6 | Home layout API | `layout=design` or design cards only | |
| 7 | Task create on project | Allowed | |
| 8 | Variation approve API as designer | PermissionError | |
| 9 | Convert Opportunity | Document: often Principal-only in process; verify write | |
| 10 | Desk: Projects + Pipeline, no Studio Finance workspace | Workspace filter | |

---

## 4. Studio Site (site and contracts / procurement)

**Persona:** Field issues, material requests, purchase orders. Not client AR or design chrome.

**Demo:** `site@spaces-demo.local`  
**Role name:** `Studio Site and Contracts` · **Profile:** Studio Site Profile  
**Surfaces:** Home (site layout), Portfolio, Projects, Work, Site. Landing: **Site**.

### 4.1 Jobs to be done (10)

| ID | Job to be done | Outcome |
|----|----------------|---------|
| S-J01 | See open site issues for projects | Site issues tab + home card |
| S-J02 | Raise a material request on a project | MR draft |
| S-J03 | Create a purchase order against a supplier | PO draft / submit |
| S-J04 | Assign an issue to a user | Assignee set |
| S-J05 | Submit draft MR/PO when ready | docstatus submitted |
| S-J06 | Find the project for a PO | Project link + name |
| S-J07 | Avoid supplier payment commercial queue | No supplier_payments_due on site home |
| S-J08 | See delivery jobs that are on site | deliver_projects / on hold |
| S-J09 | Update task status when field work blocks design | Work slice |
| S-J10 | Not touch finance invoices | No Finance nav |

### 4.2 User stories (10)

| ID | Story | Acceptance |
|----|-------|------------|
| S-US01 | As Site, I log in and land on `/studio/site`. | Landing site |
| S-US02 | As Site, I only see Home, Portfolio, Projects, Work, Site. | Nav |
| S-US03 | As Site, I create an Issue linked to PROJ-0002. | Issue listed |
| S-US04 | As Site, I create a Material Request for a project. | MR appears |
| S-US05 | As Site, I create a Purchase Order with supplier. | PO appears |
| S-US06 | As Site, I submit a draft MR. | Submitted |
| S-US07 | As Site, I assign a site issue. | Assignee saved |
| S-US08 | As Site, Home is site-focused (issues, not AR). | site layout |
| S-US09 | As Site, I open Work for project overdue tasks. | List filters |
| S-US10 | As Site, Finance deep link is not available. | No finance |

### 4.3 Step-by-step config validation (Site)

| Step | Action | Expected | Pass? |
|------|--------|----------|-------|
| 1 | Login `site@spaces-demo.local` | Profile Studio Site Profile; role `Studio Site and Contracts` | |
| 2 | Landing | `/studio/site` | |
| 3 | `get_site_create_context` | can_create flags for MR/PO/Issue | |
| 4 | Create MR on PROJ-0002 | Success | |
| 5 | Create PO | Success | |
| 6 | Create Issue | Success | |
| 7 | Home cards | No client_balance_due | |
| 8 | Pipeline deep link | Not in nav | |
| 9 | Desk Site and Procurement workspace | Visible | |
| 10 | Desk Studio Finance workspace | Hidden | |

---

## 5. Studio Finance (accountant)

**Persona:** AR/AP, payments, journals, project-linked invoices. Minimal design chrome.

**Demo:** `finance@spaces-demo.local`  
**Surfaces:** Home (finance layout), Portfolio, Finance. Landing: **Finance**.

### 5.1 Jobs to be done (10)

| ID | Job to be done | Outcome |
|----|----------------|---------|
| F-J01 | Land in finance workspace immediately | `/studio/finance` |
| F-J02 | Create draft sales invoice with project | SI draft |
| F-J03 | Submit sales invoice | SI submitted |
| F-J04 | Record client payment against SI | PE |
| F-J05 | Create / submit purchase invoice | PI |
| F-J06 | Create journal entry draft and submit | JE |
| F-J07 | Review AR/AP aging and profitability | Finance reports |
| F-J08 | Create leaf COA account when needed | COA create |
| F-J09 | See collections and draft SI on Home | finance layout cards |
| F-J10 | Not manage tasks or site procurement | No Work/Site nav |

### 5.2 User stories (10)

| ID | Story | Acceptance |
|----|-------|------------|
| F-US01 | As Finance, I log in and land on `/studio/finance`. | Landing finance |
| F-US02 | As Finance, I create a Sales Invoice draft for a customer + project. | Draft row |
| F-US03 | As Finance, I submit that SI. | docstatus 1 |
| F-US04 | As Finance, I record a payment from the SI row. | PE linked |
| F-US05 | As Finance, I open Ledger trial balance for this month. | TB rows |
| F-US06 | As Finance, I open general ledger filtered by account. | GL lines |
| F-US07 | As Finance, I create a Journal Entry draft. | JE draft |
| F-US08 | As Finance, Home shows collections cards not task board. | finance layout |
| F-US09 | As Finance, I do not see Pipeline / Projects / Work / Site. | Nav |
| F-US10 | As Finance, I may see Settings link but not Principal Admin user invite. | Admin false |

### 5.3 Step-by-step config validation (Finance)

| Step | Action | Expected | Pass? |
|------|--------|----------|-------|
| 1 | Login `finance@spaces-demo.local` | Profile Finance; role Studio Finance | |
| 2 | Landing | `/studio/finance` | |
| 3 | `get_finance_create_context` | can_create_si / pe / je flags | |
| 4 | Create SI draft on PROJ-0002 customer | Success | |
| 5 | Submit SI | Success if accounts set | |
| 6 | AR aging report tab | Data or empty-state | |
| 7 | JE create (M8c) | Draft created | |
| 8 | Mode of Payment readable | No 403 on create payment | |
| 9 | Address read for SI | No 403 | |
| 10 | Variation approve | Not available | |

**Known config dependency:** Bank / Mode of Payment / company accounts must exist for PE and some submit paths. Demo seed covers pilot; new companies need ERP accounts setup.

---

## 6. Cross-role config checklist (platform)

Run once per environment after seed.

| # | Config item | How to validate | Owner role |
|---|-------------|-----------------|------------|
| 1 | Role Profiles exist | Desk → Role Profile: five Studio * Profile | Principal / Admin |
| 2 | Module Profile Studio Desk | Blocks Stock/HR/Manufacturing | Principal |
| 3 | Demo users seeded | `seed-dummy.sh` or Admin team list | Principal |
| 4 | Company + currency AED | Admin company card | Principal |
| 5 | Domains CRM, Projects, Selling, Buying, Accounts | Company domains | Admin |
| 6 | Letter head Spaces + print formats | Print SI | Finance |
| 7 | Webhook token for lead intake | Pipeline intake card / `ensure_webhook_token` | Principal |
| 8 | Opportunity Converted automation | Convert creates Project | Principal |
| 9 | Task assign JSON valid | Work list no Server Error | Design |
| 10 | Contract audit | `contract_audit` passed true | Ops |

```bash
# VPS
cd /opt/spaces
bash scripts/agent-gate.sh
# or
docker compose -f deploy/docker-compose.yml -f deploy/docker-compose.staging.yml \
  exec -T backend bench --site spaces.keyteller.com execute spaces_studio.setup.audit.contract_audit
```

---

## 6b. JTBD status matrix (dated 2026-07-24)

**Historical scorecard.** Do not treat **Live** / **Local** here as current WR status. Current WR ids: [GAPS.md](../04-studio-web/GAPS.md). Current project tabs: Overview, Activity, Money, Payments, Docs, Variations. Hours tab was later removed from daily Work. WR-03 cancel/amend and WR-16 list power later shipped.

Honest scorecard at write time: **Live** = Wave 0 on VPS. **Local** = Wave L list power (sort / sticky / CSV / saved views) not yet ship-proven for all slices (later shipped as WR-16).

### Principal (P-J01..P-J10)

| ID | Job | Status | Notes |
|----|-----|--------|-------|
| P-J01 | Decisions today | **Partial Live** | Home attention + variations; sparse demo feels empty |
| P-J02 | Clients owe / which jobs | **Partial Live** | Money cards + Finance AR |
| P-J03 | Approve change order + bill | **Live** | Core strength |
| P-J04 | Win job → project | **Live** | Convert button + New project; need owner QA script |
| P-J05 | Assign lead architect | **Live** | Team Lead. SPOC / `communication_owner` retired from product logic (historical note on this 2026-07-24 row) |
| P-J06 | Portfolio burn / health | **Partial Live** | Review yes; not full portfolio edit OS |
| P-J06b | Per-architect activity (what they did) | **Local** | Capacity Activity log (WR-20); ship pending |
| P-J07 | Invoices on project | **Live** (tabs; dated row) | Current: Money + Payments. Dated note "Finance tab; cancel/amend still gap" is historical. WR-03 shipped. |
| P-J08 | Onboard team member | **Live** | Admin Add user |
| P-J09 | Company identity | **Live** | Admin company |
| P-J10 | Client pack | **Partial Live** | Link yes; full portal no |

### Owner Lite (OL-J01..OL-J10)

| ID | Job | Status | Notes |
|----|-----|--------|-------|
| OL-J01 | Owner decisions no Settings | **Live** | Owner home |
| OL-J02 | Approve variations | **Live** | Same as Principal |
| OL-J03 | See AR | **Live** | Finance |
| OL-J04 | Pipeline + Projects + Site | **Live** | New client / project on web |
| OL-J05 | Tasks + portfolio | **Live** | Work saved views |
| OL-J06 | Create invoices | **Partial Live** | Perms + company accounts |
| OL-J07 | No Admin clutter | **Live** | By design |
| OL-J08 | Capacity | **Partial Live** | Board loads; depth thin |
| OL-J09 | Client pack feedback | **Partial Live** | Depends on seed / packs |
| OL-J10 | Stay out of Admin | **Live** | |

### Design Lead (D-J01..D-J10)

| ID | Job | Status | Notes |
|----|-----|--------|-------|
| D-J01 | Land on Work | **Live** | |
| D-J02 | Update task status (list/kanban/cal) | **Live** | |
| D-J03 | Complete / snooze | **Live** | Bulk + row |
| D-J04 | New enquiry Lead | **Live** | Pipeline New client |
| D-J05 | Opportunity status | **Live** | Inline + create sheet |
| D-J06 | Project phase + docs | **Live** | Command sheet Overview + Docs |
| D-J07 | Log time | **Support only** (historical: Hours v1) | Hours tab removed from daily Work. WR-01 timesheet v2 is P3. Do not rebuild Hours from this row. |
| D-J08 | Lead-architect jobs | **Partial Live** | Home design cards |
| D-J09 | No AR clutter | **Live** | Design layout |
| D-J10 | Cannot approve variations | **Live** | M14 hard auto |

### Site (S-J01..S-J10)

| ID | Job | Status | Notes |
|----|-----|--------|-------|
| S-J01 | Site issues | **Live** | Site slice |
| S-J02 | Material request | **Live** | Create path |
| S-J03 | Purchase order | **Live** | Create path |
| S-J04 | Assign issue | **Live** | |
| S-J05 | Submit MR/PO | **Live** | |
| S-J06 | Project on PO | **Live** | Link |
| S-J07 | No supplier AP home | **Live** | site layout |
| S-J08 | Deliver jobs | **Partial Live** | Portfolio / home cards |
| S-J09 | Task status | **Live** | Work |
| S-J10 | No Finance | **Live** | Nav |

### Finance (F-J01..F-J10)

| ID | Job | Status | Notes |
|----|-----|--------|-------|
| F-J01 | Land Finance | **Live** | |
| F-J02 | Create SI draft | **Live** | M8d |
| F-J03 | Submit SI | **Live** | |
| F-J04 | Payment against SI | **Partial Live** | PE path; bank/MOP config; PE e2e still manual |
| F-J05 | PI create/submit | **Live** | |
| F-J06 | JE draft/submit | **Live** | M8c |
| F-J07 | Aging / profitability | **Partial Live** | Reports tabs |
| F-J08 | COA leaf create | **Live** | |
| F-J09 | Home collections | **Live** | finance layout |
| F-J10 | No Work/Site | **Live** | Nav |

### Rollup

| Role | Strong (Live) | Partial | Gap blockers for "alone 15 min" |
|------|---------------|---------|----------------------------------|
| Principal | ~6/10 | ~4/10 | Scripted win path QA; export trust; empty pilot |
| Owner Lite | ~7/10 | ~3/10 | Same commercial script |
| Design Lead | ~8/10 | ~2/10 | Timesheet depth optional |
| Site | ~8/10 | ~2/10 | Supplier master polish |
| Finance | ~7/10 | ~3/10 | PE chain e2e; cancel/amend messaging |

**Owner Share v1 still needs:** live boring QA of create loops + one unaided 15-min script (not more grid chrome).

---

## 7. Gaps (role × product)

### 7.1 RBAC and naming gaps

| ID | Gap | Impact | Severity | Suggested fix |
|----|-----|--------|----------|---------------|
| G-ROLE-01 | `lead.arch@` was Principal Profile | **Closed 2026-07-23:** seed = Design Lead Profile; re-seed on VPS | Done | `constants.py` |
| G-ROLE-02 | Role name `Studio Site and Contracts` vs profile `Studio Site Profile` | Confusion in audits and nav maps | Low | Alias docs; keep code SSOT |
| G-ROLE-03 | Owner Lite Site desk vs web | **Closed:** Site on both; product web-only | Done | desk.py + nav-access |
| G-ROLE-04 | Thin Settings for non-Principal | **Closed 2026-07-23:** sidebar Settings Principal-only; `/settings/*` path open for Appearance via profile (`route-access.ts`) | Done | nav-access + route-access |
| G-ROLE-05 | USER-GUIDE who-can-do table omits Owner Lite | **Closed** (Owner Lite column added) | Done | USER-GUIDE |
| G-ROLE-06 | Convert Opportunity process vs write perms | Process vs system mismatch | Medium | Enforce convert permission or update guide |
| G-ROLE-07 | Multi-role users: union of routes can widen access | Accidental over-grant | Medium | Document "one Studio profile per user" |
| G-ROLE-08 | Empty/unknown roles fall open to all Studio routes (nav safety) | **Closed 2026-08-17:** `canSeeStudioRoute` fails closed; Viewer is an explicit restricted role with no Admin | Done | nav-access.ts |

### 7.2 Capability gaps by role

| ID | Role | Gap | Severity | Notes |
|----|------|-----|----------|-------|
| G-CAP-P01 | Principal | Client portal SPA still partial (pack link only) | Medium | FEATURE-GAPS |
| G-CAP-P02 | Principal | Drawing revision approval workflow | Low | Deferred |
| G-CAP-OL01 | Owner Lite | Cannot manage users; must escalate to Principal | Low | By design |
| G-CAP-D01 | Design | Timesheet on web | **Historical v1 (2026-07-23):** Work → Hours list/create/submit. Hours tab later removed from daily Work. Residual is WR-01 (do not implement from this row). | Historical v1 | timesheets-panel + APIs (support only) |
| G-CAP-D02 | Design | No formal design review gate on docs categories | Medium | Docs table only |
| G-CAP-S01 | Site | Snag close loop | **Improved:** Close snag + Urgent priority + update_site_issue | Done (v1) | site-row-actions |
| G-CAP-S02 | Site | No RFQ comparison / progress claims | Medium | PO/PI only |
| G-CAP-P01 | Client | Pack empty states | **Improved:** updates/docs/invoices empty copy | Done (polish) | client-pack FE |
| G-CAP-F01 | Finance | Payment needs bank/MOP configured | Medium | Config, not code |
| G-CAP-F02 | Finance | Expense Claim off (HR blocked) | Low | Intentional |
| G-CAP-F03 | Finance | Journal create perms can 403 if role incomplete | Medium | M8c skip cases historically |

### 7.3 Validation / demo data gaps

| ID | Gap | Severity | Fix |
|----|-----|----------|-----|
| G-VAL-01 | Role matrix E2E | **Improved:** `role-nav.spec.ts` 5-role matrix (opt-in `STUDIO_E2E_ROLE_NAV=1`) | Done (v1) | e2e/role-nav |
| G-VAL-02 | Full M8-M14 journey automation | **Hard Auto (WR-10):** finance-matrix M8* + manual-matrix M9–M14 full journeys (no soft-skip shells). Optional: PE, Approve & invoice. | Done (v1) | [GAPS.md](../04-studio-web/GAPS.md#m8-m14-automation-status-wr-10) |
| G-VAL-03 | Admin M10 still mentions Invite; product now Add user | Low | Update gaps doc copy |
| G-VAL-04 | Company display name may not rename independently of ERP id | Low | Document ERP constraint |
| G-VAL-05 | Notifications often empty in pilot | Low | Seed / enable automation |

### 7.4 Capability gaps added/updated 2026-07-24

| ID | Role | Gap | Severity | Status |
|----|------|-----|----------|--------|
| G-CAP-COM01 | Principal / Design | Convert permission process vs write (who may Convert) | Medium | Live API uses Opportunity write; document Principal-first process |
| G-CAP-COM02 | All commercial | Owner 15-min unaided script not green | **High** | S-07; blocks Owner Share |
| G-CAP-COM03 | Principal | List CSV + sort/sticky not all shipped | Medium | Wave L **local** (pipeline/projects/opps/work) |
| G-CAP-F04 | Finance | Invoice cancel/amend UX | Medium | WR-03 |
| G-CAP-F05 | Finance | PE full SI→PE hard e2e | Medium | Manual |
| G-CAP-S03 | Site | Supplier master CRUD polish | Medium | WR-06 |
| G-CAP-D03 | Design | Multi-line timesheet | Low | WR-01 |
| G-CAP-P03 | Principal | Customer master beyond Pipeline lead | Medium | Wave 4 |

### 7.5 Priority backlog (role lens) — ordered for share

1. **Ship + live QA** Wave L if needed; re-verify Wave 0 creates (Lead, Opp, Convert, Task, Project, Finance CSV)  
2. **Owner 15-min script** (S-07) with demo seed health (S-06)  
3. G-ROLE-06 convert who-can vs guide  
4. Finance cancel/amend messaging (WR-03)  
5. Supplier master (WR-06) + customer master polish  
6. Photo attach on site issues  
7. Timesheet multi-line + billability  
8. Client multi-project portal (optional)  
9. G-ROLE-08 fail-closed unknown roles (**closed** on Studio Web nav)  


---

## 8. Story ID index (quick reference)

| Role | JTBD IDs | Story IDs | Demo user |
|------|----------|-----------|-----------|
| Principal | P-J01..P-J10 | P-US01..P-US10 | owner@ |
| Owner Lite | OL-J01..OL-J10 | OL-US01..OL-US10 | ownerlite@ |
| Design Lead | D-J01..D-J10 | D-US01..D-US10 | designer@ / architect1@ |
| Site | S-J01..S-J10 | S-US01..S-US10 | site@ |
| Finance | F-J01..F-J10 | F-US01..F-US10 | finance@ |

**Totals:** 5 roles × 10 JTBD = **50 jobs** · 5 × 10 stories = **50 user stories** · config steps = **50 role steps + 10 platform**.

---

## 9. How to use this doc

| When | Do this |
|------|---------|
| Owner review | Walk each role section 1.3 / 2.3 / … checklists |
| New feature | Map to role JTBD; add matrix row in JOURNEY-MATRIX if path-sensitive |
| Bug "wrong menu" | Check §0.2 matrix + G-ROLE gaps before changing code |
| Seed users | Always Role Profile from §0.1; never stack random ERP roles |

---

*End of roles JTBD / user stories / config validation / gaps. Update when ROLE_WORKSPACE_ACCESS or nav-access.ts changes.*
