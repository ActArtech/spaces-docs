# How to report an issue exactly as you had it

## Why component + situation

Triage needs two axes:

| Axis | Question | Example |
|------|----------|---------|
| **Component** | Where in Spaces? | Money, People, Work, Home |
| **Situation** | What were you doing? | Creating, submitting, wrong numbers, phone |

Without both, "invoice broken" could mean Money create, Payments allocate, Docs evidence tick, or Finance company list.

## Steps

1. Open https://github.com/ActArtech/spaces-docs/issues/new/choose
2. Choose **Bug**, **Confusion**, or **Improvement**
3. Set **Component** and **Situation** (required)
4. Write what happened in your words
5. Add Job Order / URL if you have it
6. Submit

## Good example

- Component: Money (job invoices and bills)
- Situation: Submitting or cancelling
- Role: Owner / Principal
- What happened: Submit on draft SI showed success toast but Money due stayed 0 until hard refresh
- Expected: Due updates after submit without refresh
- Steps: Home → Project 2608-0001 → Money → open draft → Submit

## Bad example

- Title: "broken"
- No component, no situation, no steps

## After you file

Maintainers add `component:*` and `situation:*` labels from [LABELS.md](./LABELS.md) if the form did not map them automatically.
