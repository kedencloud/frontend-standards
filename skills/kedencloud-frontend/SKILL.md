---
name: kedencloud-frontend
description: Implement, test, refactor, or review React and TypeScript frontends using Kedencloud standards for shared UI, accessibility, Orval-generated API clients, and automated testing. Use for frontend engineering work; not for unrelated TypeScript libraries or backend-only changes.
---

# Kedencloud frontend engineering

Apply these standards to the requested feature or review. They are Kedencloud's chosen conventions, informed by official guidance, rather than a universal industry mandate. Explicit user instructions take precedence. Preserve repository-specific instructions and explain material conflicts; do not silently migrate an existing application or rewrite unrelated code.

## Discover before implementing

Inspect repository instructions, package manifests, lockfile, runtime versions, workspace boundaries, scripts, TypeScript/ESLint configuration, routes, tokens, shared components, tests, OpenAPI contracts, and generated clients. Find the feature owner and existing patterns before adding files or dependencies. Consult official documentation matching installed versions for version-sensitive APIs. Never invent package versions, endpoint URLs, DTOs, or generated hook names.

For a new React browser application, use TypeScript strict mode, Vite, Tailwind CSS v4, shadcn/ui, TanStack Query for server state, TanStack Table behind shared tables, React Hook Form with Zod for substantial forms, React Router, and Lucide React. Reuse compatible existing choices; propose migrations separately. A framework requirement such as SSR can justify a different build setup. Pin tooling through the repository's manifest/lockfile and use its package manager.

## Implementation rules

- Organize by feature ownership. Keep app composition, reusable UI, feature logic, generated API code, and transport infrastructure distinct. Keep shared code independent of feature modules; avoid circular dependencies and speculative abstraction.
- Use strict TypeScript and narrow `unknown` at untrusted boundaries. Avoid `any`, unsafe assertions, non-null assertions used to hide missing states, and `@ts-ignore` used to bypass failures. Prefer discriminated unions for meaningful state variants. Do not weaken compiler/lint rules to complete a task.
- Render from props/state; keep rendering pure and obey React's Hooks rules. Use effects to synchronize external systems, not to copy derived state. Keep transient UI state local, shareable navigation state in the URL, and remote data in TanStack Query. Do not duplicate remote entities in another store without a concrete requirement.
- Reuse shared UI and tokens before creating components. Standard listings use shared `DataTable`; statuses use shared `StatusBadge`. Create a missing shared component in the established UI location when it serves shared behavior. Read [UI standards](references/ui.md) for screens, forms, tables, or component changes.
- Use generated clients for all operations covered by an OpenAPI contract. Never handwrite duplicate endpoint clients or DTOs, or edit generated files. Read [API generation](references/api-generation.md) before integrating APIs, changing transport, or regenerating clients.
- Write tests for changed behavior at the appropriate level. Read [testing standards](references/testing.md) for test changes and feature delivery. Never remove, skip, or weaken tests merely to pass CI.
- Keep browser secrets out of source, builds, logs, URLs, and client environment variables. UI permission checks improve usability; server authorization enforces access. Use the existing authentication model and tenant boundary. Never trust client-supplied tenant or identity values as authorization.
- Handle loading, error, empty, success, forbidden, and not-found outcomes when applicable. Preserve cancellation and prevent stale responses or repeated submissions from producing incorrect UI. Offer recoverable errors without exposing backend internals.

## Deliver and review

Implement the smallest cohesive change with acceptance examples. Run relevant tests, lint, type checking, production build, applicable visual checks, and contract generation checks using actual repository commands. Read [delivery and CI](references/delivery.md) when configuring CI or completing a change. Report passed, failed, and unexecuted checks separately; required failures mean the task is incomplete. Do not claim a passing test establishes behavior it does not exercise.

For reviews, report actionable defects and standard deviations with file locations and consequences; do not modify code unless requested. For implementation, fix issues within scope and report remaining limitations. This skill supplies guidance; actual enforcement requires repository configuration, CI, and branch rules. It does not authorize publishing, deploying, merging, or changing unrelated settings.

## Official references

- [React](https://react.dev/learn), [TypeScript](https://www.typescriptlang.org/docs/), [Vite](https://vite.dev/guide/)
- [TanStack Query](https://tanstack.com/query/latest/docs/framework/react/overview), [React Router](https://reactrouter.com/), [ESLint](https://eslint.org/docs/latest/)
- Task-specific documentation is linked in each reference below; verify compatibility with the installed versions rather than assuming the latest examples apply unchanged.
