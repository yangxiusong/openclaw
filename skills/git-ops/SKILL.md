---
name: git-ops
description: Git 版本控制和代码仓库管理。使用场景：(1) 代码仓库初始化 (clone/init), (2) 分支管理 (branch/checkout), (3) 代码提交 (add/commit/push), (4) 合并和冲突解决 (merge/rebase), (5) 查看历史和状态 (log/status/diff), (6) 远程仓库管理 (remote/fetch/pull)。
---

# Git 版本控制操作

## 快速开始

本技能提供完整的 Git 工作流支持，从代码克隆到提交推送。

### 常用命令速查

```bash
# 克隆仓库
git clone <repo-url> [directory]

# 查看状态
git status

# 添加文件
git add <file>        # 单个文件
git add .            # 所有变更

# 提交变更
git commit -m "提交说明"

# 推送代码
git push origin <branch>

# 拉取代码
git pull origin <branch>
```

## 核心工作流

### 1. 仓库初始化

**克隆现有仓库**

```bash
git clone <repo-url>
cd <repo-directory>
```

**初始化新仓库**

```bash
git init
git remote add origin <repo-url>
```

### 2. 分支管理

**创建并切换分支**

```bash
git checkout -b <branch-name>
# 或
git switch -c <branch-name>
```

**查看分支**

```bash
git branch          # 本地分支
git branch -r       # 远程分支
git branch -a       # 所有分支
```

**合并分支**

```bash
git checkout main
git merge <branch-name>
```

### 3. 代码提交流程

**标准提交流程**

```bash
# 1. 查看变更
git status
git diff

# 2. 添加文件
git add <files>

# 3. 提交
git commit -m "type: description"

# 4. 推送
git push origin <branch>
```

**提交信息规范**

- `feat:` 新功能
- `fix:` 修复 bug
- `docs:` 文档更新
- `style:` 代码格式
- `refactor:` 重构
- `test:` 测试相关
- `chore:` 构建/工具

### 4. 远程操作

**查看远程仓库**

```bash
git remote -v
```

**同步远程变更**

```bash
git fetch origin     # 获取远程更新
git pull origin main # 拉取并合并
```

**推送分支**

```bash
git push -u origin <branch>  # 首次推送，设置上游
git push                     # 后续推送
```

### 5. 查看历史

**提交历史**

```bash
git log --oneline          # 简洁模式
git log --graph --oneline  # 图形化
git log -n 5               # 最近 5 条
```

**查看变更**

```bash
git diff HEAD~1            # 与上次提交对比
git diff <commit1> <commit2>
git show <commit>          # 查看某次提交
```

### 6. 撤销和回退

**撤销工作区修改**

```bash
git checkout -- <file>
# 或
git restore <file>
```

**撤销暂存**

```bash
git reset HEAD <file>
# 或
git restore --staged <file>
```

**撤销提交**

```bash
git reset --soft HEAD~1    # 保留变更
git reset --hard HEAD~1    # 丢弃变更
```

## 常见场景

### 场景 1: 开始新功能

```bash
git checkout main
git pull origin main
git checkout -b feature/new-feature
# ... 开发 ...
git add .
git commit -m "feat: 实现新功能"
git push -u origin feature/new-feature
```

### 场景 2: 代码审查后合并

```bash
git checkout main
git pull origin main
git merge feature/new-feature
git push origin main
git branch -d feature/new-feature  # 删除本地分支
git push origin --delete feature/new-feature  # 删除远程分支
```

### 场景 3: 解决冲突

```bash
git merge <branch>
# 出现冲突时：
# 1. 打开冲突文件，手动解决 <<<<<<< ======= >>>>>>> 标记
# 2. git add <resolved-file>
# 3. git commit -m "merge: 解决冲突"
```

### 场景 4: 临时保存工作

```bash
git stash              # 保存当前工作
git stash list         # 查看保存列表
git stash pop          # 恢复最近的工作
git stash apply        # 恢复但不删除
```

## 脚本工具

### scripts/git-status-summary.sh

快速查看仓库状态摘要（待创建）

### scripts/git-cleanup.sh

清理已合并的分支（待创建）

## 最佳实践

1. **小步提交** - 每次提交只做一件事
2. **清晰的提交信息** - 说明为什么做，而不是做了什么
3. **及时推送** - 避免本地积压太多提交
4. **定期同步** - 每天至少 pull 一次 main 分支
5. **分支命名** - `feature/xxx`, `fix/xxx`, `hotfix/xxx`
6. **推送前检查** - `git status`, `git diff`, 运行测试

## 故障排查

### 问题：推送被拒绝

```bash
# 原因：远程有新提交
git pull --rebase origin main
git push
```

### 问题：提交错了分支

```bash
# 1. 撤销提交但保留变更
git reset --soft HEAD~1

# 2. 切换到正确分支
git checkout <correct-branch>

# 3. 重新提交
git commit -m "..."
```

### 问题：误删了分支

```bash
# 查看 reflog
git reflog

# 恢复分支
git checkout -b <branch-name> <commit-hash>
```

## 相关技能

- `code-review` - 代码审查和优化
- `project-scaffold` - 项目脚手架生成
- `docker-ops` - Docker 容器管理

---

**最后更新：** 2026-03-14  
**维护者：** 小灵 🧭
