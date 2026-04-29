---
name: design-sync
description: Use when building, polishing, refactoring, porting, or auditing UI in any Anna repo (anna-ui web/mobile, anna-engine email/admin, anna-lp marketing, anna-asset-maker, anna-gtm, anna-workshops). Loads the Anna design system from anna-brand and produces faithful UI that uses the real tokens and components, follows the brand's hard rules, and reports back what it used so the engineer learns the system over time.
---

# /design-sync

You are helping the engineer ship UI for Anna — an AI personal assistant for families, made by Braid. Anna's voice is "the calm, capable friend who has read your inbox for you." The design system is rich and opinionated; your job is to produce UI that feels native to it without paraphrasing or guessing.

## How to use this skill

Work in this order, every time:

1. **Confirm you can reach the design system.** It lives in `anna-brand`. If you're already in `anna-brand`, paths below are relative to repo root. If you're in a consuming repo (anna-ui, anna-engine, etc.), check for a sibling `../anna-brand/` directory. If it isn't there, run `gh repo clone BraidApp-AI/anna-brand ../anna-brand` once. From here on, "anna-brand/" prefixes paths in this file.
2. **Read the closest existing reference before writing new code.** Find the preview card or component in anna-brand that's nearest to what you're building. Open it. Match it. Don't invent new patterns when an exemplar exists.
3. **Use the real tokens and components.** Never inline hex codes when a token exists. Never re-build a component when one is already in `mobile/components/` or `app/src/components/`.
4. **Apply the hard rules below.** They are non-negotiable. If you have to bend one, say so explicitly in the report.
5. **End with the tell-and-show report.** Required after every UI generation. Format below.
6. **For deeper or non-default work**, read `references.md` for recipes (porting a Claude Design handoff, auditing a screen, building a new mobile screen from scratch, etc.).

## Where to look (the design system, by file)

The agent reads these directly. Do not paraphrase them; open them.

| What you need | Where it lives in `anna-brand` |
| --- | --- |
| Brand context, voice rules, content fundamentals | `README.md` |
| Design tokens as TypeScript exports | `packages/tokens/src/{colors,typography,spacing,animations}.ts` |
| Design tokens as CSS custom properties | `colors_and_type.css` |
| Preview cards (visual reference for every component, color, type, motion concept) | `preview/*.html` (29 cards) |
| Web (Next.js) component scaffolds | `app/src/components/*.tsx` |
| Mobile (React Native) component scaffolds | `mobile/components/*.tsx` |
| Mobile theme | `mobile/theme/{colors,typography,spacing,animations}.ts` |
| Web theme + Tailwind config | `app/src/app/globals.css` |
| Full-screen examples (web app) | `ui_kits/web/{index.html,components.jsx}` |
| Full-screen examples (mobile app) | `ui_kits/mobile/{index.html,components.jsx,ios-frame.jsx}` |
| Logos, banners, sage backgrounds | `Logos/`, `assets/brand/` |

When the engineer is in a consuming repo, prefer that repo's local imports first (e.g., `@anna/tokens` in anna-ui), then fall back to reading the canonical files in `anna-brand` for visual reference and patterns.

## Hard rules (invariants)

These do not change per-task. If you bend one, declare it in the report.

**Visual**
- Sage-500 (#7C9A8E) is the only CTA color.
- Coral-500 is a warmth accent only, never a CTA.
- Backgrounds are cream-100 (#FAF8F5), never pure white.
- Shadows are sage-tinted, never neutral gray.
- Radii are generous; `xl` (16px) is the default card.
- Self-host Inter and Outfit; do not pull from arbitrary CDNs ad hoc.

**Voice**
- Sentence case everywhere: buttons, headings, labels, toasts, nav.
- First person singular for Anna ("I'll send Dan a reminder...").
- Never em dashes or en dashes. Break with a period or comma.
- Separate meta items with pipes (`|`), not dots (`·`) or bullets.
- Essentially never emoji in product UI.
- Use contractions. Exclamation marks very sparingly, one word ("Done!"), never two in a row.
- Oxford comma.

**Patterns**
- "Done by Anna / Needs you / Heads up" three-part briefing structure when summarizing.
- Lucide for every icon. Never another icon library, never emoji as icons.
- The product name is always "Anna", never "ANNA" or "anna".

## Tell-and-show report (required output)

After any UI work, end your response with this block. **Keep it under 150 words.** The shape is fixed: 4 to 7 short rules-applied bullets, then a one-line `Read:` pointer.

```markdown
### What I did

- ✓ <Brand rule, phrased the way the brand book talks about it>: <where it shows up>
- ✓ <Brand rule>: <user-facing detail or named component>
- ✓ <Brand rule>: <specifics>
- ⚠ <Rule bent or system-wide drift you noticed>: <one-line reason>

Read: <file in anna-brand> · <one more if useful>
```

**Phrase rules conceptually, not in token names.** "Sage-tinted surface", "generous card radius", "sentence case", "sage-only CTA" — that's how the brand book talks; that's what engineers need to remember. `colors.sage500`, `borderRadius.lg`, `12px`, `#FAF8F5` make their eyes glaze. When listing concrete details, prefer things they actually care about: user-facing labels, voice fragments, the preview card you matched, named components reused. Skip generic phrasings like "✓ followed brand rules".

The trailing `Read:` line is the teaching loop. Point at the canonical reference for what was just built; add one more only if it materially helps.

Engineers are busy. 30-second skim, not an essay.

## When to skip the skill

Skip on tasks that aren't user-facing UI: pure logic changes, data fetching, state machines, migrations, infra. The engineer can also opt out for a single task by saying *"skip design-sync for this"* — when they do, do not run the skill or output the report.

## Recipes

For non-default flows, read `references.md`:

- **New mobile screen** — scaffold, imports, navigation registration
- **New web page** — Next.js route + layout + components
- **New component** — when to add to `mobile/components/` or `app/src/components/`
- **Port a Claude Design handoff** — translate exported HTML/CSS prototype to RN/Next.js
- **Audit a screen** — token drift, copy drift, voice drift; produce a punch-list

## Where this skill lives

Canonical: `anna-brand/.claude/skills/design-sync/` (this file).

Mirrored to: `anna-ui`, `anna-engine`, `anna-asset-maker`, `anna-workshops`, `anna-lp`, `anna-gtm`. Mirrors update automatically when this canonical version changes (see `.github/workflows/mirror-design-sync-skill.yml` in `anna-brand`).

If the version in a consuming repo drifts from `anna-brand`, the canonical wins. Tell the engineer and offer to re-sync.
