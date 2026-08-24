# Spaces Studio — operational docs (public)

**Live product:** https://spaces.keyteller.com/studio/  
**This repo:** browse the system, follow along, and [report an issue](https://github.com/ActArtech/spaces-docs/issues/new/choose) with the right **component** and **situation**.

The application source stays in a private repo. This public pack is the operational understanding layer: glossary, OS overview, readiness, roles/jobs, and how to file a precise report.

---

## Start here

| Step | Read | Why |
|------|------|-----|
| 1 | [guides/FOLLOW-ALONG.md](./guides/FOLLOW-ALONG.md) | Owner week loop in plain language |
| 2 | [product/PLATFORM-GLOSSARY-AND-FUNCTIONS.md](./product/PLATFORM-GLOSSARY-AND-FUNCTIONS.md) | Say-this glossary + tab functions |
| 3 | [exports/Spaces-Platform-Glossary.pdf](./exports/Spaces-Platform-Glossary.pdf) | Clean printable export |
| 4 | [product/OPERATIONAL-READINESS-FULL-PASS.md](./product/OPERATIONAL-READINESS-FULL-PASS.md) | What is ready vs still open |
| 5 | [product/STUDIO-OPERATING-SYSTEM.md](./product/STUDIO-OPERATING-SYSTEM.md) | How the OS actually runs |
| 6 | [architecture/DOMAIN-ONTOLOGY.md](./architecture/DOMAIN-ONTOLOGY.md) | SSOT fields and write rules |
| 7 | [product/ROLES-JTBD-USER-STORIES.md](./product/ROLES-JTBD-USER-STORIES.md) | Roles and jobs-to-be-done |
| 8 | [guides/USER-GUIDE.md](./guides/USER-GUIDE.md) | Logins and surfaces |
| 9 | [product/HELPDESK.md](./product/HELPDESK.md) | Why Helpdesk asks to log in; Spaces brand not Frappe |

---

## Report an issue (exact component + situation)

1. Open **[New issue](https://github.com/ActArtech/spaces-docs/issues/new/choose)**.
2. Pick a form:
   - **Bug or broken behaviour** — something failed
   - **Confusion / cannot follow along** — docs or wording stuck you
   - **Improvement idea** — clearer path
3. Required fields:
   - **Component** — where in the product (Home, People, Money, Work, …)
   - **Situation** — what you were doing (creating, submitting, wrong numbers, phone, …)
   - What happened / what you expected / steps
4. Optional: Job Order (`YYMM-NNNN`), document name, URL, screenshot notes.

Do **not** open a blank issue. Forms are required so triage can route by component and situation. Label map: [LABELS.md](./LABELS.md).

---

## Folder map

| Path | Content |
|------|---------|
| `guides/` | Follow-along + user guide |
| `product/` | Glossary, readiness, OS, JTBD |
| `architecture/` | Ontology (SSOT) |
| `exports/` | Glossary PDF |
| `.github/ISSUE_TEMPLATE/` | Issue forms |

---

## Money and Docs (do not mix)

- **Money / Payments / Finance** = Sales Invoice, Purchase Invoice, Payment Entry (legal books).
- **Docs checklist** may list quotation / tax invoice / receipt as **evidence links** only ("do we have the file"). Ticking Docs is not posting an invoice.

---

## Source of truth

Curated from the private Spaces monorepo pack `docs/09-public-ops/`. Maintainers sync with `scripts/publish-spaces-docs.sh`.
