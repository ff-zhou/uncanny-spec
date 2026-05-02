# uncanny-spec

Turn a vague feature idea into a complete development blueprint.

## What it does

`/uncanny-spec` walks you through a structured pipeline:

- **Phase 0** — Reads your existing database/SQL files and confirms understanding before anything else
- **Phase 1** — Asks one question at a time, with recommended answers, to nail down requirements
- **Phase 2** — Generates a full specification: feature overview, detailed requirements, implementation recommendations (evaluated on architecture fit, intranet feasibility, and complexity), change checklist (code + DDL + config + deps), test strategy, and API docs
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

### Claude Code
```bash
cp SKILL.md ~/.claude/skills/uncanny-spec/SKILL.md
```

### Codex
```bash
mkdir -p ~/.codex/skills/uncanny-spec
cp SKILL.md ~/.codex/skills/uncanny-spec/SKILL.md
```

### Other agents
Copy `SKILL.md` into your agent's skills directory. No dependencies.

## Credits

Inspired by [obra/superpowers](https://github.com/obra/superpowers) and [mattpocock/skills](https://github.com/mattpocock/skills).

## License

MIT
