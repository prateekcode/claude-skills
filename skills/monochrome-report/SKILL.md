---
name: monochrome-report
description: Build a standalone, strictly monochrome (white/black) HTML report and publish it as an Artifact. Use whenever the user asks for a "report", "read-out", "write-up", "analysis doc", or wants findings presented as a document rather than terminal output or a markdown file.
---

# Monochrome report

A report is a **standalone HTML file published via the Artifact tool**. Never a markdown file, never a
terminal dump — unless the user explicitly says otherwise.

The palette is **strict monochrome, white and black**. This is deliberate and independent of whatever
theme the surrounding product uses. Only deviate when the user explicitly names other colours.

## Tokens

| Token | Meaning | Light | Dark |
|---|---|---|---|
| `--paper` | background | `#FFFFFF` | `#0C0C0C` |
| `--ink` | primary text | `#0B0B0B` | `#F0F0F0` |
| `--ink-2` | secondary text | `#4A4A4A` | `#B0B0B0` |
| `--muted` | labels, captions | `#8E8E8E` | `#7A7A7A` |
| `--rule` | borders, dividers | `#E3E3E3` | `#2A2A2A` |
| `--track` | bar / chart tracks | `#F2F2F2` | `#1C1C1C` |

Ship **both themes**: define tokens on `:root`, redefine them under `@media (prefers-color-scheme: dark)`
**and** under `:root[data-theme="dark"]` / `:root[data-theme="light"]`. The explicit `data-theme` rules must
win over the media query in both directions — the Artifact viewer's theme toggle stamps `data-theme` on the
root element.

`reference/template.html` is a working skeleton with the tokens, typography, and a true-to-scale bar
chart already wired up. Start from it.

## No hue carries meaning

There is no colour channel to encode with, so encode with **form**:

- solid vs hatched fill (SVG `<pattern>` of diagonal lines)
- stripe weight / dash pattern
- filled vs open markers
- rule weight for emphasis

**Always pair the form with a text label.** Never require the reader to decode a shape from a legend alone.

## Typography

| Role | Stack |
|---|---|
| Explanatory prose | `Iowan Old Style, "Palatino Linotype", Palatino, Georgia, serif` |
| Headings, labels | `ui-sans-serif, -apple-system, "Helvetica Neue", Arial, sans-serif` |
| **Every figure** | `ui-monospace, "SF Mono", Menlo, monospace` with `font-variant-numeric: tabular-nums` |

Every number in the document — in prose, in tables, in chart labels — is mono and tabular.

## Layout

- Single column, ~74ch measure.
- Charts drawn **true-to-scale**. Never rescale a tiny bar to make it readable. If a number is small,
  that smallness is the finding — let the reader see it.
- Wide content (tables, wide charts) scrolls inside its own `overflow-x: auto` container. The page body
  must never scroll horizontally.
- Relative units, `max-width: 100%` on any embedded media.

## Writing

- **Lead with the verdict.** The first paragraph says what the answer is, not what you did.
- Define every cohort **in words** where it first appears. "Users who scanned but never opened chat"
  beats "Cohort B".
- Say what a number *means*, don't assume the reader will infer it. `4.2%` → `4.2% — roughly 1 in 24`.
- Plain language. No jargon the data doesn't force you into.
- State the caveat next to the number it undermines, not in a footnote at the bottom.

## Publishing

Write the HTML to a file, then call the Artifact tool with that path:

- Set a stable `<title>` — it names the artifact in the tab and gallery. Keep it identical across redeploys.
- Pass a one-sentence `description` (the gallery card subtitle) and a `favicon` emoji. Keep the favicon
  stable across redeploys; only change it on a hard topic pivot.
- The file is wrapped in `<!doctype html><head>…</head><body>` at publish time — write page content
  directly, no `<html>`/`<head>/`<body>` tags of your own.
- Fully self-contained: inline all CSS and JS, embed images as `data:` URIs. A strict CSP blocks every
  external host — no CDN scripts, no web fonts, no remote images, no fetch.
- To update a report, edit the same file and call Artifact again with the same path — it redeploys to the
  same URL.

## Checklist before publishing

- [ ] Zero hue anywhere — grep the CSS for anything that isn't a grey, `currentColor`, or `transparent`
- [ ] Both light and dark defined, and `data-theme` overrides beat the media query
- [ ] Every figure is mono + `tabular-nums`
- [ ] Every series/severity distinction has a text label, not just a form
- [ ] Charts true-to-scale, no axis truncation that flatters a bar
- [ ] Nothing loads from an external host
- [ ] First paragraph states the verdict
