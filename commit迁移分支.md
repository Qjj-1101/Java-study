### ✅ 场景 1：commit 错分支（还没 push）

**目标**：把**最新一次 commit**从当前分支“剪”下来，贴到另一个分支。

# 1. 先记住这次提交的哈希（copy 一下）

git log --oneline -1

# 2. 回到目标分支（没有就新建）

git switch 目标分支        # 或 git switch -c 目标分支

# 3. 把刚才的 commit 搬过来

git cherry-pick <刚才的哈希>

# 4. 回到原分支删掉那次提交

git switch 原分支
git reset --hard HEAD~1





### ✅ 场景 2：commit 已 push，只想“复制”过去



# 1. 切到目标分支

git switch 目标分支

# 2. 直接复制指定提交

git cherry-pick <commit哈希>





### ✅ 场景 3：批量迁移多个连续 commit（交互式 rebase）



# 1. 基于起点创建临时分支

git switch -c temp 起点哈希

# 2. 把一段提交搬过来

git rebase --onto temp 起点哈希^ 终点哈希

# 3. 再把 temp 合并到真正目标分支

git switch 目标分支
git merge temp


