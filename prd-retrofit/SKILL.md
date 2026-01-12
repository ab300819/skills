---
name: prd-retrofit
description: Retrofit existing projects to PRD workflow by identifying or specifying existing documents. Use when users want to adapt existing projects to PRD process, migrate legacy documentation, or standardize project documents. Triggers on keywords like "retrofit", "改造", "适配", "迁移文档", "标准化".
allowed-tools: Read, Write, Glob, Grep, AskUserQuestion, Bash
---

# PRD Retrofit

将已有工程按 PRD 流程改造，自动识别或手动指定各阶段文档。

## Language

- Accept questions in both Chinese and English
- Always respond in Chinese
- Generate all documents in Chinese

## Trigger Conditions

- 用户希望将现有项目适配 PRD 流程
- 用户需要标准化项目文档
- 用户要迁移或整理已有文档

## Workflow

```
1. 扫描项目结构
   │
   ▼
2. 自动识别已有文档
   │
   ▼
3. 展示识别结果，询问用户确认/修正
   │
   ▼
4. 分析文档覆盖情况
   │
   ▼
5. 生成改造计划
   │
   ▼
6. 逐阶段执行改造
   │
   ▼
7. 生成改造报告
```

## Step 1: 扫描项目结构

扫描常见文档位置：

```bash
# 扫描文档目录
docs/ doc/ documentation/ 文档/

# 扫描根目录文档
README.md CHANGELOG.md CONTRIBUTING.md

# 扫描设计文档
design/ architecture/ 设计/

# 扫描测试文档
tests/ test/ __tests__/ spec/
```

## Step 2: 自动识别文档

### 识别规则

| PRD 阶段 | 识别关键词 | 常见文件名 |
|----------|------------|------------|
| 需求文档 | requirement, PRD, 需求, spec, feature | `*requirement*.md`, `*prd*.md`, `*spec*.md` |
| 系统设计 | design, architecture, 设计, 架构, technical | `*design*.md`, `*architecture*.md`, `*tech*.md` |
| 测试方案 | test, QA, 测试, case | `*test*.md`, `*qa*.md`, `test-plan.*` |
| 开发任务 | task, todo, 任务, sprint, backlog | `*task*.md`, `*todo*.md`, `*backlog*.md` |

### 识别逻辑

```
1. 按文件名模式匹配
2. 读取文件内容，按关键词评分
3. 识别文档结构（标题、章节）
4. 综合判断文档类型
```

## Step 3: 用户确认

使用 AskUserQuestion 展示识别结果：

```markdown
## 文档识别结果

| 阶段 | 识别状态 | 识别到的文件 |
|------|----------|--------------|
| 需求文档 | ✅ 已识别 | `docs/requirements.md` |
| 系统设计 | ✅ 已识别 | `docs/architecture.md` |
| 测试方案 | ❌ 未找到 | - |
| 开发任务 | ⚠️ 待确认 | `TODO.md` (相似度 60%) |

请确认或修正以上识别结果。
```

### 用户选项

1. **确认识别结果** - 使用自动识别的映射
2. **手动指定文档** - 用户提供各阶段文档路径
3. **混合模式** - 部分确认，部分手动指定

### 手动指定格式

```markdown
请按以下格式指定各阶段文档（可选，留空表示需要新建）：

- 需求文档: <文件路径>
- 系统设计: <文件路径>
- 测试方案: <文件路径>
- 开发任务: <文件路径>
```

## Step 4: 分析覆盖情况

对已识别的文档进行内容分析：

### 需求文档检查项

| 检查项 | 状态 | 说明 |
|--------|------|------|
| 需求背景 | ✅/❌ | 是否包含背景说明 |
| 目标用户 | ✅/❌ | 是否定义用户群体 |
| 功能清单 | ✅/❌ | 是否列出功能点 |
| 用户故事 | ✅/❌ | 是否有用户故事 |
| 验收标准 | ✅/❌ | 是否有验收条件 |
| 范围边界 | ✅/❌ | 是否明确范围 |

### 系统设计检查项

| 检查项 | 状态 | 说明 |
|--------|------|------|
| 架构图 | ✅/❌ | 是否有架构描述 |
| 技术选型 | ✅/❌ | 是否说明技术栈 |
| 模块设计 | ✅/❌ | 是否有模块划分 |
| 接口定义 | ✅/❌ | 是否有 API 设计 |
| 数据模型 | ✅/❌ | 是否有数据结构 |

### 测试方案检查项

| 检查项 | 状态 | 说明 |
|--------|------|------|
| 单元测试 | ✅/❌ | 是否有单测方案 |
| 集成测试 | ✅/❌ | 是否有集成测试 |
| E2E 测试 | ✅/❌ | 是否有端到端测试 |
| 测试用例 | ✅/❌ | 是否有具体用例 |

### 开发任务检查项

| 检查项 | 状态 | 说明 |
|--------|------|------|
| 任务列表 | ✅/❌ | 是否有任务清单 |
| 依赖关系 | ✅/❌ | 是否标注依赖 |
| 优先级 | ✅/❌ | 是否有优先级 |
| 完成标准 | ✅/❌ | 是否有验收条件 |

## Step 5: 生成改造计划

