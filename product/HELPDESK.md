# Spaces Helpdesk (how login and brand work)

**Product tickets:** https://help.spaces.keyteller.com/helpdesk  
**Studio launcher:** https://spaces.keyteller.com/studio/help-center  
**Do not bookmark:** https://help.spaces.keyteller.com/#login

## Why it asked for login again

Helpdesk is a **separate site** from Studio (`help.spaces.keyteller.com` vs `spaces.keyteller.com`). Logging into Studio does not log you into Helpdesk. The two products do not share cookies or the User table.

Open Helpdesk from **Studio → Helpdesk / Help Center**, or click **Continue from Studio** on the Helpdesk login page. Studio issues a short signed link and Helpdesk signs you in as a Helpdesk Contact on **/helpdesk/my-tickets** (customer portal). **/helpdesk/tickets** is the agent list and will bounce a Contact back to login.

If you open the host root (`/` or `/#login` or `#login-with-email-link`) you get the Helpdesk login page, not the app. That is a different site from Studio.

## Why it showed a Frappe logo

Unconfigured Helpdesk uses Frappe defaults: "Login to Frappe" and the Frappe framework logo. The live site is branded **Spaces Helpdesk** (name, logo, favicon) via HD Settings and Website Settings. Password login for agents is still `/login`, with the Spaces logo.

## Knowledge base

Helpdesk Knowledge Base (category **Spaces guides**) publishes:

- What Spaces Helpdesk is
- Sign in: Studio vs Helpdesk
- Follow along: owner week loop
- First 15 minutes in Studio
- How to report: component and situation
- Component and situation catalog
- Words we use
- Money: invoices, bills, payments
- People: who is on the job
- Project tabs

## How to report

1. In Studio, open Help Center and pick **component** + **situation**.
2. Or use the Helpdesk portal at `/helpdesk` after SSO.
3. Public GitHub issue forms on [spaces-docs](https://github.com/ActArtech/spaces-docs/issues/new/choose) are for browse-along reports, not the owner helpdesk UX.
