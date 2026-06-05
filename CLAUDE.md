# CLAUDE.md

此文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指导。

## 仓库概述

这是一个新建的制造执行系统（MES）项目。仓库目前处于初始状态——尚未设置任何源代码、构建系统或依赖项。

## Git Flow 标准化工作流

本项目采用 **Git Flow** 分支模型进行版本控制和协作开发。

### 分支类型

| 分支类型 | 命名格式 | 来源分支 | 合并目标 | 说明 |
|----------|----------|----------|----------|------|
| `master` | `master` | — | — | 生产环境分支，始终保持可发布状态，只接受 `release` 和 `hotfix` 的合并 |
| `develop` | `develop` | — | — | 主开发集成分支，包含下一个版本的全部功能 |
| **Feature** | `feature/<描述>` | `develop` | `develop` | 功能开发分支，开发完成后合并回 `develop` |
| **Release** | `release/<版本号>` | `develop` | `master` + `develop` | 发布准备分支，用于版本发布前的Bug修复和文档更新 |
| **Hotfix** | `hotfix/<描述>` | `master` | `master` + `develop` | 紧急修复分支，用于修复生产环境的问题 |

### 工作流规则

#### 1. Feature 分支
```bash
# 从 develop 创建功能分支
git checkout develop
git checkout -b feature/xxx

# 开发完成后合并回 develop（不要直接合并到 master）
git checkout develop
git merge --no-ff feature/xxx
git branch -d feature/xxx
```

#### 2. Release 分支
```bash
# 从 develop 创建发布分支
git checkout develop
git checkout -b release/v1.0.0

# 修复Bug、更新版本号等...

# 发布完成后合并到 master 和 develop
git checkout master
git merge --no-ff release/v1.0.0
git tag -a v1.0.0

git checkout develop
git merge --no-ff release/v1.0.0
git branch -d release/v1.0.0
```

#### 3. Hotfix 分支
```bash
# 从 master 创建紧急修复分支
git checkout master
git checkout -b hotfix/xxx

# 修复完成后合并到 master 和 develop
git checkout master
git merge --no-ff hotfix/xxx
git tag -a v1.0.1

git checkout develop
git merge --no-ff hotfix/xxx
git branch -d hotfix/xxx
```

### 提交信息规范

提交信息建议使用如下格式：

```
<type>(<scope>): <subject>

<body>
```

- **type**: `feat` (新功能), `fix` (修复), `docs` (文档), `refactor` (重构), `test` (测试), `chore` (构建/工具)
- **scope**: 影响范围（可选）
- **subject**: 简洁的描述（不超过50字）

### 版本号规范

遵循语义化版本 **[SemVer](https://semver.org/)**：

- **v主版本.次版本.修订号**
  - 主版本：不兼容的API修改
  - 次版本：向下兼容的新功能
  - 修订号：向下兼容的问题修复

## 当前仓库状态

- **GitHub**: `git@github.com:huangxn29/mes.git`
- **当前活跃分支**: `develop`
- 暂无构建、测试或 lint 命令——项目正处于起始阶段
