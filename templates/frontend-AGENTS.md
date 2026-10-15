# Frontend engineering standards

Merge these instructions into the frontend's applicable AGENTS.md; preserve existing project-specific instructions.

- Apply the installed `kedencloud-frontend` skill for React/TypeScript implementation, tests, refactoring, and reviews. Keep it available in the repository or development environment; this reference alone does not install it.
- Inspect existing shared UI, tokens, routes, OpenAPI contracts, generated clients, package versions, and tests before implementation.
- Use strict TypeScript and the established React application stack. Reuse shared DataTable, StatusBadge, form fields, and layout components. Preserve accessibility and responsive behavior.
- Generate TypeScript API models, request functions, and TanStack Query hooks with Orval from pinned backend OpenAPI contracts. Prefer Fetch with centralized authentication/error transport. Never handwrite clients/DTOs for covered operations, invent endpoints, or edit generated files.
- Keep generated output in per-contract generated/ directories. Regenerate on contract changes and reject tracked or untracked generation drift in CI. Type check after regeneration and review breaking API changes.
- Use Vitest, React Testing Library, MSW, and Playwright at the appropriate levels. Cover relevant loading, error, success, and empty states, critical journeys, shared-component visual regressions, and applicable permission/tenant boundaries. Run smoke tests on pull requests.
- Never weaken tests, compiler settings, or CI gates to make a task pass. Run relevant tests, lint, type checking, build, and generation checks. Report failed and unexecuted checks; required failures leave delivery incomplete.
- For backend changes in scope, use xUnit, WebApplicationFactory, and Testcontainers with the production database engine; keep unit and integration projects separate and follow the applicable backend standards.

The full skill contains task-specific official documentation links. Configure actual application CI and required branch checks to enforce these rules.
