---
title: AI 能写代码了，为什么我反而开始要求它先写文档？
uuid: bd7eb227-7e56-49ed-b19b-905018025a99
date: 2026-06-18
tags:
  - 博客
  - spec-kit
categories:
  - 博客
---
![image.png](https://upyun-assets.4van.top/avan/20260619020755111.png)

最近在尝试用 AI 参与项目开发。

刚开始我的方式很简单：

```text
提需求
↓
让 AI 直接实现
↓
不断返工
↓
继续补需求
```

结果非常熟悉：

- 功能能跑
- 代码越来越多
- 需求越来越乱
- AI 上下文越来越长
- 后面谁都不敢接手

尤其是涉及：

- 前后端联动
- 权限体系
- 数据结构变更
- API 契约
- 多阶段迭代

时，问题会迅速放大。

后来我接触到了 GitHub 开源的 Spec Kit。

它让我第一次把 AI 开发从：

```text
直接写代码
```

变成：

```text
先规格
↓
再设计
↓
再拆任务
↓
最后实现
```

整个过程开始变得可控。

---

# Spec Kit 到底是什么？

如果用一句话概括：

> Spec Kit = 把一次开发过程工程化。

它不会让 AI 直接开始改代码。

而是要求先产出一套完整工程文档。

例如：

```text
constitution
spec
plan
tasks
checklists
contracts
research
```

最终形成类似结构：

```text
specs/
 ├── spec.md
 ├── plan.md
 ├── tasks.md
 ├── research.md
 ├── data-model.md
 ├── quickstart.md
 ├── contracts/
 └── checklists/
```

看起来像是多写了一堆 Markdown。

但实际作用是：

> 把需求从聊天记录里解放出来。

让需求、设计、任务、实现都能够被追踪、被审查、被交接。

---

# 为什么 AI 项目越来越需要 Spec？

以前开发靠的是：

```text
需求文档
↓
开发
↓
测试
```

现在很多团队变成：

```text
需求
↓
AI
↓
代码
```

中间设计环节直接没了。

于是会出现几个经典问题。

---

## 问题 1：需求隐藏在聊天记录里

AI 写了很多代码。

一个月后再看：

```text
为什么这样设计？
```

没人知道。

因为答案在某次聊天记录里。

---

## 问题 2：实现范围不断膨胀

最开始：

```text
做一个文章管理
```

后来：

```text
加权限
加审核
加草稿
加发布
加版本
```

最后：

```text
已经完全不是最初那个需求
```

但没人知道边界在哪里。

---

## 问题 3：Agent 交接困难

A Agent 做了一半。

换 B Agent。

B Agent：

```text
重新理解项目
重新分析代码
重新推导需求
```

大量时间浪费在上下文恢复。

---

# Spec Kit 最有价值的其实不是 Spec

很多人第一次接触会觉得：

```text
不就是生成几个 md 文件吗？
```

实际上真正有价值的是：

## Clarify

也就是需求澄清。

这是我实践下来收益最大的环节。

例如：

```text
删除是软删除还是硬删除？
```

看起来简单。

但会影响：

- 数据库设计
- API 设计
- 权限体系
- 审计记录
- 自动化测试

---

又比如：

```text
上线策略是什么？
```

是：

```text
全部完成后开放
```

还是：

```text
灰度发布
```

会直接影响：

- 发布方案
- 数据迁移方案
- 风险控制方案

这些问题如果在代码完成后才发现。

返工成本非常高。

---

# 我目前的使用流程

经过几轮实践后。

我基本固定成下面这套流程：

```text
Constitution
↓
Specify
↓
Clarify
↓
Checklist
↓
Plan
↓
Tasks
↓
Analyze
↓
Implement
```

对应就是：

```text
项目原则
↓
需求规格
↓
需求澄清
↓
质量检查
↓
技术设计
↓
任务拆解
↓
一致性分析
↓
代码实现
```

整个过程更像：

```text
产品经理
↓
架构师
↓
Tech Lead
↓
开发
```

而不是：

```text
需求
↓
AI
↓
代码
```

---

# 一套我实际在用的工作流

对于正式功能开发。

我基本都会按照下面的顺序推进：

```text
1. Constitution
建立项目级原则

2. Specify
生成功能规格

3. Clarify
澄清需求边界

4. Checklist
检查需求质量

5. Plan
生成技术方案

6. Tasks
拆分可执行任务

7. Analyze
检查文档一致性

8. Implement
按任务实现代码
```

如果是实验性功能。

我会简化成：

```text
Specify
↓
Plan
↓
Tasks
↓
Implement
```

如果涉及：

- 数据库
- 权限
- API
- 发布策略

那么 Clarify、Checklist、Analyze 基本不会跳过。

---

# 如果你想体验 Spec Kit

实际上上手成本并不高。

首先安装 uv：

```bash
brew install uv
```

然后安装 Spec Kit：

```bash
uv tool install specify-cli \
  --from git+https://github.com/github/spec-kit.git
```

安装完成后验证：

```bash
specify version
```

---

进入项目目录：

```bash
cd your-project
```

初始化当前项目：

```bash
specify init --here \
  --integration codex \
  --integration-options="--skills"
```

或者：

```bash
uvx --from git+https://github.com/github/spec-kit.git \
  specify init .
```

初始化完成后。

项目里会出现类似结构：

```text
.agents/
  skills/
    speckit-constitution/
    speckit-specify/
    speckit-clarify/
    speckit-plan/
    speckit-tasks/
    speckit-implement/

.specify/
  templates/
  memory/

specs/
```

后续所有需求都会围绕这些文档进行演进。

---

如果你的 Agent 没有自动识别这些 Skill。

可以主动调用：

```text
[$speckit-constitution]
[$speckit-specify]
[$speckit-clarify]
[$speckit-checklist]
[$speckit-plan]
[$speckit-tasks]
[$speckit-analyze]
[$speckit-implement]
```

我自己的实践里。

并不是通过一个统一命令完成所有事情。

而是把它理解成：

```text
一套 AI 软件工程工作流
```

然后按阶段调用。

---

# 我认为最应该写好的两个文件

如果时间有限。

不要追求把所有文档都写到极致。

优先保证：

## 1. Constitution

项目宪章。

例如：

```text
禁止随意增加依赖
优先复用现有组件
必须通过 TS 检查
核心逻辑必须有测试
API 必须考虑兼容
```

这决定了 AI 后面会怎么做事。

---

## 2. Plan

技术计划。

因为后续 Agent 最常看的其实不是 Spec。

而是：

```text
plan.md
```

这里会记录：

- 技术方案
- 数据模型
- API 设计
- 测试策略
- 发布策略

很多时候看 Plan 就能快速恢复上下文。

---

# Spec Kit 不适合所有项目

如果只是：

```text
改个按钮颜色
修一个 Bug
写一个脚本
```

直接让 AI 改更快。

Spec Kit 更适合：

✅ 中大型功能

✅ 前后端联动

✅ 数据结构变更

✅ 权限体系

✅ 多 Agent 协作

✅ 长周期迭代

这种场景收益会非常明显。

---

# 参考资料

GitHub Spec Kit

https://github.com/github/spec-kit

官方文档

https://github.github.com/spec-kit/

Installation Guide

https://github.github.com/spec-kit/installation.html

Quick Start Guide

https://github.github.com/spec-kit/quickstart.html

GitHub 官方介绍

https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/

---

# 最后的感受

用了几轮之后。

我最大的变化不是：

```text
AI 写代码更快了
```

而是：

```text
AI 开发终于开始像工程开发了
```

Spec Kit 的核心从来不是生成多少 Markdown。

而是强迫你在实现之前回答清楚：

- 要解决什么问题
- 用户到底是谁
- 验收标准是什么
- 数据边界是什么
- 权限边界是什么
- 如何测试
- 如何发布
- 什么才算真正完成

对于复杂项目来说。

这些问题想清楚之后。

代码反而成了最简单的部分。

我觉得 Spec Kit 真正解决的不是 AI 写代码的问题。

而是 AI 项目失控的问题。

当功能开始涉及多人协作、长期迭代、数据模型、权限体系和发布策略时。

代码往往不是最难的部分。

真正难的是：

> 如何让所有人、所有 Agent，对同一件事保持一致理解。

而 Spec Kit 本质上是在给 AI 开发补回软件工程里曾经被省略掉的那一层。