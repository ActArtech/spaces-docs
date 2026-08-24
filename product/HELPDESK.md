# Spaces Helpdesk (how login and brand work)

**Product tickets:** https://help.spaces.keyteller.com/helpdesk  
**Studio launcher:** https://spaces.keyteller.com/studio/help-center  
**Do not bookmark:** https://help.spaces.keyteller.com/#login

## Why it asked for login again

Helpdesk is a **separate site** from Studio (`help.spaces.keyteller.com` vs `spaces.keyteller.com`). Logging into Studio does not log you into Helpdesk. The two products do not share cookies or the User table.

Open Helpdesk from **Studio → Helpdesk / Help Center** (Open Helpdesk home), or click **Continue from Studio** on the Helpdesk login page. That signs you in on **/helpdesk/my-tickets**. **/helpdesk/tickets** is the agent list. Do not use it as an owner.

Helpdesk is a different User table. The Studio password does not exist there until the demo owner is seeded, or until you use Continue from Studio. Demo Principal email is the same (`owner@spaces-demo.local`); after seed the Helpdesk password matches Studio (`SpacesDemo2026!`).

If you open the host root (`/` or `/#login` or `#login-with-email-link`) you get the Helpdesk login page, not the app. That is a different site from Studio.

## Why it showed a Frappe logo

Unconfigured Helpdesk uses Frappe defaults: "Login to Frappe" and the Frappe framework logo. The live site is branded **Spaces Helpdesk** (name, logo, favicon) via HD Settings and Website Settings. Password login for agents is still `/login`, with the Spaces logo.

## Knowledge base

Helpdesk Knowledge Base has two categories: **Getting started** and **Jobs to be done**. Articles link to each other (`/helpdesk/kb-public/articles/...`) and to Studio routes.

Start at **Start here: Knowledge Base map**. Jobs include P-J01 (Home decisions), P-J03 (approve and bill), P-J04 (win from pipeline), P-J05 (assign delivery), P-J07 (invoices and collect), P-J10 (client pack), D-J01 (Work first), OL-J01 (Owner Lite), S-J01 (site), F-J01 (finance). Also: Sign in, week loop, Words we use, Money, People, Capacity, Variations, How to report.

## How to report

1. In Studio, open Help Center and pick **component** + **situation**.
2. Or use the Helpdesk portal at `/helpdesk` after SSO.
3. Public GitHub issue forms on [spaces-docs](https://github.com/ActArtech/spaces-docs/issues/new/choose) are for browse-along reports, not the owner helpdesk UX.
