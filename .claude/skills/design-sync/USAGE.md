# /design-sync — engineer's quickstart

In-repo doc for engineers. The richer human-readable version lives at brand.hianna.ai (under "For engineers" once the next design system export ships).

## What it is

A Claude Code skill that loads the Anna design system into your editor and helps you ship UI faithfully and fast. It auto-invokes when your task involves UI; you never need to type a slash command.

## How to use it

In any of the Anna repos, just describe what you want:

- "Add a Manage subscriptions screen to mobile settings"
- "Polish this onboarding step to match the design system"
- "Port this Claude Design export to Next.js"

The skill auto-invokes. You can also call it explicitly: `/design-sync`.

## What it does

1. **Reads the design system directly** — tokens from `@anna/tokens`, components from `mobile/components/` and `app/src/components/`, preview cards from `anna-brand/preview/`, voice rules from `anna-brand/README.md`. No paraphrasing — the agent works from the canonical files.
2. **Generates UI that's faithful by default** — uses real components, imports real tokens, follows the hard rules (sage-500 only CTA, cream-100 backgrounds, sage-tinted shadows, sentence case, no em dashes, pipes between meta items, no emoji in product copy).
3. **Tells you what it did and why** — every UI generation ends with a short report: which tokens, which components, which preview card it matched, which rules applied (and any it bent, with a reason). It points at "what to read next" so you absorb the system over time.

## What the report looks like

```markdown
### What I built and why

Used from the design system:
- Tokens: sage500 (CTA), cream100 (bg), coral500 (accent)
- Components: <Button variant="primary">, <Card>, <TaskItem>
- Reference: preview/comp-task-rows.html
- Voice: sentence case throughout, "I'll send Dan..." first-person

Rules applied (and one I bent):
- ✓ Sage-tinted shadow on the card
- ✓ Pipes between meta items, not dots
- ⚠ Used Lora at 18px instead of canonical 16px because the layout
  needed extra weight; flag to revert.

Read next to internalize:
- preview/comp-task-rows.html
- references/recipes.md#new-mobile-screen
```

## When to skip

For pure logic changes, state machines, data fetching, infra. Or one-off:
just tell the agent: *"skip design-sync for this"*.

## Where it lives

Canonical: `anna-brand/.claude/skills/design-sync/`. Mirrored to `anna-ui`, `anna-engine`, `anna-asset-maker`, `anna-workshops`, `anna-lp`, `anna-gtm`. The mirrors auto-update from `anna-brand` via GitHub Action.

## More

For recipes (porting Claude Design handoffs, auditing a screen, building a new mobile screen from scratch), see `references.md` next to this file.
