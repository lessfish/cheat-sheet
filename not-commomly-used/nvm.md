最好先删除全局的 node 和 npm，以及全局的 node_module，以便可以按照具体版本安装不同的 node_module

nvm 不能通过 homebrew 安装，不然出错（详见文档）

nvm 安装后，修改 .zshrc 或者 .bashrc 文件

运行 nvm 或者 command nvm -v 命令来查看 nvm 是否安装成功

如果没生效，可以用 source ~/.zshrc 命令使得文件生效

- nvm install v7.9.0
- nvm use v7.9.0
- nvm ls
- nvm ls-remote
- nvm alias default v8.0.0 // 将 v8.0.0 当作命令行打开时的环境的默认版本
