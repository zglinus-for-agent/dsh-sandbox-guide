# Changelog

## 0.1.0

- 初版。
- 内置 `host_view_probe` 只读探测工具:沙盒模式、workspaceRoot、extraRoots、宿主可见性(桌面/X、Wayland、VNC 端口、进程用户、/home)与 guidance。
- 在工具 description 内嵌"宿主操作提权策略"(sandbox_permissions、workspace-write→danger-full-access、宿主视角受限判定)。
- 提供 `cordis.patch.yml` bundle 接入与 `AGENTS.md` 开发提示。