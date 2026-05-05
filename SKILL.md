---
name: uncanny-spec
description: >
  Generate a complete feature development specification from requirements —
  including change checklist, DDL, test strategy, and API docs for Java/Python
  projects. Intranet-aware: evaluates dependencies and deployment feasibility.
  Use when the user asks to design a new feature, write a feature spec, plan
  implementation before coding, or when a feature request is too vague to start
  building.
---

# Feature Spec

You receive a feature idea and produce a complete development specification.
**Phases: understand affected boundaries → collect requirements → design with targeted code/data reads → self-review → present → confirm → write file.**

**Design philosophy:** Design based on real code and real constraints, not assumptions. Intentionally slow down — ask one question at a time, verify every fact, never skip a phase. A good spec prevents more bugs than a good test.

## Quick Start

Minimal invocation — the user says: "I need a feature: user points system. Users earn points for completing tasks, and can view their point balance."

Your job: start Phase 0 (identify affected boundaries and whether persisted data is involved), then Phase 1 (ask one question at a time), then Phase 2 (read the specific code/data artifacts needed for design and generate the spec in conversation), self-review, get approval, and write the file. That's it.

## When NOT to Use This Skill

Do not invoke `/uncanny-spec` for:

| Situation | Use instead |
|-----------|-------------|
| Bug fix (no new functionality) | Direct fix + test |
| One-line config or env var change | Direct change |
| Documentation-only update | Direct edit |
| Pure frontend UI tweak with zero backend/DB impact | Direct implementation |
| The user explicitly says "no spec needed, just do it" | Respect their call — but warn once if it touches DB or API |

## Core Rules

**Understand first.** Never design anything you don't understand. Ask — do not guess — about any unclear term, table, field, rule, or constraint.

**Git discipline.** Never commit proactively. Suggest saving only when a complete feature or independent sub-feature is done. Always wait for explicit user authorization.

**Doc stays alive.** During implementation, if requirements, steps, interfaces, or DDL change, update the spec file immediately. Tell the user what changed. Do not batch updates.

**No fabrication.** Table schemas, field meanings, business rules, external APIs, deployment constraints — ask the user for every one. Never invent.

**One question at a time.** When gathering requirements, ask one question per turn. Prefer multiple-choice when options are clear. This keeps the conversation focused and prevents overwhelming the user.

## Anti-Patterns (Never Do These)

| Excuse | Reality |
|--------|---------|
| "This is too simple to need a spec" | Simple features cause the worst regressions — a spec prevents scope creep |
| "The user already explained everything" | Their explanation may miss edge cases, error handling, and DB impact |
| "Let me read every DB file first, then ask requirements" | Read only what is needed for the current phase; broad DB reading belongs in design only when persistence is involved |
| "It's just a small change, verbal confirmation is enough" | No record = no accountability. Three months later, nobody remembers the decision |

---

## Phase 0: Understand Affected Boundaries

**HARD GATE: Do not proceed to Phase 1 until the affected system boundaries are identified and the user confirms whether persisted data, queries, transactions, reporting, or data compatibility are involved.**

### Step 1 — Identify boundaries

Ask: "Which parts of the system does this feature touch? API/UI, service/module, persistence, external dependencies, config, deployment, or permissions?"

If the user is unsure, offer a recommended boundary guess from the feature idea and ask them to confirm.

### Step 2 — Check scope size

If the feature crosses multiple independent subsystems, stop and decompose before collecting detailed requirements. Do not force a platform-sized request into one spec.

Ask the user to choose the first sub-feature to specify. Each sub-feature should be independently understandable, implementable, and verifiable.

### Step 3 — Check data impact

Ask whether the feature changes or depends on persisted data:
- New or modified tables/columns/indexes
- New queries or changed query conditions
- Status transitions, transactions, or consistency requirements
- Reporting/statistics definitions
- Historical data migration or compatibility

Only request database DDL, migrations, Entity/Model classes, Mapper/Repository files, or manual schema descriptions if one of these applies.

### Step 4 — Confirm scope

Summarize: affected boundaries, data impact yes/no/unknown, and the artifacts that may need to be read later. Ask: "Is this scope correct? Anything missing?"

---

## Phase 1: Collect Requirements

Ask in order, one question at a time. Skip any the user has already answered. **For each question, provide your recommended answer** based on what you've already learned — this turns interrogation into collaboration.

1. Feature name and one-sentence summary
2. Who uses it, when, and what problem it solves
3. Core user flow (1–3 key paths)
4. Technical context — which service/module? Which interfaces, data stores, configs, or external dependencies?
5. Constraints — perf (QPS/latency)? Compatibility? Auth/ACL?
6. Deployment environment — intranet (no internet)? OS? Containerized or bare metal?
7. Edge cases and failure modes — what happens when things go wrong?

If the user's answer is vague, drill down — but offer your best guess first. For example: "You said 'user management' — I'm guessing admin CRUD (create/edit/delete users, assign roles). Or did you mean self-service registration and profile editing?"

