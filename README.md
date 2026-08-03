# claude-skills

Skills for [Claude Code](https://claude.com/claude-code).

## Skills

| Skill | What it does |
|---|---|
| [`monochrome-report`](skills/monochrome-report/) | Builds standalone, strictly monochrome (white/black) HTML reports and publishes them as Artifacts. Fires on "make me a report", "read-out", "write-up". |

### monochrome-report

A house style for analysis documents. Reports come out as a single self-contained HTML file — no
markdown, no terminal dump — with:

- A six-token greyscale palette, light and dark, honouring both `prefers-color-scheme` and the
  viewer's explicit theme toggle.
- **No hue anywhere.** Series and severity are encoded with *form* — solid vs hatched fill, stripe
  weight, filled vs open markers — always paired with a text label, never colour alone.
- Serif for prose, sans for headings, mono + `tabular-nums` for every single figure.
- Single column at ~74ch, charts drawn true-to-scale. A tiny bar stays tiny; if a number is small,
  that smallness is the finding.
- Prose that leads with the verdict, defines every cohort in words, and says what a number *means*.

`skills/monochrome-report/reference/template.html` is a working skeleton with all of the above
already wired up.

## Install

**Personal** — available in every project:

```bash
git clone https://github.com/prateekcode/claude-skills.git /tmp/claude-skills
cp -R /tmp/claude-skills/skills/monochrome-report ~/.claude/skills/
```

**Per project** — copy into the repo's `.claude/skills/` instead, and commit it so your team gets it:

```bash
cp -R /tmp/claude-skills/skills/monochrome-report /path/to/project/.claude/skills/
```

Restart Claude Code afterwards — skills are scanned at session start.

## Use

Skills fire automatically off their `description`. Ask for a report in the ordinary way:

> make me a report on last month's signups

or invoke it explicitly with `/monochrome-report`.

## Licence

MIT
