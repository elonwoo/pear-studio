# Logging and Tracing

日志与追踪。动手前核对 `src/main/services/LoggerService.ts`、`src/renderer/src/services/LoggerService.ts` 与 `packages/mcp-trace/`，深度用法见 [../../docs/en/guides/logging.md](../en/guides/logging.md)。

## Logging

```typescript
import { loggerService } from "@logger";
const logger = loggerService.withContext("moduleName");
// Renderer only: loggerService.initWindowSource('windowName') first
logger.info("message", CONTEXT);
logger.warn("message");
logger.error("message", error);
```

- Backend: Winston with daily log rotation
- Log files in `userData/logs/`
- Never use `console.log` — always use `loggerService`

## Tracing (OpenTelemetry)

- `packages/mcp-trace/` provides trace-core and trace-node/trace-web adapters
- `NodeTraceService` exports spans via OTLP HTTP
- `SpanCacheService` caches span entities for the trace viewer window
- IPC calls can carry span context via `tracedInvoke()`

## Related

- `@logger` 别名解析到 main / renderer 两处实现，见 [architecture.md](architecture.md) 的别名表。
- 追踪窗口入口 `traceWindow.html` 见 [architecture.md](architecture.md) 的 Multi-Window 一节。
- 日志不得使用 `console.log` 属于硬性规范，见 [conventions.md](conventions.md)。
