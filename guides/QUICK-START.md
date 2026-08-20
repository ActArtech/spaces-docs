# Spaces — Quick Start

**One page:** logins, links to check, and a 10-minute path.  
**Desk:** https://spaces.keyteller.com/desk  
**Studio Web (local / gated deploy):** `/studio/` — see [apps/studioweb/README.md](./apps/studioweb/README.md)  
**Full tour:** [WALKTHROUGH.md](./WALKTHROUGH.md) · **User guide:** [USER-GUIDE.md](./USER-GUIDE.md)

---

## 1. Login

| | |
|--|--|
| **URL** | https://spaces.keyteller.com/desk |
| **After deploy** | Hard refresh: **Ctrl+Shift+R** |

### Demo accounts (pilot / training)

**Password for all demo users:** `SpacesDemo2026!`  
(Unless changed on VPS via seed config.)

| Email | Role profile | Use for |
|-------|--------------|---------|
| `owner@spaces-demo.local` | Studio Principal | Full desk, Studio Home + Health, settings |
| `lead.arch@spaces-demo.local` | Studio Design Lead | Lead architect / Work |
| `architect1@spaces-demo.local` | Studio Design Lead | Design / tasks |
| `architect2@spaces-demo.local` | Studio Design Lead | Design / tasks |
| `architect3@spaces-demo.local` | Studio Design Lead | Design / tasks |
| `designer@spaces-demo.local` | Studio Design Lead | Design workflow |
| `finance@spaces-demo.local` | Studio Finance | Invoices, payments, AR/AP |
| `site@spaces-demo.local` | Studio Site Profile | Material requests, POs, site issues |

**Start here for demos:** `owner@spaces-demo.local`

### Real team accounts

Create under **Settings → User**. Assign a **Role Profile** (not raw ERPNext roles):

| Role profile | Access |
|--------------|--------|
| Studio Principal Profile | All seven workspaces (Home, Health, Pipeline, Projects, Site, Finance, Settings) |
| Studio Design Lead Profile | Home, Pipeline, Projects |
| Studio Site Profile | Home, Projects, Site |
| Studio Finance Profile | Home, Finance |

Known principal (if already set up): `alaaalmallah1@gmail.com` (Studio Principal) — password is **not** the demo password (user-owned).

### Administrator

| | |
|--|--|
| User | `Administrator` |
| Password | Shelve / `deploy/.env` only (never commit) |

---

## 2. Start in 5 steps

