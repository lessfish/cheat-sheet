## 一图胜千言

![](../images/git.png)

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

## 基本命令

| 命令                                             | 作用                                                         |
| :----------------------------------------------- | :----------------------------------------------------------- |
| git status                                       |                                                              |
| git add .                                        |                                                              |
| git commit -m 'xxx'                              |                                                              |
| git commit --amend -m "YOUR-NEW-COMMIT-MESSAGE"  | commit 信息写错了，修改 // 或者直接用 git commit —amend 打开一个 vi 交互窗口进行修改 |
| git push origin master                           |                                                              |
| git push -u origin master                        | 将本地分支与远程分支关联，之后仅需要使用 git push 即可       |
| git push -f origin master                        | 远程仓库更新，但是你还是想用老版本覆盖，强制（-f）push       |
| git pull                                         | fetch + merge                                                |
| git pull origin master                           | 之前如果 push 时用过 -u，那么就可以省略为 git pull           |
| git log                                          | 查看历史 commit 记录                                         |
| git log --pretty=oneline                         | 查看历史提交记录，一行显示，也可以 git log —oneline 显示更清爽（短的 commit hash） |
| git log --pretty=oneline --abbrev-commit         | 同上，commit_id 字符串变短点                                 |
| git log --graph                                  | 查看分支合并图                                               |
| git log --graph --decorate --abbrev-commit --all |                                                              |
| git log -p \<filename\>                          | 查看具体文件的修改历史                                       |
| git reflog                                       | 查看命令历史，以便确定回退版本号                             |
| git reset --hard commit_id                       | 回到过去某个版本，这时如果要 push，是 push 不到 origin 的，因为远端版本更新，可以用 `git push -f` 命令。如果代码已经 push 到远端了，不建议用该命令（可以用 revert 代替），只是在本地 commit 过了的情况下用，默认是 --mixed 选项，可以选择 —soft 或者 --hard，只有 —hard 会回滚代码 |
| git revert \<commit-id\>                         | git reset --hard 命令会丢失一些版本，如果用 git revert 命令，可以将某次操作反向操作一次，并新增一条 commit 记录 |
| git checkout -- \<filename\>                     | 丢弃修改，针对还没 add 到 stage 的部分，貌似可以省略 --      |
| git reset HEAD \<fileName\>                      | 如果文件已经 commit，先用该命令回到工作区，然后用上面的命令取消修改 |
| git update-index --assume-unchanged \<filename\> | make Git not to track this file anymore. 如果要恢复的话，用  git update-index --no-assume-unchanged \<filename\> |
| git stash                                        | 储藏，没有 commit 过的修改内容就会被储藏起来，切换分支时就不会将修改带过去 |
| git stash pop                                    | 恢复                                                         |
| git show \<commit-id\>                           | 查看某次提交的具体变化                                       |
| git clean -xdf / git clean -f                    | 去除所有修改（新增文件、修改等等，甚至会根据 .gitignore 去删除 node_modules 等） |

## 分支相关

| 命令                                       | 作用                             |
| :--------------------------------------- | :----------------------------- |
| git branch                               | 查看本地分支                         |
| git branch -r                            | 查看远程分支                         |
| git branch -a                            | 查看本地和远程分支                      |
| git branch dev                           | 新建分支（从当前所在分支新建）                |
| git checkout dev                         | 切换分支                           |
| git checkout -b dev                      | 新建并切换分支                        |
| git checkout -b dev origin/dev           | 从远端抓取 dev 分支到本地并切换             |
| git branch -d dev                        | 删除分支                           |
| git merge dev                            | 命令用于合并指定分支到当前分支                |
| git reflog --date=local \| grep \<branchname\> | 查看分支是基于哪个分支 checkout 的（拉到最后查看） |

## tags

| 命令                     | 作用                         |
| :--------------------- | :------------------------- |
| git tag                | 查看所有标签                     |
| git tag v1.0           | 打标签                        |
| git tag v2.0 4632917   | 在某一个 commit （默认为 HEAD）上打标签 |
| git tag -d v2.0        | 删除标签                       |
| git show v1.0          | 查看标签具体信息                   |
| git push origin v1.0   | 将标签推送到远程                   |
| git push origin --tags | 一次性推送尚未推送到远程的本地标签          |

## reset

| 命令                             | 作用                                       |
| :----------------------------- | :--------------------------------------- |
| git reset HEAD .               | 不小心 add 了，可以回到过去（将所有 add 的东西都取消），如果需要取消个别文件的 add，可以用 git reset HEAD index.html |
| git reset                      | 将 add 到缓冲区的部分全部回退到工作区                    |
| git reset --hard HEAD^         | 不小心 commit 了，该命令回到过去                     |
| git reset \<file-name\>        | 重置暂存区的文件，与上一次 commit 保持一致，但工作区不变         |
| git reset --hard \<file-name\> | 重置暂存区的文件，和上一次 commit 保持一致                |
| git reset --soft               |                                          |
| git rebase -i HEAD~4           | 多个 commit 合并为 1 个                        |
