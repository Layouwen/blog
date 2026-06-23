---
title: agent 偏
uuid: 4dba6565-2222-4338-b88f-0663d543311f
date: 2026-06-05
tags:
  - 汇总
  - agent
categories:
  - 汇总
  - ai
---
# Skill

## find-skills

## ponyo-cover-anchor-system 小红书封面图

```bash
npx skills add https://github.com/ponyodong2026/ponyo-cover-anchor-system.git
```

## obsidian-skills

```bash
npx skills add https://github.com/kepano/obsidian-skills
```

## Humanizer-zh

```bash
npx skills add https://github.com/op7418/Humanizer-zh.git
```

## nextlevelbuilder/ui-ux-pro-max-skill

它的用途：
给 AI 提供 UI/UX 设计指导，覆盖 React、Next.js、Vue、Svelte、Tailwind、shadcn/ui、React Native、Flutter 等栈
内置大量设计资料：UI 风格、色板、字体搭配、产品类型规则、图表类型、交互/布局/可访问性建议
可以按关键词搜索设计建议，比如 dashboard、glassmorphism、form validation
可以生成项目级 design system，比如 SaaS、fintech、spa、美容、dashboard 等场景
可用于 UI 审查、改进、重构、可访问性检查、响应式优化、配色/字体建议

```bash
npx skills add nextlevelbuilder/ui-ux-pro-max-skill
```

## pbakaus/impeccable

它的主要用途是让 Codex、Claude Code、Cursor、Gemini CLI 这类工具在做界面时少生成“AI 味”很重的设计，比如默认 Inter 字体、紫蓝渐变、卡片套卡片、低对比度灰字等。它提供设计上下文、设计原则、反模式检查，以及一组命令来指导 AI 做更好的 UI/UX。

常见用途包括：
/impeccable init：初始化项目设计上下文，生成 PRODUCT.md / DESIGN.md
/impeccable shape：写代码前先规划界面结构和交互
/impeccable audit：检查 UI 质量、可访问性、响应式等问题
/impeccable critique：做 UX/视觉设计评审
/impeccable polish：上线前细节打磨
/impeccable animate：加入更有目的的动效
/impeccable colorize / typeset / layout：分别改善颜色、字体、布局

```bash
npx skills add pbakaus/impeccable
```

### DavidHDev/react-bits

React 动效组件库

```bash
npx skills add DavidHDev/react-bits
```

## shadcn-vue

```bash
npx skills add unovue/shadcn-vue
```

## high-end-visual-design

```bash
npx skills add https://github.com/leonxlnx/taste-skill --skill high-end-visual-design
```

## stitch-design-taste

```bash
npx skills add https://github.com/leonxlnx/taste-skill --skill stitch-design-taste
```

## design-an-interface

```bash
npx skills add https://github.com/mattpocock/skills --skill design-an-interface
```

## web-design-guidelines

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines
```

```bash
npx skills add https://github.com/antfu/skills --skill web-design-guidelines
```

## vue-router-best-practices

```bash
npx skills add https://github.com/antfu/skills --skill vue-router-best-practices
```

## vue-best-practices

```bash
npx skills add https://github.com/antfu/skills --skill vue-best-practices
```

## vue-testing-best-practices

```bash
npx skills add https://github.com/antfu/skills --skill vue-testing-best-practices
```

## frontend-design

```bash
npx skills add https://github.com/anthropics/skills --skill frontend-design
```

## Superpowers

### codex app

- plugins
- search `superpowers`
- add

### opencode

tell opencode `Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md`

### cursor

- In Cursor Agent chat, install from marketplace:
   `/add-plugin superpowers`
- Or search for "superpowers" in the plugin marketplace.

# MCP

## shadcn-vue

```bash
npx shadcn-vue@latest mcp init --client codex
npx shadcn-vue@latest mcp init --client opencode
npx shadcn-vue@latest mcp init --client cursor
npx shadcn-vue@latest mcp init --client vscode
npx shadcn-vue@latest mcp init --client claude
```

## context7

执行

```bash
npx ctx7 setup
```

选择对应的 agent 工具进行安装

## figma

mcp 地址: `https://mcp.figma.com/mcp`

```bash
claude plugin install figma@claude-plugins-official
```

## cowart 无限画布

```bash
请从 https://github.com/zhongerxin/cowart.git 安装 Cowart Codex 插件。
请 clone 仓库到 ~/plugins/cowart，确认 .codex-plugin/plugin.json 存在，
把插件加入 personal marketplace，先运行 codex plugin marketplace add ~，
再运行 codex plugin add cowart@personal。
安装后请校验插件，并告诉我是否需要开启一个新对话来加载新技能和 MCP 工具。
```

# DESIGN.md

[awesome-design-md](https://github.com/VoltAgent/awesome-design-md/tree/main)