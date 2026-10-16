# Git & GitHub Engineering Workflow Guide | Git 与 GitHub 完整工程化操作指南

> 从底层原理到生产实践：覆盖 Git 工作区流转、分支策略、PR/CR 评审流程、CI/CD 门禁与现代开发运维避坑。

---

## 目录

1. [Git 的两层世界：本地 vs 远端](#一git-的两层世界本地-vs-远端)
2. [Git 基础：四个工作区](#二git-基础四个工作区)
3. [核心操作：fetch / pull / push](#三核心操作fetch--pull--push)
4. [提交规范：Conventional Commits](#四提交规范conventional-commits)
5. [分支策略：三种主流模型](#五分支策略三种主流模型)
6. [分支操作：创建、切换、合并、删除](#六分支操作创建切换合并删除)
7. [合并策略：merge / rebase / squash](#七合并策略merge--rebase--squash)
8. [冲突解决：原理与实战](#八冲突解决原理与实战)
9. [Pull Request 工程化流程](#九pull-request-工程化流程)
10. [Code Review 规范与实践](#十code-review-规范与实践)
11. [Git 高级操作](#十一git-高级操作)
12. [Tag 与 Release 管理](#十二tag-与-release-管理)
13. [Git Hooks：自动化质量门禁](#十三git-hooks自动化质量门禁)
14. [CI/CD 集成（GitHub Actions）](#十四cicd-集成github-actions)
15. [.gitignore 与仓库管理](#十五gitignore-与仓库管理)
16. [SSH / Token 认证配置](#十六ssh--token-认证配置)
17. [故障排查与急救手册](#十七故障排查与急救手册)
18. [工程化最佳实践清单](#十八工程化最佳实践清单)

---

## 一、Git 的两层世界：本地 vs 远端

```
┌──────────────────────────┐          ┌──────────────────────────┐
│       本地仓库           │  push →  │       远端仓库           │
│   (你的电脑上)           │  ← fetch │   (GitHub / GitLab 上)   │
│                          │          │                          │
│  Working Directory       │          │  https://github.com/     │
│  Staging Area            │          │  user/repo               │
│  Local Repo (.git)       │          │                          │
└──────────────────────────┘          └──────────────────────────┘
```

**远端（Remote）** 是托管在 GitHub / GitLab / Gitee 等平台上的仓库副本。

```bash
git remote -v
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)
```

- `origin` — 远端的**别名**（约定俗成的默认名称）
- `(fetch)` — 从这个地址**拉取**数据，只拉取但是不合并
- `(pull)` — 拉取数据并自动合并到当前分支
- `(push)` — 向这个地址**推送**数据

### 添加 / 修改远端

```bash
git remote add upstream https://github.com/original/repo.git   # 添加上游仓库（Fork 场景）
git remote set-url origin git@github.com:user/repo.git         # 修改远端地址（HTTPS → SSH）
git remote remove upstream                                      # 删除远端
git remote -v                                                   # 查看所有远端
```

---

## 二、Git 基础：四个工作区

Git 有四个逻辑区域，理解它们是掌握 Git 的关键：

```
工作目录         暂存区           本地仓库          远端仓库
(Working        (Staging         (Local            (Remote
 Directory)      Area)            Repo)             Repo)
                                                    
  ┌─────┐         ┌─────┐          ┌─────┐           ┌─────┐
  │ 修改│  add →  │ 暂存│  commit →│ 提交│  push →   │ 远端│
  │ 文件│         │ 变更│          │ 历史│   ← fetch │ 仓库│
  └─────┘         └─────┘          └─────┘           └─────┘
     ↑                                  │
     └────── git checkout / restore ────┘
```

### 各区域的操作命令

```bash
# 工作目录 → 暂存区
git add <file>           # 添加指定文件
git add .                # 添加所有变更
git add -p               # 交互式选择要暂存的代码块（推荐）

# 暂存区 → 本地仓库
git commit -m "message"  # 提交
git commit -am "message" # 跳过 add，直接提交已跟踪文件的修改（新文件不会被添加）

# 撤销操作
git restore <file>       # 放弃工作区的修改（用暂存区/仓库版本覆盖）
git restore --staged <file>  # 取消暂存（从暂存区退回工作区）
git reset HEAD~1         # 撤销最近一次 commit，变更回到工作区（--soft 则回到暂存区）
git reset --hard HEAD~1  # 彻底删除最近一次 commit 及其变更（危险！）
```

### 文件状态生命周期

```
未跟踪 (Untracked)  →  git add  →  已暂存 (Staged)  →  git commit  →  已提交 (Committed)
                                        ↑                                  │
                                        └──── 修改后变成 已修改 (Modified) ←┘
```

```bash
git status               # 查看当前状态（最常用的命令之一）
git status -s             # 简短格式
```

---

## 三、核心操作：fetch / pull / push

### `git fetch` — 下载，但不合并

```
执行前:
  本地 main:  A---B---C
  远端 main:  A---B---C---D

执行 git fetch 后:
  本地 main:              A---B---C
  远端跟踪分支 (origin/main):  A---B---C---D
```

- `origin/main` 叫做**远端跟踪分支**（remote-tracking branch）
- 它代表"我上次看的时候，远端长这样"
- **你的本地 `main` 完全没被动过**，只是知道远端有新东西了

### `git pull` — 下载 + 合并

`git pull` = `git fetch` + `git merge`（默认行为）

```bash
git pull origin main
# 等价于:
git fetch origin main
git merge origin/main

# 推荐使用 rebase 方式 pull（保持历史线性）：
git pull --rebase origin main
```

### `git push` — 上传你的提交

```bash
git push origin main                    # 推送到 origin 的 main 分支
git push -u origin feature/login        # 首次推送并设置上游跟踪（之后只需 git push）
git push origin --delete old-branch     # 删除远端分支
```

### 典型场景：推送被拒绝

```
 ! [rejected]        main -> main (non-fast-forward)
hint: Updates were rejected because the tip of your current branch is behind
```

**原因：** 远端有你本地没有的提交。Git 要求远端必须是本地的子集（fast-forward）。

```
✅ 可以 fast-forward:
本地:  A---B---C
远端:  A---B          ← 远端是本地的前缀

❌ 不能 fast-forward:
本地:  A---B---C
远端:  A---B---D      ← 远端有本地没有的 D
```

**解决方案：**

```bash
# 1. 先拉取远端更新
git pull --rebase origin main

# 2. 解决冲突（如果有）
# 编辑冲突文件 → git add → git rebase --continue

# 3. 再推送
git push origin main
```

**极端情况——无关历史：**

```bash
# 本地和远端没有共同祖先（如 GitHub 初始化时勾选了 README）
git pull origin main --allow-unrelated-histories

# 这会生成一个 merge commit，将两条独立的历史线汇合
```

### 排查技巧

```bash
# 本地有但远端没有的提交（需要 push 的）
git log origin/main..main --oneline

# 远端有但本地没有的提交（需要 pull 的）
git log main..origin/main --oneline

# 找共同祖先
git merge-base main origin/main

# 可视化所有分支历史
git log --oneline --all --graph --decorate
```

---

## 四、提交规范：Conventional Commits

工程化团队必须统一提交信息格式。**Conventional Commits** 是业界标准。

### 格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

| Type | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat(auth): add OAuth2 login support` |
| `fix` | Bug 修复 | `fix(api): handle null response from /users` |
| `docs` | 文档变更 | `docs(readme): update installation guide` |
| `style` | 代码格式（不影响逻辑） | `style: format with prettier` |
| `refactor` | 重构（不改变功能） | `refactor(db): extract connection pool` |
| `perf` | 性能优化 | `perf(query): add index on user_id` |
| `test` | 添加/修改测试 | `test(auth): add unit tests for login` |
| `build` | 构建系统或外部依赖 | `build: upgrade webpack to v5` |
| `ci` | CI 配置变更 | `ci: add GitHub Actions workflow` |
| `chore` | 杂项（不影响源码） | `chore: update .gitignore` |
| `revert` | 回滚之前的提交 | `revert: revert feat(auth): add OAuth2` |

### Scope 范围

- `feat(api)` — 功能所属模块
- `fix(ui)` — Bug 所在模块
- 可选，但**强烈推荐**在多人协作中使用

### 实战示例

```bash
# 好的提交信息
git commit -m "feat(user): add email verification flow

- Add verification token generation
- Send verification email via SendGrid
- Add /verify/:token endpoint

Closes #42"

# 坏的提交信息（不要这样写！）
git commit -m "fix bug"
git commit -m "update"
git commit -m "wip"
```

### 工具辅助

```bash
# commitizen — 交互式生成规范提交信息
npm install -g commitizen cz-conventional-changelog
git cz   # 替代 git commit

# commitlint — 在 CI 或 Git Hook 中校验提交格式
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

---

## 五、分支策略：三种主流模型

### 模型对比

| 维度 | Git Flow | GitHub Flow | Trunk-Based |
|------|----------|-------------|-------------|
| 分支数量 | 多（main, develop, feature, release, hotfix） | 少（main + feature） | 极少（main + 短命 feature） |
| 发布节奏 | 定期/按版本 | 持续部署 | 持续部署（日多次） |
| 适合团队 | 传统发布周期 | SaaS / Web 应用 | 高成熟度 DevOps |
| 复杂度 | ⭐⭐⭐ | ⭐ | ⭐⭐ |
| 推荐场景 | 移动 App、嵌入式、游戏 | Web 后端/前端 | 微服务、平台团队 |

---

### 5.1 Git Flow（经典模型）

```
main     ★─────── ⋯ ───────── ●(v1.0)────────── ●(v1.1-hotfix)
           \                  /       \         /
develop    ●──●──●──●──●──●──●──●──●──●──●──●──●
                 \         /     \         /
feature/login     ●──●──●──       \       /
feature/payment    ●──●──●────────/       \
                          \               /
release/v1.0               ●──●──●──●────
                                   \
hotfix/v1.1                           ●──●
```

**分支角色：**

| 分支 | 用途 | 命名 | 合并到 |
|------|------|------|--------|
| `main` | 生产代码，每个 commit 对应一个发布版本 | `main` | — |
| `develop` | 开发主线，集成所有 feature | `develop` | — |
| `feature/*` | 新功能开发 | `feature/login`, `feature/payment` | `develop` |
| `release/*` | 发布准备（版本号、文档、bug 修复） | `release/v1.0.0` | `main` + `develop` |
| `hotfix/*` | 生产环境紧急修复 | `hotfix/v1.0.1` | `main` + `develop` |

**操作流程：**

```bash
# 1. 从 develop 创建 feature 分支
git checkout -b feature/user-avatar develop

# 2. 开发完成后合并回 develop
git checkout develop
git merge --no-ff feature/user-avatar   # --no-ff 保留分支历史
git branch -d feature/user-avatar

# 3. 从 develop 创建 release 分支
git checkout -b release/v1.0.0 develop
# 修版本号、更新 CHANGELOG、修小 bug...

# 4. release 合并到 main（打 tag）和 develop
git checkout main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release v1.0.0"
git checkout develop
git merge --no-ff release/v1.0.0
git branch -d release/v1.0.0

# 5. Hotfix：从 main 创建，修复后合并回 main 和 develop
git checkout -b hotfix/v1.0.1 main
# 修 bug...
git checkout main
git merge --no-ff hotfix/v1.0.1
git tag -a v1.0.1 -m "Hotfix v1.0.1"
git checkout develop
git merge --no-ff hotfix/v1.0.1
git branch -d hotfix/v1.0.1
```

**适用场景：**
- 有明确版本号的产品（移动 App、桌面软件、嵌入式固件）
- 发布周期较长（周/月级别）
- 需要维护多个并行版本

---

### 5.2 GitHub Flow（简化模型，推荐大多数团队使用）

```
main     ●──●──●──────────●──────────●────
          \     /         |          |
feature/A  ●──●──          |          |
                \          |          |
feature/B        ●──●──●───           |
                           \          |
feature/C                    ●──●──●──●
```

**核心原则：**
1. `main` 分支始终可部署
2. 从 `main` 创建描述性分支
3. 通过 Pull Request 合并
4. 合并后立即部署

```bash
# 1. 从 main 创建功能分支
git checkout main
git pull --rebase origin main
git checkout -b feature/add-dark-mode

# 2. 开发和提交
git add -p
git commit -m "feat(theme): add dark mode toggle"

# 3. 推送并创建 PR
git push -u origin feature/add-dark-mode
# 在 GitHub 上创建 Pull Request

# 4. Code Review 通过后合并（在 GitHub 上操作）
# 5. 删除远端和本地分支
git branch -d feature/add-dark-mode
```

**适用场景：**
- Web 应用、SaaS 服务
- 持续部署（CD）团队
- 小到中型团队

---

### 5.3 Trunk-Based Development（干线开发）

```
main     ●─●─●─●─●─●─●─●─●─●─●─●
          \/  |/  |/  |/  |/
feature    ●   ●   ●   ●   ●    （极短命分支，< 1天）
```

**核心原则：**
- 所有开发者直接向 `main`（trunk）提交
- 分支生命周期极短（几小时到一天）
- 大功能通过 **Feature Flag** 控制上线
- 高频集成，小步快跑

```bash
# 从 main 创建极短命分支
git checkout -b short-lived-feature main
# 实现一个很小的增量 → 提交 → 立即 PR → 合并
```

**配套实践：**
- **Feature Flag**：代码合入 main 但不激活，通过配置开关控制
- **Branch by Abstraction**：通过抽象层逐步替换实现
- 高频自动化测试和部署

**适用场景：**
- 成熟的 DevOps 团队
- CI/CD 基础设施完善
- 微服务架构

---

### 分支命名规范

```bash
feature/<模块>-<描述>    # feature/user-avatar-upload
bugfix/<issue-id>-<描述>  # bugfix/42-fix-login-timeout
hotfix/<版本>-<描述>      # hotfix/v1.2.1-critical-payment-fix
release/<版本>            # release/v2.0.0
chore/<描述>              # chore/update-dependencies
docs/<描述>               # docs/api-documentation
```

---

## 六、分支操作：创建、切换、合并、删除

### 基础命令

```bash
# 查看分支
git branch                 # 本地分支列表
git branch -r              # 远端分支列表
git branch -a              # 所有分支（本地 + 远端）
git branch -v              # 各分支最新提交

# 创建和切换
git branch feature/new     # 创建分支（不切换）
git checkout -b feature/new        # 创建并切换到新分支
git switch -c feature/new          # 同上（Git 2.23+，语义更清晰）
git checkout main                  # 切换到已有分支
git switch main                    # 同上（推荐）

# 删除分支
git branch -d feature/old          # 安全删除（已合并才允许）
git branch -D feature/old          # 强制删除（即使未合并）
git push origin --delete feature/old   # 删除远端分支

# 重命名分支
git branch -m old-name new-name    # 重命名当前分支
```

### 比较分支差异

```bash
git diff main..feature/login                   # 两个分支之间的差异
git diff main...feature/login                  # feature 自与 main 分叉后的差异（三点语法）
git diff --name-only main..feature/login       # 只列出变更的文件名
git diff --stat main..feature/login            # 统计每个文件的变更行数
```

---

## 七、合并策略：merge / rebase / squash

### 三种策略对比

```
初始状态:
main     A---B---C
              \
feature        D---E

═══════════════════════════════════════
策略 1: git merge (保留真实历史)
═══════════════════════════════════════
main     A---B---C-------M   (M 是 merge commit)
              \         /
feature        D---E───

✅ 优点: 完整保留分支历史和上下文
❌ 缺点: 历史图复杂，二分查找困难

═══════════════════════════════════════
策略 2: git rebase (线性历史)
═══════════════════════════════════════
main     A---B---C---D'---E'   (D', E' 是重新应用的提交)
feature              (原 D, E 被丢弃)

✅ 优点: 历史干净线性，git bisect 友好
❌ 缺点: 丢失了"这些提交来自 feature 分支"的上下文
        改写历史，多人协作分支慎用

═══════════════════════════════════════
策略 3: git merge --squash (单提交)
═══════════════════════════════════════
main     A---B---C---S   (S 包含了 D+E 的所有变更)
              
✅ 优点: main 分支极度干净，每个 commit 对应一个完整功能
❌ 缺点: 丢失了 feature 分支内的细粒度提交历史
```

### Merge 操作

```bash
# 标准 merge（如果可 fast-forward 则快进）
git merge feature/login

# 强制生成 merge commit（保留分支痕迹）
git merge --no-ff feature/login

# 合并但不自动提交（方便检查）
git merge --no-commit feature/login
```

### Rebase 操作

```bash
# 将当前分支 rebase 到 main 之上
git checkout feature/login
git rebase main

# 交互式 rebase（整理提交历史）
git rebase -i HEAD~3          # 整理最近 3 个提交
# 可执行: pick, reword, squash, fixup, drop, edit

# 如果产生冲突
git add <resolved-file>
git rebase --continue
# 或者放弃 rebase
git rebase --abort
```

**交互式 rebase 常用操作：**

| 命令 | 作用 |
|------|------|
| `pick` | 保留该提交（默认） |
| `reword` | 保留但修改提交信息 |
| `squash` | 合并到上一个提交，保留两个提交信息 |
| `fixup` | 合并到上一个提交，丢弃本提交信息 |
| `drop` | 删除该提交 |
| `edit` | 停下来让你修改该提交的内容 |

### Squash Merge（在 GitHub PR 上操作）

```
在 GitHub Pull Request 页面点击 "Squash and merge"
→ D 和 E 被压缩成一个提交 S，然后合并到 main
```

### 选择建议

| 场景 | 推荐策略 |
|------|----------|
| feature 分支 → main（多人协作） | **Squash merge**（GitHub 默认推荐） |
| feature 分支 → main（想保留细节） | `git merge --no-ff` |
| 同步 main 到 feature 分支 | `git rebase main` 或 `git merge main` |
| 个人分支整理提交 | `git rebase -i` |
| hotfix → main | `git merge --no-ff`（保留痕迹） |

---

## 八、冲突解决：原理与实战

### 冲突是怎么产生的

```
main       A---B---C---D  (修改了 config.js 第 10 行)
                \
feature          E---F    (也修改了 config.js 第 10 行)

合并时 Git 无法自动决定用哪个版本 → 冲突
```

### 解决冲突的标准流程

```bash
# 1. 触发冲突
git merge feature/login
# Auto-merging config.js
# CONFLICT (content): Merge conflict in config.js
# Automatic merge failed; fix conflicts and then commit the result.

# 2. 查看冲突文件
git status                    # 看哪些文件有冲突
git diff                      # 看冲突详细内容

# 3. 编辑冲突文件
```

冲突文件中的标记：

```
<<<<<<< HEAD
const PORT = 3000;           // main 分支的版本
=======
const PORT = 8080;           // feature 分支的版本
>>>>>>> feature/login
```

手动编辑为最终想要的版本：

```javascript
const PORT = 8080;  // 选择 feature 的版本，或手写新的
```

```bash
# 4. 标记为已解决
git add config.js

# 5. 完成合并
git commit -m "merge: resolve conflict in config.js"

# 如果是 rebase 中的冲突
git add config.js
git rebase --continue

# 6. 放弃合并（如果搞砸了）
git merge --abort
# 或
git rebase --abort
```

### 冲突预防策略

```bash
# 策略 1：频繁同步 main（减小冲突面）
git checkout feature/large-feature
git rebase main                    # 每天 rebase 一次 main

# 策略 2：小 PR（keep PRs small）
# PR 变更行数控制在 400 行以内

# 策略 3：模块化代码
# 不同功能的代码放在不同文件，减少同一文件被多人修改的概率
```

### VS Code 内置冲突解决工具

在 VS Code 中，冲突文件会高亮显示，点击：
- `Accept Current Change` — 采用当前分支的版本
- `Accept Incoming Change` — 采用合并分支的版本
- `Accept Both Changes` — 两者都保留
- `Compare Changes` — 并排对比

---

## 九、Pull Request 工程化流程

Pull Request 是现代软件开发的核心协作机制。

### PR 生命周期

```
创建分支 → 开发 → 推送 → 创建 PR → Code Review → 修改 → 审批 → 合并 → 部署 → 删除分支
   │                                           │                 │
   └── Draft PR（WIP）←─────────────────────────┘                 │
                                                                  └── 自动触发 CI/CD
```

### 标准操作流程

```bash
# Step 1: 同步 main 并创建功能分支
git checkout main
git pull --rebase origin main
git checkout -b feature/add-2fa

# Step 2: 开发，遵循 Conventional Commits
git add -p
git commit -m "feat(auth): add TOTP-based 2FA

- Generate TOTP secret on user registration
- Add QR code display for authenticator apps
- Validate TOTP code on login

Refs: #156"

# Step 3: 推送前整理提交历史
git rebase -i HEAD~3    # 将 3 个 commit 合并整理
# 或 rebase main 以同步最新代码
git fetch origin main
git rebase origin/main

# Step 4: 推送到远端
git push -u origin feature/add-2fa

# Step 5: 在 GitHub 上创建 Pull Request
# 可以通过 gh CLI 创建:
gh pr create \
  --title "feat(auth): add TOTP-based 2FA" \
  --body "## Summary
  Add Time-based One-Time Password 2FA support.

  ## Changes
  - Generate TOTP secret on user registration
  - Add QR code display for authenticator apps
  - Validate TOTP code during login

  ## Testing
  - [ ] Unit tests for TOTP generation
  - [ ] Integration test for login flow
  - [ ] Manual test with Google Authenticator

  ## Screenshots
  (attach screenshots)

  Closes #156" \
  --base main \
  --head feature/add-2fa

# Step 6: 根据 Review 意见修改
git add -p
git commit -m "fix(auth): address review - use constant-time comparison"
git push origin feature/add-2fa    # PR 自动更新

# Step 7: Review 通过后在 GitHub 上合并
# 合并后删除分支
git checkout main
git pull --rebase origin main
git branch -d feature/add-2fa
git push origin --delete feature/add-2fa   # 删除远端分支
```

### PR 描述模板

在仓库根目录创建 `.github/PULL_REQUEST_TEMPLATE.md`：

```markdown
## Summary
<!-- 简要描述这个 PR 做了什么 -->

## Motivation
<!-- 为什么需要这个变更？关联的 Issue -->

## Changes
<!-- 详细的变更列表 -->

- 
- 

## Testing
<!-- 如何测试这些变更 -->

- [ ] 单元测试通过
- [ ] 集成测试通过
- [ ] 手动测试步骤:

## Screenshots / Recordings
<!-- UI 变更请附带截图或录屏 -->

## Checklist
- [ ] 代码符合团队规范
- [ ] 添加了必要的测试
- [ ] 更新了相关文档
- [ ] CHANGELOG 已更新（如适用）

Closes #
```

### Draft PR（草稿 PR）

在 GitHub 上创建 PR 时选择 "Create draft pull request"，或者通过 CLI：

```bash
gh pr create --draft --title "WIP: refactor payment module"
```

Draft PR 适用于：
- 希望提前获得设计反馈
- 功能尚未完成但想让团队看到进度
- 触发 CI 运行但还不能合并

---

## 十、Code Review 规范与实践

### Review 者检查清单

#### 设计层面
- [ ] 代码是否实现了需求（对照 Issue / Spec）
- [ ] 架构是否合理，有没有更简单的方案
- [ ] 有没有过度设计（over-engineering）

#### 功能层面
- [ ] 边界条件是否处理（null、空数组、超时、并发）
- [ ] 错误处理是否完善（不要吞异常）
- [ ] 是否存在竞态条件（race condition）
- [ ] 安全性：SQL 注入、XSS、CSRF、权限校验

#### 代码质量
- [ ] 命名是否自解释（变量、函数、类）
- [ ] 函数是否单一职责（一个函数只做一件事）
- [ ] 是否有重复代码（DRY 原则）
- [ ] 魔法数字是否替换为命名常量
- [ ] 注释是否解释了"为什么"而非"是什么"

#### 测试与性能
- [ ] 是否有足够的测试覆盖（单元 + 集成）
- [ ] 数据库查询是否有 N+1 问题
- [ ] 是否引入了不必要的依赖

### Review 评论规范

```markdown
<!-- ✅ 好的 Review 评论 -->

<!-- 具体 + 建设性 + 解释原因 -->
nit: 建议将 `data` 重命名为 `userProfile`，更能表达这个变量的用途。

<!-- 提出问题而非命令 -->
这里处理了超时的情况吗？如果 API 在 30s 后返回可能会出问题。

<!-- 赞赏好的实践 -->
👍 这个错误处理写得很清晰，边界条件覆盖全面。

<!-- ❌ 坏的 Review 评论 -->

<!-- 模糊 -->
这段代码不好。

<!-- 人身攻击 -->
你怎么又写这种代码？

<!-- 纯主观 -->
我不喜欢这种写法。
```

### Review 分级标记

| 标记 | 含义 |
|------|------|
| `nit:` | 小建议，不阻塞合并（nitpick） |
| `suggestion:` | 具体改进建议 |
| `issue:` | 需要修复的问题 |
| `question:` | 疑问，需要作者澄清 |
| `praise:` | 赞赏 |
| `blocking:` | 必须修复才能合并 |

---

## 十一、Git 高级操作

### `git stash` — 暂存工作现场

```bash
git stash                    # 暂存当前所有修改
git stash save "WIP: refactoring user service"  # 带描述的暂存
git stash list               # 查看暂存列表
git stash pop                # 恢复最近一次暂存并删除记录
git stash apply stash@{1}    # 恢复指定暂存（保留记录）
git stash drop stash@{0}     # 删除指定暂存
git stash show -p stash@{0}  # 查看暂存的内容
```

**典型场景：** 你在 feature 分支开发到一半，需要紧急切到 main 修 bug。

```bash
git stash                    # 保存当前工作
git checkout main
git checkout -b hotfix/critical
# 修 bug ...
git checkout feature/login
git stash pop                # 恢复之前的工作
```

### `git cherry-pick` — 摘取提交

```bash
# 把某个提交"复制"到当前分支
git cherry-pick <commit-hash>
git cherry-pick abc1234 def5678    # 摘取多个提交
git cherry-pick abc1234..def5678   # 摘取一个范围的提交（不包含 abc1234）
git cherry-pick --no-commit <hash> # 摘取但不自动提交（可以修改）
```

**典型场景：** hotfix 的提交需要同时应用到 `main` 和 `develop`。

```bash
git checkout develop
git cherry-pick <hotfix-commit-hash>
```

### `git reflog` — 后悔药

```bash
git reflog
# 输出:
# abc1234 HEAD@{0}: commit: feat: add login
# def5678 HEAD@{1}: rebase (finish): returning to refs/heads/main
# 9012abc HEAD@{2}: reset: moving to HEAD~3

# 恢复被 reset --hard 删除的提交
git reset --hard HEAD@{2}
# 或恢复被删除的分支
git checkout -b recovered-branch HEAD@{5}
```

**reflog 记录所有 HEAD 的移动**，默认保存 90 天。即使 `git reset --hard` 或删除分支，也能找回。

### `git bisect` — 二分法定位 Bug

```bash
git bisect start                # 开始二分查找
git bisect bad HEAD             # 标记当前版本有 Bug
git bisect good v1.0.0          # 标记 v1.0.0 是好的
# Git 会自动 checkout 到中间的一个版本

# 测试这个版本...
# 如果有 Bug:
git bisect bad
# 如果没问题:
git bisect good

# 重复几次后 Git 会告诉你第一个引入 Bug 的提交
# 结束二分查找:
git bisect reset
```

### `git blame` — 查看代码作者

```bash
git blame src/app.js                    # 查看每行代码的作者和提交
git blame -L 10,50 src/app.js           # 只看第 10-50 行
git blame -L '/function login/' src/app.js  # 从 function login 开始看
git blame --ignore-rev <commit> src/app.js  # 跳过格式化提交
```

### `git revert` — 安全地撤销

```bash
# 创建一个新提交来撤销指定提交的变更（安全，不改写历史）
git revert abc1234
git revert abc1234..def5678    # 撤销一个范围的提交
git revert -m 1 <merge-commit> # 撤销 merge commit（1 表示保留第一个父提交）
```

### `git reset` — 重置历史

```bash
git reset --soft HEAD~1    # 撤销 commit，变更回到暂存区（最安全）
git reset --mixed HEAD~1   # 默认行为，变更回到工作区
git reset --hard HEAD~1    # 彻底删除 commit 和变更（危险！）

# reset 到远端状态（丢弃本地所有修改）
git reset --hard origin/main
```

### 查看历史的高级用法

```bash
git log --oneline --graph --all --decorate   # 最常用
git log -p                                    # 查看每个 commit 的 diff
git log -S "searchTerm"                       # 搜索引入/删除某段代码的提交
git log -G "regexPattern"                     # 正则搜索提交内容
git log --author="John"                       # 按作者过滤
git log --since="2024-01-01" --until="2024-01-31"  # 按时间过滤
git log main..feature --oneline               # feature 有但 main 没有的提交
git show <commit-hash>                        # 查看某次提交的详细信息
git show <commit-hash>:src/app.js             # 查看某个文件在某次提交时的内容
```

---

## 十二、Tag 与 Release 管理

### Tag（标签）

Tag 用于标记重要的版本点（如发布版本）。

```bash
# 轻量标签（只是一个指针）
git tag v1.0.0

# 附注标签（含作者、日期、说明，推荐）
git tag -a v1.0.0 -m "Release v1.0.0: new payment module"

# 给过去的提交打标签
git tag -a v0.9.0 -m "Beta release" abc1234

# 推送标签到远端
git push origin v1.0.0          # 推送单个标签
git push origin --tags            # 推送所有标签

# 删除标签
git tag -d v1.0.0                # 删除本地标签
git push origin --delete v1.0.0  # 删除远端标签

# 查看标签
git tag                          # 列出所有标签
git tag -l "v1.*"                # 按模式过滤
git show v1.0.0                  # 查看标签详细信息

# 基于标签创建分支（回退到旧版本修复 Bug）
git checkout -b hotfix/v1.0.1 v1.0.0
```

### 语义化版本（Semantic Versioning）

```
v<MAJOR>.<MINOR>.<PATCH>

MAJOR: 不兼容的 API 变更        (v1.0.0 → v2.0.0)
MINOR: 向后兼容的新功能          (v1.0.0 → v1.1.0)
PATCH: 向后兼容的 Bug 修复       (v1.0.0 → v1.0.1)
```

### GitHub Release

GitHub Release 基于 Tag，附带 Release Notes 和二进制附件。

```bash
# 通过 CLI 创建 Release
gh release create v1.0.0 \
  --title "v1.0.0 - Initial Release" \
  --notes-file CHANGELOG.md \
  --generate-notes                    # 自动从 PR 生成 Release Notes

# 上传二进制附件
gh release upload v1.0.0 ./build/app.exe ./build/app.dmg
```

### 自动生成 CHANGELOG

配合 Conventional Commits，使用工具自动生成：

```bash
# standard-version
npm install --save-dev standard-version
npx standard-version --release-as minor

# 或使用 git-cliff
# 基于 Conventional Commits 自动生成结构化的 CHANGELOG
```

---

## 十三、Git Hooks：自动化质量门禁

Git Hooks 是在特定 Git 事件发生时自动执行的脚本。

### Hooks 目录

```bash
ls .git/hooks/
# applypatch-msg.sample
# commit-msg.sample
# pre-commit.sample
# pre-push.sample
# pre-rebase.sample
# ...
```

### 常用 Hooks 场景

| Hook | 触发时机 | 典型用途 |
|------|----------|----------|
| `pre-commit` | `git commit` 之前 | lint、格式化、检查调试代码 |
| `commit-msg` | 编辑完提交信息后 | 校验提交信息格式（Conventional Commits） |
| `pre-push` | `git push` 之前 | 运行测试、检查未解决的冲突标记 |
| `post-checkout` | checkout 之后 | 安装依赖 |
| `post-merge` | merge 之后 | 安装/更新依赖 |

### 使用 Husky（推荐，跨平台）

原生 `.git/hooks/` 不能提交到仓库共享。**Husky** 解决了这个问题。

```bash
# 安装 Husky
npm install --save-dev husky

# 初始化
npx husky init
# 会在 .husky/ 目录下创建 hook 脚本

# 添加 pre-commit hook（提交前 lint）
# .husky/pre-commit
npx lint-staged

# 添加 commit-msg hook（校验提交信息）
# .husky/commit-msg
npx --no -- commitlint --edit $1

# 添加 pre-push hook（推送前跑测试）
# .husky/pre-push
npm test
```

### lint-staged（只检查暂存的文件）

```json
// package.json
{
  "lint-staged": {
    "*.{js,ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{css,md,json}": ["prettier --write"]
  }
}
```

### 完整前端项目 Hook 配置示例

```bash
# 项目根目录结构
# .husky/
#   pre-commit      → npx lint-staged
#   commit-msg      → npx commitlint --edit $1
#   pre-push        → npm test
# .commitlintrc.json
# {
#   "extends": ["@commitlint/config-conventional"]
# }
```

---

## 十四、CI/CD 集成（GitHub Actions）

### 基础概念

```
代码推送 → GitHub Actions 触发 → 运行 Workflow → 结果反馈
                                          │
                           ┌──────────────┼──────────────┐
                           ▼              ▼              ▼
                         Lint           Test          Build
                                           │
                                           ▼
                                         Deploy
```

### 示例：PR 检查 Workflow

在 `.github/workflows/pr-check.yml`：

```yaml
name: PR Check

on:
  pull_request:
    branches: [main, develop]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test -- --coverage
      - name: Upload coverage
        uses: actions/upload-artifact@v4
        with:
          name: coverage
          path: coverage/

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
```

### 示例：自动生成 Release Notes

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - run: npx changelogithub
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 分支保护规则（Branch Protection）

在 GitHub 仓库 **Settings → Branches → Add rule** 中配置：

| 规则 | 说明 |
|------|------|
| Require a pull request before merging | 必须通过 PR 合并 |
| Require approvals (≥1) | 至少 N 人审批 |
| Dismiss stale approvals | 有新提交时旧审批失效 |
| Require status checks to pass | CI 必须通过 |
| Require conversation resolution | 所有 Review 讨论必须解决 |
| Require linear history | 禁止 merge commit（只能 rebase/squash） |
| Do not allow bypassing | 管理员也要遵守以上规则 |

---

## 十五、.gitignore 与仓库管理

### .gitignore 最佳实践

```gitignore
# ===== 操作系统 =====
.DS_Store          # macOS
Thumbs.db          # Windows
desktop.ini        # Windows

# ===== 编辑器 =====
.vscode/
.idea/
*.swp
*.swo
*~

# ===== 依赖 =====
node_modules/
vendor/
.pnpm-store/

# ===== 构建产物 =====
dist/
build/
*.o
*.class

# ===== 环境变量 =====
.env
.env.local
.env.*.local

# ===== 日志 =====
*.log
logs/

# ===== 缓存与虚拟环境 =====
.cache/
__pycache__/
*.pyc
.venv/
env/
uv.lock.cache/

# ===== AI / 深度学习大模型产物 (严禁入库) =====
*.pt
*.pth
*.bin
*.safetensors
*.onnx
checkpoints/
wandb/
runs/
.tensorboard/
data/
datasets/

# ===== 测试 =====
coverage/
.nyc_output/

# ===== 临时文件 =====
tmp/
temp/
```

### 全局 .gitignore 与中文路径支持

```bash
# 1. 解决 Windows 终端中文文件名显示为类似 \345\205\250 的八进制乱码问题（极关键）：
git config --global core.quotepath false

# 2. 配置全局忽略（对所有仓库生效）
git config --global core.excludesfile ~/.gitignore_global

# ~/.gitignore_global 内容建议：
# .DS_Store
# Thumbs.db
# .vscode/
# *.log
```

### 仓库结构建议

```
my-project/
├── .github/
│   ├── workflows/           # GitHub Actions
│   │   ├── pr-check.yml
│   │   └── release.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md
│       └── feature_request.md
├── .husky/                  # Git Hooks (Husky)
│   ├── pre-commit
│   └── commit-msg
├── src/                     # 源代码
├── tests/                   # 测试
├── docs/                    # 文档
├── scripts/                 # 工具脚本
├── .gitignore
├── .editorconfig            # 编辑器配置（跨 IDE 统一）
├── .commitlintrc.json       # 提交信息校验
├── CHANGELOG.md
├── CONTRIBUTING.md          # 贡献指南
├── README.md
├── package.json
└── LICENSE
```

---

## 十六、SSH / Token 认证配置

### 方式一：SSH Key（推荐）

```bash
# 1. 生成 SSH Key
ssh-keygen -t ed25519 -C "your-email@example.com"
# 一路回车，默认保存在 ~/.ssh/id_ed25519

# 2. 启动 ssh-agent 并添加 Key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. 复制公钥
cat ~/.ssh/id_ed25519.pub
# 或 Windows PowerShell:
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard

# 4. 在 GitHub → Settings → SSH and GPG keys → New SSH Key 中粘贴

# 5. 测试连接
ssh -T git@github.com
# 输出: Hi username! You've successfully authenticated...

# 6. 使用 SSH 地址克隆
git clone git@github.com:user/repo.git

# 7. 将已有仓库从 HTTPS 改为 SSH
git remote set-url origin git@github.com:user/repo.git
```

### 方式二：Personal Access Token（HTTPS 场景）

```bash
# 1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
# 2. Generate new token，勾选 repo 权限
# 3. 复制 token（只显示一次！）

# 克隆时使用 token
git clone https://<TOKEN>@github.com/user/repo.git

# 配置凭证缓存（避免每次输入）
git config --global credential.helper cache      # 默认缓存 15 分钟
git config --global credential.helper "cache --timeout=3600"  # 1 小时

# Windows 上使用 Git Credential Manager（推荐）
git config --global credential.helper manager
```

### 方式三：GitHub CLI（Windows 最推荐）

```bash
# 安装 GitHub CLI: winget install GitHub.cli

gh auth login
# 按提示选择:
# ? What account do you want to log into? GitHub.com
# ? What is your preferred protocol for Git operations? HTTPS
# ? Authenticate Git with your GitHub credentials? Yes
# ? How would you like to authenticate GitHub CLI? Login with a web browser

# 之后 Git 操作会自动使用 CLI 凭证
gh auth setup-git
```

---

## 十七、故障排查与急救手册

### 场景 1：我不小心 commit 了一个大文件/敏感文件

```bash
# 如果还没 push
git reset --soft HEAD~1     # 撤销 commit，保留修改
# 编辑 .gitignore 添加该文件
git add .gitignore
git commit -m "chore: update .gitignore"

# 如果已经 push 了
git rm --cached <large-file>    # 从 Git 跟踪中删除但不删除本地文件
git commit -m "chore: remove large file from tracking"
git push origin main
# 注意！文件仍存在于历史中。如果需要彻底清除：
# 使用 git filter-repo 或 BFG Repo-Cleaner（见下方）

# 如果提交了敏感信息（密码、Token）
# 1. 立即修改/撤销泄露的凭证
# 2. 使用 git filter-repo 清除历史
# 3. 强制推送
git filter-repo --path secrets.txt --invert-paths
git push --force origin main
# 4. 通知团队成员重新 clone
```

### 场景 2：我把 commit 提交到了错误的分支

```bash
# 假设在 main 上做了 2 个 commit，但应该在 feature/login 上
git log --oneline -3                    # 记录下 commit hash
git checkout -b feature/login           # 基于当前 main 创建正确的分支
git checkout main                       # 切回 main
git reset --hard HEAD~2                 # 把 main 回退 2 个提交
git checkout feature/login              # 回到正确分支继续工作
```

### 场景 3：merge 到一半想放弃

```bash
git merge --abort       # 放弃 merge，回到 merge 前的状态
git rebase --abort      # 放弃 rebase，回到 rebase 前的状态
```

### 场景 4：push --force 后想撤回

```bash
# 找到被覆盖前的 commit
git reflog
# abc1234 HEAD@{0}: commit: fix: update config
# ...
# 9012abc HEAD@{5}: commit: feat: old feature    ← 被 force push 覆盖的提交

# 恢复
git reset --hard 9012abc
git push --force origin main     # 再次 force push 恢复到之前的状态
```

### 场景 5：远端分支被他人 force push 覆盖了

```bash
git fetch origin
# 如果你本地有未推送的提交，先备份
git branch backup-branch

# 强制同步到远端
git reset --hard origin/main
```

### 场景 6：.git 文件夹过大

```bash
# 分析仓库大小
git count-objects -vH

# 清理不可达对象
git gc --aggressive --prune=now

# 查找大文件
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '/^blob/ {print $3, $4}' | sort -rn | head -20

# 从历史中彻底删除大文件
git filter-repo --path-glob '*.zip' --invert-paths
```

### 场景 7：Windows 换行符问题 (CRLF vs LF)

```bash
# Windows 上配置（推荐）
git config --global core.autocrlf true
# checkout 时 LF → CRLF，commit 时 CRLF → LF

# macOS/Linux 上
git config --global core.autocrlf input
# commit 时 CRLF → LF，checkout 时不转换

# 项目中添加 .gitattributes
* text=auto
*.js text eol=lf
*.bat text eol=crlf
*.png binary
```

---

## 十八、工程化最佳实践清单

### 日常开发 Checklist

- [ ] 开始工作前 `git pull --rebase`
- [ ] 从 `main` 创建功能分支（不是从其他 feature 分支）
- [ ] 提交遵循 Conventional Commits 格式
- [ ] 每个提交粒度合理（一个逻辑变更 = 一个 commit）
- [ ] 推送前 `git rebase -i` 整理提交历史
- [ ] 推送前跑本地 lint 和 test
- [ ] PR 描述填写完整（背景、变更、测试、截图）
- [ ] 自己先 Review 一遍自己的 PR diff
- [ ] PR 保持小规模（< 400 行变更）
- [ ] 合并后删除远端和本地功能分支

### 团队规范建议

**1. 分支策略：** 推荐 GitHub Flow 作为起点，需要支持多版本时升级到 Git Flow。

**2. 提交规范：** 强制 Conventional Commits，通过 commitlint + Husky 自动化校验。

**3. 合并策略：** 推荐 Squash Merge（保持 main 干净）+ 本地 `git rebase -i` 整理。

**4. 分支保护：**
- `main` 分支：必须 PR + 至少 1 人审批 + CI 通过
- `develop` 分支：必须 PR + CI 通过
- 禁止直接 push 到受保护分支

**5. CI/CD 流水线：**
```
PR 创建 → Lint → Test → Build
    ↓
PR 合并 → Deploy Staging → E2E Test → Deploy Production
```

**6. 代码审查：** 每个 PR 至少 1 人审批，24 小时内响应 Review。

**7. 文档：** 每个仓库必须有 README.md、CONTRIBUTING.md、CHANGELOG.md。

### Git 配置推荐

```bash
# ~/.gitconfig

[user]
    name = Your Name
    email = your-email@example.com

[core]
    autocrlf = true                    # Windows
    editor = code --wait               # 使用 VS Code 作为 Git 编辑器
    excludesfile = ~/.gitignore_global

[alias]
    # 常用别名
    co = checkout
    br = branch
    ci = commit
    st = status
    lg = log --oneline --graph --all --decorate
    unstage = reset HEAD --
    last = log -1 HEAD
    undo = reset --soft HEAD~1

[fetch]
    prune = true                       # fetch 时自动清理已删除的远端分支

[pull]
    rebase = true                      # pull 时默认使用 rebase

[push]
    autoSetupRemote = true             # 自动设置上游分支

[init]
    defaultBranch = main               # 新仓库默认分支名为 main
```

---

## 附录：常用命令速查表

### 信息查看

| 命令 | 用途 |
|------|------|
| `git status` | 查看工作区状态 |
| `git log --oneline --graph --all` | 可视化提交历史 |
| `git diff` | 查看未暂存的变更 |
| `git diff --staged` | 查看已暂存的变更 |
| `git remote -v` | 查看远端地址 |
| `git branch -a` | 查看所有分支 |
| `git reflog` | 查看 HEAD 移动历史 |

### 本地操作

| 命令 | 用途 |
|------|------|
| `git add -p` | 交互式暂存 |
| `git commit -m "..."` | 提交 |
| `git commit --amend` | 修改最近一次提交 |
| `git stash` / `git stash pop` | 暂存/恢复工作区 |
| `git reset --soft HEAD~1` | 撤销 commit（保留修改） |
| `git reset --hard HEAD~1` | 彻底删除 commit |
| `git checkout -- <file>` | 放弃文件修改 |
| `git rebase -i HEAD~3` | 交互式整理提交 |

### 分支操作

| 命令 | 用途 |
|------|------|
| `git checkout -b <branch>` | 创建并切换分支 |
| `git switch -c <branch>` | 同上（Git 2.23+） |
| `git merge <branch>` | 合并分支 |
| `git rebase <branch>` | 变基到目标分支 |
| `git branch -d <branch>` | 删除本地分支 |
| `git cherry-pick <hash>` | 摘取提交 |

### 远端操作

| 命令 | 用途 |
|------|------|
| `git fetch <remote>` | 下载远端更新（不合并） |
| `git pull --rebase <remote> <branch>` | 下载并以 rebase 方式合并 |
| `git push <remote> <branch>` | 推送 |
| `git push -u <remote> <branch>` | 首次推送并设置上游 |
| `git push --force-with-lease` | 安全强制推送 |
| `git remote add <name> <url>` | 添加远端 |

### 故障恢复

| 命令 | 用途 |
|------|------|
| `git merge --abort` | 放弃合并 |
| `git rebase --abort` | 放弃变基 |
| `git reflog` | 查看操作历史（后悔药） |
| `git bisect` | 二分法定位 Bug |
| `git revert <hash>` | 安全撤销某次提交 |
| `git clean -fd` | 删除未跟踪的文件和目录 |

---

> **最后的话：** Git 是一个强大的工具，但真正重要的是团队对工作流程的共识。选好分支策略，统一提交规范，做好 Code Review，配上自动化 CI/CD — 这些工程实践比 Git 命令本身更能决定项目的成败。