### Domain language

If the user uses ambiguous terms, synonyms, or overloaded business language, clarify the canonical terms before design. Examples: "user" vs "account" vs "customer", "order" vs "request", "status" vs "state".

When needed, include a short **Domain Terms** section in the spec:
- **Canonical term** — one-sentence definition
- **Avoid saying** — synonyms that should not appear in code, API fields, or docs
- **Relationship** — how this term relates to nearby terms

---

## Phase 2: Design and Generate the Specification

Before generating the spec, read the smallest set of artifacts needed to make concrete design decisions:
- Similar existing implementations for API shape, layering, errors, auth, logging, transactions, and tests
- DDL/migrations/Entity/Model/Mapper/Repository files only when persistence or query behavior is in scope
- Config/deployment files only when runtime behavior, dependencies, or intranet feasibility are in scope
- External API docs or wrappers only when integration behavior is in scope

Generate in conversation. If the spec exceeds ~200 lines, split into two batches:
- **Batch 1**: Feature overview + requirements + implementation recommendations → get direction confirmed
- **Batch 2**: Change checklist + test strategy + API docs → confirm and write file

### 2.1 Feature Overview

1–2 paragraphs: what it is, what problem it solves, business value, one-sentence core flow.

### 2.2 Detailed Requirements

For each function point:
- **Name**, **Description**, **Preconditions**, **Main flow** (numbered steps), **Error flow**, **Acceptance criteria** (`- [ ]` checkboxes)

### 2.3 Implementation Recommendations

For each key technical decision, compare options on three dimensions:

| Dimension | What to evaluate |
|-----------|-----------------|
| Architecture consistency | Does it match the existing codebase style? Reuse existing components? |
| Intranet deployment | External Maven/PyPI deps? External API calls? New infrastructure? Prefer zero-dependency options. |
| Implementation complexity | Code volume, change scope, test difficulty, team familiarity |

Format each decision as: options with trade-offs → **recommendation with reasoning**. If only one reasonable option exists, explain why — don't fabricate alternatives. See Appendix A for format example.

**YAGNI:** Spec detail should match risk. Do not invent architecture options, dependencies, files, DDL, APIs, or tests just to fill the template. If a section is not applicable, say why briefly and omit the unnecessary detail.

**Vertical slicing guidance:** When the spec is done, suggest tracer-bullet slices. Each slice should deliver a narrow but complete, end-to-end behavior through the relevant layers (for example API → Service → DB → tests), and be demoable or verifiable on its own. Prefer several thin slices over one large slice.

Mark each slice:
- **AFK** — the agent can implement it without another product or architecture decision
- **HITL** — human confirmation is required before or during the slice, such as field semantics, permission policy, migration risk, or UX choice

### 2.4 Change Checklist

For each item, mark type (New / Modify / Delete) and assess impact.

**For Java:** Controller → Service → Repository → Entity → DTO
**For Python:** Routes → Services → Models → Schemas

Include: code files, DDL when persistence changes are required (full CREATE/ALTER statements — must match naming and types from the targeted schema artifacts read during design), config changes, and dependency changes (flag intranet availability for each new dependency). See Appendix B for format.

### 2.5 Test Strategy

- **Unit tests** — grouped by test class, each with method name and what it verifies
- **Integration tests** — key cross-component scenarios (Service→DB, Controller→Service→DB, transaction rollback, concurrency)
- **E2E / manual tests** — full user flows, error states, auth boundaries, data edge cases

### 2.6 API Documentation

- **Endpoint table** — Method, Path, Description, Auth
- **Per-endpoint details** — Request body (field, type, required, constraints), success response, error responses
- **Error code table** — Code, HTTP status, description

See Appendix C for format.

---

## Self-Review (Before Presenting to User)

Complete this checklist before showing the spec to the user:

- [ ] Every function point has concrete acceptance criteria (no vague "works correctly")
- [ ] DDL column types and naming match the targeted schema artifacts read during design, or the spec explicitly states that no persistence change is required
- [ ] Every new dependency is flagged with intranet availability status
- [ ] No TODO, TBD, "implement later", or placeholder text
- [ ] Error code table covers every error response in the API docs
- [ ] Test cases cover both happy path and at least one error path per endpoint
- [ ] Implementation recommendations cite specific architectural constraints, not generic advice
- [ ] Ambiguous domain terms are resolved into canonical terms, or the spec explicitly says no domain ambiguity was found
- [ ] The scope is small enough for one implementation plan, or the spec decomposes it into sub-features
- [ ] No invented artifacts exist just to satisfy the template
- [ ] Suggested slices are end-to-end, verifiable, and marked AFK or HITL

---

## Phase 3: Confirm and Write File

