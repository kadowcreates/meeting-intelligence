> **Moved.** This skill now lives in [kadowcreates/kadow-mode](https://github.com/kadowcreates/kadow-mode/tree/main/skills/meeting-intelligence), with the other kadow-mode skills. This repo is archived.

# Meeting Intelligence

Skill that turns raw meeting transcripts into structured, actionable summaries for engineering and operations teams.

Built for follow-up. Decisions, owners, due dates, parking lot, risks. Learning summaries belong in Content Intelligence.

- **Version:** 1.0.0
- **License:** MIT
- **Repo:** https://github.com/kadowcreates/meeting-intelligence
- **Skill file:** `SKILL.md`
- **Compatible with:** Grok custom skills, Claude Code, OpenCode

## What it does

1. Reads a transcript with speaker labels and timestamps, plus optional metadata.
2. Maps speakers when the source supports it. Marks low-confidence attributions instead of guessing.
3. Extracts explicit decisions and assigned actions. Under-extracts implied items.
4. Separates parking-lot items that were explicitly deferred.
5. Writes a fixed Markdown summary, plus a Word doc unless you ask for Markdown only.

It does not invent owners, dates, or decisions. Sensitive meetings get a confidentiality banner.

## Emphasis by meeting type

| Type | What gets more weight |
|---|---|
| Design review / technical deep-dive | Assumptions, rationale, specs, risks |
| Root cause / failure investigation | Traceability, blockers, open questions |
| Status sync | Decisions and clear action items |
| Supplier / cross-functional | Dependencies, ownership, parking lot |

The heading structure stays fixed.

Ask for `JSON mode` or `structured data` when you want a machine-readable block after the Markdown.

## Install

### Grok

```text
~/.grok/skills/meeting-intelligence/SKILL.md
```

### Claude Code / OpenCode

```text
.claude/skills/meeting-intelligence/SKILL.md
```

Keep the folder name `meeting-intelligence` so it matches the frontmatter `name`.

### Update from this repo

```bash
git clone https://github.com/kadowcreates/meeting-intelligence.git
cp meeting-intelligence/SKILL.md ~/.grok/skills/meeting-intelligence/SKILL.md
```

## Usage

```text
Summarize this with meeting-intelligence:

[paste transcript]
```

## File map

```text
meeting-intelligence/
  README.md
  LICENSE
  SKILL.md
```

## Version history

| Version | Date | Changes |
|---|---|---|
| **v1.0.0** | **2026-10-02** | Initial publish. Conservative extraction, fixed ops summary contract, JSON export, Word doc generation. |

## License

MIT. See `LICENSE`.
