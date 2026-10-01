# AGENTS.md

Context for any AI agent (Claude Code or otherwise) working in this repository.

## What this repo is

A single self-contained HTML slide deck for a conference talk: **"Red Teaming
Agents: The New Attack Surface of Tools and State"**, built for Mass Open
(massopen.ai), an Agentic AI workshop series in Boston. Hosted via GitHub
Pages directly from this repo.

There is exactly one file that matters: `index.html`. There is no build
step, no package.json, no bundler. Treat any future addition of one as a
decision that needs to be flagged to the user first — the whole point of
this repo is that it deploys with zero tooling.

## Tech stack

- **reveal.js 4.6.1**, loaded from cdnjs (not vendored). CSS and JS are both
  pulled via `<link>`/`<script>` tags with `crossorigin="anonymous"` set —
  do not remove that attribute, it unmasks real errors in console instead of
  the browser hiding them behind a generic "Script error."
- **Fonts**: IBM Plex Sans (headings/body) and IBM Plex Mono (tool names,
  data labels, used sparingly), both via Google Fonts `<link>` tags.
- **Diagrams**: hand-written inline SVG, no diagramming library. Every
  diagram on every slide is literal markup in `index.html`, fully editable
  as text.
- **Theme**: light. See "Color system" below before changing anything
  visual.

## Repo / deploy workflow

- `main` branch, deployed via GitHub Pages set to "Deploy from a branch:
  main / root". Any push to `main` redeploys automatically within ~1-2
  minutes. Live at `https://williamcaban.github.io/261001-red-teaming-agents/`.
- No CI, no tests, no lint step. A commit is "done" when the file opens
  correctly in a browser.
- To preview locally before pushing: `python3 -m http.server` in the repo
  root, then open `localhost:8000`. Opening the file directly via
  `file://` sometimes behaves differently for cross-origin script loading
  than a real server does — prefer the local server when debugging
  anything that smells like a loading issue.

## Color system — read this before touching any color

Most colors are CSS custom properties defined once in `:root`:

```css
--ink, --panel, --panel-raised, --line, --text, --text-dim,
--attack, --attack-dim, --defend, --defend-dim, --neutral-accent
```

Changing a slide's background, card colors, or text colors should go
through these variables.

**The gotcha**: every inline SVG diagram uses hardcoded hex values
(`fill="#c43e2c"`, `stroke="#5b6472"`, etc.) directly as presentation
attributes, NOT `var(--attack)` references. SVG presentation attributes do
not resolve CSS custom properties the way stylesheet rules do. If you ever
re-theme this deck (e.g. dark mode again), you cannot just edit `:root` —
you also need to find-and-replace the literal hex strings inside every
`<svg>` block. Current semantic mapping, keep this consistent:

| Meaning | Hex |
|---|---|
| attack / threat / red-team | `#c43e2c` |
| defend / mitigation / guardrail | `#2d7a72` |
| neutral text-dim / arrows | `#5b6472` |
| neutral accent (sequence markers) | `#93700f` |
| structural line / border | `#d6dbe2` |

## Content conventions

- **No em-dashes.** Use a period, comma, or semicolon instead.
- **No time markers or lettered sub-sections in slide kickers.** An earlier
  version had "0:14 —" prefixes and "5a / 5b / 5c" labels on kickers; both
  were deliberately removed because the numbering scheme was inconsistent
  with the rest of the deck. Kickers are now short plain-text labels only
  (e.g. "Threat model walkthrough"). Don't reintroduce numbering unless the
  user explicitly asks for a consistent scheme across every slide.
- Direct assertions, scannable formatting, short sentences. Avoid stock
  transitions ("Furthermore," "It's worth noting") and avoid generic
  AI-design tells: no tracked-out ALL-CAPS eyebrows, no accenting a single
  word in a headline, no symmetrical three-card layouts unless the content
  is genuinely a 3-item sequence.
- When citing open-source tools (garak, promptfoo, NeMo Guardrails, EvalHub,
  etc.), keep claims about what they do current — this space moves fast
  (e.g. PyRIT was archived by Microsoft in March 2026; promptfoo joined
  OpenAI in March 2026 but stayed MIT-licensed). Verify before stating
  anything time-sensitive about a tool's status, ownership, or license.
- This deck is intentionally vendor-neutral/open-source-grounded, not a
  product pitch. Content pulled from internal Red Hat material (roadmap
  dates, unreleased product UI, internal org/governance tables) has been
  deliberately excluded. Don't add Red Hat product branding, version
  numbers, or roadmap specifics without the user explicitly asking for that
  framing shift.

## Known fixed bugs (don't reintroduce)

1. **Speaker notes crash**: `<aside class="notes">` blocks require the
   reveal.js Notes plugin to be loaded and registered in
   `Reveal.initialize({ plugins: [...] })`. If you add notes anywhere,
   confirm the plugin script tag and the `plugins` array both still include
   `RevealNotes`.
2. **`hash: false`**: Reveal is initialized with `hash: false`. This was
   set because `hash: true` throws a `SecurityError` on `history.replaceState`
   inside sandboxed preview iframes (e.g. Claude's own artifact preview,
   which runs the file at `about:srcdoc`). It's safe to flip back to `true`
   once this is confirmed running on the real GitHub Pages domain, where
   deep-linkable slide URLs (`#/7`) would be a nice-to-have.

## Making content changes

If asked to add/edit a slide: find the `<!-- SLIDE N: ... -->` HTML comment
markers, which number every section in order — keep this numbering/labeling
scheme in the comments (it's for maintainers reading the source, separate
from the on-slide kicker text discussed above, which has no numbering).

If asked to add a new diagram: match the existing pattern — a `<div
class="diagram-wrap">` wrapping a raw `<svg viewBox="...">`, reusing the
`.diag-rect`, `.diag-rect-attack`, `.diag-rect-defend`, `.diag-arrow`,
`.box-label`, `.box-sub` classes already defined in the `<style>` block
where possible, falling back to literal hex (per the color table above)
only for marker arrowheads and anything else CSS classes can't reach.
