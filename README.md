# Workflow Dashboard

A React + TypeScript dashboard for managing workflows and inspecting their runs — workflow list, run history, and a detailed run timeline view.

## Quick Start

```
npm install && npm run dev
```

Open `http://localhost:5173`

---

## Tech Stack

- React 19
- TypeScript
- React Router
- Tailwind CSS v4
- shadcn/ui
- TanStack Virtual
- date-fns

---

## Why shadcn/ui Over MUI

shadcn/ui gives lightweight, composable primitives that sit directly on top of Tailwind, with full control over the visual layer instead of fighting a default design system. For a dashboard where the styling needed to be specific rather than generic, that control mattered more than MUI's larger built-in component set.

---

## Architecture

### Routing

Three routes: `/workflows`, `/runs`, `/runs/:id`.

### Data Layer

Mock JSON files stand in for API responses. Custom hooks (`useWorkflows`, `useRuns`, `useRun`) abstract the fetch, with a simulated network delay to reproduce realistic loading states.

### State Management

No external state library. State lives in local React state, URL query params, and derived `useMemo` values — enough for this size of app without adding a dependency.

---

## URL State

The Workflows page persists search, status filter, and sort order to the URL:

```
/workflows?search=kyc
/workflows?status=published
/workflows?sort=name
```

This gives refresh persistence, deep linking, and shareable filtered views for free.

---

## Performance Decisions

- **Workflows** — the grid is virtualized with TanStack Virtual so render cost stays flat as the dataset grows.
- **Runs** — built with larger datasets in mind; the structure supports applying the same virtualization if the run count grows significantly, even though it isn't needed at the current data size.

---

## Hover & Active States

- **Workflow + Run cards** — `hover:border-violet-300 hover:shadow-md`. Elevates the card without a layout shift, signals clickability without relying on cursor change alone.
- **Status filter pills** — inactive pills use `hover:border-slate-300 hover:bg-slate-50`; active pill gets the primary navy fill. Keeps the interaction cost low for frequent filter switching.
- **Run modal** — Escape closes via Radix Dialog's built-in keyboard handling; Enter submits from the subject ID input.
- **Bulk action bar** — slides in from the bottom on first selection, stays sticky so it's reachable without scrolling back up.

---

## Status Badge Palette

| State | Color |
|---|---|
| Published / Completed / Succeeded | Emerald |
| Draft / Not Started | Slate |
| Archived | Stone |
| Running | Violet |
| Waiting / Pending | Amber |
| Failed | Rose |

---

## Edge Cases Handled

- **Long workflow names** — truncated in cards to preserve layout.
- **Future last-modified dates** — surfaced rather than hidden, so data-quality issues stay visible.
- **Workflow with 0 nodes** — node count displayed as-is.
- **Missing subject ID** — renders an em dash (`—`) instead of empty content.
- **Empty run steps** — waiting runs with no steps render an empty state.
- **Warning messages** — runs with warnings get a dedicated warning banner.

---

## Features

**Workflows** — search, debounced filtering, status filter pills with counts, sorting, URL sync, responsive grid, multi-select, bulk actions, run dialog, loading/error/empty states.

**Runs** — status filter pills with counts, progress visualization, relative timestamps, duration display, cancel actions, navigation to run detail, loading/error/empty states.

**Run Detail** — warning banner, execution timeline, step status indicators, JSON output rendering, error rendering, metadata sidebar, collapsible trigger input, empty/not-found states.

---

## What I'd Improve With More Time

- Runs virtualization tuned for very large datasets
- Keyboard navigation improvements
- Animated expand/collapse interactions
- Deeper accessibility audit
- More detailed execution analytics
- Reusable loading skeleton system
