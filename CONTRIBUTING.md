# Contributing to spaces-docs

## Browse

Start at [README.md](./README.md) then [guides/FOLLOW-ALONG.md](./guides/FOLLOW-ALONG.md).

## Report

Always use a form: https://github.com/ActArtech/spaces-docs/issues/new/choose

Required on every report:

1. **Component** — product surface
2. **Situation** — what you were doing
3. What happened / expected / steps (bugs)

See [HOW-TO-REPORT.md](./HOW-TO-REPORT.md).

## Do not

- Paste secrets, passwords, or live bank tokens
- Open blank issues (`blank_issues_enabled: false`)
- Ask for Desk as the daily product path

## Maintainers

Publish from the private monorepo:

```bash
bash scripts/publish-spaces-docs.sh
bash scripts/sync-spaces-docs-labels.sh
```