```markdown
## 改造计划

### 文档状态总览

| 阶段 | 当前状态 | 改造动作 |
|------|----------|----------|
| 需求文档 | 部分完整 (70%) | 补充验收标准、范围边界 |
| 系统设计 | 缺失 | 新建完整文档 |
| 测试方案 | 部分完整 (40%) | 补充单元测试、回归方案 |
| 开发任务 | 已存在 | 格式标准化 |

### 改造步骤

1. [ ] 补充需求文档缺失章节
2. [ ] 新建系统设计文档
3. [ ] 完善测试方案
4. [ ] 标准化开发任务格式

### 预计产出

docs/prd/
├── 01-requirements.md      # 补充自 docs/requirements.md
├── 02-system-design.md     # 新建
├── 03-test-plan.md         # 补充自 tests/README.md
└── 04-dev-tasks.md         # 标准化自 TODO.md
```

## Step 6: 执行改造

### 改造模式

使用 AskUserQuestion 询问改造模式：

1. **完整改造** - 按 PRD 模板重新生成所有文档
2. **增量补充** - 保留原有内容，仅补充缺失部分
3. **仅标准化** - 保留内容，调整格式和结构

### 改造原则

- [ ] 保留原有文档的核心内容
- [ ] 不删除用户已有的信息
- [ ] 新增内容标注 `[待补充]` 提醒用户
- [ ] 生成的文档放入 `docs/prd/` 目录
- [ ] 原文档保留备份（如需要）

### 各阶段改造

#### 需求文档改造

```
1. 读取原文档内容
2. 提取已有信息（背景、功能等）
3. 按 PRD 模板重组
4. 标注缺失部分为 [待补充]
5. 写入 docs/prd/01-requirements.md
```

#### 系统设计改造

```
1. 读取原文档（如有）
2. 扫描代码结构推断架构
3. 提取已有设计信息
4. 按模板生成，缺失部分标注
5. 写入 docs/prd/02-system-design.md
```

#### 测试方案改造

```
1. 读取测试相关文档
2. 扫描测试代码目录
3. 提取已有测试策略
4. 按模板补充完善
5. 写入 docs/prd/03-test-*.md
```

#### 开发任务改造

```
1. 读取任务/TODO 文档
2. 提取任务列表
3. 按 TAR 原则补充
4. 建立依赖关系
5. 写入 docs/prd/04-dev-tasks.md
```

## Step 7: 生成改造报告

```markdown
# PRD 改造报告

## 改造概览

- **项目名称**：<project>
- **改造时间**：<timestamp>
- **改造模式**：增量补充

## 改造结果

| 阶段 | 原文档 | 新文档 | 改造类型 |
|------|--------|--------|----------|
| 需求 | `docs/req.md` | `docs/prd/01-requirements.md` | 补充 |
| 设计 | - | `docs/prd/02-system-design.md` | 新建 |
| 测试 | `tests/README.md` | `docs/prd/03-test-plan.md` | 补充 |
| 任务 | `TODO.md` | `docs/prd/04-dev-tasks.md` | 标准化 |

## 待完善项

以下内容标记为 [待补充]，需要人工完善：

### 需求文档
- [ ] 验收标准 AC-03 至 AC-05
- [ ] 非功能性需求-性能指标

### 系统设计
- [ ] API 响应示例
- [ ] 错误码列表

### 测试方案
- [ ] UI 自动化测试用例
- [ ] 回归测试检查清单

## 下一步建议

1. 完善标记为 [待补充] 的内容
2. 运行 `/prd-requirements` 审查需求文档
3. 运行 `/prd-system-design` 审查系统设计
4. 运行 `/prd-test-plan` 完善测试方案
```

## Output

```
docs/prd/
├── 01-requirements.md
├── 02-system-design.md
├── 02-system-design-api.md      # 如需要
├── 03-test-plan.md
├── 03-test-unit.md              # 如需要
├── 03-test-e2e.md               # 如需要
├── 03-test-manual.md            # 如需要
├── 03-test-regression.md        # 如需要
├── 04-dev-tasks.md
└── 00-retrofit-report.md        # 改造报告
```

## Constraints

### 识别约束
- [ ] 必须扫描常见文档目录
- [ ] 识别结果必须让用户确认
- [ ] 支持用户手动指定/修正

### 改造约束
- [ ] **不得删除原有文档的有效内容**
- [ ] 缺失内容标注 `[待补充]`
- [ ] 必须生成改造报告
- [ ] 改造前询问用户确认改造模式

### 输出约束
- [ ] 输出目录统一为 `docs/prd/`
- [ ] 文件命名遵循 PRD 规范
- [ ] 每个阶段改造后询问用户确认

## Error Handling

### 无法识别文档
```
未能自动识别到相关文档。

请手动指定各阶段文档路径，或选择跳过相应阶段：
- 需求文档: [请输入路径或留空]
- 系统设计: [请输入路径或留空]
- 测试方案: [请输入路径或留空]
- 开发任务: [请输入路径或留空]
```

### 文档格式不兼容
```
检测到文档格式可能不兼容：
- 文件：<path>
- 问题：<issue>

建议：选择"完整改造"模式重新生成文档。
```

### 项目无文档
```
当前项目未检测到任何文档。

建议：
1. 使用 /prd-requirements 从头创建需求文档
2. 或提供外部文档路径进行导入
```
