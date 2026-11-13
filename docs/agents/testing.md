# Testing

写测试或跑测试之前读这一份，并核对 `vitest.config.ts` 的 project 配置与 `tests/` 下的 setup 文件。

## Testing Guidelines

- Tests use Vitest 3 with project-based configuration
- Main process tests: Node environment, `tests/main.setup.ts`
- Renderer tests: jsdom environment, `tests/renderer.setup.ts`, `@testing-library/react`
- aiCore tests: separate `packages/aiCore/vitest.config.ts`
- All tests run without CI dependency (fully local)
- Coverage via v8 provider (`pnpm test:coverage`)
- **Features without tests are not considered complete**

## Related

- 测试命令与各变体（`test:main`、`test:renderer`、`test:aicore`、`test:watch`、`test:coverage`、`test:e2e`）见 [commands.md](commands.md)。
- 提交前必须通过 `pnpm build:check`；PR 审查阶段不要本地重跑，见 [workflow.md](workflow.md)。
- 测试计划与范围见 [../../docs/en/guides/test-plan.md](../en/guides/test-plan.md)。
