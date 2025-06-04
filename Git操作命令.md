```bash
# git开启代理
git config --global http.proxy http://127.0.0.1:7890（7980替换为自己的端口号）
# 保存配置
ipconfig /flushdns
# 取消代理
git config --global --unset http.proxy
```

```bash
git init # 初始化仓库
git config --global user.name "你的名字" # 设置全局的Git用户名
git config --global user.email "你的邮箱" # 设置全局的Git邮箱
git clone <仓库URL>
git remote add <远程仓库名> <仓库URL>
git remote -v # 查看当前配置的远程仓库
git remote remove <远程仓库名>
git status # 查看工作区和暂存区的状态
git add <文件名> # 将当前文件的更改从工作区添加到暂存区
git add . 或 git add -A # 将当前目录下所有已修改或新增的文件
git commit -m "提交说明" # 将暂存区中的所有更改提交到本地仓库，并附带一条说明性的消息。
git push <远程仓库名> <本地分支名> # 提交到远程仓库
git branch # 列出所有本地分支
git branch <新分支名> # 创建一个新的本地分支
git checkout -b <新分支名> (较旧的用法) 或 git switch -c <新分支名> (较新的推荐用法) # 创建并立即切换到一个新的本地分支
git merge <要合并的分支名> # 将指定分支的更改合并到当前分支
git fetch <远程仓库名> # 从指定的远程仓库下载最新的提交历史和分支信息到本地，但不会自动合并到你当前的工作分支。
git pull <远程仓库名> <远程分支名> # 相当于 git fetch 之后紧接着执行 git merge。它会从远程仓库拉取最新的更改并尝试自动合并到你当前的本地分支。

git checkout -- <文件名> # 撤销工作区的修改 (还未 git add)
git reset HEAD <文件名> # 撤销暂存区的修改 (已经 git add，但还未 git commit)
git commit --amend # 撤销提交 (还未 git push 到共享远程仓库)
git commit --amend --no-edit # --no-edit 表示不修改原提交信息
git reset HEAD~<数字> # 撤销最近的N次提交，并将这些提交的更改放回工作区，同时取消暂存。
git reset --soft HEAD~<数字> # 撤销最近的N次提交，但将这些提交的更改保留在暂存区。你可以重新组织这些更改后进行一次新的提交。
git reset --hard HEAD~<数字> # 彻底撤销最近的N次提交，同时删除工作区和暂存区中这些提交带来的所有更改。这是一个危险的操作，一旦执行，相关的更改很难恢复，请务必谨慎使用，并确保你知道自己在做什么！
git revert <commit-hash> # 创建一个新的提交，这个新的提交会撤销指定 <commit-hash> 的提交所做的更改。
git push <远程仓库名> <分支名> --force 或 git push <远程仓库名> +<分支名> # 强制推送本地分支覆盖远程分支。这是一个非常危险的操作，尤其是在多人协作的项目中！ 



```

