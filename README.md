# garden-check

Read-only audit of your Claude Code setup. Finds what has rotted: dead file references, rules whose `paths:` never match anything, orphaned memory entries, oversized `CLAUDE.md`, and the same instruction written in two places.

It reports. It never edits, never deletes.

## Why

A Claude Code setup decays quietly. You rename a folder, and the `CLAUDE.md` line pointing at it keeps telling Claude to go there. You write a rule, its glob matches nothing, and it never loads once. You never find out, because none of this produces an error.

Run on a real setup, the first pass typically turns up twenty findings. That is normal for a setup that gets used.

## What it checks

| # | Check | Why it matters |
|---|---|---|
| 1 | Weight | `CLAUDE.md` is re-read every session, so every line is rent |
| 2 | Dead paths | An instruction pointing at nothing is worse than none; Claude follows it anyway |
| 3 | Broken front matter | Malformed front matter makes a rule invisible, with no warning |
| 4 | Rules that never fire | A `paths:` pattern matching no file has never loaded, and you cannot tell |
| 5 | Rules that always fire | No `paths:` field means it loads on every session, forever |
| 6 | Orphaned memory | Checked both ways: entries pointing at missing files, and files nothing indexes |
| 7 | Unlinked skill files | A file in a skill folder that `SKILL.md` never links to will never load |
| 8 | Duplicates | The same instruction twice, drifting apart until they contradict |

Findings are grouped **dead / stale / too heavy**, one line each with a concrete fix, ending on the single change worth making first.

## Install

```bash
git clone https://github.com/alexadark/garden-check-skill.git ~/.claude/skills/garden-check
```

Then ask Claude for a garden check.

**Read it before you install it.** The whole skill is one file of plain English, deliberately, so that you can. That also applies to every other skill you find online, including the rest of mine.

## Prerequisites

Claude Code. Nothing else. No package, no dependency, no API key.

## Usage

```
> run a garden check on my setup
> weed the garden
> are my rules still working?
> why does my skill never fire?
```

It asks which setup to look at if that is ambiguous: your global `~/.claude/`, the project you are in, or both.

## Design notes

**Read-only on purpose.** Finding and fixing are separate jobs. A tool that silently fixes what it finds cannot be audited, and you learn nothing about your own setup from a diff you never saw.

**No scripts.** The body is a plain-English checklist rather than shell code, so a non-coder can read every line, disagree with it, and change it. It was built live in a beginners workshop, and that constraint is the point.

## Origin

Built live in [Beginner's Corner](https://github.com/alexadark/skills), the Claude Code sessions for non-coders in the Early AI-dopters community. Session 4: weed the garden.

## Related

- [All my skills](https://github.com/alexadark/skills)
- [skill-creator](https://github.com/alexadark/skill-creator-skill) — writes skills for you
- [review-skill](https://github.com/alexadark/review-skill-skill) — audits a skill's structure and design

## License

MIT

## Author

Alexandra Spalato
