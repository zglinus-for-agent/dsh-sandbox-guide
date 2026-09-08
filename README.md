# dsh-sandbox-guide

一个 DSH(cordis)插件:注册只读的沙盒模式与宿主可见性探测工具,并在工具说明中内嵌"宿主操作提权策略",让模型针对宿主命名空间操作时能正确判断"宿主视角受限"并主动提权。

## host_view_probe 用法与输出字段

工具名默认 `host_view_probe`,可在插件 config 的 `toolName` 覆盖。无需必填参数。

```
host_view_probe
```

返回 JSON 对象:

| 字段 | 类型 | 含义 |
| --- | --- | --- |
| `mode` | string | 当前沙盒模式:`workspace-write` / `danger-full-access` / `unknown` / `unavailable` |
| `workspaceRoot` | string|null | workspace-write 的写根(如不可得为 `null`) |
| `extraRoots` | string[] | 额外可写根列表(可能为空) |
| `hostVisibility` | object | 宿主命名空间可见性探测结果 |
| `hostVisibility.xDisplaySeen` | boolean | `/tmp/.X11-unix` 是否存在 `X*` 条目 |
| `hostVisibility.waylandSeen` | boolean | `/run/user/*` 是否存在 `wayland-*` socket |
| `hostVisibility.vncPorts` | string[] | `ss -tln`(或 `/proc/net/tcp`)解析出的 5900/6080 端口 |
| `hostVisibility.procUsersVisible` | number | `ps -eo user=` 去重后的进程用户数 |
| `hostVisibility.homeDirs` | string[] | `/home` 下可读目录名 |
| `guidance` | string | 提权建议,覆盖两条分支(workspace-write、danger-full-access) |

所有探测只读;单点失败会降级为保守值而非报错。

## 接入方式

1. 将本插件目录放入 DSH bundle patches(`dsh.bundle.patch` → `cordis.patch.yml` 已含 insert 清单)。
2. 把 `dsh-sandbox-guide` 加入 `link`(链接依赖)。
3. 重启 DSH 服务(cordis 会依据 `cordis.patch.yml` 插入并加载插件)。

## 语言选型依据

`dsh-sandbox-guide` 的底座是 Node 生态的 cordis 运行时,`defineTool`/`Schema` 均为 JS API;按规范使用 JS(`"type": "module"` + `index.js`)可获得最直接的运行时集成,避免编译步骤与类型声明噪声。