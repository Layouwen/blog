---
title: ollama 模型推荐
date: 2026-05-25T18:22:12.000Z
tags:
  - 博客
  - ai
  - ollama
categories:
  - 博客
  - 工具
uuid: 66306640-eca3-4fb5-b392-b13329c4a41d
---
> 本文章由 DeepSeek 生成

# Ollama 本地模型推荐（2026）

| 模型名称             | 主要领域                | 模型特点                | 推荐配置                  | 最低可用配置         |
| ---------------- | ------------------- | ------------------- | --------------------- | -------------- |
| qwen3.6          | 全能主力 / 日常开发         | 综合能力最均衡，适合作为默认主力模型  | 24GB+ VRAM / 64GB RAM | 16GB RAM（8B量化） |
| qwen3:8b         | 小显存万能模型             | 小模型里综合能力极强，适合轻量本地AI | 8GB VRAM / 16GB RAM   | 16GB RAM CPU运行 |
| kimi-k2.6        | Agent Coding / 自动开发 | 多文件工程、重构、工具调用极强     | 48GB+ VRAM / 多卡       | 云端更现实          |
| deepseek-coder   | 代码生成 / 补全           | 写函数、补全、算法能力强        | 16GB VRAM             | 8GB VRAM       |
| deepseek-r1      | 推理 / 数学 / 面试        | reasoning 极强，适合复杂分析 | 24GB+ VRAM            | 16GB RAM       |
| glm-5.1          | 中文工程 / 企业开发         | 中文理解优秀，偏工程师风格       | 24GB VRAM             | 16GB RAM       |
| mistral-small    | RAG / 知识库           | 幻觉低，适合文档问答          | 16GB RAM              | 8GB VRAM       |
| ministral        | 本地知识助手              | 轻量RAG、低资源部署优秀       | 8GB VRAM              | 16GB RAM CPU运行 |
| qwen-vl          | OCR / 多模态 / UI分析    | 看图、识别界面、OCR 很强      | 24GB VRAM             | 16GB VRAM      |
| qwen3-tts        | 语音 / TTS            | 中文语音生成效果优秀          | 16GB RAM              | 8GB RAM        |
| gemma4           | 轻量通用聊天              | Google系小模型，适合轻量聊天   | 8GB VRAM              | 16GB RAM       |
| gemma4:31b       | 中大型通用模型             | 通用能力不错，但非coding特化   | 24GB+ VRAM            | 64GB RAM CPU运行 |
| nemotron-3-super | 推理实验 / 结构化输出        | NVIDIA系，适合研究用途      | 24GB+ VRAM            | 不推荐低配运行        |
| llama3.3         | 通用英文助手              | 英文生态成熟，兼容性优秀        | 16GB VRAM             | 8GB VRAM       |
| phi4             | 超轻量办公助手             | 微软系，小模型能力优秀         | 8GB RAM               | 4GB RAM        |
| codestral        | 专业代码模型              | Mistral系 coding 特化  | 16GB VRAM             | 8GB VRAM       |
| command-r        | 长上下文 / RAG          | 长文档与检索能力优秀          | 24GB VRAM             | 64GB RAM CPU运行 |
| yi-34b           | 中文长文本               | 中文长文、总结能力不错         | 24GB VRAM             | 64GB RAM CPU运行 |

---

# 推荐搭配方案

## 1. 普通开发者（最推荐）

| 用途 | 模型 |
|---|---|
| 主力日常 | qwen3.6 |
| 推理分析 | deepseek-r1 |
| 代码补全 | deepseek-coder |

---

## 2. AI Agent 开发

| 用途 | 模型 |
|---|---|
| Agent主力 | kimi-k2.6 |
| 日常辅助 | qwen3.6 |
| OCR/UI分析 | qwen-vl |

---

## 3. 低配置笔记本

| 配置 | 推荐模型 |
|---|---|
| 16GB RAM | qwen3:8b |
| 8GB RAM | phi4 |
| CPU-only | ministral |

---

# 当前 Ollama 最值得长期保留的模型（个人向）

| 类型 | 推荐 |
|---|---|
| 默认万能模型 | qwen3.6 |
| 最强 Agent | kimi-k2.6 |
| 最强推理 | deepseek-r1 |
| 最强补全 | deepseek-coder |
| 最强中文 | glm-5.1 |
| 最佳轻量模型 | qwen3:8b |
| 最佳视觉模型 | qwen-vl |

---

# 显存经验（非常重要）

| 模型规模 | 实际体验 |
|---|---|
| 7B~8B | 轻量可用 |
| 14B | 开始进入“能干活” |
| 27B~35B | 当前最佳甜点区 |
| 70B+ | 需要高端显卡 |
| 200B+ | 基本属于服务器领域 |

---

# Ollama 常用命令

```bash
# 查看模型
ollama list

# 拉取模型
ollama pull qwen3:8b

# 运行模型
ollama run qwen3:8b

# 删除模型
ollama rm qwen3:8b
