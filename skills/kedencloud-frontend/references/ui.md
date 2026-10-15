# Shared UI and accessible screens

## Design system and layout

Inspect the existing shell, sidebar, page headers, fields, tables, dialogs, drawers, notifications, and state components. Reuse their public interfaces and established import paths. Use semantic tokens for colors, typography, spacing, borders, radii, elevation, and focus. With Tailwind v4, extend the existing CSS theme/token mapping rather than introducing a parallel v3 configuration or arbitrary page-specific styles. Preserve configured light/dark themes. Avoid introducing another styling or component framework as part of ordinary feature work.

Keep feature business rules outside generic primitives. Extend shared components for reusable behavior and update relevant stories when their API or states change. shadcn/ui code is application-owned: inspect and maintain its accessibility instead of assuming a copied primitive guarantees compliance.

Use responsive layouts that support narrow screens, zoom, long text, localization, and content growth. Contain deliberate table scrolling; avoid unintended page overflow. Reuse consistent page widths, heading hierarchy, toolbars, search/filter placement, and dialog sizing. Keep navigation and interactive elements semantic.

## Tables and statuses

All standard listings use the shared `DataTable` backed by TanStack Table as needed. Standardize columns, padding, row actions, selection, loading skeletons, empty/error states, and pagination. Use stable domain IDs for rows. Choose server-side sorting/filtering/pagination when the API operates over the full dataset; do not imply that sorting one downloaded page sorts all records. Reset or validate pagination when filters change. Keep URL state where users need shareable/back-button navigation.

All statuses use shared `StatusBadge`. Centralize labels, icons, and semantic variants; never scatter status colors through pages. Use text or an accessible name so meaning does not depend on color. Handle unknown future status values safely without pretending they are a known success. Format dates, times, numbers, and currency through shared locale-aware utilities; make timezone assumptions explicit and keep date-only values distinct from instants.

## Forms

Use React Hook Form with Zod for forms that benefit from managed validation/state; simple forms need not become an abstraction exercise. Reuse shared labels, controls, descriptions, messages, and layouts. Form schemas describe UI input and may intentionally differ from generated API DTOs; map to the generated request type without redefining that contract. Client validation complements server validation. Map documented field and form errors consistently, preserving user input on failure.

Use appropriate input types, autocomplete, accessible required/error indicators, and submit behavior. Disable or guard repeated submission while pending and announce completion/failure. Keep save, cancel, and destructive action patterns consistent. Protect meaningful unsaved changes using routing/browser mechanisms supported by the app; cover both internal navigation and relevant unload cases and document platform limits. Avoid persistent storage of sensitive form drafts without a requirement.

## Accessibility and performance

Target WCAG 2.2 AA for application UI. Verify keyboard order, visible and unobscured focus, accessible names, contrast, reflow, touch targets, validation messages, and reduced motion. Use native semantics before ARIA. For dialogs, verify focus entry, focus containment when modal, Escape behavior, and focus restoration. Ensure icon-only controls have names and live announcements are useful rather than noisy. Automated checks supplement manual keyboard and screen-reader checks.

Measure performance before adding memoization or architectural complexity. Use route-level splitting where useful, appropriately sized images, bounded queries, and virtualization for demonstrated large-list needs while retaining accessibility. Avoid unnecessary network waterfalls and oversized dependencies. Preserve established performance budgets; do not invent results without measurements.

## Official documentation

- [Tailwind theme variables](https://tailwindcss.com/docs/theme/), [responsive design](https://tailwindcss.com/docs/responsive-design)
- [shadcn/ui](https://ui.shadcn.com/docs), [Tailwind v4 integration](https://ui.shadcn.com/docs/tailwind-v4)
- [TanStack Table](https://tanstack.com/table/latest/docs/introduction)
- [React Hook Form](https://react-hook-form.com/docs), [Zod](https://zod.dev/)
- [Lucide React](https://lucide.dev/guide/packages/lucide-react), [Storybook](https://storybook.js.org/docs)
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/), [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
