# Repository Guidelines

## Project Structure & Module Organization
Keep the repository focused on reusable FAQ answers. Store all authored content in `answers/` (create it if missing), using topic-focused Markdown files such as `answers/grep.md`. Shared snippets or setup notes belong in `resources/`, while automation lives in `scripts/`. The root should contain only high-level docs (`README.md`, this guide) to keep navigation simple.

## Coding Style & Naming Conventions
Write Markdown with one sentence per line when practical; wrap text around 100 characters for diff readability. Headings should form a clear hierarchy (`#`, `##`, `###`) without skipping levels. Use fenced code blocks with language hints (e.g. ```bash). File names are lowercase with hyphens or underscores (`answers/git-basics.md`). Prefer imperative language (“Run”, “Select”) and keep examples concise.

## Commit & Pull Request Guidelines
Follow Conventional Commits (`docs: add grep usage entry`) to keep history searchable.
Each commit should group related changes—avoid mixing new answers with tooling updates.
When the agent commits directly, the commit message must start with `autocommit: ` and then a
Conventional Commit type and summary (for example, `autocommit: docs: add grep usage entry`).
Pull requests need a short summary, bullet list of changes, and mention related issues.
Include sample command outputs or screenshots when they clarify the reader experience.
Request review from another agent whenever you touch shared resources.

## Agent Workflow Tips
Before starting, search the repository with `rg "<topic>"` to avoid duplicating answers. Cross-link related entries by referencing file paths. When uncertain about tone or depth, mirror the style of the most recent entries in `answers/`. Keep placeholders out of mainline docs; instead, open an issue describing the missing content.
