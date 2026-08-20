# Spaces operational readiness full pass

**Date:** 2026-08-20  
**Live tip (honest):** `4a1d2cf` on `origin/main` and VPS `/opt/spaces` (Studio Web image rebuilt after this tip)  
**Product UI:** https://spaces.keyteller.com/studio/  
**Policy:** UPDATE-RULES G1-G4 - Studio Web only; one writer per field; money = SI / PI / PE / GL; no second ledger

**Matched to:** [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md) · [ROLES-JTBD-USER-STORIES.md](./ROLES-JTBD-USER-STORIES.md) · [ONTOLOGY-JTBD-ALIGNMENT.md](./ONTOLOGY-JTBD-ALIGNMENT.md) · [PRODUCT-GLOSSARY.md](./PRODUCT-GLOSSARY.md) · [PLATFORM-GLOSSARY-AND-FUNCTIONS.md](./PLATFORM-GLOSSARY-AND-FUNCTIONS.md) · [OWNER-SHARE-READINESS.md](./OWNER-SHARE-READINESS.md) · [PRODUCTION-READINESS-AND-SPRINTS.md](./PRODUCTION-READINESS-AND-SPRINTS.md) · [GAP-ANALYSIS-2026-08.md](./GAP-ANALYSIS-2026-08.md) · [DESK-GAPS.md](./DESK-GAPS.md)

This file is the **consolidated** operational verdict + gap list + multi-wave plan. It does not replace WR ids in [GAPS.md](../04-studio-web/GAPS.md) or the dated snapshot in GAP-ANALYSIS-2026-08.

---

## Verdict

**Operational for a staffed studio principal and team on Studio Web.**  
**Not owner-share / client-hand-off ready** until Wave A proofs are logged PASS.

The owner can run the core business loop in `/studio/` without Desk for daily work: win jobs, staff them, track work, invoice, collect, pay suppliers, capacity, snags, overhead, credit notes, activity, and export. Desk remains for rare setup (print-format design, letterhead design, fiscal year / opening GL / freeze-merge, full User form, salary structure).

| Question | Answer |
|----------|--------|
| Can the owner run the studio week without Desk? | **Mostly yes** for job + money + people + capacity + snags + overhead |
| Can a new principal finish unaided in 15 minutes? | **Not proven** (script exists; no logged PASS) |
| Does Viewer fail closed on writes? | **In code yes**; live login smoke not logged |
| Is money a second studio ledger? | **No.** SI / PI / PE / GL only |
| Are Docs checklist finance titles a second invoice book? | **No.** Evidence links only ("do we have the file"); legal money stays SI/PI/PE |
| Is Desk the product? | **No.** Infrastructure and rare support only |
| Live tip honesty | VPS + GitHub at `4a1d2cf` after verify-and-ship + ship-studio-web |

---

## Owner can-do / cannot-do (full-business loops)

### Can do on Studio Web today

| Loop | How | Ontology SSOT |
|------|-----|---------------|
| See what needs me | Home ranked action list + strip | Cockpit cards; money figures from SI |
| Win work | Pipeline Leads / Opportunities; convert | Lead, Opportunity |
| Open a job | Projects + Job Order `YYMM-NNNN` | Project + `studio_project_code` |
| Staff a job | People tab (Lead + roster table) | `lead_architect` + Project Team Member |
| Assign task work | Work People column / picker | Task `_assign` Lead=index 0 |
| Talk on the job | Activity tab + mentions | Studio Activity + `studio_mentions` |
| Track delivery papers | Docs checklist (incl. finance **evidence** titles) | Project Document Link checklist |
| Invoice / bill | Money + Finance create SI/PI (optional category lines) | Sales Invoice / Purchase Invoice |
| Credit / return | Credit note sheet (amount or lines) | Return SI/PI |
| Collect / pay | Payments + Finance PE | Payment Entry |
| Studio overhead | Finance Overhead Add (one-time / monthly) | PI with empty project |
| This week load | Capacity Add / Adjust | `studio_job_scale` + team share |
| Snags / site buys | Site + project panel | Issue, MR, PO |
| Export / print invoices | Money / Finance CSV + printview | SI/PI print; not PE as invoice |
| Agent assist | Named tools over existing writers | `mcp_studio_agent` wrap only |