1. Log in as **`owner@spaces-demo.local`** / **`SpacesDemo2026!`**
2. Confirm landing: **Studio Home** — **Attention Required** strip + action cards  
3. Open hub project: [PROJ-0002 — Al Reem Villa Interior](https://spaces.keyteller.com/app/project/PROJ-0002)  
4. Check **Client Invoices** (kickoff **Paid**), **Documentation** table, **Progress** tab delivery panel  
5. Open [Studio Portfolio](https://spaces.keyteller.com/desk/workspaces/Studio%20Portfolio) for burn %, phase pipeline, collections

---

## 3. Workspaces (sidebar)

| Workspace | Link |
|-----------|------|
| Studio Home | https://spaces.keyteller.com/desk/workspaces/Studio%20Home |
| Studio Portfolio | https://spaces.keyteller.com/desk/workspaces/Studio%20Portfolio |
| Clients and Pipeline | https://spaces.keyteller.com/desk/workspaces/Clients%20and%20Pipeline |
| Projects and Design | https://spaces.keyteller.com/desk/workspaces/Projects%20and%20Design |
| Site and Procurement | https://spaces.keyteller.com/desk/workspaces/Site%20and%20Procurement |
| Studio Finance | https://spaces.keyteller.com/desk/workspaces/Studio%20Finance |
| Studio Settings | https://spaces.keyteller.com/desk/workspaces/Studio%20Settings |
| Cockpit card settings | https://spaces.keyteller.com/app/owner-cockpit-settings |

---

## 4. Links to check (by area)

### Public

| What | Link |
|------|------|
| Enquiry form | https://spaces.keyteller.com/studio-enquiry |
| Login | https://spaces.keyteller.com/login |

### Studio Web (thin UI — local dev or after VPS deploy)

| What | Link / path |
|------|-------------|
| Prod (when deployed) | https://spaces.keyteller.com/studio/ |
| Local dev | http://localhost:5173 (Vite proxy to Frappe) |
| Home | `/` — cockpit cards |
| Projects | `/projects` — inline edits |
| Pipeline | `/pipeline` |
| Work | `/work` — tasks + Complete / Snooze |
| Opportunities | `/opportunities` |
| Portfolio | `/portfolio` — health cockpit |
| What we built | [apps/studioweb/STUDIO-WEB-BUILT.md](./apps/studioweb/STUDIO-WEB-BUILT.md) |
| Build checklist | [apps/studioweb/STUDIO-WEB-CHECKLIST.md](./apps/studioweb/STUDIO-WEB-CHECKLIST.md) |

```bash
cd apps/studioweb/shadcn-admin
cp .env.example .env.local   # VITE_FRAPPE_URL=https://spaces.keyteller.com
pnpm install && pnpm run dev
```

Login with **`owner@spaces-demo.local`** / **`SpacesDemo2026!`**. Desk at `/desk` stays available; Studio Web does not replace desk code.

### Demo hub

| What | Link |
|------|------|
| Project PROJ-0002 | https://spaces.keyteller.com/app/project/PROJ-0002 |
| Project list | https://spaces.keyteller.com/app/project |
| Lead list | https://spaces.keyteller.com/app/lead |
| Opportunity list | https://spaces.keyteller.com/app/opportunity |

### Delivery

| What | Link |
|------|------|
| Tasks | https://spaces.keyteller.com/app/task |
| Task Kanban (Studio Tasks) | https://spaces.keyteller.com/app/task/view/kanban/Studio%20Tasks |
| Task Calendar | https://spaces.keyteller.com/app/task/view/calendar |
| Task Gantt | https://spaces.keyteller.com/app/task/view/gantt |
| Timesheet | https://spaces.keyteller.com/app/timesheet |
| Project Update | https://spaces.keyteller.com/app/project-update |

### Money

| What | Link |
|------|------|
| Sales Invoice | https://spaces.keyteller.com/app/sales-invoice |
| Purchase Invoice | https://spaces.keyteller.com/app/purchase-invoice |
| Payment Entry | https://spaces.keyteller.com/app/payment-entry |
| Customer | https://spaces.keyteller.com/app/customer |
| Supplier | https://spaces.keyteller.com/app/supplier |
| Accounts Receivable report | https://spaces.keyteller.com/app/query-report/Accounts%20Receivable |
| Accounts Payable report | https://spaces.keyteller.com/app/query-report/Accounts%20Payable |

**Print:** open a Sales Invoice → Print → format **Spaces Tax Invoice** (letterhead **Spaces**).

### Site

| What | Link |
|------|------|
| Purchase Order | https://spaces.keyteller.com/app/purchase-order |
| Material Request | https://spaces.keyteller.com/app/material-request |
| Issue | https://spaces.keyteller.com/app/issue |

---

## 5. What “good” looks like (demo)

| Check | Expect |
|-------|--------|
| Studio Home | Attention Required loads; action cards show empty-state CTAs when sparse |
| Studio Portfolio | Burn table + phase pipeline visible on demo data |
| PROJ-0002 kickoff milestone | Status **Paid** |
| PROJ-0002 burn | ~**38%** (if PIs linked) |
| PROJ-0002 documentation | Design / Photos / Approvals / Shop Drawings rows |
| Timesheets | At least 2 demo sheets |
| Project Updates | At least 2 with progress notes |
| Task list | No Server Error |
| Finance login | Invoices visible; limited design chrome |

Full click paths: [WALKTHROUGH.md](./WALKTHROUGH.md)

---

## 6. Keyboard

| Key | Action |
|-----|--------|
| Ctrl+K | Search DocTypes / reports |
| Ctrl+G | Go to route |
| Ctrl+S | Save |
| Ctrl+Shift+R | Hard refresh (after deploy) |

---

## 7. Ops (admin only)

| Item | Value |
|------|--------|
| VPS | `ssh deploy@187.77.140.216` |
| App path | `/opt/spaces` |
| Site | `spaces.keyteller.com` |
| GitHub SoT | https://github.com/ActArtech/spaces (private) |
| Pull on VPS | `cd /opt/spaces && git pull --ff-only origin main` |
| Ship | `bash scripts/verify-and-ship.sh` |
| Seed demo | `bash scripts/seed-dummy.sh` |
| Purge demo | `bash scripts/purge-dummy.sh` |
| Contract gate | `bash scripts/agent-gate.sh` |
| Secrets | Shelve `ktteam/spaces` · never commit `deploy/.env` |

```bash
# After git pull on VPS
cd /opt/spaces
git pull --ff-only origin main
bash scripts/verify-and-ship.sh
```

---

## 8. Role cheat sheet

| I want to… | Login as | Open |
|------------|----------|------|
| See everything | `owner@spaces-demo.local` | Home → Health → PROJ-0002 |
| Design / tasks | `designer@spaces-demo.local` | Projects, Task calendar |
| Invoices only | `finance@spaces-demo.local` | Studio Finance |
| Public lead | (no login) | /studio-enquiry |

---

## 9. More docs

| Doc | When |
|-----|------|
| [WALKTHROUGH.md](./WALKTHROUGH.md) | Full 12-flow demo tour |
| [OWNER-ONBOARDING.md](./OWNER-ONBOARDING.md) | Weekly owner rhythm |
| [PLATFORM-SITEMAP.md](./PLATFORM-SITEMAP.md) | Every form URL |
| [FEATURE-GAPS.md](./FEATURE-GAPS.md) | Built vs open |
| [DOMAIN-ONTOLOGY.md](./DOMAIN-ONTOLOGY.md) | Glossary / SSOT |
