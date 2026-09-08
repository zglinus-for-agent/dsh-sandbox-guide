# Changelog

本仓库遵循工作流分册:0.x.y 在 scaffolding 分支累积,1.0.0 时随首次合并进入 main。

## 0.1.1
- 修复: @deepseek-ai/schemastery 改为默认导入(`import z from ...`),修正 `Schema` 命名导入崩溃;
- 修复: output.schema 改为 plain JSON-Schema(满足 dsh-tools enforced subset),移除 `.optional()` 与 `z.object` 用法;
- 修复: parameters 恢复扁平空对象 `{}`(defineTool 参数为字段映射,非 {type,properties} 包装);
- 验证: 运行时冒烟通过(模块加载 / apply 注册 / 真实宿主探测 / 降级逻辑)。

## 0.1.0
- 初版:内置 host_view_probe 只读探测工具(沙盒模式、workspaceRoot、extraRoots、宿主可见性:桌面/X、Wayland、VNC 端口、进程用户、/home)与 guidance;
- 工具 description 内嵌"宿主操作提权策略"(sandbox_permissions、workspace-write→danger-full-access、宿主视角受限判定);
- 提供 cordis.patch.yml bundle 接入与 AGENTS.md 开发提示。
