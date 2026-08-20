# Spaces Studio as an operating system

Spaces is a design-studio operating system on ERPNext. It is not Slack, not Notion, and not a Macro clone. Daily work happens at [https://spaces.keyteller.com/studio/](https://spaces.keyteller.com/studio/). Frappe Desk at `/desk` is infrastructure: DocTypes, auth, APIs, rare support. Product language is job, client, project manager, Job Order. System names (Lead, Opportunity, Sales Invoice) appear only where the mapping matters.

Waves W0-W7 are the shipped program. The 2026-08-16 isolated ship adds Viewer persist, Activity `@` mentions, agent dispatch/MCP wrap, and project Money/Payments tabs. Confirm live HEAD on the VPS after that pull.

**Related:** [PRODUCT-GLOSSARY.md](./PRODUCT-GLOSSARY.md) · [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md) · [ASSIGNMENT.md](../02-architecture/ASSIGNMENT.md) · [DEVELOPMENT-PRIORITIES.md](./DEVELOPMENT-PRIORITIES.md) · [UPDATE-RULES.md](../01-agents/UPDATE-RULES.md)

---

## 1. Narrative

A villa enquiry arrives from the website or a phone call. Someone on the studio team opens **Leads** and captures a person, a project type, and a brief. That row is the enquiry. It is not a job yet.

They talk to the client. The lead status moves. When a quotation is sent, the same pursuit becomes a row on **Pipeline** (an Opportunity). The owner can still convert a lead to a **Client** without opening a job. Deal closed or Won means a Customer exists. A job exists only when someone converts a won opportunity or creates a **Job Order** on purpose.

The Job Order is the human identity of the job (`2608-0001` style). The computer still stores `PROJ-0002`. The team never bills against a second "studio job" table. One Project is the hub: people, documents, commercial plan, site buys, and every legal invoice that should roll up.

The principal names a **project manager** (the Lead on the job). Designers are Support. Work happens as **tasks**: some hang off the job, some are loose calendar items. Priority is a single **P1 / P2 / P3** tag. Status walks Open, In progress, Review, Final approval, Done.

On site, people raise snags, material requests, and purchase orders. Finance never invents a studio cashbook. Client bills are Sales Invoices. Supplier bills are Purchase Invoices. Cash is Payment Entry. The Money tab on the job shows Budget (Net), Due, Spent, Burn derived from those documents plus the commercial plan. VAT is computed on project value (default 5% UAE). Due / Spent / Burn stay Net.

The conversation about the job is **Activity**, not email. People write updates and `@` other records. Mentions live on that Activity row. Client Pack is a guest link to a safe slice: only rows marked Visible for client. Default is internal.

When the month closes, Finance opens **Statement of account**, overhead (job-less purchase invoices), and monthly costs (salary slips plus expense journals). **Facts and Figures** shows burn, AR, pipeline, and a capacity snapshot. Capacity itself is weekly load from job scale, not a second staffing database.

### Product words to DocTypes

| You say | It is | Stored as |
|---------|-------|-----------|
| Enquiry | Unqualified pursuit | `Lead` (`studio_lead_status`, `project_type`, `project_brief`) |
| Client | Billing party | `Customer` |
| Pipeline job | Qualified pursuit | `Opportunity` |
| Job / Job Order | The engagement | `Project` + `Project.studio_project_code` |
| System id | URL / ledger key | `Project.name` (`PROJ-xxxx`) |
| Project manager / Lead | Job owner | `Project.lead_architect` + team row `Lead Architect / Designer` |
| Support | Other studio people on the job | `Project Team Member` role `Designer` |
| Task Lead / Support | People on a task | `Task._assign` JSON; index 0 is Lead |
| Project value | Commercial envelope | `Project.overall_budget` (Net) |
| Invoice / payment | Legal money | Sales Invoice, Purchase Invoice, Payment Entry, Journal Entry |
| Job thread | Conversation + progress | `Studio Activity` |
| Docs checklist | Delivery pack plus finance evidence links | Child `Project Document Link` (`is_checklist`) |
| Site snag | Field issue | `Issue` |
| Client Pack guest | Token reader | Guest pack; fail-closed on `client_visible` |

Bounded contexts (ontology): Pipeline does not own cash. Commercial plans are not invoices. Finance owns the ledger. Site owns MR / PO / Issue. One Project is the hub.

---

## 2. Who it is for

Assign a **Role Profile**, then run `normalize_studio_user`. Do not hand people raw ERPNext roles.

| Role | Demo (password in [USER-GUIDE.md](../06-guides/USER-GUIDE.md)) | Lands on | Can | Cannot |
|------|----------------------------------------------------------------|----------|-----|--------|
| **Principal / Owner** (`Studio Principal`) | `owner@spaces-demo.local` | Home | All product routes + Admin (company, users, theme). Approve variations and Approve-and-invoice. Set job Lead. Convert opportunities. Create SI/PI/PE/JE. Delete Activity (with write on the referenced record). | Should not be trained on Desk as daily UX |
| **Owner Lite** | `ownerlite@spaces-demo.local` | Home (owner layout) | Same commercial loop as Principal: Home, Facts, Pipeline, Projects, Work, Capacity, Finance, Site | Admin, company/user invite, desk Settings |
| **Design Lead** | `designer@`, `architect1@`, `lead.arch@` | Work | Pipeline, Projects, Work, Capacity, Facts. Create leads and tasks. Edit job docs and design fields | Finance, Site, Admin, variation approve, AR clutter on Home |
| **Finance** | `finance@spaces-demo.local` | Finance | SI/PI/PE/JE, reports, ledger, Home finance cards, Facts | Work, Site, Pipeline, Projects nav, variation approve, Admin user invite |
| **Site** | `site@spaces-demo.local` | Site | MR, PO, issues, Projects, Work, site Home cards | Finance, Pipeline, Capacity, Admin. Independent Client create is denied even if a leftover Frappe perm exists |
| **Viewer** | Profile `Studio Viewer` (local increment; live login smoke still open) | Neutral product Home | Read product routes including Finance | Admin, export/CSV, Activity write, Kanban drag, assignment, any persist. Session `can_mutate=false`. One `PermissionError`: "Studio Viewer is read-only" |
| **Client Pack guest** | Token URL from the job | Guest pack SPA | See explicit `client_visible=1` Project/Customer Activity, progress marked visible, variation **intent** (Accept / Decline / Questions) | Change legal variation status, money, or unpublished Activity. Archived jobs reject mint and old tokens |

Home layouts: owner (money, risk, decisions), design (overdue work, lead jobs, no AR), finance (balance due, payments, draft SI), site (issues, deliver jobs). Unknown Studio role fails closed.

---

## 3. The operating system (how work actually flows)

A day is not "open Excel and hunt tabs." Each surface is a job in the OS.

**Home.** Role-filtered cockpit cards. Existing cards group as **Signal** vs **Noise** (`classifyHomeSignal`: overdue, money due, assigned-to-me, mention-of-my-records). No second inbox DocType. Principal/Owner Lite approve variations here (including client feedback on changes). Empty states stay inside `/studio/*`.

**Work.** Design Lead lands here. List, product kanban (Open → In progress → Review → Final approval → Done), calendar. Click a day to create a task with that due date. Project is optional. Bulk complete / snooze. People column: Lead + Support. One P1-P3 chip. Cmd+K can create a task on a job.

**Projects / command sheet.** New Job Order allocates `{YYMM}-{NNNN}` on the server. List shows Job Order, category, job status, location, project manager. Open a job and you get a command sheet, not a 40-field ERP form.

Command-sheet tabs, left to right (default **Overview**; the tab strip uses `h-auto` so it wraps instead of a vertical slider):

| Tab | Job |
|-----|-----|
| **Overview** | People, attendees/contacts, commercial plan teaser, materials/suppliers. No dominant project value |
| **Activity** | Job thread. Timeline + Mentioned in. Progress update is an Activity category. Legacy `Project Update` is read-only history |
| **Money** | Budget (Net) + VAT + Gross; invoiced / due / spent / margin / remaining from SI / PI / PE. Links into Payments |
| **Payments** | Collected / still due / paid suppliers / still to pay. Terms stay plans; legal cash is PE |
| **Docs** | Checklist + optional http(s) link. New jobs seed all 10 titles; existing append missing only |
| **Variations** | Last. Owner approve / reject / approve-and-invoice. Guest intent is feedback only |

**Pipeline / Leads / Clients.** Leads is the enquiry list. Clients is an independent Customer list (company or individual, location, last contact). Pipeline is opportunities. Quotation sent / Proposal sent ensures an Opportunity. Convert opportunity → Project + starter tasks. Convert lead to client = Customer only.

**Finance.** AR/AP, create/submit SI/PI/PE/JE, COA, trial balance, GL, CSV. SOA, overhead, monthly costs as read-only derivations. Invoice names deep-link to Finance filters, not Desk.

**Site.** Material Request, Purchase Order, Issue (snag loop: assign, attach, resolve, close).

**Capacity.** Weekly complexity: open jobs × `studio_job_scale` × weekly activity × 1.25 if you are Lead. Engagement share is secondary (FT/PT/Advisory on the same team row Overview uses). Expand a person for their activity log.

**Activity.** One writer: `record_studio_activity`. Mounted on project, task, client, opportunity (and lead backlinks). This is the job thread.

**Cmd+K.** Jump via the mention catalog hrefs. Create-task and finance shortcuts stay on existing APIs.

---

## 4. Systems and features (complete inventory)

### 4.1 Studio Web shell

**What.** The only product UI. Brand splash, Spaces SEO / OG image, Frappe session cookie.

**SSOT.** `nav-access.ts` for routes. Session from `get_session_context`. CSRF from session-boot (`fetchStudioSession` / `setStudioCsrfToken`), not a stale cookie alone.

**UI.** Sidebar: Home, Projects, Work, Leads, Clients, Pipeline, Facts and Figures, Capacity, Finance, Site, Admin (Principal). Cmd+K. Header bell. Appearance via profile menu.

**Why.** Desk workspaces trained owners into ERP. Role landings and capability flags (`can_mutate`, create flags) keep buttons honest. Two agents must never edit `studio_web.py` at once (A1).

### 4.2 Home / cockpit / Signal-Noise

**What.** Decision cards, money due, variations, role layouts.

**SSOT.** Home dashboard APIs + `classifyHomeSignal`. Overdue tasks: `status` not Completed/Cancelled and `exp_end_date` < today (`contract_overdue_count_ssot`). Priority Home is `StudioDecisionCard`, not a dead PriorityCockpit.

**Why.** Owner JTBD is "what needs me today" without Excel. Signal/Noise reuses cards instead of inventing an inbox.

### 4.3 Pipeline / Leads / Opportunities / convert

**What.** Enquiry capture, status, convert to client, quotation → opportunity, convert won job.

**SSOT.** `Lead`, `Opportunity`. Product status on `Lead.studio_lead_status`. Wave 5 set: Open, No reply, Replied, Interested, Quotation sent, Key lead, Dead lead, Deal closed. Later local review: New → Contacted → Engaged → Qualified → Proposal sent → Negotiating → Won/Lost, with independent Normal/High priority (legacy Key lead → Qualified + High). Proposal/Quotation sent still idempotently `ensure_opportunity_for_lead`. Won / Deal closed → Customer only (no auto Project).

**UI.** `/pipeline`, `/opportunities`, create sheets, inline cells, saved views, convert confirm → command sheet.

**Why.** Separate enquiry list from client list. A quotation is a pursuit, not a job. Creating a Project is an explicit win.

### 4.4 Clients / comm log / last contact

**What.** Independent clients. Communication history. Last contact column.

**SSOT.** `Customer`. Log is `Studio Activity` (not a second comm table). Last contact = `MAX(activity_date)` where reference is that Customer. New Client (`create_studio_customer`) creates Customer only; duplicate names return the existing readable row.

**UI.** `/clients` list + drawer timeline.

**Why.** A client can exist before a job. Last contact must be derived from the same Activity the team types.

### 4.5 Projects / Job Order / command sheet

**What.** Create and run jobs.

**SSOT.** `Project`. Job Order `studio_project_code`: backend `resolve_create_job_order_code` under a lock; format `{YYMM}-{NNNN}`; global non-resetting sequence; no user override; immutable after save. ERP id unchanged. Category `studio_discipline` (Design / Fitout / Printing / Construction + user-added). Job status `project_lifecycle_status` (Moodboard prep, Under design, Shop drawings preparation, Construction, On hold, Completed). Location `site_address`. Archive is `studio_archived` (Studio operational, not ERP status). Archived jobs reject Studio writes and pack mint; finance history stays readable.

**UI.** `/projects?project=PROJ-xxxx`. New Job Order sheet. Command tabs above.

**Why.** Phone identity must not be `PROJ-0002`. Sequence must not reset each month or the studio will collide. Overview without value stops money from living in two places.

### 4.6 Docs checklist seed + optional link

**What.** Docs checklist on the job (delivery artifacts plus optional finance evidence links).

**SSOT.** Child `Project Document Link`. `is_checklist=1`. Optional http(s) URL. Completion is on the same row. New create seeds: Quotation, Proforma invoice, Tax invoice, Receipt vouchers, Mood board, Design, Technical drawings, NOCs, Warranties, Handover / delivery note (`seed_default_docs_checklist`). Existing jobs append missing titles only.

**UI.** Docs tab. Checkbox plus optional URL. Archived jobs: history readable, edits hidden.

**Why.** One child table. Finance titles on Docs are evidence ("do we have the file"), not a second invoice book. Legal money stays on SI/PI/PE. Seed never wipes a custom pack.

### 4.7 Work / tasks / kanban / calendar / P1-P3 / `_assign`

**What.** Delivery work.

**SSOT.** `Task`. Priority `studio_priority` (one tag). Due `exp_end_date`. People: `Task._assign` = `json.dumps([email, ...])` ordered, deduped. Lead is index 0. Writer `set_task_people`. Notes = `Task.description`. Kanban maps to existing ERP statuses; Final approval = Working + `studio_waiting_on` (default Approval); Review = Pending Review. Soft/hard deadline hidden in product UI.

**UI.** `/work` list, kanban, calendar, task detail (Overview, Notes, Activity). Hours tab removed from daily Work (timesheet remains support/API; WR-01 still a gap).

**Why.** A second assignee field caused `JSONDecodeError`. Two priority chips next to one control confused owners. Tasks without a project are real (admin, marketing, internal).

### 4.8 Project people

**What.** Several studio users on a job. One Lead.

**SSOT.** Writer `set_project_people`. Storage: `Project Team Member`. Lead role `Lead Architect / Designer` also writes `Project.lead_architect` (Capacity still reads that field). Support role `Designer`. External unnamed rows kept. At most one Lead; if none marked, first person becomes Lead. Full command-sheet role table (PM / execution lead / partners) is not replaced.

**UI.** Projects People column; command sheet team.

**Why.** Capacity and Overview must not disagree (W1). This is not RACI.

### 4.9 Capacity

**What.** Who is overloaded this week.

**SSOT.** Model `weekly_complexity_lead`. `Project.studio_job_scale` (Room 0.5, Full Villa 1.5, Landscape 1.0, Full Project 2.0). Weekly activity 1.0 if task/timesheet/update this week, else 0.4 idle open. Lead ×1.25. Engagement on the same team child (`allocation_pct` cleared consistently).

**UI.** `/capacity` board + architect activity sheet.

**Why.** FT-primary + 20% buffer is not the product model. Job scale is complexity SSOT (W8). Owner asked to hide scale on the Projects *product* list; engineering later put inline scale on Projects so Capacity has data (Wave M). Capacity remains studio-wide until WR-04 multi-company.

### 4.10 Site

**What.** Buy and snag.

**SSOT.** Material Request, Purchase Order, Issue. Project link required for roll-up.

**UI.** `/site` create, assign, submit, snag attach / resolve / close.

**Why.** Site must not own client fee design or AR.

### 4.11 Finance

**What.** Legal money and read-only studio panels.

**SSOT.** Sales Invoice, Purchase Invoice, Payment Entry, Journal Entry, GL. Plan rows (`project_client_milestones`, `project_supplier_payments`) are intent, not cash. SOA: submitted SI/PI + PE in the **active company**; project credit is the PE **allocation**, never the whole multi-project PE. Overhead: submitted PI with empty project. Monthly: `Salary Slip.net_pay` plus JE expense GL (no payroll double count). Burn: `compute_project_spent` → `sync_burn_rate`. VAT: project value Net, default 5%, strip shows Net + VAT = Gross.

**UI.** `/finance` tabs including `soa`, `overhead`, `monthly`. Project **Money** and **Payments** are a per-job glance: figures are summed from that job's submitted Sales Invoices, Purchase Invoices, and Payment Entry allocations. There is no second studio ledger and no Square calculator. `/portfolio` Facts metrics.

**Why.** A studio ledger would drift from UAE tax invoices. G4 is non-negotiable.

### 4.12 Activity + `@` mentions + backlinks + create-task

**What.** The job thread and a light graph on top of it.

**SSOT.** DocType `Studio Activity`. Writer `record_studio_activity` only. Mentions parsed from subject/notes (`utils/mentions.py`), resolved against a permission catalog, stored on `studio_mentions` JSON. Backlinks: `list_studio_activity_backlinks` scans readable Activity (no index table). Catalog: `list_studio_mention_catalog`. Tokens: `@project/Name`, `@client/Acme`, `@task/...`, `@opp/...`, `@lead/...`, or unique `@Label`. Unresolved tokens stay text.

**UI.** Timeline on client, task, project, opportunity. Mentioned in on those plus lead. Composer `@` picker. Create task uses `create_studio_task`. Job-confidential updates should be Project Activity (Customer-visible rows appear on every pack for that client).

**Why.** Macro's useful idea is bidirectional links. Implementing that as a graph DB or email inbox would fork the product and the license.

### 4.13 Client Pack

**What.** Shareable guest status pack.

**SSOT.** Token + `client_pack_token_version` revoke. Fail-closed: only explicit `client_visible=1`. Guest POST can set variation feedback only. Auto-hooks stay internal.

**UI.** Copy/email link from the job. Guest SPA. Home card for client feedback.

**Why.** Default internal so a chatty job thread cannot leak supplier or burn.

### 4.14 Notifications

**What.** Header bell inbox.

**SSOT.** Own Notification Log rows; assign emit; overdue strip (`contract_notifications`, matrix `studio_web_notifications`).

**Why.** Stay on Frappe Notification Log. Do not build a second inbox.

### 4.15 Admin / intake webhook

**What.** Principal Admin: company, users, role profiles, theme. Intake: marketing POST or `/studio-enquiry`.

**SSOT.** `create_lead_from_webhook` + `ensure_webhook_token`. Pipeline `get_intake_setup` returns `token_configured` true/false. **Never** show the webhook token, curl, or HMAC secret in the UI (enquiry card is status + operator hint only). Token lives in site config / server env.

**Why.** A leaked header in the SPA is a public Lead writer.

### 4.16 Assignment APIs

See [ASSIGNMENT.md](../02-architecture/ASSIGNMENT.md). One writer per object. `list_project_people` for read. Task lists still expose `assignee` as the first `_assign` member. Viewer cannot assign.

### 4.17 Persist / Viewer deny

**What.** Every Studio mutation goes through `_safe_doc_save` / `_safe_doc_insert` (`utils/persist.py`). Viewer hits `deny_studio_viewer_write` **before** persist.

**Why.** Frappe ValidationError used to become bare Invalid Request. UI gates plus DocPerms plus one persist deny is the fail-closed stack. Real Viewer login on VPS is still a smoke gap.

### 4.18 Agentic prep

**What.** Agents are callers of existing methods. Prep only; not shipped until isolated.

**Tools** ([AGENTIC-LAYER.md](../02-architecture/AGENTIC-LAYER.md)): `lookup_records`, `list_activity`, `list_backlinks`, `search_entities` (includes Customer), `record_activity`, `create_task`, `set_project_people`, `set_task_assignees`, `list_tools`, `weekly_brief` (compose only), `digest` (Home cards). Preferred new read APIs live under `spaces_studio.api.agent`.

**Why.** No second memory DB (G3). No SI/PI/PE tools. No MCP/session runner yet. Persist a brief only by recording Activity.

### 4.19 Print formats / letterhead

**What.** Letter Head name `Spaces`. Print formats: Spaces Tax Invoice, Purchase Invoice, Quotation (`ensure_print_formats`, `contract_print_formats`).

**Why.** UAE invoices must not look like generic ERP. Brand is Spaces, not a leftover atelier name.

### 4.20 Facts and Figures

**What.** `/portfolio` metrics: avg burn, AR due/overdue, open pipeline, capacity snapshot. Burn/AR/pipeline active-company scoped.

**Why.** Owner language is Facts and Figures, not Portfolio. Read-only derivation; no metrics table that writes money.

### 4.21 Variations

**What.** Change orders on the job. Owner approve / reject / approve-and-invoice (draft SI path). Guest intent does not change legal status.

**Why.** Commercial approval is an owner job (P-J03), not a designer job.

---

## 5. Decisions and why

| Decision | Alternative rejected | Why |
|----------|----------------------|-----|
| Studio Web only; Desk retired as product UX | Train owners on Desk workspaces | Desk is 40 shortcuts and ERP jargon. New journeys ship in `/studio/*` + APIs. Desk stays for auth and rare GL support |
| One SSOT writer per field (G2) | Extra helpers that append the same child | Dual engagement writes and dual money writers already broke Capacity vs Overview |
| No parallel DB (G3) | NocoDB/APITable-style free tables | Product is ERP documents. A second store splits truth and license risk |
| Money is SI/PI/PE/GL | Studio cashbook or "paid" flags as truth | Tax invoices and bank cash already exist. Plan rows are intent. Sync fights manual Paid |
| Mentions only on Activity JSON | Graph DB, second backlink table, Macro fork | Same conversation record; backlinks are a read. AGPL Macro is learn-only (UX ideas, not code) |
| Task people = ordered `_assign` (Lead index 0) | Extra assignee DocType, Informed list, plain email string | Frappe already stores assignees as JSON. A string crashes lists |
| Project people = team child + `lead_architect` | Only header fields, or a second team API | Capacity reads the header; Overview reads the table. One setter writes both |
| Informed role deferred | Slack watchers / RACI third list | Needs a product spec. A third list would be a second assignment SSOT |
| Macro AGPL learn-only | Fork `macro-inc/macro` | Different product (email+chat+CRDT vs job graph). License and scope explosion |
| Isolated ships (do not dump dirty tree) | Ship the whole local worktree | Unrelated local review (calculator, leftover `insert()`, session dumps) must not ride a feature ship |
| Priority is a single tag | Tag + duplicate text badge | `studio_priority` is the SSOT. Two chips looked like two fields |
| Client Pack default internal | Auto-publish Activity to guests | Fail-closed `client_visible`. Customer-visible rows leak across that client's jobs |
| Empty-only seeds | Wipe and re-seed checklists / colors / job scale | W2/W3: never destroy demo-custom or live values |
| Job scale as complexity SSOT | Hide the field entirely, or FT+buffer as load | Capacity math needs a number on the Project. Owner hid it from the *story* of Projects; the field remains |
| VAT Net + 5% default | Gross-as-budget | UAE standard. Due/Spent/Burn stay comparable Net |
| Deal closed = Client only | Auto-create Project | A client without a live job is normal |
| Progress writes Activity, not new `Project Update` | Four log systems | W7: one store, many surfaces. Legacy rows stay readable |
| CSRF session-boot | "It worked once without the header" | Cookie-only POST died in production |
| `contract_audit` as the ship gate | Trust the PR description | Path bugs recur. Missing fields must fail the contract, not only wrong constants |

---

## 6. Insights from building it

**Frappe lists are sparse.** Link fields come back null. Zod must be nullish or the Projects list dies (`0ef09d7`). Every new API field needs schema + types + columns in the same change (T3).

**Bind-mount and asset cache lie.** VPS is `/opt/spaces`. After Python or SPA changes: `verify-and-ship.sh`, `ship-studio-web.sh` if the SPA moved, then **Ctrl+Shift+R**. Stale JS is the most common "we shipped but I do not see it."

**`studio_web.py` is a collision magnet.** It grew into a facade for half the product. Parallel agents overwrite each other. Split new reads into `api/activity`, `api/assignment`, `api/agent`, `api/project_command`. Keep transport files serial.

**CSRF and Invalid Request.** Session token + `_safe_doc_save` so ValidationError text reaches the UI. Global toast plus slice toast doubled "Something went wrong."

**Viewer is a persist problem, not only a nav problem.** Hiding buttons is not enough. One deny in persist, DocPerm upgrade that clears write flags, and `can_mutate` on the session. Until a human logs in as Viewer on VPS, do not claim the role.

**Capacity formula drift.** Reintroducing FT-primary + 20% buffer without updating contracts and copy together will ship a silent wrong board (W7).

**Home had two SSOTs.** A dead Priority cockpit was documented as live. Decision cards won. Digest vs cockpit is still a design tension for agents (compose over `get_home_dashboard`, do not query all Activity).

**TypeScript coupling.** Slice Zod, session schema, and vitest product helpers (`flow-board`, mention catalog) are the real FE contracts. Node/vitest on Windows vs `agent-gate.sh` on Bash is why local "green" is not a VPS gate.

**Windows cannot run the ship gate.** WSL Bash access denied / Git Bash missing utilities. Gate belongs on VPS or Docker. Do not invent a second gate.

**Lead lists broke the same way Projects did.** Null fields + Zod. Enquiry UI must not dump webhook curl or tokens (intake card is configured yes/no).

**Money tabs need browser smoke.** SOA allocation and archived-write guards are easy to unit-test and easy to miss in the SPA if the Finance extras stay local.

**Desk retirement is a copy problem as much as a route problem.** Empty-state `/app/*` hrefs kept sending people back. `contract_empty_states` exists because of that.

---

## 7. Areas for improvement (practical)

### Ship-ready polish (spec exists)

| Item | Why it is next |
|------|----------------|
| **Owner Share S-07** | 15-min unaided human path. S-06 seed retry is done. Blocks Owner Share v1 ([OWNER-SHARE-READINESS.md](./OWNER-SHARE-READINESS.md)) |
| Mention picker + backlinks on live VPS | Built locally; owners only feel the graph after an isolated ship + hard refresh |
| Viewer live smoke | Persist deny untested as a real login |
| Money/Payments browser smoke | Local Finance extras (SOA CSV, invoice hrefs, archived write guards) need VPS/Docker |
| Leftover raw `insert()` on some create paths | Should go through `safe_doc_insert` like the rest |
| Dirty tree hygiene | Do not ship Square calculator stubs, session dumps, or unrelated WIP with a feature |
| vitest/node vs agent-gate | Keep FE unit tests; do not pretend they replace `contract_audit` |

### Product-spec needed (do not invent)

| Item | Missing decision |
|------|------------------|
| Informed role | Third assignment list vs Activity `@user` |
| `@user` mentions | User catalog + notify writer |
| Square calculator | Units, output field, whose job |
| MCP / session-bound agent | Auth, CSRF, service user policy |
| Backlinks at 10k+ Activity | Scan-all is fine at studio size; needs an index later |
| Home digest vs cockpit | Agent digest must not become a second Home query |
| Timesheet v2 (WR-01) | Hours removed from Work UI; retirement vs rebuild |
| Multi-company (WR-04) | Capacity still studio-wide; rare GL packs still support |
| Quotation PDF (WR-05) | Selling edge |
| Supplier master CRUD (WR-06) | Site/Finance polish |
| Lead status set | Wave 5 labels vs later New/Contacted/... review: pick one owner-facing SSOT and finish copy |
| Job scale on Projects list | Owner "hide on Projects" vs Capacity data entry |

### Do not

- Fork Macro, add email/Slack/CRDT/canvas
- Mention SI/PI/PE or build a studio ledger
- Second Activity, assignment, or mention table
- Nightly LLM memory file
- New Desk product journeys
- Wave 8 without new owner notes
- Force-push `main` or commit `deploy/.env` / demo passwords into new files
- Wipe seeds or mix untagged demo into real books

---

## 8. How to run it

| Item | Value |
|------|-------|
| Product | [https://spaces.keyteller.com/studio/](https://spaces.keyteller.com/studio/) |
| Desk (infra) | [https://spaces.keyteller.com/desk](https://spaces.keyteller.com/desk) |
| Demo logins | [USER-GUIDE.md](../06-guides/USER-GUIDE.md) (do not copy passwords into new docs) |
| VPS | `deploy@187.77.140.216` → `/opt/spaces` |
| Company | `space` · AED · UAE |
| Local SPA | `apps/studioweb/shadcn-admin` (`pnpm run dev`) |

After pull on VPS:

```bash
cd /opt/spaces
git pull --ff-only origin main
bash scripts/verify-and-ship.sh
bash scripts/ship-studio-web.sh   # if Studio Web changed
```

Hard refresh: Ctrl+Shift+R on `/studio/`.

Local / small change:

```bash
bash scripts/agent-gate.sh
```

Tier 1 ship gate is `contract_audit` + `owner_readiness_audit`. Owner demo smoke: create client, convert, new task, capacity scale edit, one finance create.

Assignment unit check:

```text
python apps/spaces_studio/spaces_studio/utils/test_assignment_roles.py
```

This file is the narrative OS. Field-level SSOT stays in [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md). What to build next stays in [DEVELOPMENT-PRIORITIES.md](./DEVELOPMENT-PRIORITIES.md).
