# 本地项目 ↔ GitHub 同步速查表

&gt; 适用于已安装 Git（≥2.30）并注册 GitHub 账号的场景。  
&gt; 所有命令均在 **项目根目录** 内执行；Windows 可用 Git Bash。

---

## 1. 首次上传（本地已有代码 → 新建空仓库）

| 步骤              | 命令 / 操作                                                               |
| --------------- | --------------------------------------------------------------------- |
| ① 在 GitHub 新建仓库 | `+` → New repository → **不勾选** README                                 |
| ② 初始化本地仓库       | `git init`                                                            |
| ③ 添加并提交文件       | `git add .` &lt;br&gt; `git commit -m "first commit"`                 |
| ④ 关联远程          | `git remote add origin https://github.com/&lt;用户名&gt;/&lt;仓库&gt;.git` |
| ⑤ 推送            | `git branch -M main` &lt;br&gt; `git push -u origin main`             |

---

## 2. 日常更新（文件改动后再上传）

| 步骤         | 命令                   |
| ---------- | -------------------- |
| 查看状态       | `git status`         |
| 加入暂存区      | `git add .` （或指定文件）  |
| 提交本地版本     | `git commit -m "描述"` |
| 推送到 GitHub | `git push`           |

---

## 3. 多仓库切换

| 要点          | 说明                                     |
| ----------- | -------------------------------------- |
| 一个目录 = 一个仓库 | 每个目录下都有 `.git` 文件夹                     |
| 换仓库就 `cd`   | `cd ../另一个项目` 后再执行 add / commit / push |
| 查看远程地址      | `git remote -v`                        |

---

## 4. 常见报错速解

| 报错提示                                    | 一键修复                                                               |
| --------------------------------------- | ------------------------------------------------------------------ |
| `refusing to merge unrelated histories` | `git pull origin main --allow-unrelated-histories`                 |
| `src refspec main does not match any`   | 本地还没任何提交，先 `git add` + `git commit`                                |
| 403 / 密码失败                              | 改用 [Personal Access Token](https://github.com/settings/tokens) 当密码 |

---

## 5. 网页直传（无命令行）

1. 打开仓库 → `Add file` → `Upload files`  
2. 拖拽文件 → 填写 Commit message → `Commit changes`  
3. 单文件 ≤ 25 MB（开源库）/ 100 MB（付费），单次最多 100 个文件

---

复制保存为 `git-cheat-sheet.md`，随时查阅。



commit相当于把修改完的内容保存到本地仓库，如果不commit只修改内容，那么改文件还在工作区，删了就没了，但是在本地仓库，删了还可以通过reflog找回来，再push就能到远程仓库了
