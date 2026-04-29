# Brief for Claude Design: Add an "Engineers' guide" page

Paste this whole file into Claude Design and ask it to build the page described below.

---

## What to build

Add a new preview card to the Anna Design System website (the Vercel-hosted brand.hianna.ai). The page teaches engineers how to use the `/design-sync` Claude Code skill to build Anna-styled UI quickly and correctly. It is the entry point for any engineer who lands on the design system site needing to ship code.

## Where it goes

- **File:** `preview/engineers-skill.html` (matching the naming convention of other preview cards)
- **Navigation:** add a new top-level group to the sidebar in `index.html` called **"For engineers"**, placed directly under "Start here" and above "Brand". The group should contain one nav item:
  ```js
  { id: 'engineers-skill', label: 'Use the design system', src: 'preview/engineers-skill.html', sub: 'Build Anna UI with /design-sync' }
  ```

## Visual style

Match the existing preview cards exactly:

- Same `_base.html` shell, page padding, header band
- Lora 700 for display headings; Inter for body and code blocks
- Sage-500 (#7C9A8E) on cream-100 (#FAF8F5) surfaces
- Generous radii (16px+ for cards), sage-tinted soft shadows
- Code blocks: monospace, soft cream background, sage left-border for callouts
- Lucide icons only — never emoji, never another library

Reference cards to study before starting:
- `preview/brand-voice.html` — for prose-heavy layout with example blocks
- `preview/comp-buttons.html` — for code blocks and component examples
- `preview/animations-ruleset.html` — for the rules-list layout

## Content

The page has seven sections in this order. Use the prose below verbatim or with light edits for fit; do not change the substantive rules, examples, or technical pointers.

### 1. Hero

> # Build Anna UI without leaving the design system
>
> The `/design-sync` Claude Code skill loads this entire system — tokens, components, preview cards, voice rules — into your editor. Describe what you want; it builds it, tells you what it used, and teaches you the system as it goes.

### 2. Quick start (a single sage callout box)

> In Claude Code, in any of the Anna repos, just describe what you want:
>
> - "Add a Manage subscriptions screen to mobile settings"
> - "Polish this onboarding step to match the design system"
> - "Port this Claude Design export to Next.js"
>
> The skill auto-invokes when your task involves UI. No setup, no command to memorize. You can also call it explicitly: `/design-sync`.

### 3. What it does (three cards in a row)

**Reads the design system directly**
Tokens from `@anna/tokens`, components from `mobile/components/` and `app/src/components/`, preview cards from this site, voice rules from the brand README. No paraphrasing. The agent works from the actual files, so updates to the system flow through automatically.

**Generates UI that's faithful by default**
Uses real components. Imports real tokens. Follows the hard rules (Sage-500 for CTAs, cream-100 backgrounds, sage-tinted shadows, no em dashes, pipes between meta items, sentence case, no emoji in product copy).

**Tells you what it did, and why**
Every UI generation ends with a short report: which tokens it used, which components, which preview card it matched, which rules applied (and any it bent, with a reason). It points at "what to read next" so you absorb the system over time.

### 4. The output report (styled example block)

Under 150 words, all rules-applied bullets phrased conceptually (the way the brand book talks, not the token names), one-line `Read:` pointer. Show the following as a styled markdown / code block, like a sample Claude Code response:

```markdown
### What I did

- ✓ All values from tokens, no inline hex or px
- ✓ Sage-tinted surface with sage accent shadow, generous card radius
- ✓ Reused existing components: Button, Card, TaskItem
- ✓ Sentence case throughout; first-person ("I'll send Dan..."); pipes between meta items
- ⚠ Display type one step larger than canonical — layout needed extra weight; flag to revert.

Read: preview/comp-task-rows.html · references.md#new-mobile-screen
```

### 5. Hard rules cheatsheet (three columns or stacked groups)

**Visual**
- Sage-500 (#7C9A8E) is the only CTA color
- Coral-500 is a warmth accent only, never a CTA
- Backgrounds are cream-100 (#FAF8F5), never pure white
- Shadows are sage-tinted, never neutral gray
- Radii are generous; `xl` (16px) is the default card

**Voice**
- Sentence case everywhere: buttons, headings, labels, toasts
- First person singular for Anna ("I'll send Dan...")
- Never em dashes or en dashes; break with a period or comma
- Pipes between meta items, not dots or bullets
- Essentially never emoji in product UI

**Patterns**
- "Done by Anna / Needs you / Heads up" three-part briefing structure
- Lucide for every icon
- Self-host Inter and Outfit; do not pull from other CDNs ad hoc

### 6. When to use, when to skip (two columns)

**Use it for**
- Any new screen, component, or layout
- Polishing or refactoring existing UI
- Porting a Claude Design export to Next.js or React Native
- Auditing a screen against the design system

**Skip it for**
- Pure logic changes (data fetching, state machines, business rules)
- Non-UI bug fixes
- Internal admin tooling not user-facing

> To opt out for a single task, tell the agent: *"skip design-sync for this"*.

### 7. Where it lives (small technical footer, smaller type)

The skill lives at `.claude/skills/design-sync/` in:

- `anna-brand` (canonical source)
- `anna-ui`
- `anna-engine`
- `anna-asset-maker`
- `anna-workshops`
- `anna-lp`
- `anna-gtm`

The version in `anna-brand` is the source of truth. The other repos mirror it automatically when this repo updates, so the skill in every repo stays in lockstep with the design system.

---

## Constraints

- No em dashes, no en dashes anywhere on the page (this is a brand rule the page itself documents — the page must obey it)
- Sentence case for all headings, including section titles
- No emoji decoration; Lucide icons are fine where icons help
- The seven section structure above is the spec; do not reorder or merge sections
- Keep the page focused. It is a how-to-use, not a marketing page

## Acceptance check

The page is good when:
1. An engineer who has never seen the skill can read this page in two minutes and know how to invoke it
2. A skeptical engineer reads the rules cheatsheet and understands the constraints they will be held to
3. The visual style is indistinguishable from the existing preview cards
4. The page renders cleanly in the iframe shell of `index.html` with no layout overflow
