# AGENTS notes for impossible-states-skill

This file is a reminder of things the AI agent (and future maintainers) tends to get wrong or forget when maintaining this impossible-states agent skill.

It is **not** user-facing documentation. It exists to reduce repeated mistakes.

## 1. Skill structure

- **SKILL.md** — Main entry point with frontmatter (`name`, `description`). Keep it concise. It should tell the agent which references to read (must-read vs load-when-needed).
- **references/** — One file per topic. Agent loads these on demand based on the task. Keep each reference focused and concise (~100–250 lines).
- **examples.md** — Do/don’t pairs. Keep examples short (few lines each).

## 2. Description trigger terms

The `description` field in SKILL.md frontmatter is critical for skill discovery. Include:

- Concept: "impossible states", "domain modeling", "invariants"
- Languages: "TypeScript", "Gleam", "Elm", "F#"
- File hints: ".ts", ".tsx", ".gleam"
- Topics: "state machines", "discriminated unions", "opaque types", "branded types", "smart constructors", "valid transitions"

## 3. References organization

- **Must-read**: overview.md, impossible-data-structures.md
- **Load when needed**: hidden-failure-state.md, constrained-values.md, private-data.md, valid-transitions.md, typescript-techniques.md, gleam-techniques.md

Do not duplicate content between references. Each reference should be self-contained but can link to external docs (e.g. Feldman talk, Gleam Tour, TypeScript handbook).

## 4. Content style

- Prefer bullet rules and small code snippets over long narrative.
- Keep examples minimal (5–15 lines). Use ❌/✅ or Don’t/Do where helpful.
- Remove blog-style frontmatter and table-of-contents from skill content.

## 5. Examples

- **examples.md** should show do/don’t pairs for the main patterns (discriminated unions, optional fields, hidden failure, non-empty, transitions).
- Cover both TypeScript and Gleam where it adds value; keep each pair short.

This file should grow over time as more recurring pitfalls are discovered.
