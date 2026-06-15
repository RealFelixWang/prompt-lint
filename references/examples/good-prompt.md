# 优秀 Prompt 示例

> 本文件展示高质量 prompt 的范例，帮助用户理解各维度的最佳实践。

---

## 示例 1：Cursor 代码重构（评分 8.5/10）

### 用户原始输入

> 帮我重构 Cursor 中的认证模块

### 优化后的 Prompt

```
在 Cursor 中重构项目 `my-app` 的认证模块。

【目标工具】Cursor（使用 Composer 功能进行多文件编辑）

【当前状态】
- 认证逻辑散落在 `src/utils/auth.ts`、`src/components/LoginForm.tsx`、`src/api/user.ts` 三个文件中
- 使用硬编码的 JWT secret
- 无 token 刷新机制
- 无权限角色管理

【目标状态】
- 认证逻辑集中到 `src/modules/auth/` 目录
- 实现统一的 `authService.ts` 服务层
- 添加 token 自动刷新（过期前 5 分钟触发）
- 支持 admin/user/guest 三种角色的权限校验

【约束】
- 技术栈：React 18 + TypeScript 5.0 + Zustand 状态管理
- 不修改 `src/api/user.ts` 的接口签名（保持向后兼容）
- 不修改 `tests/` 目录下的现有测试文件
- 使用现有的 `@my-app/utils/crypto` 工具函数，不引入新的加密库
- 保持现有的错误提示文案不变

【完成条件】
- 所有现有测试通过
- 新增 `src/modules/auth/authService.ts` 和 `src/modules/auth/types.ts`
- `LoginForm.tsx` 调用方式改为 `authService.login()`
- 修改后请列出改动文件清单并确认无遗漏
```

### 评分诊断

| 维度 | 分数 | 说明 |
|------|------|------|
| 目标工具匹配度 | 9 | 明确指定 Cursor + Composer；利用多文件编辑能力 |
| 意图清晰度 | 9 | 核心任务"重构认证模块"一句话清晰；目标状态明确 |
| Token 效率 | 8 | 结构紧凑，信息密度高；少量冗余可精简 |
| 上下文完整性 | 9 | 包含项目名称、技术栈、文件路径、起始/目标状态 |
| 格式规范 | 8 | 使用结构化标签；有完成条件；缺少示例模板 |
| 安全检查 | 9 | 未泄露任何敏感信息 |
| 错误模式检测 | 8 | 零 critical；有明确边界、禁止清单、审核节点 |
| 中文语法质量 | 8 | 语法正确，术语规范；可进一步精炼 |

**加权总分：8.5 / 10（良好）**

**亮点分析：**
- 使用结构化标签（目标工具/当前状态/目标状态/约束/完成条件）组织信息
- 明确的"不要动"约束保护了测试文件和接口签名
- 完成条件具体可验证（"所有测试通过"、"列出改动文件清单"）

---

## 示例 2：Claude Code 功能开发（评分 8.0/10）

### 用户原始输入

> 用 Claude Code 给我的待办应用添加标签功能

### 优化后的 Prompt

```xml
<task>
  在待办应用中实现标签（Tag）功能，支持为每个待办项添加多个标签。
</task>

<tool>
  Claude Code（使用 AGENTS.md 中定义的项目约定）
</tool>

<starting_state>
- 项目名：todo-app
- 技术栈：Next.js 14 + Prisma ORM + PostgreSQL
- 现有模型：`prisma/schema.prisma` 中有 `Todo` 模型，字段：id, title, done, createdAt
- 无标签相关功能
</starting_state>

<target_state>
- 新增 `Tag` 模型：id, name, color
- 新增 `TodoTag` 关联表（多对多）
- Todo 详情页显示标签，支持按标签筛选
- API 支持批量添加/移除标签
</target_state>

<constraints>
- 使用 Prisma 的显式多对多关系（非隐式），以便未来扩展关联属性
- 标签名称唯一（同一用户不能创建重名标签）
- 颜色值使用 HEX 格式（如 #FF5733），不要使用颜色名称
- 不修改现有的 Todo CRUD API 的请求/响应格式
- 前端使用 Tailwind CSS，标签颜色作为背景色
</constraints>

<stop_conditions>
- 当 Prisma migration 执行成功且 `npx prisma generate` 无报错时
- 当新增的单元测试全部通过时（`npm test -- --filter tag`）
- 当现有的 Todo 相关测试不受影响时（`npm test -- --filter todo`）
</stop_conditions>

<review_triggers>
- 修改 schema.prisma 后，先执行 `npx prisma migrate diff` 确认变更符合预期
- 修改 API 路由后，手动测试 POST /api/todos/[id]/tags 接口
- 完成后请列出所有新增/修改的文件，并说明每个文件的改动内容
</review_triggers>
```

### 评分诊断

| 维度 | 分数 | 说明 |
|------|------|------|
| 目标工具匹配度 | 8 | 指定 Claude Code；引用 AGENTS.md 约定；未深度利用工具特性 |
| 意图清晰度 | 8 | 核心任务明确；动词精确（"实现"、"支持"、"筛选"）；少量范围模糊 |
| Token 效率 | 7 | XML 结构清晰但较冗长；标签嵌套增加了 token 消耗 |
| 上下文完整性 | 9 | 项目名称、技术栈、现有模型结构、起始/目标状态完整 |
| 格式规范 | 7 | 使用 XML 标签结构化；有完成条件；缺少输出示例模板 |
| 安全检查 | 9 | 未泄露任何敏感信息 |
| 错误模式检测 | 8 | 有明确的停止条件和审核触发器；零 critical |
| 中文语法质量 | 8 | 语法正确，专业术语规范 |

**加权总分：8.0 / 10（良好）**

**亮点分析：**
- 使用 XML 标签清晰分隔各信息块（task/tool/starting_state/target_state/constraints/stop_conditions/review_triggers）
- 技术决策有理由（"使用显式多对多关系，以便未来扩展关联属性"）
- 停止条件具体可执行（migration 成功、测试通过）
- 审核触发器明确（先 diff、手动测试、列出文件清单）

---

## 评分对比总结

| 示例 | 目标工具 | 意图 | Token | 上下文 | 格式 | 安全 | 错误 | 中文 | 总分 |
|------|----------|------|-------|--------|------|------|------|------|------|
| 示例 1 | 9 | 9 | 8 | 9 | 8 | 9 | 8 | 8 | **8.5** |
| 示例 2 | 8 | 8 | 7 | 9 | 7 | 9 | 8 | 8 | **8.0** |

**共同优点：**
1. 意图一句话可理解，无歧义
2. 上下文完整：项目名称 + 技术栈 + 文件路径 + 起始/目标状态
3. 有明确的完成条件和验证方式
4. 有边界声明和禁止清单，避免模型越界
5. 零安全风险，无敏感信息泄露
