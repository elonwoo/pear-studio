# Security

安全边界。涉及 preload、主进程 IPC handler、网络入口与 API server 时先读这一份，再核对 `src/preload/index.ts`、`packages/shared/IpcChannel.ts` 与 API server 相关依赖。

## Security

- Never expose Node.js APIs directly to renderer; use `contextBridge` in preload
- Validate all IPC inputs in main process handlers
- URL sanitization via `strict-url-sanitise`
- IP validation via `ipaddr.js` (API server)
- `express-validator` for API server request validation

## Related

- IPC 链路的完整形状见 [architecture.md](architecture.md) 的 IPC Communication 一节。
- 依赖补丁清单（含 `electron-updater` 等）见 [conventions.md](conventions.md)。
- 检出密钥/凭据时的处置属于提交纪律，见 [workflow.md](workflow.md)。
