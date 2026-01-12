# Skills 模板集合

Claude Code Agent Skills 模板项目，包含 PRD 全流程和通用工具 skills。

## 语言规则

所有 skill 统一遵循：
- 支持中英文提问
- 统一中文回复
- 使用中文生成文档

---

## Skills 概览

| Skill | 命令 | 用途 | 输出文件 |
|-------|------|------|----------|
| [需求扩写](#1-prd-requirements-需求扩写) | `/prd-requirements` | 将需求扩展为详细 PRD | `01-requirements.md` |
| [系统设计](#2-prd-system-design-系统设计) | `/prd-system-design` | 技术架构和 API 设计 | `02-system-design*.md` |
| [测试方案](#3-prd-test-plan-测试方案) | `/prd-test-plan` | 单元/E2E/手动测试用例 | `03-test-*.md` |
| [开发任务](#4-prd-dev-tasks-开发任务) | `/prd-dev-tasks` | 可执行的开发任务拆分 | `04-dev-tasks*.md` |
| [代码提交](#5-git-commit-代码提交) | `/git-commit` | Conventional Commits 规范提交 | - |

## PRD 工作流

```
/prd-requirements → /prd-system-design → /prd-test-plan → /prd-dev-tasks
       │                   │                   │                 │
       ▼                   ▼                   ▼                 ▼
  需求文档             系统设计            测试方案          开发任务
```

---

# 1. prd-requirements (需求扩写)

将用户简短需求扩展为详细的产品需求文档。

## 元数据

```yaml
name: prd-requirements
description: Expand user requirements into detailed PRD documents
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion
```

## 触发条件

- 用户提供功能需求或想法
- 用户要求创建/编写 PRD
- 用户想要澄清或记录需求

## 工作流程

1. **理解需求**：读取用户输入
2. **探索代码库**：了解现有架构（如适用）
3. **起草需求**：创建完整需求文档
4. **用户确认**：获得用户批准

## 输出文件

```
docs/prd/
├── 01-requirements.md           # 主文档
├── 01-requirements-stories.md   # 用户故事（如超长）
└── 01-requirements-nfr.md       # 非功能性需求（如超长）
```

## 文档模板

```markdown
# 需求文档：<功能名称>

## 1. 需求背景
<为什么需要这个功能，解决什么问题>

## 2. 目标用户
<谁会使用这个功能，用户画像>

## 3. 核心功能点
- [ ] 功能点 1：<描述>
- [ ] 功能点 2：<描述>

## 4. 用户故事

| 编号 | 角色 | 期望 | 目的 |
|------|------|------|------|
| US-01 | 作为<角色> | 我希望<功能> | 以便于<价值> |

## 5. 验收标准
- [ ] AC-01：<可量化的标准>
- [ ] AC-02：<可量化的标准>
- [ ] AC-03：<可量化的标准>

## 6. 非功能性需求
- **性能**：<要求>
- **安全**：<要求>
- **兼容性**：<要求>

## 7. 范围边界

### 本次包含
- <内容>

### 本次不包含
- <内容>

## 8. 假设与依赖
- <前提条件>
```

## 约束

- [ ] 必须与用户确认核心功能点是否完整
- [ ] 必须定义至少 3 条验收标准
- [ ] 必须列出范围边界，避免需求蔓延
- [ ] 不得添加用户未提及且未确认的大功能
- [ ] 用户故事必须遵循 "作为...我希望...以便于..." 格式
- [ ] 验收标准必须可量化、可验证

## 下一步

完成后建议运行 `/prd-system-design`

---

# 2. prd-system-design (系统设计)

根据需求文档创建系统设计方案。

## 元数据

```yaml
name: prd-system-design
description: Create system design documents based on requirements
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion
```

## 前置条件

- 需求文档：`docs/prd/01-requirements.md`
- 如不存在，建议先运行 `/prd-requirements`

## 设计前必问

**必须使用 AskUserQuestion 询问用户**：

1. **技术栈偏好**
   - 是否有偏好的技术栈？
   - 选项：指定技术栈 / 无偏好（将根据需求推荐）

2. **目标平台**
   - 目标运行平台是什么？
   - 选项：Web / Mobile / Desktop / Server / 跨平台

3. **部署环境**
   - 部署在哪里？
   - 选项：云服务 / 私有化 / 混合

如用户无偏好，则根据需求设计最优方案。

## 输出文件

```
docs/prd/
├── 02-system-design.md          # 架构概览和技术选型
├── 02-system-design-api.md      # API 设计（如超长）
└── 02-system-design-data.md     # 数据模型（如超长）
```

## 文档结构

1. **运行平台** - 平台、版本要求、部署环境
2. **架构概览** - 高层架构图（ASCII）
3. **技术选型** - 技术选择及理由
4. **模块划分** - 模块职责和依赖
5. **数据模型** - 实体定义和关系
6. **API 设计** - 接口定义含请求/响应示例
7. **状态流转** - 关键业务状态机
8. **异常处理** - 错误码和处理策略
9. **扩展性考虑** - 未来扩展点

## 约束

- [ ] **必须先询问用户技术栈偏好和目标平台**
- [ ] 技术选型必须说明理由
- [ ] 若用户无偏好则选择最适合需求的方案
- [ ] 必须明确运行平台和最低版本要求
- [ ] API 设计必须包含请求/响应示例
- [ ] 数据模型必须考虑索引和查询场景
- [ ] 必须识别与现有系统的集成点
- [ ] 设计复杂度应与需求规模匹配，避免过度设计
- [ ] 优先使用项目已有技术栈

## 下一步

完成后建议运行 `/prd-test-plan`

---

# 3. prd-test-plan (测试方案)

创建完整的测试方案，包含单元测试、UI 自动化、手动测试和上线回归。

## 元数据

```yaml
name: prd-test-plan
description: Create comprehensive test plans including unit tests, UI automation, and manual test cases
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion
```

## 前置条件

- 需求文档：`docs/prd/01-requirements.md`
- 系统设计：`docs/prd/02-system-design.md`

## 测试策略

| 测试类型 | 覆盖范围 | 覆盖要求 |
|----------|----------|----------|
| 单元测试 | 核心业务逻辑 | 行覆盖率 ≥ 80%，分支覆盖率 ≥ 80% |
| UI 自动化 | 核心用户场景 | 覆盖所有 P0 场景 |
| 手动测试 | 全部场景 | 覆盖所有验收标准 |

## 输出文件

```
docs/prd/
├── 03-test-plan.md          # 测试策略概览和覆盖矩阵
├── 03-test-unit.md          # 单元测试用例和 Mock 策略
├── 03-test-e2e.md           # UI 自动化测试用例
├── 03-test-manual.md        # 手动测试用例
└── 03-test-regression.md    # 上线前回归检查清单
```

## 单元测试方案

### 覆盖率要求
- **行覆盖率**：≥ 80%
- **分支覆盖率**：≥ 80%

### 用例格式

| 用例ID | 测试函数 | 场景 | 输入 | 预期输出 |
|--------|----------|------|------|----------|
| UT-001 | `functionName()` | 正常情况 | `{input}` | `{output}` |

### Mock 策略

| 依赖 | Mock 方式 | 说明 |
|------|-----------|------|
| 数据库 | Mock | 使用内存 Mock |
| 外部 API | Stub | 返回预定义响应 |

## UI 自动化测试方案

### 核心场景覆盖

| 用例ID | 场景名称 | 操作步骤 | 断言点 | 优先级 |
|--------|----------|----------|--------|--------|
| E2E-001 | <场景> | 1. xxx<br>2. xxx | <断言> | P0 |

## 手动测试用例

### 分类

| 分类 | 用例ID前缀 | 说明 |
|------|------------|------|
| 正向测试 | TC-P-XXX | 正常业务流程 |
| 反向测试 | TC-N-XXX | 异常输入和错误处理 |
| 边界测试 | TC-B-XXX | 边界值和极限情况 |
| 兼容性测试 | TC-C-XXX | 跨浏览器/设备 |
| 安全测试 | TC-S-XXX | 基础安全验证 |

## 上线前回归步骤

### 回归检查清单

| # | 检查项 | 执行方式 | 通过标准 | 责任人 |
|---|--------|----------|----------|--------|
| 1 | 代码合并 | 手动 | PR 已合并，无冲突 | 开发 |
| 2 | 单元测试 | CI 自动 | 全部通过，覆盖率达标 | 开发 |
| 3 | UI 自动化 | CI 自动 | 全部 P0 用例通过 | QA |
| 4 | 核心功能验证 | 手动 | P0 用例全部通过 | QA |
| 5 | 兼容性验证 | 手动 | 主流环境无异常 | QA |
| 6 | 性能验证 | 自动/手动 | 达到性能指标 | 开发 |
| 7 | 安全检查 | 自动 | 无高危漏洞 | 开发 |

### 回滚条件

| 级别 | 触发条件 | 回滚策略 |
|------|----------|----------|
| P0 | 核心功能不可用 | 立即回滚 |
| P1 | 严重影响用户体验 | 评估后决定 |
| P2 | 轻微问题 | 热修复 |

### 上线后验证

- [ ] 冒烟测试通过
- [ ] 核心监控指标正常
- [ ] 错误日志无异常增长
- [ ] 用户反馈渠道监控

## 约束

- [ ] **单元测试必须覆盖核心业务逻辑，行覆盖率 ≥ 80%，分支覆盖率 ≥ 80%**
- [ ] **UI 自动化测试必须覆盖所有 P0 场景**
- [ ] 手动测试用例覆盖所有验收标准
- [ ] 每个核心功能点至少 1 个正向用例
- [ ] 每个用户输入至少 1 个反向用例
- [ ] 必须提供验收标准覆盖矩阵
- [ ] 优先级标注：P0（阻塞）、P1（重要）、P2（一般）
- [ ] 用例步骤必须可执行，不得含糊
- [ ] 单元测试必须说明 Mock 策略
- [ ] **必须包含上线前回归检查清单**
- [ ] **必须定义回滚条件和上线后验证项**

## 下一步

完成后建议运行 `/prd-dev-tasks`

---

# 4. prd-dev-tasks (开发任务)

将系统设计拆解为可执行的开发任务。

## 元数据

```yaml
name: prd-dev-tasks
description: Break down system design into executable development tasks
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion, TodoWrite, Bash
```

## 前置条件

- 需求文档：`docs/prd/01-requirements.md`
- 系统设计：`docs/prd/02-system-design.md`
- 测试方案：`docs/prd/03-test-plan.md`

## TAR 原则

每个任务必须满足：

| 原则 | 说明 | 必填内容 |
|------|------|----------|
| **Testable** | 可测试 | 测试方法和预期结果 |
| **Acceptable** | 可验收 | 可量化的完成标准 |
| **Reviewable** | 可审查 | Review 检查要点 |

## 输出文件

```
docs/prd/
├── 04-dev-tasks.md              # 概览和依赖图
├── 04-dev-tasks-infra.md        # 基础设施任务（如超长）
├── 04-dev-tasks-core.md         # 核心逻辑任务（如超长）
├── 04-dev-tasks-api.md          # 接口层任务（如超长）
└── 04-dev-tasks-test.md         # 测试任务（如超长）
```

## 文档模板

```markdown
# 开发任务：<功能名称>

## 任务概览

- **总任务数**：X 个
- **执行顺序**：按依赖关系

## 任务设计原则

每个任务必须满足 **TAR 原则**：
- **可测试 (Testable)**：有明确的测试方法
- **可验收 (Acceptable)**：有可量化的完成标准
- **可审查 (Reviewable)**：有独立的代码审查点

## 依赖关系图

T-01 ─┬─> T-03 ─> T-05
T-02 ─┘           │
                  ▼
            T-06 ─> T-07

## 任务列表

### T-01: <任务名称>

| 属性 | 内容 |
|------|------|
| **描述** | <任务描述> |
| **依赖** | 无 |
| **优先级** | P0 |
| **涉及文件** | `src/db/schema.ts` |

**测试方法**：
- [ ] 运行数据库迁移脚本
- [ ] 验证表结构创建成功

**验收标准**：
- [ ] 迁移脚本执行无错误
- [ ] 数据库表结构与设计文档一致

**Review 要点**：
- [ ] 字段类型是否正确
- [ ] 索引设计是否合理
- [ ] 是否有安全隐患

---

## 执行检查清单

| 任务 | 测试 | 验收 | Review | 提交 |
|------|------|------|--------|------|
| T-01 | [ ] | [ ] | [ ] | [ ] |
| T-02 | [ ] | [ ] | [ ] | [ ] |

## 风险项

| 任务 | 风险 | 缓解措施 |
|------|------|----------|
| T-XX | <风险> | <措施> |
```

## 任务执行流程

```
1. 开始任务
   │
   ▼
2. 编写代码
   │
   ▼
3. 执行测试方法
   ├── 通过 ──────────────────┐
   └── 失败 → 修复 → 重新测试 │
                              ▼
4. 检查验收标准
   ├── 全部满足 ─────────────┐
   └── 未满足 → 补充 → 重检  │
                             ▼
5. 自查 Review 要点
   │
   ▼
6. 询问用户：是否提交代码？
   ├── 是 → git add & commit
   └── 否 → 继续修改
```

## Commit 格式

```
feat(T-XX): <任务名称>

- <完成内容1>
- <完成内容2>

测试: <测试结果>
验收: <验收状态>
```

## 约束

- [ ] **单个任务必须在 4 小时内可完成**
- [ ] **必须标注任务依赖关系**
- [ ] **必须按依赖顺序排列，无循环依赖**
- [ ] **文件路径必须具体，不得写"相关文件"**
- [ ] **必须提供依赖关系图**
- [ ] 优先级：P0（阻塞）、P1（重要）、P2（一般）
- [ ] 任务 ID 格式：T-XX（顺序编号）
- [ ] **每个任务必须包含测试方法**
- [ ] **每个任务必须包含验收标准**
- [ ] **每个任务必须包含 Review 要点**
- [ ] 测试方法必须可执行
- [ ] 验收标准必须可量化
- [ ] Review 要点必须针对任务类型

---

# 5. git-commit (代码提交)

使用 Conventional Commits 规范创建标准化的 git 提交。

## 元数据

```yaml
name: git-commit
description: Create git commits following Conventional Commits format
allowed-tools: Bash, AskUserQuestion, Read, Glob, Grep
```

## 触发条件

- 用户要求提交代码
- 用户说 "提交" 或 "commit"

## Commit 格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type（必填）

| Type | 用途 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat(auth): add login API` |
| `fix` | Bug 修复 | `fix(ui): resolve button alignment` |
| `refactor` | 重构 | `refactor(utils): simplify date helper` |
| `perf` | 性能优化 | `perf(query): optimize database query` |
| `docs` | 文档 | `docs(readme): update installation guide` |
| `test` | 测试 | `test(auth): add login unit tests` |
| `ci` | CI/CD | `ci(github): add deploy workflow` |
| `build` | 构建/依赖 | `build(deps): upgrade lodash to 4.17.21` |
| `chore` | 其他杂项 | `chore: update .gitignore` |

### Scope（可选）

- 受影响的模块或组件名
- 示例：`auth`, `ui`, `api`, `db`, `config`
- 如变更是全局的则省略

### Subject（必填）

- 使用祈使句："add" 而非 "added" 或 "adds"
- 结尾不加句号
- 最多 50 字符
- 首字母小写

### Body（可选）

- 解释 **做了什么** 和 **为什么**，而非怎么做
- 每行不超过 72 字符
- Subject 和 Body 之间空一行

### Footer（可选）

- 破坏性变更：`BREAKING CHANGE: <描述>`
- Issue 引用：`Closes #123` 或 `Fixes #456`

## 工作流程

```
1. 检查 git 状态
   │
   ▼
2. 显示变更内容
   │
   ▼
3. 询问用户确认提交范围
   │
   ▼
4. 分析变更，推荐 type
   │
   ▼
5. 生成 commit message
   │
   ▼
6. 询问用户确认或修改
   │
   ▼
7. 执行 git add & commit
   │
   ▼
8. 询问是否 push
```

## 示例

### Feature
```
feat(user): add profile avatar upload

- Support image upload up to 5MB
- Auto-resize to 200x200
- Store in S3 bucket

Closes #142
```

### Bug Fix
```
fix(cart): correct total price calculation

Price was not including tax for international orders.
Added tax calculation based on shipping country.

Fixes #89
```

### Refactor
```
refactor(api): extract validation middleware

Move validation logic from controllers to dedicated
middleware for better reusability.
```

### 简单提交
```
docs(api): add authentication section to README
test(utils): add edge case tests for date formatter
chore: update .eslintrc rules
ci(github): add Node 20 to test matrix
build(deps): bump axios from 1.4.0 to 1.6.0
perf(search): add index for frequently queried fields
```

## 约束

- [ ] Type 必须是：`feat | fix | refactor | perf | docs | test | ci | build | chore`
- [ ] Subject 不超过 50 字符
- [ ] Subject 使用祈使句
- [ ] Subject 结尾不加句号
- [ ] Body 每行不超过 72 字符
- [ ] 提交前必须显示 diff
- [ ] 执行 commit 前必须用户确认
- [ ] push 前必须询问用户

## 错误处理

### 无变更
```
当前没有可提交的变更。请先修改文件后再提交。
```

### 未解决冲突
```
检测到未解决的合并冲突，请先解决冲突后再提交。
```

### 非 Git 仓库
```
当前目录不是 Git 仓库。请先运行 git init 或切换到正确的目录。
```

---

# 项目结构

```
skills/
├── README.md                           # 本文档
├── agent-skill.md                      # Skill 规范参考
├── prd-requirements/
│   └── SKILL.md
├── prd-system-design/
│   ├── SKILL.md
│   └── templates/
│       └── design-template.md
├── prd-test-plan/
│   ├── SKILL.md
│   └── templates/
│       ├── unit-test-template.md
│       ├── e2e-test-template.md
│       ├── manual-test-template.md
│       └── regression-template.md
├── prd-dev-tasks/
│   └── SKILL.md
└── git-commit/
    └── SKILL.md
```

---

# 使用方式

## 安装

将 skill 目录复制到：
- 个人使用：`~/.claude/skills/`
- 项目使用：`.claude/skills/`

## 调用

直接输入命令或使用触发词：

```
/prd-requirements 我需要一个用户登录功能
/prd-system-design
/prd-test-plan
/prd-dev-tasks
/git-commit
```

或自然语言触发：

```
帮我写一个用户注册的需求文档
设计一下系统架构
写测试用例
拆分开发任务
提交代码
```
