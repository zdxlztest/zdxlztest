# 贡献指南 | Contributing Guide

感谢您对 cqlib 的关注！cqlib 是开源项目，我们欢迎社区任何形式的贡献。

---

## 目录

- [行为准则](#行为准则)
- [如何贡献](#如何贡献)
- [开发环境搭建](#开发环境搭建)
- [提交规范](#提交规范)
- [代码规范](#代码规范)
- [PR 提交流程](#pr-提交流程)

---

## 行为准则

参与本项目即代表您同意遵守 [行为准则](CODE_OF_CONDUCT.md)。
请在与社区成员的互动中保持尊重与友善。

---

## 如何贡献

cqlib 欢迎以下类型的贡献：

| 贡献类型   | 方式                                                         |
| ---------- | ------------------------------------------------------------ |
| 🐛 报告 Bug | 使用 [Bug Report 模板](https://github.com/ctq-quantum/cqlib/issues/new?template=bug_report.md) 提交 Issue |
| 💡 建议功能 | 使用 [Feature Request 模板](https://github.com/ctq-quantum/cqlib/issues/new?template=feature_request.md) 提交 Issue |
| 🔧 提交代码 | Fork 仓库后提交 Pull Request                                 |
| 📖 改进文档 | 提交 PR 修改 `docs/` 目录下的文档                            |
| 🧪 添加测试 | 提交 PR 增加或改进测试用例                                   |
| 💬 回答问题 | 前往 [Discussions](https://github.com/ctq-quantum/cqlib/discussions) 帮助其他用户 |

> **新手友好**：查看带有 [`good first issue`](https://github.com/ctq-quantum/cqlib/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) 标签的 Issue，这些任务适合首次贡献者。

---

## 分支说明

cqlib 采用以下分支策略：

| 分支      | 用途       | 说明                                       |
| --------- | ---------- | ------------------------------------------ |
| `main`    | 稳定发布版 | 只接受来自 `develop` 的合并，不直接提交    |
| `develop` | 开发主线   | **所有 PR 的目标分支**，新功能先合入此分支 |

> **外部贡献者请注意**：请基于 `develop` 分支进行开发，PR 也请提交到 `develop` 分支。

## 开发环境搭建

### 前置条件

- Python 3.9 或以上版本
- pip 21.0 或以上版本

### 步骤

```bash
# 1. Fork 本仓库到你的 GitHub 账号
#    （在 GitHub 页面点 Fork 按钮）

# 2. 克隆你 Fork 的仓库到本地
git clone https://github.com/<你的用户名>/cqlib.git
cd cqlib

# 3. 添加上游仓库（用于同步最新代码）
git remote add upstream https://github.com/ctq-quantum/cqlib.git

# 4. 切换到 develop 分支，并基于它创建你的功能分支
git checkout develop
git checkout -b feat/你的功能描述

# 5. 安装开发依赖
pip install -e ".[dev]"

# 6. 验证安装
pytest tests/ -v
ruff check .
```

### 同步上游更新

当上游 `develop` 分支有新的提交时，请及时同步到你的本地分支：

```bash
# 拉取上游最新代码
git fetch upstream

# 切换到你的工作分支
git checkout feat/你的功能描述

# 将上游 develop 的更新合并到你的分支
git merge upstream/develop
```

---

## 提交规范

### 分支命名

| 类型     | 格式                 | 示例                         |
| -------- | -------------------- | ---------------------------- |
| 新功能   | `feat/简短描述`      | `feat/add-grover-algorithm`  |
| Bug 修复 | `fix/issue编号-描述` | `fix/42-circuit-depth-error` |
| 文档改进 | `docs/简短描述`      | `docs/update-install-guide`  |
| 测试改进 | `test/简短描述`      | `test/add-qaoa-coverage`     |

> **注意**：分支请从 `develop` 创建，不要从 `main` 创建。

### Commit 信息规范（Conventional Commits）

每次提交请遵循以下格式：

```
<类型>(<范围>): <简短描述>

[可选的正文]

[可选的 Footer：关联 Issue]
```

**类型说明**：

| 类型       | 含义                   |
| ---------- | ---------------------- |
| `feat`     | 新功能                 |
| `fix`      | Bug 修复               |
| `docs`     | 文档改动               |
| `test`     | 测试相关               |
| `refactor` | 代码重构（不改变行为） |
| `perf`     | 性能优化               |
| `chore`    | 构建/工具链改动        |

**示例**：

```
feat(circuit): add support for custom gate definitions

Allow users to define arbitrary single-qubit gates
via the new `Gate.custom()` method.

Fixes #123
```

### 关联 Issue

在 PR 或 Commit 中关联 Issue，让代码改动与问题描述对应起来：

- `Fixes #123` → 此 PR 合并后自动关闭 Issue #123
- `Closes #456` → 同上
- `Relates to #789` → 关联但不自动关闭

---

## 代码规范

### Python 代码风格

- 使用 **ruff** 进行代码检查和格式化
- 行宽限制：120 字符
- 缩进：4 个空格

```bash
# 自动格式化
ruff check --fix .
ruff format .
```

### 类型注解

核心模块（`cqlib/core/`）的函数必须添加类型注解：

```python
def apply_gate(circuit: Circuit, gate: Gate) -> Circuit:
    """Apply a quantum gate to the circuit."""
    ...
```

### 测试要求

- 新增代码须包含对应的单元测试
- 测试覆盖率须保持在 **80% 以上**
- 测试文件命名：`tests/test_<模块名>.py`

```bash
# 运行测试并查看覆盖率
pytest tests/ -v --cov=cqlib --cov-report=term-missing
```

### 文档要求

- 所有公开 API 必须包含 docstring（遵循 Google 风格）
- 新增功能须更新 `docs/` 下的对应文档

```python
def my_function(param: int) -> bool:
    """简短描述功能。

    Args:
        param: 参数说明。

    Returns:
        返回值说明。

    Raises:
        ValueError: 何时抛出此异常。
    """
```

---

## PR 提交流程

### 提交 PR 前检查清单

- [ ] 已基于 `develop` 分支创建功能分支
- [ ] 已在本地运行 `pytest tests/`，全部通过
- [ ] 已运行 `ruff check .`，无报错
- [ ] 已添加/更新对应测试用例
- [ ] 已更新相关文档（如适用）
- [ ] PR 标题遵循 Conventional Commits 规范
- [ ] 已关联对应的 Issue
- [ ] PR 目标分支设置为 `develop`（不是 `main`）

### PR 被审核时

- 审核意见请在对应的 PR 行内回复
- 修改后提交到同一个 PR 分支（**不要开新的 PR**）
- 所有讨论解决后，维护者会合并 PR 到 `develop`

### 合并方式

- 小型 PR（< 100 行）：Squash Merge（保持历史干净）
- 大型 PR（> 100 行）：Rebase Merge

### develop → main 发布流程

外部贡献者的 PR 合并到 `develop` 后，代码会先在 `develop` 中进行集成验证。当积累足够多的变更、准备发布新版本时，由内部维护者执行以下操作：

**方式一：在 GitHub 上提 PR（推荐，有审核记录）**

1. 进入仓库 → **Pull requests** → **New pull request**
2. `base` 选 `main`，`compare` 选 `develop`
3. 标题填：`Release: vX.X.X`（填写本次版本号）
4. 描述中列出本次发布包含的 PR 和变更要点
5. 指定另一位内部维护者进行 Review
6. Review 通过后，以 **Merge commit** 方式合并（保留 develop 的历史）
7. 合并完成后打上版本 Tag：`git tag vX.X.X && git push origin vX.X.X`

**方式二：本地命令行（快速）**

```bash
git checkout main
git pull origin main
git merge origin/develop
git push origin main
git tag vX.X.X
git push origin vX.X.X
```

> 📌 **注意**：`main` 分支受保护，必须通过 PR 合并（方式一），方式二仅适用于有 Admin 权限的账号且已临时放行的情况。正式流程请使用方式一。

---

## Issue 处理流程（社区运营必读）

cqlib 的 Issue 由**两个团队**共同处理，请按以下规则分类和分配：

| Issue 类型 | 标签            | 负责团队               | 处理方式                           |
| ---------- | --------------- | ---------------------- | ---------------------------------- |
| 使用咨询   | `question`      | 社区运营团队           | 直接回复，解答后关闭               |
| 文档问题   | `documentation` | 社区运营团队           | 由运营团队修改文档或指派文档负责人 |
| Bug 报告   | `bug`           | 研发团队               | 指派给对应模块的研发负责人         |
| 功能建议   | `enhancement`   | 研发团队（运营先评估） | 运营初步评估后转交研发评估可行性   |
| 性能问题   | `perf`          | 研发团队               | 指派给性能优化负责人               |

### 分配 Issue 的权限

- **有 Write 权限的内部成员**（研发团队 + 社区运营团队）都可以分配 Issue
- 建议由**社区运营统一做第一道分类**，避免重复分配或遗漏
- 分配方法：打开 Issue → 右侧面板 **Assignees** → 输入用户名或团队名

---

## 社区资源

- 📖 [官方文档](https://docs.cqlib.ctq-quantum.com)
- 💬 [GitHub Discussions](https://github.com/ctq-quantum/cqlib/discussions)
- 🐛 [Issue 跟踪](https://github.com/ctq-quantum/cqlib/issues)
- 📧 社区邮件：community@ctq-quantum.com

再次感谢您的贡献！🎉
