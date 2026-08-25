---
title: deepseek harness 篇
uuid: 0793872e-d49c-42e4-8dca-b15404a49b4d
date: 2026-08-20
tags:
  - 汇总
  - agent
  - deepseek-harness
categories:
  - 汇总
  - ai
---
# DeepSeek Harness 常用插件

用于记录 DeepSeek Harness（`dsh`）的常用插件、安装命令和配置备注。通用 Agent Skills、MCP 仍记录在 [[blog/summary/ai/agent 篇|Agent 工具与技能汇总]]。

> [!note] Profile
> `--profile web` 表示只把插件安装到 Web profile。安装或更新 Web 插件后，重启 `dsh web` 才会加载新版本。

## 插件

| 插件 | 用途 | 安装 / 更新 |
| --- | --- | --- |
| [dshmarket](https://github.com/dsh-market/dsh-market) | DeepSeek Harness 内的可视化插件市场，用于浏览、搜索和一键安装社区插件。该命令未锁定版本。 | `npx @deepseek-ai/dsh plugin --profile web add dshmarket` |
| [dsh-at-file](https://github.com/omdsh-dev/dsh-at-file) `v0.6.5` | 给 Web 输入框增加类似 Codex 的 `@文件` / `@目录` 路径引用与搜索。插件只附加工作区相对路径，不会自动读取文件内容。 | `npx @deepseek-ai/dsh plugin --profile web add https://github.com/omdsh-dev/dsh-at-file/archive/refs/tags/v0.6.5.tar.gz` |

## 常用命令

### 启动 Web UI

```bash
npx @deepseek-ai/dsh web
```

### 查看 Web profile 的最终配置

```bash
npx @deepseek-ai/dsh --profile web --dump-config
```

### 移除插件

```bash
npx @deepseek-ai/dsh plugin --profile web remove <plugin-name>
```

## 参考

- [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)
- [DeepSeek Harness CLI reference](https://github.com/deepseek-ai/deepseek-harness/blob/master/apps/cli/reference/README.md)
- [dsh-at-file 中文说明](https://github.com/omdsh-dev/dsh-at-file/blob/v0.6.5/README.zh.md)

