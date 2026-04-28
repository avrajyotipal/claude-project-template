# Frontend Next.js Rules

Apply these rules when editing Next.js and TypeScript files.

## Architecture
- Use Next.js App Router with strict TypeScript — no `any` types.
- Keep presentational and data-fetching concerns separate.
- Avoid oversized pages; split into focused components.
- Reuse shared UI primitives instead of duplicating layouts.
- Keep API calls centralized in dedicated client modules.

## Real-time UI
- Use custom WebSocket hooks for live chat, call status, and dashboard updates.
- Handle WebSocket reconnection and connection state in the UI.
- Show real-time indicators (typing, agent status, call quality).
- Debounce rapid state updates to prevent render thrashing.

## States and UX
- Add loading, empty, and error states for all async UI.
- Show skeleton loaders for dashboard and analytics views.
- Add optimistic updates for chat message sending.
- Handle offline/reconnection gracefully.

## Agent dashboard
- Keep the agent copilot panel lightweight and non-blocking.
- Show AI suggestions inline without disrupting agent workflow.
- Support keyboard shortcuts for common agent actions.

## Admin panel
- Use role-based rendering — hide controls the user cannot access.
- Add confirmation dialogs for destructive actions.
- Keep configuration forms validated client-side and server-side.