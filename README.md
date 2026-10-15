# Kedencloud frontend standards

A reusable Codex skill for React and TypeScript engineering: shared UI and design tokens, accessible screens, contract-first API generation, automated testing, and delivery gates.

The default stack is React, strict TypeScript, Vite, Tailwind CSS v4, shadcn/ui, TanStack Query/Table, React Hook Form with Zod, React Router, and Lucide React. Testing uses Vitest, React Testing Library, MSW, and Playwright. Orval generates API models, request functions, and Query hooks from pinned ASP.NET Core OpenAPI contracts.

These are Kedencloud conventions, not a formal certification or a claim that every project must use this stack. Existing applications retain their compatible setup; migrations require their own scope.

## Install

After downloading or cloning this repository, run from its root:

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/kedencloud-frontend "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Inspect an existing installation before replacing it so local customizations are preserved. Start a new Codex session after installation. Invoke explicitly with `$kedencloud-frontend`, or allow normal automatic discovery for relevant frontend work.

For a repository-local installation instead, copy the skill into `.agents/skills/kedencloud-frontend/` in that application. Choose one installation scope to avoid duplicate skill copies drifting apart.

## Contents

- `skills/kedencloud-frontend/SKILL.md`: discovery, core constraints, workflow, and reference routing.
- `references/ui.md`: shared components, forms, tables, statuses, layout, accessibility, and performance.
- `references/api-generation.md`: Orval, transport behavior, tenant-aware cache boundaries, and reproducible generation.
- `references/testing.md`: frontend unit/component tests, E2E, visual checks, and .NET boundary requirements.
- `references/delivery.md`: CI gates and honest reporting of check outcomes.
- `templates/frontend-AGENTS.md`: optional frontend instructions to merge into a consuming repository's applicable `AGENTS.md`.

Reference paths above are relative to the skill directory unless prefixed otherwise. Official documentation links are maintained in the skill and its references. Consult documentation compatible with the consuming project's installed versions.

## Enforcement

The skill guides Codex. It does not install frontend dependencies or create CI/branch rules automatically. Each application must configure its actual lint/typecheck/build commands, tests, contract generation, and required status checks. The .NET testing section applies when backend work is in scope and complements the existing Kedencloud backend conventions.
