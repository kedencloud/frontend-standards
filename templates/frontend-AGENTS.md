# Frontend engineering standards

Merge these instructions into the frontend's applicable AGENTS.md; preserve existing project-specific instructions.

- Apply the installed `kedencloud-frontend` skill for React/TypeScript implementation, tests, refactoring and reviews. Keep it available in the repository or development environment; this reference alone does not install it.
- Inspect shared UI/tokens, routes, gateway contracts, generated clients, runtime/package versions and tests first. Preserve the supplied component system (Catalyst in Endorz), accessibility and responsive behavior.
- Keep composition/providers in `src/app`, feature screens/logic in `src/features/<feature>`, shell/navigation in `src/layouts`, standalone pages in `src/pages`, reusable UI in `src/components`, and transport/generated clients in `src/api`. Keep small features flat.
- Prefer small explicit components, ordinary props and direct conditionals. Give each application component/page a named file and clear responsibility. Private helpers/types/constants may stay with their only consumer; supplied/generated code retains its upstream layout. Avoid generic screen engines, unnecessary providers/wrappers and speculative abstraction.
- Keep page JSX separate from request handling/auth actions. Extract hooks for real behavior. Reuse shared DataTable, StatusBadge, fields, dialogs, filter drawer, pagination and notifications.
- Manually run `npm run generate:api` with pinned Orval against the latest running gateway before deployment and commit generated output. Use generated Axios functions/models/hooks and minimal shared CSRF configuration in Endorz. Never edit generated files or duplicate covered endpoint clients/DTOs. Do not add contract checksums, drift scripts or automatic API generation to CI.
- Keep OAuth tokens server-side. Use full document navigation/native CSRF-protected POST for gateway auth endpoints. Immediately hide protected content, cancel queries and discard scoped caches on session end; isolate users/organisations and distinguish operation errors from session denial.
- Keep drawer filter drafts separate until Apply. Cancel/Escape discard drafts and preserve background route/query state. Dialog retries retain input and the existing request identifier; success refreshes relevant data and uses shared toasts. Do not fabricate metrics, permissions or unsupported navigation.
- Serve production assets through the standard unprivileged Nginx container/configuration with same-origin API proxying. Do not add a custom Node static server/proxy or put secrets/internal service addresses in client variables.
- Use strict TypeScript, Vitest, React Testing Library, MSW and Playwright at the appropriate levels. Cover changed states/journeys and high-risk session/tenant behavior. Shared page structure changes require desktop/mobile browser, accessibility and screenshot checks without automatic baseline replacement.
- Run actual repository lint, formatting, typecheck, tests and build commands. Never weaken compiler settings, tests or CI to pass; report failed/unexecuted checks. Mocked browser tests do not prove backend authorization or real secure-cookie/SSO integration.
- For backend changes in scope, follow the applicable Kedencloud .NET standards, including executable Gherkin, real application boundaries and production-engine Testcontainers where required.

Configure actual application CI and required branch checks to enforce these rules. The skill supplies guidance, not an enforcement hook.
