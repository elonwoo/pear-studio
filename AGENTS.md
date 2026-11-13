# Notes for coding agents

This file provides guidance to AI coding assistants when working with code in this repository. Adherence to these guidelines is crucial for maintaining code quality and consistency.

这一份只是导航。每节说明「动手做什么之前先读哪份文档」，具体细节都在 `docs/agents/` 与 `LESSONS.md` 里；文档与源码不一致时以源码为准，读完文档要回去核对对应路径的实现。

## 动工之前

- 读 [docs/agents/workflow.md](docs/agents/workflow.md)：指导原则、PR / Issue / Review 工作流、分支策略（`main` 冻结、`v2` 分支）、v2 重构期间被 BLOCKED 的文件。
- 读 [LESSONS.md](LESSONS.md)：本项目已记录的踩坑教训与对应规则。
- 涉及架构判断时读 [docs/en/guides/branching-strategy.md](docs/en/guides/branching-strategy.md)，并核对仓库当前分支状态。

## 命令与提交前检查

- 跑任何命令之前读 [docs/agents/commands.md](docs/agents/commands.md)：安装、开发、调试、构建、测试、lint、format、typecheck、i18n、bundle 分析、agents DB 命令及其前置条件。
- 提交前必须过 `pnpm build:check`；改完代码按 [docs/agents/testing.md](docs/agents/testing.md) 补测试。

## 改代码之前

- 读 [docs/agents/architecture.md](docs/agents/architecture.md)：Electron 三层结构、路径别名、主进程服务、渲染进程目录、Redux slices、IPC、多窗口、技术栈。改动前先按别名表和目录树核对源码实际位置。
- 读 [docs/agents/conventions.md](docs/agents/conventions.md)：TypeScript 配置、代码风格、文件命名、i18n 规范、带自定义补丁的依赖清单。核对 `.biome` / `eslint` / `tsconfig` 配置与 `patches/` 目录现况。
- 读 [docs/agents/security.md](docs/agents/security.md)：`contextBridge`、IPC 输入校验、URL 清洗、IP 校验、API server 请求校验。涉及 preload、主进程 handler、网络入口时先读。

## 按子系统

- 数据层（IndexedDB / Dexie 表与 schema 版本、SQLite / Drizzle、BLOCKED 约束）：读 [docs/agents/database.md](docs/agents/database.md)，并核对 `src/renderer/src/databases/index.ts` 与 `src/main/services/agents/` 的实际状态。
- AI Core（`packages/aiCore/`）：读 [docs/agents/ai-core.md](docs/agents/ai-core.md) 的摘要，再读深度文档 [docs/en/guides/ai-core-architecture.md](docs/en/guides/ai-core-architecture.md)，并核对 `packages/aiCore/src/` 目录。
- 日志与追踪：读 [docs/agents/logging.md](docs/agents/logging.md) 与 [docs/en/guides/logging.md](docs/en/guides/logging.md)，核对 `src/main/services/LoggerService.ts`、`src/renderer/src/services/LoggerService.ts` 与 `packages/mcp-trace/`。
- i18n：读 [docs/agents/conventions.md](docs/agents/conventions.md) 的 i18n 一节与 [docs/en/guides/i18n.md](docs/en/guides/i18n.md)，核对 `src/renderer/src/i18n/`。

## 测试

- 写或跑测试之前读 [docs/agents/testing.md](docs/agents/testing.md)：Vitest 三类环境、setup 文件、覆盖率、e2e、无测试不算完成。核对 `vitest.config.ts` 的 project 配置。

## 仓库工作流与外部文档

- 创建 PR 用 `gh-create-pr` skill；不可用时读 [.agents/skills/gh-create-pr/SKILL.md](.agents/skills/gh-create-pr/SKILL.md) 手动执行。
- 创建 Issue 用 `gh-create-issue` skill；不可用时读 [.agents/skills/gh-create-issue/SKILL.md](.agents/skills/gh-create-issue/SKILL.md) 手动执行。
- 审查 PR 的流程见 [docs/agents/workflow.md](docs/agents/workflow.md)，不要本地重跑 lint / test / format。
- 贡献流程与开发环境搭建见 [CONTRIBUTING.md](CONTRIBUTING.md)、[docs/en/guides/development.md](docs/en/guides/development.md)；skills 目录约定见 [.agents/skills/README.md](.agents/skills/README.md)。
