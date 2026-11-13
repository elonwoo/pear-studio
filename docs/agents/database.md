# Database

数据层。改动前核对源码实际状态与 v2 冻结窗口，见 [workflow.md](workflow.md)。

## Database Layer

- **IndexedDB** (Dexie): `src/renderer/src/databases/index.ts`
  - Tables: `files`, `topics`, `settings`, `knowledge_notes`, `translate_history`, `quick_phrases`, `message_blocks`, `translate_languages`
  - Schema versioned with upgrade functions (`upgradeToV5`, `upgradeToV7`, `upgradeToV8`)
  - **BLOCKED**: Do not modify schema until v2.0.0.
- **SQLite** (Drizzle ORM + LibSQL): `src/main/services/agents/`
  - Used for the agents subsystem (removed in this fork)
  - DB path: `{userData}/Data/agents.db` (e.g., on macOS: `~/Library/Application Support/Pear Studio/Data/agents.db`)

## Related

- Redux 状态形状同样处于冻结：见 [architecture.md](architecture.md) 的 BLOCKED 约束。
- 占位文件头与 v2 重构规则：见 [workflow.md](workflow.md)。
