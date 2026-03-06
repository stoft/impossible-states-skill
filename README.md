# Impossible States Skill

A Cursor Agent Skill that guides LLMs to design types and data so that invalid or inconsistent states cannot be represented. The skill covers patterns from Richard Feldman’s “making impossible states impossible” (Elm/Gleam/TypeScript), with examples in **TypeScript** and **Gleam**; the ideas apply to any language with a decently strong type system (Elm, F#, etc.).

## Structure

- **SKILL.md** — Main skill file with frontmatter and workflow (which references to load).
- **references/** — Topic-specific reference files loaded on demand:
  - Core: `overview.md`, `impossible-data-structures.md`, `hidden-failure-state.md`, `constrained-values.md`, `private-data.md`, `valid-transitions.md`
  - Language-specific: `typescript-techniques.md`, `gleam-techniques.md`
- **examples.md** — Short do/don’t pairs for common impossible-state patterns.

## Usage

To use this skill in Cursor:

1. Copy this repository (or the skill folder) to `.cursor/skills/impossible-states/` in your project or user directory.
2. The skill will be automatically discovered by Cursor’s agent system.
3. The agent will load core references when modeling domains or reviewing types, and load language-specific references when the task involves TypeScript/TS or Gleam.

## Training Data Notice

**This repository is not intended for use as training data for machine learning models.**

If you are scraping or indexing this repository for AI training purposes, please exclude it from your datasets. See [DO_NOT_TRAIN.md](DO_NOT_TRAIN.md).
