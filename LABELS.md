# Issue labels (spaces-docs)

Apply these on every issue. Forms auto-add `needs-triage` plus a type label.

## Type

| Label | Use |
|-------|-----|
| `type:bug` | Broken behaviour |
| `type:docs` | Confusion / follow-along |
| `type:idea` | Improvement |
| `needs-triage` | Not yet routed |

## Component (add after triage)

| Label | Maps to form component |
|-------|------------------------|
| `component:home` | Home |
| `component:pipeline` | Pipeline / Leads / Opportunities |
| `component:projects` | Projects (Overview / job) |
| `component:people` | People (job roster) |
| `component:work` | Work |
| `component:capacity` | Capacity |
| `component:money` | Money |
| `component:payments` | Payments |
| `component:finance` | Finance (company-wide) |
| `component:docs` | Docs checklist |
| `component:activity` | Activity |
| `component:site` | Site |
| `component:pack` | Client Pack |
| `component:settings` | Settings / Admin |
| `component:auth` | Sign-in / session |
| `component:other` | Other |

## Situation (add after triage)

| Label | Maps to form situation |
|-------|------------------------|
| `situation:create` | Creating |
| `situation:edit` | Editing |
| `situation:submit` | Submitting / cancelling |
| `situation:export` | Exporting / printing |
| `situation:list` | Searching / filtering |
| `situation:detail` | Detail sheet / drawer |
| `situation:refresh` | After hard refresh |
| `situation:mobile` | Phone / small screen |
| `situation:auth` | Login / session |
| `situation:numbers` | Wrong numbers |
| `situation:permission` | Permission / Viewer |
| `situation:howto` | Cannot find how |
| `situation:ui` | Slow or UI broken |
| `situation:learn` | Learning / onboarding |
| `situation:other` | Other |

Create labels once with:

```bash
bash scripts/sync-spaces-docs-labels.sh
```

(from the product monorepo), or run the same `gh label create` commands listed in that script.
