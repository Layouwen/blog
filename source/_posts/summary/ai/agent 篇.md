---
title: agent 篇
uuid: 4dba6565-2222-4338-b88f-0663d543311f
date: 2026-06-05
tags:
  - 汇总
  - agent
categories:
  - 汇总
  - ai
---
# Agent 工具与技能汇总

用于记录常用 Agent 插件、Skills、MCP 的用途与安装方式。

## 专题

- [[blog/summary/ai/deepseek harness 篇|DeepSeek Harness 常用插件]]

## Plugin

| 名称          | 用途                                                                                                                          | 安装方式                                                                                                                                                                                                                                                                 |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Superpowers | 给 Agent 增加结构化工作流能力，例如头脑风暴、写计划、TDD、系统化调试、执行计划、验收验证等。适合复杂开发任务或需要严谨流程的工作。<br><br>备注：停用 `Test-Driven Development`，手动指定使用 `TDD`。 | Codex App：`plugins` → search `superpowers` → add<br>Cursor：`/add-plugin superpowers`，或在 plugin marketplace 搜索 `superpowers`<br>opencode：`Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md` |

## Skills

### Agent 工作流

| Skill       | 用途                                                        | 安装方式                                                      |
| ----------- | --------------------------------------------------------- | --------------------------------------------------------- |
| find-skills | 搜索、发现和安装适合当前任务的 Agent Skills。适合“不知道该用哪个 skill”或想扩展技能库时使用。 | 未记录；安装后直接调用 `find-skills`。                                |
| grill-me    | 用连续追问的方式拷打一个计划、想法、PRD 或技术方案，帮助暴露假设、遗漏、风险和优先级问题。           | `npx skills add mattpocock/skills --skill=grill-me -y -g` |
| eli5        | 把复杂概念用“Explain Like I’m 5”的方式讲清楚。适合快速理解陌生技术、术语、方案或长文内容，并产出更直白的解释。 | `claude plugin marketplace add anthropics/claude-plugins-community`<br>`claude plugin install eli5@claude-community` |

### UI / UX 设计

#### 推荐使用顺序

- **最小优化**：`frontend-design` 主导 → `emil-design-eng` 补细节 → `web-design-guidelines` 做检查。
- **最优效果**：`impeccable` 主导 → `design-taste-frontend` 定审美方向 → `high-end-visual-design` 补质感判断。

#### 保留：测试下来可用的 Skill

| Skill                  | 用途                                                                                                                                                        | 安装方式                                                                                                                                                                           |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| frontend-design        | 生产级前端设计技能。适合创建高质量网页、落地页、仪表盘、产品界面，并提升视觉层次、排版、动效和完成度。                                                                                                       | `npx skills add https://github.com/anthropics/skills --skill frontend-design`                                                                                                  |
| emil-design-eng        | 面向设计工程师的 UI 质量技能。适合补充界面结构、组件细节、交互状态、动效判断和整体产品质感。                                                                                                          | `npx skills@latest add emilkowalski/skills`                                                                                                                                    |
| web-design-guidelines  | Web 界面设计规范审查。适合检查页面的信息层级、布局节奏、可读性、交互状态、响应式和无障碍问题。                                                                                                         | `npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines`<br>或：`npx skills add https://github.com/antfu/skills --skill web-design-guidelines` |
| impeccable             | 减少 AI 生成界面的“AI 味”，避免默认 Inter 字体、紫蓝渐变、卡片套卡片、低对比度灰字等问题。常用命令：`/impeccable init`、`shape`、`audit`、`critique`、`polish`、`animate`、`colorize`、`typeset`、`layout`。 | `npx skills add pbakaus/impeccable`                                                                                                                                            |
| design-taste-frontend  | `Leonxlnx/taste-skill` 技能集合里的具体前端审美 skill。适合定审美方向，补充更明确的视觉风格、版式气质、色彩 / 字体方向和整体设计品味。 | `npx skills add Leonxlnx/taste-skill --skill design-taste-frontend` |
| high-end-visual-design | 高端视觉设计审美指导。适合补质感判断，让网页、落地页、品牌视觉、作品集和产品界面显得更贵、更精致、更像顶级 agency。                                                                                             | `npx skills add https://github.com/leonxlnx/taste-skill --skill high-end-visual-design`                                                                                        |

#### 不推荐：测试下来表达不好用的 Skill

| Skill                                | 用途                                                                                                                                   | 安装方式                                                                                 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| nextlevelbuilder/ui-ux-pro-max-skill | 给 AI 提供 UI/UX 设计指导，覆盖 React、Next.js、Vue、Svelte、Tailwind、shadcn/ui、React Native、Flutter 等技术栈。可用于 design system、UI 审查、响应式优化、配色 / 字体建议。 | `npx skills add nextlevelbuilder/ui-ux-pro-max-skill`                                |
| stitch-design-taste                  | 面向 Google Stitch / AI 设计生成的设计审美技能，用语义化方式描述视觉风格、布局层级和组件质感。                                                                            | `npx skills add https://github.com/leonxlnx/taste-skill --skill stitch-design-taste` |
| design-an-interface                  | 生成多个差异明显的界面设计方向，而不是只给一个常规方案。适合早期探索、比稿、找视觉方向。                                                                                         | `npx skills add https://github.com/mattpocock/skills --skill design-an-interface`    |

