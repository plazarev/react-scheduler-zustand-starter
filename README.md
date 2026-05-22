# DHTMLX React Scheduler with Zustand State Management

A working demo of **DHTMLX React Scheduler** integrated with **Zustand** as the client-side store for event data. Demonstrates store-driven event CRUD, undo/redo, and a custom view/navigation toolbar — all wired through a single Zustand store. Built with React, TypeScript, and Vite.

**Related tutorial**:
[https://docs.dhtmlx.com/scheduler/integrations/react/state/zustand/](https://docs.dhtmlx.com/scheduler/integrations/react/state/zustand/)

## What is DHTMLX React Scheduler with Zustand Starter
 
This demo shows how to connect DHTMLX React Scheduler to Zustand, a minimal and unopinionated state management library for React. All calendar events live in a Zustand store, and every user interaction — creating, editing, deleting, dragging, or resizing an event — flows through Zustand actions rather than directly updating Scheduler's internal state.
 
The critical integration point is Scheduler's `data.save(...)` callback, which fires after any user mutation. The demo maps this callback to Zustand actions, giving the store full ownership of event data. Undo/redo is implemented by recording event state snapshots in the store; navigation changes (view switches, date moves) are intentionally excluded from history.
 
A lightweight custom toolbar handles view switching, date navigation, and undo/redo controls alongside the Scheduler.
 
## When to Use
 
- Use this demo when you want to manage DHTMLX Scheduler events through a centralized Zustand store rather than local component state.
- Use this when you need undo/redo for event mutations and want it implemented through store history rather than a custom history stack.
- Use this when your Scheduler data fits in memory and persistence is handled externally (e.g., saving to an API on user action).

## Quick Start
 
```bash
git clone https://github.com/DHTMLX/react-scheduler-zustand-starter
cd react-scheduler-zustand-starter
npm install
npm run dev
```
 
Open [http://localhost:5173](http://localhost:5173) in your browser.
 
**Requirements:** Node.js `20.19+` (required by Vite 7), npm.
 
## Try it
 
Once running, these interactions demonstrate the Zustand integration:
 
1. Create a new event — it appears immediately, driven by the store.
2. Drag or resize an event — the store updates; the change is undoable.
3. Click **Undo** — the last event mutation is reverted.
4. Switch views or navigate dates — notice that **Undo** still targets event edits, not navigation.
## Architecture
 
```
src/
├── components/
│   └── Scheduler.tsx     # Scheduler initialization + data.save → Zustand bridge
├── store.ts              # Zustand store: events, actions, undo/redo history
├── seed/
│   └── data.ts           # Initial events, default view, and starting date
└── App.tsx
```
 
The store (`store.ts`) owns all event state. `Scheduler.tsx` subscribes to the store and passes events to Scheduler as props. When Scheduler fires `data.save(...)` after a mutation, the component calls the matching Zustand action (add, update, or delete). The store records a state snapshot before each mutation, enabling undo/redo by replaying snapshots.
 
## Key Patterns
 
- **`data.save` to Zustand bridge** — Scheduler's `data.save(...)` callback is the single entry point for all mutations. `Scheduler.tsx` maps each save operation type to a Zustand action, keeping the component thin and the store the only place where event state changes.
- **Store-owned undo/redo** — the Zustand store maintains a history array of past event states. Undo pops the stack and restores the previous snapshot; redo replays the next one. Navigation is excluded from history by design.
- **Navigation state in the store** — active view and current date are stored in Zustand alongside event data, so the custom toolbar and Scheduler config read from and write to the same store without prop drilling.
- **Seed-driven initialization** — `seed/data.ts` provides deterministic initial events and default view/date config, making the demo reproducible and easy to swap for real data loading.

## Features
 
| Feature | Details |
|---|---|
| Event CRUD | Create, edit, and delete events via Zustand store actions |
| Drag-and-drop | Rescheduling and resizing persisted to the store |
| Views | Day, week, and month views |
| Custom toolbar | View switcher, date navigation (Prev/Next/Today), undo/redo |
| Undo/redo | Snapshot-based history for event mutations; navigation excluded |
| TypeScript | Full type coverage for events and store shape |
| Vite | Fast dev server and production build |

## Production Notes
 
This demo is a starter, not a production-ready application. For production use:
 
- **Add persistence** — events are in-memory only; page refresh resets to the seed dataset. Connect `store.ts` actions to an API or database to persist changes.
- **Scope the undo history** — the demo records every event mutation. In a real app, consider limiting history depth or selectively excluding bulk operations.
- **Add authentication** — no auth or multi-user support is included.
- **Scheduler license** — DHTMLX React Scheduler requires a valid commercial license for production use (see License section below).

## Related Resources
 
- [Zustand integration guide (tutorial on the official blog)](https://dhtmlx.com/blog/using-zustand-state-management-apps-dhtmlx-react-gantt-scheduler/)
- [DHTMLX React Scheduler product page](https://dhtmlx.com/docs/products/dhtmlxScheduler-for-React/)
- [DHTMLX Scheduler product page](https://dhtmlx.com/docs/products/dhtmlxScheduler/)
- [DHTMLX Scheduler documentation](https://docs.dhtmlx.com/scheduler/)
- [DHTMLX Scheduler React integration guide](https://docs.dhtmlx.com/scheduler/react.html)
- [Jotai scheduler starter (alternative atomic state approach)](https://github.com/DHTMLX/react-scheduler-jotai-starter)
- [Community forum](https://forum.dhtmlx.com/)
- [Report an issue](https://github.com/DHTMLX/react-scheduler-zustand-starter/issues)

## License
 
The source code in this repository is released under the **MIT License**.

**DHTMLX React Scheduler** is a commercial library - use it under a valid [DHTMLX license](https://dhtmlx.com/docs/products/licenses.shtml) or evaluation agreement.
 
**Commercial License**
Required for proprietary or commercial applications. Includes access to PRO features, dedicated technical support, and long-term maintenance.
[Learn more →](https://dhtmlx.com/docs/products/dhtmlxScheduler-for-React/#licensing)

**Try before you buy**
A free evaluation of DHTMLX React Gantt is available — no credit card required.
[Start your evaluation →](https://dhtmlx.com/docs/products/dhtmlxScheduler-for-React/download.shtml)
