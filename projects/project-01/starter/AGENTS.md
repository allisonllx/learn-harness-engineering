# AGENTS.md -- Project 01: Knowledge Base Electron App

Designed for long-running coding-agent work. Leave the repo restartable so the
next session can continue without guessing.

## Project Overview

Build an Electron app that can show documents and answer questions.

Desktop knowledge base: import `.txt`/`.md`, index locally, answer with
citations. No external APIs—all processing is local.

## Startup Workflow

Before writing code:

1. Confirm `pwd` (project root: `starter/`).
2. Read `claude-progress.md` and `feature_list.json`; pick highest-priority
   unfinished feature.
3. Review `git log --oneline -5`.
4. Run `./init.sh` and required verification.

If baseline verification fails, fix that first.

## Features

Record status and evidence in `feature_list.json`.

| ID | Feature | Verification |
|----|---------|--------------|
| `window-launch` | Electron window starts successfully | `npm run dev` opens 1200×800 window |
| `document-list` | UI shows the document-list area | Sidebar with empty state or documents |
| `question-panel` | UI shows the QA panel | Input + Ask button submit via IPC |
| `data-directory` | App creates and uses a local data directory | `userData/knowledge-base-data` on startup |

## Build and Test Commands

```bash
npm install && npm run check && npm run build   # verify compile
npm run dev                                     # launch Electron
npm test                                        # Vitest (single run)
npm run test:watch                              # Vitest watch
./init.sh                                       # startup gate
```

## Code Style Guidelines

**Layers** — Main (`src/main/`): window, IPC, services. Preload: `contextBridge`
only. Renderer: React via `window.knowledgeBase`; no Node imports. Services:
main-process logic with injected `PersistenceService`.

**Conventions** — TypeScript strict; no `any` without comment; named exports;
IPC in `src/shared/types.ts` (`IPC_CHANNELS`); async Promises in renderer.

**Key files** — `main.ts`, `ipc-handlers.ts`, `preload.ts`, `App.tsx`,
`DocumentList.tsx`, `QuestionPanel.tsx`, `persistence-service.ts`.

## Testing Instructions

Run `npm run check`, `npm run build`, and `npm run dev` (visible window, no
console errors). Manually verify each feature above. Run `npm test` when tests
exist; never weaken tests to pass. Record evidence in `feature_list.json` or
`claude-progress.md`.

## Security Considerations

- `contextIsolation: true`, `nodeIntegration: false`.
- Preload: typed APIs only—no raw `ipcRenderer` or Node modules.
- Filesystem in main process; data under `app.getPath('userData')`.
- Validate renderer-supplied paths; `.txt`/`.md` only, 10 MB max.
- No network or API keys; Q&A is mock/local.

## Working Rules

One feature at a time. Code added ≠ done. Stay in scope. Do not change
verification rules silently. Prefer repo artifacts over chat summaries.

## Required Artifacts

`feature_list.json` (feature state), `claude-progress.md` (session log),
`init.sh` (startup gate).

## Definition of Done

Behavior verified, `npm run check` passes, `npm run dev` clean, evidence
recorded, layer boundaries respected, `./init.sh` succeeds.

## End of Session

Update `claude-progress.md` and `feature_list.json`. Record blockers. Commit
when safe. Leave `./init.sh` runnable.
