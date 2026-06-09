# Git 治理架构方案

## 概述

本方案定义了 MES 项目的 Git 治理架构，规范了分支策略、提交规范、发布流程和代码审查体系。目标是确保代码仓库整洁、协作高效、发布可追溯。

## 分支策略（Git Flow）

| 分支 | 命名 | 来源 | 合并目标 | 说明 |
|------|------|------|----------|------|
| `master` | `master` | — | — | 生产环境（唯一生产分支） |
| `develop` | `develop` | — | — | 日常集成分支 |
| Feature | `feature/<MES-编号>-<描述>` | `develop` | `develop` | 功能开发 |
| Release | `release/<版本>` | `develop` | `master` + `develop` | 发布准备 |
| Hotfix | `hotfix/<MES-编号>-<描述>` | `master` | `master` + `develop` | 紧急修复 |

**Feature/Hotfix 分支名必须包含 Multica Issue 编号**（如 `feature/MES-42-xxx`），Multica GitHub App 通过扫描分支名实现 PR ↔ Issue 自动关联。

## 提交规范

采用约定式提交（Conventional Commits）：

```
<类型>(<范围>): <简短描述>

<详细描述>
```

| 类型 | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | Bug 修复 |
| `docs` | 文档 |
| `refactor` | 重构 |
| `test` | 测试 |
| `chore` | 杂项 |
| `style` | 格式 |
| `perf` | 性能 |
| `build` | 构建 |
| `ci` | CI |

## PR 基本规则

- **流向**: `feature/*` → `develop`；`release/*` / `hotfix/*` → `master`
- **标题**: `<类型>(<范围>): <描述>`
- **单一职责**: 一个 PR 只做一个变更
- **大小**: 不超过 400 行代码变更
- **Draft**: 开发中 PR 用 Draft 模式
- **清理**: 合并后自动删除源分支

## 合并策略

| 流向 | 策略 |
|------|------|
| `feature/*` → `develop` | Squash and Merge |
| `release/*` → `master` | Merge Commit |
| `release/*` → `develop` | Merge Commit |
| `hotfix/*` → `master` | Merge Commit |
| `hotfix/*` → `develop` | Merge Commit |

## 版本管理

- 版本号格式：`v<major>.<minor>.<patch>`（语义化版本）
- 发布标签不打 `-mes` 后缀，统一为 `vX.Y.Z`
- 发布分支命名：`release/<版本>`（如 `release/1.0.0`）
- 变更日志维护在 `CHANGELOG.md`，首次发布开始记录

## 代码审查

- 至少 1 人 Approve 后方可合并
- 使用 `/review` 命令触发 review
- Review 通过前不得合并到目标分支

## 分支清理策略

- Feature/Hotfix 分支合并后立即删除源分支
- Release 分支完成双向合并后立即删除
- Agent 工作分支仅限本地使用，**不得推送到远程**
- 定期审查远程分支，清理超过 30 天无活动的分支

## Git Hooks

- commitlint 用于提交信息格式校验（`commitlint.config.js`）
- hooks 通过 `husky` 或手动 `cp` 安装到 `.git/hooks/commit-msg`
