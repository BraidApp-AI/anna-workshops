# /design-sync recipes

Lazy-loaded recipes for non-default flows. Read these only when the task matches.

---

## Recipe: New mobile screen (React Native, anna-ui)

**Goal:** Add a new screen to the mobile app that fits the design system.

**Steps**

1. **Find the closest preview card.** Walk `anna-brand/preview/` and `anna-brand/ui_kits/mobile/` for layouts that match what you're building. Read the HTML/CSS to see exact spacing, type sizes, and component compositions.
2. **Create the screen file** under `mobile/screens/<NewScreenName>.tsx` (or your project's equivalent).
3. **Import tokens, not literals:**
   ```ts
   import { colors, typography, spacing } from '@anna/tokens';
   ```
4. **Reuse existing components** from `mobile/components/` when they fit. Do not rebuild `Button`, `Card`, `MessageBubble`, `TaskItem`, etc. — they are in the system already. If you genuinely need a new primitive, propose adding it to `anna-brand/mobile/components/` (and surface that in the report).
5. **Register the screen in the navigator** (find the existing nav stack — usually `mobile/navigation/`).
6. **Apply the hard rules.** Sage-500 only on CTAs, cream-100 backgrounds, sage-tinted shadows, sentence case, first-person Anna voice, no em dashes.
7. **Run the typechecker** (`pnpm --filter @anna/mobile typecheck`) and the dev server to verify it renders.
8. **Output the tell-and-show report.**

**Common mistakes to avoid**
- Inlining hex codes (always import from `@anna/tokens`)
- Adding a new icon library (Lucide only)
- Title-casing button labels ("Add Task" → "Add task")
- Using dots between meta items ("Tom · Today, 3pm" → "Tom | Today, 3pm")

---

## Recipe: New web page (Next.js, anna-ui)

**Goal:** Add a new route under `app/` that fits the design system.

**Steps**

1. **Find the closest preview card or ui_kit web example.** `anna-brand/ui_kits/web/index.html` is the canonical full-screen reference for the web app.
2. **Create the route** at `app/src/app/<route>/page.tsx`.
3. **Use Tailwind tokens already wired up** in `app/src/app/globals.css`. The tokens mirror `@anna/tokens`. Sage / cream / coral / priority families are all available as Tailwind classes (`bg-cream-100`, `text-sage-700`, etc.).
4. **Reuse components** from `app/src/components/`. The drop-in pack includes Button, Card, ChatInterface, BottomNav, SplashScreen, TrialBanner, AddTaskFAB, Toast, TaskPreviewCard, AnnaLogo, LoginPage, HowThisHelpsBanner, SuggestionChips.
5. **Apply the hard rules.** Same as mobile.
6. **Run `pnpm --filter @anna/web dev`** and verify the page loads, the layout doesn't overflow, and styles apply correctly.
7. **Output the tell-and-show report.**

---

## Recipe: New component

**Decision rule:** Add a new component only when no existing component (or composition of existing components) covers the use case. Most "new" UI is a composition of existing primitives.

**If you do need to add one**

1. **Decide which platform** — `anna-brand/mobile/components/` (RN) or `anna-brand/app/src/components/` (Next.js), or both.
2. **Match the API conventions of neighboring components.** Read 2-3 existing ones first. Note prop naming, default-prop patterns, how variants are handled, how tokens are imported.
3. **Write it in the canonical location first** (in `anna-brand`). If the engineer is working in `anna-ui`, you may need to mirror it back to `anna-ui/mobile/components/` or `anna-ui/app/src/components/` so they can use it immediately. Flag this in the report and suggest a follow-up to push the canonical change to `anna-brand` via PR.
4. **Add a preview card** in `anna-brand/preview/comp-<name>.html` that demonstrates the component visually. This is part of the design system contract — a component without a preview card is invisible to future agents.
5. **Update `anna-brand/index.html`** GROUPS array to include the new preview card under the right group (Components, etc.).
6. **Output the tell-and-show report**, with explicit pointers to the new files for review.

---

## Recipe: Port a Claude Design handoff

**Goal:** Translate a Claude Design export (HTML/CSS prototype) into production code for `anna-ui` web or mobile.

**Inputs**
- Either a `.tar.gz` handoff bundle (the canonical Claude Design export format), or a URL the engineer was given.
- A target — usually a screen path or component to land it at.

**Steps**

1. **Extract / inspect the bundle.** Find the chat transcripts in `<bundle>/anna-design-system/chats/` and read them — the chats encode user intent in a way the final HTML doesn't. Read them first.
2. **Find the primary design file** under `<bundle>/anna-design-system/project/`. The chats will tell you which file the user was last iterating on.
3. **Read the prototype HTML/CSS top to bottom**, then follow imports (shared CSS, scripts). Do not render or screenshot unless explicitly asked — everything you need is in the source.
4. **Map prototype primitives to real components.** A prototype `<button class="primary">` becomes `<Button variant="primary">`. A prototype card maps to `<Card>`. Reuse, don't recreate.
5. **Map prototype tokens to real tokens.** A prototype `#7C9A8E` is `colors.sage500`. Never preserve hex values.
6. **Match the layout, not the structure.** The prototype's DOM tree is not the spec — the visual output is. Build it the way React/RN would naturally express it.
7. **Apply the hard rules** — the prototype may have drifted (em dashes, dots in meta, etc.). Fix as you port.
8. **Output the tell-and-show report**, with an explicit map of prototype-element-to-real-component for the engineer to spot-check.

---

## Recipe: Audit a screen

**Goal:** Inventory the design-system drift on an existing screen and produce a punch-list.

**Steps**

1. **Read the screen file** end to end.
2. **Compare against the canonical system:**
   - Tokens: any inlined hex, rgb, or px values that should be tokens? (Cross-reference `anna-brand/packages/tokens/src/*.ts`.)
   - Components: any duplicated primitives the engineer rebuilt instead of reusing? (Cross-reference `mobile/components/` or `app/src/components/`.)
   - Voice: any em dashes, dots in meta rows, title-cased labels, emoji in product copy?
   - Patterns: does any summary follow "Done by Anna / Needs you / Heads up"? Any Lucide icons used? Sage-500 the only CTA?
3. **Produce a punch-list** as a numbered markdown report:
   ```markdown
   ## design-sync audit: <ScreenName>

   ### Token drift (3)
   1. Line 42: hex `#7C9A8E` → use `colors.sage500`
   2. ...

   ### Component reuse (1)
   1. Custom `Pill` component at line 88 duplicates `<Chip>` from `mobile/components/`

   ### Voice (2)
   1. Line 14: "Add Task" → "Add task" (sentence case)
   2. Line 99: meta separator `·` → `|`

   ### Patterns (0)
   No issues.
   ```
4. **Do not auto-fix.** Audits are read-only. If the engineer wants the fixes applied, they say so as a follow-up and you do them in a separate pass.
5. **Output the tell-and-show report** *in addition to* the audit punch-list (the report explains the audit method; the punch-list is the deliverable).

---

## Recipe: Token drift across repos

**Goal:** Verify that consuming repos (`anna-ui`, etc.) have tokens consistent with `anna-brand/packages/tokens/`.

**Steps**

1. **Read the canonical tokens** in `anna-brand/packages/tokens/src/{colors,typography,spacing,animations}.ts`.
2. **Read the consuming repo's tokens.** In `anna-ui`, that's `packages/tokens/src/*.ts`. In other repos that don't have a `@anna/tokens` package, look for theme files, CSS custom properties, or Tailwind configs that define color/type/spacing values.
3. **Diff:** for each token, do values match? Is the consuming repo missing tokens that exist canonically? Does it have tokens that don't exist canonically (drift the other way)?
4. **Report the diff** as a table. Do not auto-fix — let the engineer decide which side is right (sometimes the consuming repo intentionally extends tokens).
