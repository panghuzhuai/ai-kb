# Git 常用命令片段

## 基础操作

```bash
# 初始化
git init
git clone <url>

# 暂存与提交
git add .
git commit -m "feat: description"
git commit -am "feat: description"

# 查看状态
git status
git diff
git log --oneline -10
```

## 分支操作

```bash
# 创建并切换
git checkout -b feature/new-feature

# 切换分支
git checkout main

# 删除分支
git branch -d feature/old
git branch -D feature/old  # 强制删除

# 合并分支
git merge feature-branch
```

## 暂存工作

```bash
# 暂存当前工作
git stash
git stash push -m "message"

# 恢复暂存
git stash pop
git stash apply

# 查看暂存列表
git stash list
```

## 撤销操作

```bash
# 撤销文件修改
git checkout -- file.txt
git restore file.txt

# 撤销提交（软撤销）
git reset --soft HEAD~1

# 撤销提交（硬撤销）
git reset --hard HEAD~1

# 回退到指定提交
git reset --hard abc123
```

## 高级操作

```bash
# 查看被删除的文件
git log --diff-filter=D --name-only | head -20

# 恢复删除的文件
git checkout HEAD -- path/to/file

# 挑选提交
git cherry-pick abc123

# 查看远程分支
git branch -r

# 清理远程已删除的分支
git fetch --prune
```

## 配置

```bash
# 设置用户信息
git config user.name "Your Name"
git config user.email "your@email.com"

# 设置别名
git config alias.co checkout
git config alias.br branch
git config alias.st status
```