### 前端动效

| Skill | 用途 | 安装方式 |
| --- | --- | --- |
| gsap | 前端动效技能集。用于 GSAP 动画、timeline、ScrollTrigger、插件、React / Vue / Svelte 集成和性能优化。 | `npx skills add https://github.com/greensock/gsap-skills` |
| DavidHDev/react-bits | React 动效组件库技能。适合快速找到、生成或接入 React 动画组件，让页面更有视觉表现力。 | `npx skills add DavidHDev/react-bits` |

### Vue / 组件 / 最佳实践

| Skill | 用途 | 安装方式 |
| --- | --- | --- |
| shadcn-vue | 管理 shadcn-vue 组件和项目。适合 Vue 项目里添加组件、配置主题、处理组件依赖和遵循 shadcn-vue 实践。 | `npx skills add unovue/shadcn-vue` |
| vue-best-practices | Vue.js 最佳实践。适合组件设计、组合式 API、状态管理、项目结构、性能和可维护性建议。 | `npx skills add https://github.com/antfu/skills --skill vue-best-practices` |
| vue-router-best-practices | Vue Router 4 最佳实践。适合处理路由结构、导航守卫、嵌套路由、懒加载、路由元信息和权限控制。 | `npx skills add https://github.com/antfu/skills --skill vue-router-best-practices` |
| vue-testing-best-practices | Vue 测试最佳实践。适合 Vitest、Vue Test Utils、组件测试、组合式函数测试和测试策略设计。 | `npx skills add https://github.com/antfu/skills --skill vue-testing-best-practices` |

### 内容创作

| Skill | 用途 | 安装方式 |
| --- | --- | --- |
| dashiai-ppt | PPT / 演示文稿生成技能，适合从主题、提纲或素材生成幻灯片结构、内容和设计建议。 | `npx skills add https://github.com/chuspeeism/dashiAI-ppt-skill --skill dashiai-ppt` |
| gzh-design-skill | 公众号文章生成与设计排版技能，适合把文章内容整理成更适合公众号阅读的结构、标题、排版和视觉风格。 | `npx skills add https://github.com/isjiamu/gzh-design-skill` |
| fpv-immersive-video-prompting | 根据简短提示词生成完整的第一人称沉浸式视频提示词。适合 image-to-video、套图生成、Seedance 等视频模型场景。 | 未记录。 |
| ponyo-cover-anchor-system | 小红书 / 公众号封面图设计系统。核心是“强钩子 + 视觉锚点 + 高信息密度”，用于生成更容易被点击的封面方案。 | `npx skills add https://github.com/ponyodong2026/ponyo-cover-anchor-system.git` |
| Humanizer-zh | 中文文本润色和去 AI 味。适合把生硬、模板化、AI 感强的文字改得更自然、更有人味。 | `npx skills add https://github.com/op7418/Humanizer-zh.git` |

### Obsidian / 知识库

| Skill | 用途 | 安装方式 |
| --- | --- | --- |
| obsidian-skills | Obsidian 相关技能包，用于 Markdown 笔记整理、知识库结构、模板、链接、标签、Dataview 等工作流。 | `npx skills add https://github.com/kepano/obsidian-skills` |

## MCP

| 名称             | 用途                                                                                          | 安装方式                                                                                                                                                                                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shadcn-vue MCP | 把 shadcn-vue 接入 MCP，让 Agent 可以更方便地读取、安装或管理 shadcn-vue 组件。                                   | `npx shadcn-vue@latest mcp init --client codex`<br>`npx shadcn-vue@latest mcp init --client opencode`<br>`npx shadcn-vue@latest mcp init --client cursor`<br>`npx shadcn-vue@latest mcp init --client vscode`<br>`npx shadcn-vue@latest mcp init --client claude` |
| context7       | 查询库、框架、SDK、API、CLI、云服务的最新文档。适合确认最新 API、配置方式、版本迁移和官方示例。                                      | `npx ctx7 setup`，安装时选择对应的 Agent 工具。                                                                                                                                                                                                                               |
| figma          | 连接 Figma，让 Agent 能读取设计稿、理解组件 / 布局 / 样式，辅助设计生成、设计到代码或设计审查。MCP 地址：`https://mcp.figma.com/mcp` | `claude plugin install figma@claude-plugins-official`                                                                                                                                                                                                             |
| cowart 无限画布    | 无限画布类 Codex 插件。适合画布式创作、空间化组织信息、可视化草图或设计探索。                                                  | `git clone https://github.com/zhongerxin/cowart.git ~/plugins/cowart` → 确认 `~/plugins/cowart/.codex-plugin/plugin.json` 存在 → `codex plugin marketplace add ~` → `codex plugin add cowart@personal`                                                                |

## 参考

| 名称                                                                            | 用途                      |
| ----------------------------------------------------------------------------- | ----------------------- |
| [awesome-design-md](https://github.com/VoltAgent/awesome-design-md/tree/main) | 收集 DESIGN.md 写法和设计文档参考。 |
