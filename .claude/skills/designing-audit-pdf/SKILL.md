---
name: designing-audit-pdf
description: Renders an agent-discoverability audit markdown file into a client-ready styled HTML/PDF. Use when the user says "render the audit", "make the PDF", "design the audit PDF", "format the audit for client send", "style the draft", "/design-audit", "turn this audit into a PDF", or after the running-an-audit skill produces a draft markdown and the operator wants the client-facing deliverable. Reads the markdown audit, embeds the bundled audit-styles.css, writes styled HTML to the client folder, and auto-opens in Brave for print-to-PDF (Cmd+P → Save as PDF).
---

# Designing the Audit PDF

Closes the gap between the auditor's markdown draft and a client-ready deliverable. Reads `clients/<slug>/draft-audit-<date>.md`, converts to HTML with the bundled stylesheet, and opens in Brave for the operator to print to PDF.

The audit template section structure is the source of truth; this skill only handles visual rendering. Never edit the audit content during rendering.

## Context Required

- `deliverables/audit-template.md` — section structure reference (so the renderer knows what to expect)
- `.claude/skills/designing-audit-pdf/assets/audit-styles.css` — bundled stylesheet (read into the HTML output)

## Steps

### Step 1 — Locate the source markdown

Default: the most recent `clients/<slug>/draft-audit-*.md` for the slug the operator specified. If the operator passes a specific path, use that. If multiple drafts exist for the same date, ask the operator which to render. Never silently pick.

### Step 2 — Read the markdown and the stylesheet

```bash
cat clients/<slug>/draft-audit-<date>.md
cat .claude/skills/designing-audit-pdf/assets/audit-styles.css
```

### Step 3 — Convert markdown to HTML

Do the conversion inline (don't shell out to pandoc — keep the skill dependency-free). Handle these elements from the audit template:

- `#` H1 → `<h1>` (cover title, serif via CSS)
- `##` H2 → `<h2>` (section headers, sans bold)
- `###` H3 → `<h3>`
- `####` H4 → `<h4>` (treated as eyebrow/uppercase)
- Lines starting with `- ` or `* ` → `<ul><li>`
- Lines starting with `1. ` etc. → `<ol><li>`
- GFM tables (pipe syntax) → `<table>` with `<thead>`/`<tbody>`
- ` ```lang ... ``` ` → `<pre><code>`
- Inline `` `code` `` → `<code>`
- `**bold**` → `<strong>`
- `*italic*` or `_italic_` → `<em>`
- `[text](url)` → `<a href="url">`
- `>` blockquote → `<blockquote>`
- Blank-line-separated paragraphs → `<p>`
- `---` → `<hr>`

### Step 4 — Apply audit-specific HTML enrichments

After basic conversion, apply these transformations to make the audit visually load-bearing:

**Section 1 cover meta** — The audit-template's Section 1 lists Client name, URL+date, Auditor, Headline. Wrap these in `<p class="cover-meta">` to get the serif-muted treatment.

**Section 2 overall score** — Find the line like `Overall score: 47/100` (with or without a cap callout). Replace with:

```html
<div class="score-overall">47<span class="denom">/100</span></div>
```

If the line includes "Capped at X because Y", render an additional `<div class="cap-callout">` immediately after the score with the reason.

**Section 4 pillar table** — Add `class="pillar-table"` to the main pillar scores table so the column widths align (pillar name 22%, score 10% centered, severity centered).

**Severity tags** — Wherever the text reads "Critical", "Important", or "Nice-to-have" inside a `<td>`, wrap in:

```html
<span class="severity-critical">Critical</span>
<span class="severity-important">Important</span>
<span class="severity-nice">Nice-to-have</span>
```

For pillars with no severity tag (scores 8–10), render as plain text or `<span class="severity-strong">Strong</span>` if surfaced in Section 2 as a strength.

### Step 5 — Wrap in HTML5 boilerplate

Template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Agent Discoverability Audit — [Client Name]</title>
  <style>
    [PASTE audit-styles.css HERE]
  </style>
</head>
<body>
[CONVERTED HTML]
</body>
</html>
```

Embed the CSS inline (don't link to the file path — the PDF needs to be self-contained so the operator can email the HTML if needed).

### Step 6 — Write and open

Write to `clients/<slug>/audit-<YYYY-MM-DD>.html` (note: drops the "draft-" prefix because this is the client-facing render). Then open in Brave:

```bash
open -a "Brave Browser" clients/<slug>/audit-<YYYY-MM-DD>.html
```

### Step 7 — Hand off to operator

Output a short message:

```
Audit rendered: clients/<slug>/audit-<date>.html
Opened in Brave. Print to PDF: Cmd+P → Destination: Save as PDF → Save.

Suggested filename: <ClientName>-AgentDiscoverabilityAudit-<YYYY-MM-DD>.pdf
```

Do not save the PDF for the operator. Print-to-PDF preserves the operator's preview before send and lets them save to the location they prefer.

## Output Format

The rendered HTML file at `clients/<slug>/audit-<YYYY-MM-DD>.html`. Self-contained (embedded CSS), letter-page-sized, print-optimized. Section structure preserved from the source markdown — no content edits.

## Gotchas

- **Don't edit the audit content.** Visual rendering only. If a typo or claim issue is in the markdown, fix it in the markdown first, then re-render.
- **Don't link to the external CSS file.** Embed inline so the HTML is portable (operator may want to send a colleague the raw HTML, or archive).
- **Markdown tables with empty cells break GFM parsers.** If the audit has empty pillar cells, fill with `—` (em dash) in the source markdown before rendering.
- **Pillar 3 row gets no severity tag.** It's signal-only. Don't apply severity coloring; render as plain text "—" or "Reported" in the severity column.
- **Don't auto-print to PDF.** Print-to-PDF triggers a system dialog that requires user interaction; trying to script it via `lp` or `cupsfilter` produces uglier output than Brave's print preview. Operator does the print step.
- **Page break before each major section.** The CSS handles this via `h2 { page-break-before: ... }` and `@page` size letter. If a section is short and the prior one ends near the bottom of a page, Brave's print engine handles widow/orphan reasonably; don't add manual page breaks.
- **Brave is required (not Chrome).** Per operator preference, Chrome isn't installed. Use `open -a "Brave Browser"` exactly — `open -a "Google Chrome"` will fail. Never use `--incognito` or `-na`.
- **Don't re-open the file every time.** If the operator runs the skill twice in a row, the second invocation overwrites the HTML; Brave will need a hard reload (`Cmd+Shift+R`) to see updated styles. Don't auto-spawn additional Brave windows.

## Constraints

- **Visual rendering only.** No content edits, no copy changes, no severity-tag re-derivation. The auditor + QA pass own audit correctness; this skill owns visual delivery.
- **Self-contained HTML.** Embedded CSS, no external references. Operator may forward the HTML directly to a colleague.
- **Open in Brave.** Per operator's browser preference. Never Safari, never Chrome.
- **Drop the "draft-" prefix on output filename.** The rendered HTML is the client-facing artifact. The markdown stays prefixed as draft until manually flipped.
- **Letter paper, US conventions.** Operator serves Calgary + US clients; A4 isn't the right default. CSS uses `@page size: letter`.
