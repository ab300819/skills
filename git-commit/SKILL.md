---
name: git-commit
description: Create git commits following Conventional Commits format. Use when users want to commit code, stage changes, or create commit messages. Triggers on keywords like "commit", "git commit", "提交代码", "提交".
allowed-tools: Bash, AskUserQuestion, Read, Glob, Grep
---

# Git Commit

Create standardized git commits following Conventional Commits specification.

## Language

- Accept questions in both Chinese and English
- Always respond in Chinese
- Commit messages in English (type and scope) with Chinese description supported

## Trigger Conditions

- User asks to commit code
- User wants to create a commit
- User says "提交" or "commit"

## Commit Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type (Required)

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(auth): add login API` |
| `fix` | Bug fix | `fix(ui): resolve button alignment` |
| `refactor` | Code refactoring (no feature/fix) | `refactor(utils): simplify date helper` |
| `perf` | Performance improvement | `perf(query): optimize database query` |
| `docs` | Documentation only | `docs(readme): update installation guide` |
| `test` | Add or update tests | `test(auth): add login unit tests` |
| `ci` | CI/CD changes | `ci(github): add deploy workflow` |
| `build` | Build system or dependencies | `build(deps): upgrade lodash to 4.17.21` |
| `chore` | Other changes (no src/test) | `chore: update .gitignore` |

### Scope (Optional)

- Module or component name affected
- Examples: `auth`, `ui`, `api`, `db`, `config`
- Omit if change is global or unclear

### Subject (Required)

- Imperative mood: "add" not "added" or "adds"
- No period at the end
- Max 50 characters
- Lowercase first letter

### Body (Optional)

- Explain **what** and **why**, not how
- Wrap at 72 characters
- Blank line between subject and body

### Footer (Optional)

- Breaking changes: `BREAKING CHANGE: <description>`
- Issue references: `Closes #123` or `Fixes #456`

## Workflow

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

## Execution Steps

### Step 1: Check Status

```bash
git status
git diff --stat
```

### Step 2: Show Changes

```bash
git diff              # Unstaged changes
git diff --staged     # Staged changes
```

### Step 3: Confirm Scope

Use AskUserQuestion to ask:
- "以下文件将被提交，是否确认？"
- Options: "全部提交" / "选择文件" / "取消"

If "选择文件", ask which files to include.

### Step 4: Analyze & Recommend

Based on changed files and diff content:
- New files → likely `feat`
- Modified existing logic → check if fix or refactor
- Test files only → `test`
- Docs only → `docs`
- Config/CI files → `ci`, `build`, or `chore`

### Step 5: Generate Message

Propose commit message based on analysis:
```
推荐的 commit message:

feat(auth): add user registration endpoint

- Add POST /api/users/register endpoint
- Implement email validation
- Add password hashing

是否使用此 message？
```

### Step 6: Confirm Message

Use AskUserQuestion:
- "是否使用推荐的 commit message？"
- Options: "使用" / "修改" / "取消"

If "修改", ask user for their preferred message.

### Step 7: Execute Commit

```bash
git add <files>
git commit -m "<message>"
```

### Step 8: Ask Push

Use AskUserQuestion:
- "提交成功，是否推送到远程？"
- Options: "推送" / "稍后推送"

If "推送":
```bash
git push
```

## Examples

### Feature Commit
```
feat(user): add profile avatar upload

- Support image upload up to 5MB
- Auto-resize to 200x200
- Store in S3 bucket

Closes #142
```

### Bug Fix Commit
```
fix(cart): correct total price calculation

Price was not including tax for international orders.
Added tax calculation based on shipping country.

Fixes #89
```

### Refactor Commit
```
refactor(api): extract validation middleware

Move validation logic from controllers to dedicated
middleware for better reusability.
```

### Simple Commits
```
docs(api): add authentication section to README

test(utils): add edge case tests for date formatter

chore: update .eslintrc rules

ci(github): add Node 20 to test matrix

build(deps): bump axios from 1.4.0 to 1.6.0

perf(search): add index for frequently queried fields
```

## Constraints

- [ ] Type must be one of: `feat | fix | refactor | perf | docs | test | ci | build | chore`
- [ ] Subject must be <= 50 characters
- [ ] Subject must use imperative mood
- [ ] Subject must not end with period
- [ ] Body lines should wrap at 72 characters
- [ ] Must show diff before committing
- [ ] Must confirm with user before executing commit
- [ ] Must ask before pushing to remote

## Error Handling

### No Changes
```
当前没有可提交的变更。请先修改文件后再提交。
```

### Uncommitted Conflicts
```
检测到未解决的合并冲突，请先解决冲突后再提交。
```

### Not a Git Repository
```
当前目录不是 Git 仓库。请先运行 git init 或切换到正确的目录。
```
