# Git Reset 完整流程速查表

| 目标             | 命令序列                                                                                            | 影响范围   |
| -------------- | ----------------------------------------------------------------------------------------------- | ------ |
| **仅改提交说明**     | `git reset --soft HEAD~1` &lt;br&gt; `git commit -m "新说明"`                                      | 只动本地仓库 |
| **重新整理暂存区**    | `git reset HEAD~1` &lt;br&gt; `git reset HEAD 文件` &lt;br&gt; `git commit -m "新提交"`              | 仓库+暂存区 |
| **彻底回退（代码不要）** | `git reset --hard &lt;commit_id&gt;` &lt;br&gt; `git push origin &lt;分支&gt; --force-with-lease` | 三线全回退  |

## 安全步骤

1. `git reflog` → 找提交号  
2. `git branch backup` → 先备份  
3. 公共分支避免 `--hard + force`

## 口诀

soft 留代码，mixed 重挑文件，hard 全清空；  
先 reflog，后 force-with-lease，reset 不踩坑。



### ✅ 场景 1：刚 commit，发现注释写错 → `--soft`

git reset --soft HEAD~1
git commit -m "正确的注释"

*影响范围：仅本地仓库；暂存区、工作区纹丝不动。*



### ✅ 场景 2：commit 里多加了文件，想重新挑 → 默认 `--mixed`



git reset HEAD~1                # 回到上一次提交，改动全部回到暂存区
git reset HEAD 多余文件          # 把不需要的文件踢出暂存区
git commit -m "干净的提交"

*影响范围：本地仓库 + 暂存区；工作区代码仍在，可重新挑选。*



### ✅ 场景 3：私有分支（个人学习/PR 分支）需要彻底回退 → `--hard` + `force-with-lease`



git branch backup               # 先留备份
git reset --hard <目标commit>    # 三线统一回退
git push origin feature --force-with-lease





*满足“干净历史”需求，同时用 `--force-with-lease` 防止覆盖他人提交。*










