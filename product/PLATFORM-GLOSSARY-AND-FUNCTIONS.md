# Spaces Studio - Platform glossary and functions

**Export companion:** [Spaces-Platform-Glossary.pdf](./exports/Spaces-Platform-Glossary.pdf)  
**Date:** 2026-08-20  
**Product:** https://spaces.keyteller.com/studio/  
**Ontology SSOT:** [DOMAIN-ONTOLOGY.md](../02-architecture/DOMAIN-ONTOLOGY.md)  
**JTBD SSOT:** [ROLES-JTBD-USER-STORIES.md](./ROLES-JTBD-USER-STORIES.md)

Clean language for owners. System names in backticks for implementers.

---

## 1. What Spaces is

Spaces Studio is the operating system for an interior design studio.  
Daily work happens in Studio Web. Desk is infrastructure only.  
Money is ERPNext Sales Invoice, Purchase Invoice, and Payment Entry. There is no second studio ledger.

| Layer | Role |
|-------|------|
| Frappe | Engine: users, documents, permissions, API |
| ERPNext | Company books and jobs |
| Spaces | Studio cockpit, Activity, job scale, pack, agent wrap |

---

## 2. Glossary (say this)

| Say this | Means | In the system | Do not confuse with |
|----------|-------|---------------|---------------------|
| Job | A live delivery project | `Project` | Task, Opportunity |
| Job Order | Human job number `YYMM-NNNN` | `Project.studio_project_code` | ERP id `PROJ-0002` |
| Client | Who pays and receives the pack | `Customer` | Lead |
| Lead | Enquiry not yet a job | `Lead` | Lead Architect person |
| Opportunity | Qualified job we might win | `Opportunity` | Quotation PDF |
| Lead (person) | Owns the job | `Project.lead_architect` | CRM Lead |
| Support | Other people on the job or task | Project Team Member / Task `_assign` rest | Informed (not built) |
| Work | Tasks for delivery | `Task` | Capacity |
| People (job) | Who is on this job | People tab | Work assignees |
| Activity | Job thread and notes | Studio Activity | Ledger |
| Mention | `@` link to a record | `studio_mentions` | Invoice |
| Money | This job invoices and bills | Money tab = SI + PI | Payments |
| Payments | Cash in and out on this job | Payment Entry | Invoice |
| Overhead | Studio cost with no job | PI with empty project | Job supplier bill |
| Capacity | This week load | Job scale × team share | Timesheet hours |
| Snag | Site issue to close | Issue | Variation |
| Variation | Scope change | Project variation flow | Invoice (cash after SI) |
| Pack | Client-visible files | `client_visible` fail-closed | Internal docs |
| Viewer | Read-only Studio user | persist deny | Guest pack |

---

## 3. Project tabs (functions)

| Tab | Owner sentence | Relates |
|-----|----------------|---------|
| Overview | Job at a glance: status, next date, links | People holds the roster |
| People | Who is on this job, one row per person | Lead field + Team Member roster |
| Activity | Job thread; not the ledger | Mentions link records; money stays SI/PI/PE |
| Money | Job value, invoices, bills (VAT Net) | Payments is cash |
| Payments | Collected and paid on this job | Still due comes from Money |
| Docs | Delivery checklist and evidence links (quotation / tax invoice / receipt rows mean "do we have the file") | Not the ledger; SI/PI/PE stay money |
| Variations | Approve scope change | Cash only after Sales Invoice + Payment Entry |

---

## 4. Finance tabs (functions)

| Tab | Owner sentence |
|-----|----------------|
| Accounts receivable | Client invoices we are waiting to collect |
| Accounts payable | Supplier bills this studio still needs to pay |
| Payments | Cash in and cash out allocated to invoices and bills |
| Reports | Totals and VAT Net across posted invoices |
| Statement | One client's invoices and receipts together |
| Overhead | Studio running costs not billed to a single job |
| Monthly costs | This month's salaries (read) and journal expenses |
| Ledger | GL lines posted from invoices and payments (read) |

---

## 5. Core write paths (one writer each)

| Function | Writer | Notes |
|----------|--------|-------|
| Record Activity | `record_studio_activity` | Mentions applied before save |
| Create task | `create_studio_task` | Same writer from Activity / updates |
| Task people | `set_task_assignees` | Ordered `_assign` |
| Job people | `set_project_people` / team upsert | Lead + Team Member |
| Job scale | Capacity patch | `studio_job_scale` |
| Client invoice | Sales Invoice | Money / AR |
| Supplier bill | Purchase Invoice | Job PI requires project |
| Overhead | `create_studio_overhead` | PI, project empty, one time or monthly |
| Payment | Payment Entry | Allocate to SI/PI |
| Snag | Issue create/update | Site + project panel |
| Viewer block | `deny_studio_viewer_write` | Before every Studio write |

---

## 6. Role landings (JTBD)

| Role | Lands on | Primary job |
|------|----------|-------------|
| Principal | Home | Decide money and delivery risk |
| Owner Lite | Home | Same without Admin |
| Design Lead | Work | Clear overdue and own jobs |
| Finance | Finance | Collect and pay |
| Site | Site | Close issues and site tasks |
| Viewer | Home | Read only |

---

## 7. Operational readiness waves (summary)

| Wave | Goal | Done-when |
|------|------|-----------|
| A Proof | Owner-share | 15-min PASS, Viewer 403, Pack guest PASS |
| B Site | Delivery depth | Spec in/out then build |
| C Polish | Phone, Work by project, expense claim UI | Shipped surfaces |
| D Desk honesty | Setup stays support-only | DESK-GAPS marked |
| E Agent | Laptop token MCP | Named user, no admin bot |

---

## 8. UI copy rules

1. Prefer studio words: Job, Client, Lead, Work, Money, Pack.  
2. One sentence under each Finance and Project tab (SectionExplainer).  
3. Empty states say what to do next, not "No data".  
4. Never train the owner to Desk for a closed DESK-GAPS row.  
5. Never invent a second money number on Home that is not SI-derived.
