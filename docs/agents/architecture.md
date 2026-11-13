# Architecture

项目骨架。改代码前先按这份文档核对源码实际位置，文档与源码冲突时以源码为准。

## Electron Structure

```
src/
  main/          # Node.js backend (Electron main process)
  renderer/      # React UI (Electron renderer process)
  preload/       # Secure IPC bridge (contextBridge)
packages/
  aiCore/        # @cherrystudio/ai-core — AI SDK middleware & provider abstraction
  shared/        # Cross-process types, constants, IPC channel definitions
  mcp-trace/     # OpenTelemetry tracing for MCP operations
  extension-table-plus/  # TipTap table extension
```

## Key Path Aliases

| Alias | Resolves To |
|---|---|
| `@main` | `src/main/` |
| `@renderer` | `src/renderer/src/` |
| `@shared` | `packages/shared/` |
| `@types` | `src/renderer/src/types/` |
| `@logger` | `src/main/services/LoggerService` (main) / `src/renderer/src/services/LoggerService` (renderer) |
| `@mcp-trace/trace-core` | `packages/mcp-trace/trace-core/` |
| `@cherrystudio/ai-core` | `packages/aiCore/src/` |

## Main Process (`src/main/`)

Node.js backend services. Key services:

| Service | Responsibility |
|---|---|
| `WindowService` | Electron window lifecycle management |
| `MCPService` | Model Context Protocol server management |
| `KnowledgeService` | RAG / knowledge base (via `@cherrystudio/embedjs`) |
| `AnthropicService` | Anthropic API integration |
| `LoggerService` | Winston-based structured logging (daily rotate) |
| `StoreSyncService` | Syncs Redux state to/from main process |
| `BackupManager` | Data backup/restore (WebDAV, S3, Nutstore) |
| `AppUpdater` | electron-updater auto-update |
| `ShortcutService` | Global keyboard shortcuts |
| `ThemeService` | System theme detection/application |
| `CopilotService` | GitHub Copilot OAuth integration |
| `PythonService` | Pyodide WASM Python runtime |
| `OvmsManager` | OpenVINO model server management |
| `NodeTraceService` | OpenTelemetry trace export |

Agents subsystem (`src/main/services/agents/`) — **removed in this fork**; the paths below are kept for reference:
- Drizzle ORM + LibSQL (SQLite) schema at `database/schema/index.ts`
- Migrations in `resources/database/drizzle/`
- **Currently undergoing v2 refactor** — only critical bug fixes accepted

## Renderer Process (`src/renderer/src/`)

React 19 + Redux Toolkit SPA. Key structure:

```
aiCore/          # Legacy middleware pipeline (deprecated, migrating to packages/aiCore)
api/             # IPC call wrappers (typed electron API calls)
components/      # Shared UI components (Ant Design 5 + styled-components + TailwindCSS v4)
databases/       # Dexie (IndexedDB) — topics, files, message_blocks, etc.
hooks/           # React hooks (useAssistant, useChatContext, useModel, etc.)
pages/           # Route pages (home, settings, knowledge, paintings, notes, etc.)
services/        # Frontend services (ApiService, ModelService, MemoryService, etc.)
store/           # Redux Toolkit slices
types/           # TypeScript type definitions
workers/         # Web Workers
windows/         # Multi-window entry points (mini, selection toolbar, trace)
```

## Redux Store (`src/renderer/src/store/`)

Slices (redux-persist enabled):

| Slice | State |
|---|---|
| `assistants` | AI assistant configurations |
| `settings` | App-wide settings |
| `llm` | LLM provider/model configs |
| `mcp` | MCP server configs |
| `messageBlock` | Message block rendering state |
| `knowledge` | Knowledge base entries |
| `paintings` | Image generation state |
| `memory` | Memory system config |
| `websearch` | Web search settings |
| `shortcuts` | Keyboard shortcuts |
| `tabs` | Tab management |

> **BLOCKED**: Do not add new Redux slices or change existing state shape until v2.0.0.

## IPC Communication

- Channel constants defined in `packages/shared/IpcChannel.ts`
- Renderer → Main: `ipcRenderer.invoke(IpcChannel.XXX, ...args)` via `api.*` wrappers in `src/preload/index.ts`
- Main → Renderer: `webContents.send(channel, data)`
- Tracing: `tracedInvoke()` in preload attaches OpenTelemetry span context to IPC calls
- Typed API surface exposed via `contextBridge` as `window.api`

## Multi-Window Architecture

The renderer builds multiple HTML entry points:
- `index.html` — Main application window
- `traceWindow.html` — MCP trace viewer

## Tech Stack

| Layer | Technologies |
|---|---|
| Runtime | Electron 38, Node ≥22 |
| Frontend | React 19, TypeScript ~5.8 |
| UI | Ant Design 5.27, styled-components 6, TailwindCSS v4 |
| State | Redux Toolkit, redux-persist, Dexie (IndexedDB) |
| Rich Text | TipTap 3.2 (with Yjs collaboration) |
| AI SDK | Vercel AI SDK v5 (`ai`), `@cherrystudio/ai-core` |
| Build | electron-vite 5 with rolldown-vite 7 (experimental) |
| Test | Vitest 3 (unit), Playwright (e2e) |
| Lint/Format | ESLint 9, oxlint, Biome 2 |
| DB (main) | Drizzle ORM + LibSQL (SQLite) |
| DB (renderer) | Dexie (IndexedDB) |
| Logging | Winston + winston-daily-rotate-file |
| Tracing | OpenTelemetry |
| i18n | i18next + react-i18next |

## 相关文档

- 数据层细节与 BLOCKED 约束：[database.md](database.md)
- AI Core 子系统摘要：[ai-core.md](ai-core.md)，深度文档见 [../../docs/en/guides/ai-core-architecture.md](../en/guides/ai-core-architecture.md)
- 日志与追踪：[logging.md](logging.md)
- 代码规范与文件命名：[conventions.md](conventions.md)
- 安全边界：[security.md](security.md)