### Cannot do yet (honest)

| Loop | Blocker | Wave |
|------|---------|------|
| Unaided owner-share handoff | 15-min path not logged PASS | A |
| Prove Viewer closed on live | Source deny yes; live 403 smoke open | A |
| Prove Pack guest fail-closed in room | Code live; walkthrough not logged | A |
| Deep site drawing / DLP / RFQ | Spec first; thin today | B |
| Design print / letterhead / FY / freeze / salary structure create | Stay Desk / support | D |
| Square calculator / Informed role | Spec or will-not-do | E / never |
| Laptop OAuth MCP door | Named-user token not built | E |

---

## Owner job map (JTBD × Studio surface × ontology)

| Owner job (JTBD) | Studio surface | Ontology SSOT | Status |
|------------------|----------------|---------------|--------|
| Decide what needs me today | Home action list + strip | Cockpit cards; SI outstanding | Live |
| Win work | Pipeline / Opportunities | Lead, Opportunity | Live |
| Open a job | Projects create | Project + Job Order | Live |
| See who is on the job | People tab | `lead_architect` + Team Member | Live |
| Assign delivery work | Work list / kanban / calendar | Task `_assign` | Live |
| Edit job-copied template tasks | Work surfaces when project set | Task on Project (not library master) | Live |
| Job thread | Activity | Studio Activity + mentions | Live |
| Invoice client | Money + Finance AR | Sales Invoice | Live |
| Bill supplier | Money + Finance AP | Purchase Invoice | Live |
| Classify invoice lines | SI/PI create category lines | Catalog categories on SI/PI items | Live |
| Credit / return | Credit note sheet | Return invoice | Live |
| Collect / pay | Payments + Finance | Payment Entry | Live |
| Studio overhead | Finance Overhead | PI empty project + cadence | Live |
| This week load | Capacity | `studio_job_scale` × share × activity | Live |
| Evidence papers on the job | Docs checklist | Document Link checklist (not ledger) | Live |
| Snags / handover | Site | Issue + handover tasks | Live |
| Share client-visible pack | Pack link | `client_visible=1` fail-closed | Live (proof open) |
| Design print format | Desk | Print Format | Stay Desk |
| Run payroll create | Desk / Monthly read | Salary Slip | Stay Desk |
| Fiscal year / opening GL | Desk | Company / GL setup | Stay Desk |

---

## Gap analysis (current)

| ID | Gap | Severity | JTBD hurt | Ontology note | Wave |
|----|-----|----------|-----------|---------------|------|
| R1 | 15-min unaided owner path not logged | P1 | Owner-share JTBD | Script only; no product change required | A |
| R2 | Viewer live write-403 smoke not logged | P1 | Viewer read-only job | `deny_studio_viewer_write` exists | A |
| R3 | Client Pack guest walkthrough not logged | P1 | Client pack job | Fail-closed code live | A |
| R4 | Site drawing / DLP / RFQ depth | P1 | Site delivery JTBD | Stay Issue/PO/PI; no new DocType until spec | B |
| R5 | Rare Project child tables off sheet | P1 | Job profile depth | Commercial plan SSOT stays Project children | B |
| R6 | Phone bottom nav polish | P2 | Mobile owner loop | Shell only | C |
| R7 | Work by-project swimlanes | P2 | Work clarity JTBD | Task still SSOT | C |
| R8 | `@user` notify | P2 | Activity follow-up | Mentions persist; notify via existing helper only | C |
| R9 | Expense claim UI sheet | P2 | Expense JTBD | API `create_studio_expense_claim` exists; no second money DocType | C |
| R10 | Print design, letterhead, FY, freeze/merge, salary structure | P2 setup | Rare admin | Explicit Desk / support-only | D |
| R11 | Square calculator | P3 | Spec | No second ledger | Spec / never |
| R12 | Informed role | P3 | Spec | Not a Task `_assign` third list | Spec / never |
| R13 | Laptop OAuth MCP | P3 | Agentic laptop | Wrap only; named-user token | E |
| R14 | Client/Lead sheet stubs unwired | P2 | CRM create clarity | Sheets exist; list pages not importing yet | C |

