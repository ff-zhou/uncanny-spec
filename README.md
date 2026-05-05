# uncanny-spec

Turn a vague feature idea into a complete development blueprint.

## What it does

`/uncanny-spec` walks you through a structured pipeline:

- **Phase 0** — Identifies affected system boundaries, decomposes oversized scope, and confirms whether persisted data is involved
- **Phase 1** — Asks one question at a time, with recommended answers, to nail down requirements and clarify domain terms
- **Phase 2** — Reads targeted code/data artifacts as needed, then generates a full specification: feature overview, detailed requirements, implementation recommendations (evaluated on architecture fit, intranet feasibility, and complexity), change checklist (code + DDL when needed + config + deps), test strategy, API docs, and end-to-end tracer-bullet slices
- **Self-review** — Checks its own output against 7 criteria before showing it to you
- **Phase 3** — Writes to `docs/features/YYYY-MM-DD-{name}.md` after your approval

## Design philosophy

Design based on real code and real constraints, not assumptions. Intentionally slow down. A good spec prevents more bugs than a good test.

## Quick start

```
/uncanny-spec

"I need a feature: user points system. Users earn points for completing tasks, and can view their point balance."
```

The skill handles the rest.

## Tech stack

Built for Java (Spring Boot) and Python (FastAPI/Flask/Django) projects. Intranet-aware — evaluates dependencies against offline deployment constraints.

## Install

Download the zip for your agent from the latest release, unzip it into the
agent's skills directory, then start a new session.

Each release zip contains:

```text
uncanny-spec/
  README.md
  SKILL.md
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
curl -L -o uncanny-spec-claudecode.zip \
  https://github.com/ff-zhou/uncanny-spec/releases/latest/download/uncanny-spec-claudecode.zip
unzip -o uncanny-spec-claudecode.zip -d ~/.claude/skills
```

Manual install from a clone:

```bash
mkdir -p ~/.claude/skills/uncanny-spec
cp SKILL.md README.md ~/.claude/skills/uncanny-spec/
```

### Codex

```bash
mkdir -p ~/.codex/skills
curl -L -o uncanny-spec-codex.zip \
  https://github.com/ff-zhou/uncanny-spec/releases/latest/download/uncanny-spec-codex.zip
unzip -o uncanny-spec-codex.zip -d ~/.codex/skills
```

Manual install from a clone:

```bash
mkdir -p ~/.codex/skills/uncanny-spec
cp SKILL.md README.md ~/.codex/skills/uncanny-spec/
```

### OpenClaw

```bash
mkdir -p ~/.openclaw/skills
curl -L -o uncanny-spec-openclaw.zip \
  https://github.com/ff-zhou/uncanny-spec/releases/latest/download/uncanny-spec-openclaw.zip
unzip -o uncanny-spec-openclaw.zip -d ~/.openclaw/skills
```

Manual install from a clone:

```bash
mkdir -p ~/.openclaw/skills/uncanny-spec
cp SKILL.md README.md ~/.openclaw/skills/uncanny-spec/
```

### Hermes

```bash
mkdir -p ~/.hermes/skills
curl -L -o uncanny-spec-hermes.zip \
  https://github.com/ff-zhou/uncanny-spec/releases/latest/download/uncanny-spec-hermes.zip
unzip -o uncanny-spec-hermes.zip -d ~/.hermes/skills
```

Manual install from a clone:

```bash
mkdir -p ~/.hermes/skills/uncanny-spec
cp SKILL.md README.md ~/.hermes/skills/uncanny-spec/
```

## Credits

Inspired by [obra/superpowers](https://github.com/obra/superpowers) and [mattpocock/skills](https://github.com/mattpocock/skills).

## License

MIT
