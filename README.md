# prompt-lint

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](#)
[![skills.sh](https://skills.sh/b/RealFelixWang/prompt-lint)](https://skills.sh/RealFelixWang/prompt-lint)

一个基于 Claude Skill 的 Prompt 质量审计工具。通过 **55+ 条硬规则扫描** 和 **8 维度 LLM 深度分析**，在你花真金白银使用 Prompt 之前，先帮你看清它的问题。

---

## 🔥 这个工具解决什么问题

你在 AI 工具上浪费的每一笔钱，有一部分是因为 Prompt 写得太烂。

一个模糊的 Prompt 可能让模型跑偏 5 轮对话、输出一堆废话、最终还得你自己从头来。更糟糕的是，你可能根本不知道问题出在哪——你以为是模型不行，其实是你的 Prompt 不行。

**prompt-lint 的解决方案：** 在你把 Prompt 丢给模型之前，先做一次质量审计。像代码 lint 一样，它会告诉你：

- 哪些地方写得模糊，模型会猜错
- 哪些地方缺上下文，模型会瞎编
- 哪些地方有安全隐患，真实密钥可能泄露
- 哪些地方浪费 token，效率可以翻倍

**省下来的是钱和时间，省不掉的是你的意图。**

---

## 🎯 评估维度

prompt-lint 从 8 个维度对 Prompt 进行评分（每个维度 1-10 分）：

| 维度 | 英文名 | 权重 | 说明 |
|------|--------|------|------|
| 目标工具匹配度 | Tool Fit | 15% | Prompt 是否针对目标工具（Claude/GPT/Gemini 等）的特性进行了适配 |
| 意图清晰度 | Intent Clarity | 20% | 核心意图是否明确、无歧义，动词是否精确 |
| Token 效率 | Token Efficiency | 10% | 是否以最小 token 消耗传达最大信息量 |
| 上下文完整性 | Context Completeness | 15% | 是否提供了模型完成任务所需的全部背景信息 |
| 格式规范 | Format Specification | 10% | 输出结构、格式和约束条件是否明确 |
| 安全检查 | Security | 15% | 是否存在敏感信息泄露风险（API Key、密码等） |
| 错误模式检测 | Error Patterns | 10% | 是否命中已知的常见错误模式（模糊动词、双重任务等） |
| 中文语法质量 | Chinese Quality | 5% | 中文表达质量、语法正确性、专业术语规范性 |

> 综合评分 = Σ(维度分数 × 权重)，并受附加规则约束（如安全检查 = 1 分时直接判定失败）。

---

## 🚀 安装

### 方式一：skills.sh（推荐）

一行命令安装到所有支持的 AI Agent：

```bash
npx skills add RealFelixWang/prompt-lint
```

支持 Claude Code、Cursor、Windsurf、Codex、GitHub Copilot 等 20+ Agent。

### 方式二：Claude.ai

1. 在 GitHub 上下载 prompt-lint 的 ZIP 包
2. 打开 Claude.ai → 设置 → Skills
3. 上传 ZIP 包即可使用

### 方式三：Claude Code（手动）

```bash
git clone https://github.com/RealFelixWang/prompt-lint.git ~/.claude/skills/prompt-lint
```

安装后，在 Claude Code 中即可使用 `/prompt-lint` 命令。

---

## 📖 使用方式

### 1. 评估一个 Prompt

直接粘贴 Prompt 并要求评估：

```
请评估这个 Prompt：
"""
帮我写个接口，要快一点，用那个框架。
"""
```

### 2. 指定目标工具

明确告诉 lint 你要用的工具，评估会更精准：

```
评估这个 Prompt，目标工具是 Claude Code：
"""
分析 src/ 目录下的代码，找出潜在的性能问题，输出 Markdown 报告。
"""
```

### 3. 分析已有的 Prompt

如果你有一段已经写好的 Prompt，想看看质量如何：

```
帮我检查这个 Prompt 的质量：
"""
你是一位资深工程师。请帮我重构 UserService 类，把数据库查询从 N+1 优化为批量查询，使用 TypeORM，只修改 src/services/user.service.ts。
"""
```

### 4. JSON 输出

使用 `--json` 标志获取结构化输出，适合集成到 CI/CD 流程：

```
评估这个 Prompt，输出 JSON 格式：
"""
...
"""
```

---

## 📊 输出示例

### Markdown 报告

```markdown
# Prompt 质量评估报告

## 雷达图

          目标工具匹配度 (8)
                 │
                 │
    中文语法 ────┼──── 意图清晰度
     (7)        │         (9)
                 │
    错误模式 ────┼──── Token 效率
     (6)        │         (8)
                 │
    安全检查 ────┼──── 上下文完整性
     (10)       │         (7)
                 │
          格式规范 (6)

## 评分明细表

| 维度 | 分数 | 说明 |
|------|------|------|
| 目标工具匹配度 | 8/10 | 明确指定了 Claude Code，利用了文件路径能力 |
| 意图清晰度 | 9/10 | 核心动词"重构"精确，有明确的优化目标 |
| Token 效率 | 8/10 | 整体紧凑，无冗余填充词 |
| 上下文完整性 | 7/10 | 有项目路径和技术栈，但缺少当前状态描述 |
| 格式规范 | 6/10 | 指定了文件路径，但未明确输出格式 |
| 安全检查 | 10/10 | 无敏感信息泄露 |
| 错误模式检测 | 6/10 | 命中 1 条 info 级别规则 |
| 中文语法质量 | 7/10 | 表达清晰，术语规范 |

## 问题清单

- 🟡 警告：未指定输出格式（建议添加"以 Markdown 输出"）
- 🔵 建议：可添加成功标准（如"修改后单测通过"）
- 🔵 建议：可添加负面提示（如"不要修改其他文件"）

## 优化建议

优化后的 Prompt：
"""
你是一位资深 TypeScript 工程师，精通 TypeORM。

任务：重构 src/services/user.service.ts 中的 UserService 类。

目标：将 N+1 数据库查询优化为批量查询。

约束：
- 仅修改 user.service.ts，不涉及其他文件
- 使用 TypeORM 的 QueryBuilder
- 保持现有 API 接口不变

输出：修改后的完整代码 + 变更说明。

成功标准：修改后单测全部通过。
"""
```

---

## 🏗️ 架构

prompt-lint 采用**双层评估架构**：

```
┌─────────────────────────────────────────┐
│           第一层：硬规则扫描              │
│   55+ 条确定性规则（正则 + 关键词匹配）    │
│   SEC(5) + EFF(6) + ERR(35) + FMT(5) + SCO(4) │
├─────────────────────────────────────────┤
│           第二层：LLM 深度分析            │
│   8 维度语义评分（1-10 分）               │
│   工具适配评估 + 综合加权计算             │
└─────────────────────────────────────────┘
```

**第一层（硬规则）** 是确定性的，像 ESLint 一样逐条扫描：
- `SEC` — 敏感信息泄露检测（API Key、Bearer Token、密码等）
- `EFF` — Token 效率（填充词、冗余表达、过度礼貌）
- `ERR` — 常见错误模式（模糊动词、双重任务、无成功标准等）
- `FMT` — 格式检查（无目标工具、无输出格式、Prompt 过长）
- `SCO` — 范围检查（无文件路径、无停止条件、无禁止操作）

**第二层（LLM 分析）** 是语义级的，由 LLM 对 8 个维度逐项打分：
- 每个维度独立评分，最终加权得出综合分数
- 综合分数受附加规则约束（如安全维度 = 1 分直接判定失败）
- 生成 ASCII 雷达图可视化

两层结果合并后，输出包含：雷达图 + 评分明细表 + 问题清单 + 优化建议。

---

## 🔄 与 prompt-master 的关系

prompt-lint 是 prompt-master 的补充工具，而非替代品。

| 对比项 | prompt-master | prompt-lint |
|--------|---------------|-------------|
| 定位 | Prompt 生成 + 优化 | Prompt 质量审计 |
| 核心能力 | 从需求生成高质量 Prompt | 评估已有 Prompt 的质量 |
| 使用时机 | 从零写 Prompt 时 | 写完 Prompt 后、使用前 |
| 输出 | 优化后的 Prompt 文本 | 质量评估报告 + 改进建议 |
| 类比 | 代码生成器 | 代码 Linter |
| 规则层 | 无 | 55+ 条硬规则 + 8 维度评分 |

**最佳实践：** 先用 prompt-master 生成 Prompt，再用 prompt-lint 做质量检查，双重保障。

---

## 📝 版本历史

### 1.0.0（2025-06）

初始发布：

- 55+ 条硬规则扫描（SEC/EFF/ERR/FMT/SCO 五大类）
- 8 维度 LLM 深度分析
- ASCII 雷达图可视化
- Markdown 和 JSON 双格式输出
- 支持 Claude Code 和 Claude.ai 两种安装方式
- 内置 12 个工具适配检查清单（Claude/GPT/Gemini/Copilot 等）

---

## 📄 License

[MIT](LICENSE)
