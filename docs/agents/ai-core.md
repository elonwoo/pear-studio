# AI Core

`packages/aiCore/` 的快速摘要。动手改这一块之前，先读深度文档 [../../docs/en/guides/ai-core-architecture.md](../en/guides/ai-core-architecture.md)，再核对 `packages/aiCore/src/` 目录结构与 `packages/aiCore/vitest.config.ts`。

## AI Core (`packages/aiCore/`)

The `@cherrystudio/ai-core` package abstracts AI SDK providers:

```
src/core/
  providers/    # Provider registry (HubProvider, factory, registry)
  middleware/   # LanguageModelV2Middleware pipeline (manager, wrapper)
  plugins/      # Built-in plugins
  runtime/      # Runtime execution
  options/      # Request option preparation
```

- Built on Vercel AI SDK v5 (`ai` package) with `LanguageModelV2Middleware`
- `HubProvider` aggregates multiple provider backends
- Supports: OpenAI, Anthropic, Google, Azure, Mistral, Bedrock, Vertex, Ollama, Perplexity, xAI, HuggingFace, Cerebras, OpenRouter, Copilot, and more
- Custom fork of openai package: `@cherrystudio/openai`

## Related

- 渲染进程的 `aiCore/` 是旧管线（deprecated，正在迁移到本包），见 [architecture.md](architecture.md)。
- 本包的测试配置与运行方式见 [testing.md](testing.md)。
- 相关依赖补丁见 [conventions.md](conventions.md) 的 Packages with Custom Patches。