1. After presenting the full spec: "Any adjustments needed? Once confirmed, I'll write to `docs/features/YYYY-MM-DD-{feature-name}.md`."
2. Revise based on feedback until approved.
3. **HARD GATE: Do not write the file until the user explicitly approves the spec.**
4. Write to `docs/features/YYYY-MM-DD-{feature-name}.md` using today's date (e.g., `docs/features/YYYY-MM-DD-user-points-system.md`). Prefer project root; fall back to current working directory.
5. Present the file to the user.

After the spec is written, suggest next steps: "Implementation can start. Consider vertical slices: [slice suggestions]. If you want me to implement, I'll follow TDD per slice — just say go."

---

## Tech Stack Notes

- **Java**: If confirmed, usually organize Spring Boot/MyBatis/JPA projects by Controller → Service → Repository → Entity → DTO.
- **Python**: If confirmed, usually organize FastAPI/Flask/Django projects by Routes → Services → Models → Schemas.
- **Uncertain?** Ask the user for framework, ORM, and build tool details. Never assume.

---

## Appendix A: Recommendation Format Example

```
**Decision: Caching strategy**

- Option A: Redis — best performance, but requires Redis deployment (self-host on intranet)
- Option B: Caffeine (local cache) — zero deps, JVM-native, but inconsistent across instances
- Option C: DB cache table — reuses existing MySQL, zero new deps, but slower

**Recommend B (Caffeine).** Intranet Redis deployment is high overhead. This feature
tolerates ≤30s cache inconsistency. Caffeine is zero-dependency, trivial to integrate,
and consistent with the project's Spring Boot architecture. Migrate to Redis later if distributed cache becomes necessary.
```

## Appendix B: Change Checklist Format

### Code files (Java)
```
- [New] src/main/java/com/x/controller/XController.java
  - Endpoints: POST /api/v1/x, GET /api/v1/x/{id}
  - Depends on: XService
- [Modify] src/main/java/com/x/service/XService.java
  - New methods: doX(), validateX()
  - Risk: potential circular dependency with YService — suggest extracting XValidator
- [New] src/main/java/com/x/model/dto/XRequest.java
- [New] src/main/java/com/x/model/vo/XResponse.java
- [New] src/main/java/com/x/repository/XRepository.java
  - New query: findByStatusAndTimeRange()
```

### Code files (Python)
```
- [New] app/api/v1/endpoints/x.py
  - Routes: POST /api/v1/x, GET /api/v1/x/{id}
- [Modify] app/services/x_service.py
  - New: create_x(), validate_x()
- [New] app/schemas/x.py
  - XCreateRequest, XResponse
- [New] app/models/x.py
  - XModel (SQLAlchemy / Django Model)
```

### DDL
```sql
-- [New] x_records
CREATE TABLE x_records (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT NOT NULL,
    type VARCHAR(32) NOT NULL,
    amount DECIMAL(10,2) DEFAULT 0,
    status VARCHAR(16) NOT NULL DEFAULT 'PENDING',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_user_status (user_id, status),
    INDEX idx_created_at (created_at)
);

-- [Modify] users — add column
ALTER TABLE users ADD COLUMN x_score INT DEFAULT 0;
```

### Config
```
- [Modify] application.yml (or .env / settings.py)
  + x.expire-days: 30
  + x.max-retry: 3
```

### Dependencies
```
- [New] Maven: spring-boot-starter-validation (intranet Nexus: available, v3.2.0)
- [New] pip: celery>=5.3 — ⚠️ Verify intranet PyPI mirror availability
- [New] pip: redis>=4.5 — ⚠️ May not be needed; local cache recommended instead
```

## Appendix C: API Doc Format

### Endpoint table
| Method | Path | Description | Auth |
|--------|------|-------------|------|
| POST | /api/v1/x | Create X | Bearer |
| GET | /api/v1/x/{id} | Get X detail | Bearer |
| GET | /api/v1/x | List X (paginated) | Bearer |
| PUT | /api/v1/x/{id} | Update X | Bearer |
| DELETE | /api/v1/x/{id} | Delete X | Bearer |

### Endpoint detail (POST /api/v1/x)
```json
// Request
{
  "name": "string, required, 1-64 chars",
  "type": "string, required, enum: TYPE_A | TYPE_B",
  "amount": "decimal, optional, default 0, range [0, 999999.99]",
  "remark": "string, optional, max 256 chars"
}

// Response 201
{ "code": 0, "message": "success", "data": { "id": 12345, "name": "...", "type": "TYPE_A", "amount": "100.00", "status": "PENDING", "createdAt": "2026-05-02T10:30:00Z" } }

// Response 400
{ "code": 40001, "message": "name is required", "data": null }

// Response 409
{ "code": 40901, "message": "X name already exists", "data": null }
```

### Error codes
| Code | HTTP | Description |
|------|------|-------------|
| 40001 | 400 | Validation failed |
| 40100 | 401 | Not authenticated |
| 40300 | 403 | Forbidden |
| 40400 | 404 | X not found |
| 40901 | 409 | X name conflict |
| 50000 | 500 | Internal server error |
