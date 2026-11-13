# Commands

全部开发命令。执行前核对 `package.json` 的 `scripts` 与 `.node-version` 现值。

## Development Commands

- **Install**: `pnpm install` — Install all project dependencies (requires Node ≥22, pnpm 10.27.0)
- **Development**: `pnpm dev` — Runs Electron app in development mode with hot reload
- **Debug**: `pnpm debug` — Starts with debugging; attach via `chrome://inspect` on port 9222
- **Build Check**: `pnpm build:check` — **REQUIRED** before commits (`pnpm lint && pnpm test`)
  - If having i18n sort issues, run `pnpm i18n:sync` first
  - If having formatting issues, run `pnpm format` first
- **Full Build**: `pnpm build` — TypeScript typecheck + electron-vite build
- **Test**: `pnpm test` — Run all Vitest tests (main + renderer + aiCore + shared + scripts)
  - `pnpm test:main` — Main process tests only (Node environment)
  - `pnpm test:renderer` — Renderer process tests only (jsdom environment)
  - `pnpm test:aicore` — aiCore package tests only
  - `pnpm test:watch` — Watch mode
  - `pnpm test:coverage` — With v8 coverage report
  - `pnpm test:e2e` — Playwright end-to-end tests
- **Lint**: `pnpm lint` — oxlint + eslint fix + TypeScript typecheck + i18n check + format check
- **Format**: `pnpm format` — Biome format + lint (write mode)
- **Typecheck**: `pnpm typecheck` — Concurrent node + web TypeScript checks using `tsgo`
- **i18n**:
  - `pnpm i18n:sync` — Sync i18n template keys
  - `pnpm i18n:translate` — Auto-translate missing keys
  - `pnpm i18n:check` — Validate i18n completeness
- **Bundle Analysis**: `pnpm analyze:renderer` / `pnpm analyze:main` — Visualize bundle sizes
- **Agents DB**:
  - `pnpm agents:generate` — Generate Drizzle migrations
  - `pnpm agents:push` — Push schema to SQLite DB
  - `pnpm agents:studio` — Open Drizzle Studio

## 备注

- 环境搭建与 IDE 配置见 [../../docs/en/guides/development.md](../en/guides/development.md)。
- 测试环境划分见 [testing.md](testing.md)；i18n 规范见 [conventions.md](conventions.md)。