**Closed or narrowed since older snapshots (do not re-open as live gaps):**

- Capacity Add/Adjust, invoice CSV/print, Home action list, People tab, job-template unlock, overhead Add, PE-block cancel copy, credit-note line payload, SI/PI category lines, Docs finance **evidence** titles (not a second invoice book), Studio Web tip ship after test exclude fix.

**Will not build:** second ledger, Macro fork, fake Informed, Square without units, Customize Form as product, stock/warehouse as studio path, full client portal SPA.

---

## Production-ready waves (JTBD + ontology)

### Wave A - Proof (required for owner-share)

| Wave item | JTBD | Ontology / SSOT | Done-when |
|-----------|------|-----------------|-----------|
| A1 Owner 15-min | Principal runs week unaided | Home → Project Money → Capacity → export | WHAT-WE-DID PASS with date |
| A2 Viewer smoke | Viewer stays read-only | `deny_studio_viewer_write` | GET 200 / POST 403 logged |
| A3 Pack guest | Client sees only allowed rows | `client_visible=1` | Guest walkthrough FAIL-CLOSED logged |

### Wave B - Site delivery depth (spec then build)

| Wave item | JTBD | Ontology / SSOT | Done-when |
|-----------|------|-----------------|-----------|
| B1 Drawing / DLP / RFQ in/out | Site lead closes delivery papers | Issue / PO / PI only unless product writes otherwise | Written in/out list |
| B2 Rare Project children | Principal completes job profile | Project child tables | In-scope rows on sheet + test |

### Wave C - Usability polish

| Wave item | JTBD | Ontology / SSOT | Done-when |
|-----------|------|-----------------|-----------|
| C1 Phone nav | Owner on phone | Shell routes | Decision shipped or deferred note |
| C2 Work by-project | Lead sees work per job | Task | Optional view live |
| C3 Mention notify | Follow-up on `@user` | Existing notify helper + Activity | Notify without new table |
| C4 Expense claim sheet | Staff claim costs | Expense Claim API path | Sheet on Monthly |
| C5 Wire client/lead sheets | CRM create clarity | Customer / Lead writers | Sheets opened from lists |

### Wave D - Explicit Desk / support-only

| Wave item | JTBD | Ontology / SSOT | Done-when |
|-----------|------|-----------------|-----------|
| D1 Print / letterhead / FY / freeze / salary structure | Rare admin | ERP setup DocTypes | DESK-GAPS marked support-only; no owner training to Desk |

### Wave E - Agentic laptop door

| Wave item | JTBD | Ontology / SSOT | Done-when |
|-----------|------|-----------------|-----------|
| E1 Named-user API token → `mcp_studio_agent` | Owner agent assist | Existing writers only | Viewer token cannot write |

---

## Glossary and functions export

Clean owner language + functions: [PLATFORM-GLOSSARY-AND-FUNCTIONS.md](./PLATFORM-GLOSSARY-AND-FUNCTIONS.md)  
PDF: [exports/Spaces-Platform-Glossary.pdf](./exports/Spaces-Platform-Glossary.pdf)  
Regenerate: `python scripts/export-platform-glossary-pdf.py`

Field-level SSOT remains [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md). Product phrases remain [PRODUCT-GLOSSARY.md](./PRODUCT-GLOSSARY.md).

---

## Definition of owner-share ready

Tick all before handing the URL to a new hire or client as finished product:

- [ ] Hard-refresh `/studio/` on tip `4a1d2cf` or newer
- [ ] One real job: Money matches SI/PI; Payments match PE
- [ ] Owner 15-min path PASS logged
- [ ] Viewer write 403 PASS logged
- [ ] Pack guest fail-closed PASS logged
- [ ] No training path that sends daily work to Desk
