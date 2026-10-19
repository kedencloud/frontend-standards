---
name: kedencloud-frontend
description: Implement, test, refactor, or review React and TypeScript frontends using Kedencloud standards for shared UI, accessibility, Orval-generated API clients, and automated testing. Use for frontend engineering work; not for unrelated TypeScript libraries or backend-only changes.
---

# Kedencloud frontend engineering

Apply these standards to the requested feature or review. They are Kedencloud's chosen conventions, informed by official guidance, rather than a universal industry mandate. Explicit user instructions take precedence. Preserve repository-specific instructions and explain material conflicts; do not silently migrate an existing application or rewrite unrelated code.

## Discover before implementing

Inspect repository instructions, package manifests, lockfile, runtime versions, workspace boundaries, scripts, TypeScript/ESLint configuration, routes, tokens, shared components, tests, OpenAPI contracts, and generated clients. Find the feature owner and existing patterns before adding files or dependencies. Consult official documentation matching installed versions for version-sensitive APIs. Never invent package versions, endpoint URLs, DTOs, or generated hook names.

For a new React browser application, use TypeScript strict mode, Vite, Tailwind CSS v4, the supplied/shared component system (Catalyst in Endorz; shadcn/ui when no system is established), TanStack Query for server state, TanStack Table when shared tables need its behavior, React Hook Form with Zod for substantial forms, React Router, and Lucide React. Reuse compatible existing choices; propose migrations separately. A framework requirement such as SSR can justify a different build setup. Pin tooling through the repository's manifest/lockfile and use its package manager.

## Implementation rules

- Use `src/app` for composition/providers, `src/features/<feature>` for feature screens and logic, `src/layouts` for shell/navigation, `src/pages` for standalone pages, `src/components` for reusable UI, and `src/api` for generated clients and transport configuration. Keep small features flat; add subfolders only when they improve discovery.
- Prefer small explicit components, ordinary props and direct conditionals. Give each application component/page a named file and one clear responsibility. Private helpers/types/constants may stay beside their only consumer; generated clients and supplied Catalyst source retain their upstream layout. Avoid generic screen engines, configuration-driven rendering, unnecessary context/wrapper chains and speculative abstraction. Separate substantial sidebar, shell and content responsibilities.
- Keep page JSX separate from request handling and authentication actions. Extract hooks when they own real behavior rather than hiding a few lines. Keep shared code independent of feature modules and references acyclic.
- Use strict TypeScript and narrow `unknown` at untrusted boundaries. Avoid `any`, unsafe assertions, non-null assertions used to hide missing states, and `@ts-ignore` used to bypass failures. Prefer discriminated unions for meaningful state variants. Do not weaken compiler/lint rules to complete a task.
- Render from props/state; keep rendering pure and obey React's Hooks rules. Use effects to synchronize external systems, not to copy derived state. Keep transient UI state local, shareable navigation state in the URL, and remote data in TanStack Query. Do not duplicate remote entities in another store without a concrete requirement.
- Reuse shared UI and tokens before creating components. Standard listings use shared `DataTable`; statuses use shared `StatusBadge`. Create a missing shared component in the established UI location when it serves shared behavior. Read [UI standards](references/ui.md) for screens, forms, tables, or component changes.
- Use generated clients for all operations covered by an OpenAPI contract. Never handwrite duplicate endpoint clients or DTOs, or edit generated files. Read [API generation](references/api-generation.md) before integrating APIs, changing transport, or regenerating clients.
- For cookie-gateway applications, keep OAuth tokens server-side, use document navigation for gateway auth routes, and clear scoped UI/cache immediately when the session ends. Read [sessions and access](references/sessions-and-access.md) when changing authentication, protected shells, tenancy or transport.
- Write tests for changed behavior at the appropriate level. Read [testing standards](references/testing.md) for test changes and feature delivery. Never remove, skip, or weaken tests merely to pass CI.
- Keep browser secrets out of source, builds, logs, URLs, and client environment variables. UI permission checks improve usability; server authorization enforces access. Use the existing authentication model and tenant boundary. Never trust client-supplied tenant or identity values as authorization.
- Handle loading, error, empty, success, forbidden, and not-found outcomes when applicable. Preserve cancellation and prevent stale responses or repeated submissions from producing incorrect UI. Offer recoverable errors without exposing backend internals.

## API generation and deployment

Developers manually run `npm run generate:api` against the latest running browser gateway before deployment and commit generated output. Use pinned Orval with Axios and the established minimal credential/CSRF configuration in Endorz. Do not add contract checksums, drift scripts, automatic API generation in CI, duplicate clients/DTOs, or a custom transport/error framework. Preserve another explicitly required application workflow rather than migrating it silently.

Serve built Vite assets through the standard unprivileged Nginx container/configuration, with same-origin `/api` proxying and SPA fallback for application routes. Do not add a custom Node static server/proxy. Runtime secrets and internal service addresses belong in server/deployment configuration, never client build variables.

## Deliver and review

Implement the smallest cohesive change with acceptance examples. Run relevant tests, lint, type checking, production build, applicable visual checks, and contract generation checks using actual repository commands. Read [delivery and CI](references/delivery.md) when configuring CI or completing a change. Report passed, failed, and unexecuted checks separately; required failures mean the task is incomplete. Do not claim a passing test establishes behavior it does not exercise.

For reviews, report actionable defects and standard deviations with file locations and consequences; do not modify code unless requested. For implementation, fix issues within scope and report remaining limitations. This skill supplies guidance; actual enforcement requires repository configuration, CI, and branch rules. It does not authorize publishing, deploying, merging, or changing unrelated settings.

## Official references

- [React](https://react.dev/learn), [TypeScript](https://www.typescriptlang.org/docs/), [Vite](https://vite.dev/guide/)
- [TanStack Query](https://tanstack.com/query/latest/docs/framework/react/overview), [React Router](https://reactrouter.com/), [ESLint](https://eslint.org/docs/latest/)
- Task-specific documentation is linked in each reference below; verify compatibility with the installed versions rather than assuming the latest examples apply unchanged.
