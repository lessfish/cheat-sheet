|    命令     |                    作用                    |
| :-------: | :--------------------------------------: |
|    cd     | `cd -` 退回到切换前的目录；直接 `cd` 效果同 `cd ~` 或者直接 `~`，进入 home 目录 |
|   mkdir   |                创建新目录（文件夹）                |
|   rmdir   |                 删除（空）文件夹                 |
| mvdir a b |                移动或者重命名目录                 |
|  cp a b   |    复制 a 文件到 b 文件（如果复制文件夹，需要加上 -R 参数）     |
|   rm a    |        删除文件（如果是删除文件夹，需要加上 -rf 参数）        |
|  mv a b   |             移动（重命名）文件 & 文件夹              |
|    pwd    |                  显示当前路径                  |
|   touch   |                   新建文件                   |
|    cat    |                  显示文件内容                  |
|    ls     | `ls -a` 列出目录下的所有文件，包括隐藏文件；`ls -l` 列出文件的详细信息 |
|    nl     | `nl file1 > file2` 给 file1 加上行号后，写入 file2 中 |
|  source   | 修改完配置文件后，使用该命令使之生效（比如经常使用的 `source  ~/.zshrc`） |
|   head    |      `head -20 filename`  显示文件开始几行       |
|   tail    |       `tail -20 filename` 显示文件末尾几行       |
|  ps/kill  |                 查看／杀死 进程                 |
|   find    | `find . -name "*.c" (-print)`  当前目录下查找 .c 后缀的文件 |
|   grep    | `grep thanks a.txt` 显示 a.txt 文件中带有 thanks 的行；还支持递归搜索，加上 -r 参数，可以搜索文件夹或者子文件夹下的包换 xx 的内容； 加上 -c 参数，则可以统计数量 |
|     z     |  `z filename` 快速到达指定目录，插件勾选在 .zshrc 文件   |
|  history  | `history | grep git`  查看包含 "git" 的命令行历史记录 |

