# StudyCppProject

- C/C++ 开发编码时的防御和习惯优于调试和生成问题排查。

## git

### git 基础命令

```sh
# 查看日志
git log --oneline
git log --graph

# 修订提交
# 把当前的提交和上一次的 commit 合并，可以重新编辑 commit message
git commit --amend
# 把当前的提交和上一次的 commit 合并，不需要重新编辑 commit message
git commit --amend --no-edit

# 把当前的修改放到栈中，可以多次执行
git stash
# 出栈
git stash pop

# 添加
git add -A .
git commit -m "feat: xxx"
git push
git push -f  # 强推，有风险，最好是只用在自己一个人开发中的功能分支中


# 分支
git checkout -b mybranch1  # 基于当前分支创建一个新的分支
git checkout -b localBranch origin/remoteBranch  # 基于远程分支创建一个新的分支
git chekcout -b localBranch tagName # 基于 tag 创建一个新的分支
git branch  # 查看分支
git branch -a  # 查看所有分支
git branch -r  # 查看远程分支
git checkout mybranch1  # 切换到某个分支

# 回退到某个提交点，默认就是 --mixed 模式
git reset [commit-id]
git reset --soft/--mixed/--hard [commit-id]

# 如果文件被删除了，想要恢复，可以先用 git reflog 查看，再用 git reset 恢复
git reflog
git reset [commit-id]
```

- 鼓励用修订提交。
- 以功能点提交。
- 修改代码前先拉下代码。
- 提交代码前先拉下代码。
- 最好每天都先从主分支拉取最新的代码，然后解决冲突，再开发新的功能。

### git 规范

**git 分支规范**：

修改代码需要从主干分支拉一个新的分支出来进行开发。

- 分支名字规范：
  - mdgsf/feat/addTxtForDemo
  - mdgsf/fix/xxx
  - mdgsf/chore/xxx

在功能分支的开发上会有多个提交记录，当要把功能分支合并到主分支的时候，希望只有
一个提交记录。可以用 `git rebase` 把多个提交合并为一个。这样做的话，主分支的
提交记录就会非常的干净，都是一个个的功能点。

```sh
# HEAD 表示最新的记录，~3表示往前推3个提交记录。
git rebase -i HEAD~3
```

当功能分支和主分支差别较大时，已经很难直接用 `git merge` 合入。这个时候，可以
考虑使用 `git cherry-pick`，可以选择性的合并一些功能。

```sh
# 先切换到本地主分支
git checkout master
# 然后挑选一些功能分支的提交记录，合并到主分支。
git cherry-pick [commit-id] [commit-id] [commit-id]
```

每次拉代码的时候，自动变基。应该是本地主分支拉取代码时，如果直接用 git pull，
会多一个 merge 记录，所以需要用这个命令。如果是功能分支，只有一个人开发，则没有
这个问题。

```sh
git pull --rebase
```

**git commit message规范**：

- feat(feature): 添加了新特性。
- fix(bug): 修复了 bug。
- hotfix: 补丁包。
- chore: 修改了些单词，错别字。
- test: 添加/修改了测试
- docs: 添加/修改了文档。
- merge: 从其它分支合并过来。
- revert: 回退。

[Git Commit 完整规范]: https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html

## gdb

### gdb 基础使用

## vscode 调试代码

## 优秀项目

- smallchat: <https://github.com/antirez/smallchat>

## Book

- C++ Core Guideline
- C++ 之旅
