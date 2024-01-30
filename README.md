# StudyCppProject

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

## make

```sh
make  # 编译 Makefile 项目
make -j 8  # 开8个进程同时编译
```

## netstat

```sh
netstat -anop | grep [xxx]
-t tcp
-u udp
-x unix
-a 显示所有的 socket
-n 显示IP地址，而不是显示域名
-p display PID/Program name for sockets
```

## lsof

```sh
# 列出系统中当前打开的网络连接的相关信息
lsof -i -Pn
```

- `-i`：这个选项指定显示网络相关的信息。它会列出所有打开的网络连接，
  包括TCP、UDP和UNIX域套接字。
- `-P`：这个选项指定以原始的端口号和 `IP` 地址格式显示网络连接的信息，
  而不进行反向解析。默认情况下，`lsof` 会尝试通过反向解析显示 IP 地址和端口号的
  名称，但是使用 `-P` 选项可以绕过这个解析过程。
- `-n`：这个选项指定不要尝试将网络地址和端口号的名称解析为主机名和服务名。
  默认情况下，`lsof` 会尝试解析网络连接的地址和端口号，
  使用 `-n` 选项可以禁用这个解析过程。

## nc

```sh
# nc -v ip port 用于连接服务器
nc -v 127.0.0.1 8888
```

## gdb

gdb 调试的时候，一般开 2 个窗口，一个看源码，一个执行 gdb 调试。

### gdb 基础使用

- 编译的时候需要添加 `-g -O0` 参数。

```sh
gdb exe_program  # gdb 调试某个程序
gdb --args exe_program arg1 arg2 arg3 ... # 指定参数
gdb attach [pid]  # gdb 调试某个运行中的进程

start  # 开始运行，然后自动在 main 函数最开始的地方停下来
run  # 运行程序
run arg1 arg2 arg3 .... # 带参数运行
bt(backtrace)  # 查看堆栈
frame n  # 切换栈帧
list  # 查看代码
list 10  # 查看第10行附近的代码
b(break) [function_name]  # 给某个函数添加断点
b [line_number]  # 给当前文件的指定行添加断点
b [file_name]:[line_number]  # 给指定文件的指定行添加断点
b [namespace]::[namespace]::[classname]::[function_name]  # C++下添加断点
i b / info b  # 查看所有断点信息
tb(tbreak) xxx # 添加临时断点，只会执行一次，用法和 break 一样
disable [breakpoint-id]  # 禁用指定断点
enable [breakpoint-id]  # 启用指定断点
delete [breakpoint-id]  # 删除某个断点
delete  # 删除所有断点
r(run)  # 运行
n(next)  # 运行下一行语句
c(continue)  # 运行到下一个断点
s(step)  # 进入函数
f(finish)  # 结束当前函数的执行，会把整个函数执行完
return  # 从当前停的这个位置，直接结束函数的执行，后面的部分就不会执行了
return value  # 可以带返回值
u(until) [line_number]  # 一直运行到指定的行才停下来
print variable_name  # 查看变量信息
print array@n  # 查看数组的前 n 个元素
print array@20  # 查看数组的前 20 个元素
kill  # 让 gdb 与运行中的程序脱离，并停止对程序文件的使用。
quit  # 退出 gdb
```

如果在 gdb 调试的过程中，我们修复了 bug，然后希望重新调试，但是又不希望退出
gdb，因为我们已经在 gdb 里面添加了很多断点。可以这么操作：先执行 `kill` 命令，
然后替换二进制文件，再执行 `run` 重新运行程序。

### gcore

## vscode 调试代码

`.vscode/c_cpp_properties.json` 文件内容：

```json
{
  "configurations": [
    {
      "name": "Linux",
      "includePath": [
        "/usr/include",
        "${workspaceFolder}"
      ],
      "defines": [],
      "compilerPath": "/usr/bin/gcc",
      "intelliSenseMode": "gcc-x64",
      "browse": {
        "limitSymbolsToIncludedHeaders": true,
        "databaseFilename": "",
        "path": [
          "${workspaceFolder}"
        ]
      }
    }
  ],
  "version": 4
}
```

`.vscode/launch.json` 文件内容：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "(gdb) Bash on Windows Launch",
      "type": "cppdbg",
      "request": "launch",
      "program": "/path/to/exe_program",
      "args": [],
      "stopAtEntry": false,
      "miDebuggerArgs": "",
      "cwd": "/path/to/working-directory/",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "sourceFileMap": {
        "/home/dev/git/StudyCppProject/": "/home/dev/git/StudyCppProject/"
      },
      "pipeTransport": {
        "debuggerPath": "/usr/bin/gdb",
        "pipeProgram": "/usr/bin/bash",
        "pipeArgs": [
          "-c"
        ],
        "pipeCwd": ""
      },
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ]
    },
  ]
}
```

`.vscode/tasks.json` 文件内容：

```json
{
  "version": "2.0.0",
  "options": {
    "cwd": "/path/to/working-directory/"
  },
  "tasks": [
    {
      "label": "cmake",
      "type": "shell",
      "command": "cmake",
      "args": [
        "."
      ]
    },
    {
      "label": "make",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "dependsOn": [
        "cmake"
      ],
      "command": "make",
      "args": []
    },
    {
      "label": "Build my project",
      "dependsOn": [
        "make"
      ]
    }
  ]
}
```

## 基础编程能力

- 线程
  - `thread`
  - `mutex`
  - `shared_mutex`
  - `condition_variable`
  - `semaphore`
- 文件
  - open, close, read, write
  - 目录
- 设计模式、设计思想

## 防御式编程

- C/C++ 开发编码时的防御和习惯优于调试和生成问题排查。

```sh
char currentDir[256] = {0};
getcwd(currentDir);
currentDir[255] = '\0';  // 防止溢出
std::string strCurrentDir(currentDir);
```

```sh
std::string(const char* s, size_t n);
std::string(size_t n, char c);

# 这个会导致调用到第二个构造函数
char c = 'a';
size_t n = 16;
std::string str(c, n);
```

## C++ 编码规范

`Google C++ Styles`: <https://google.github.io/styleguide/cppguide.html>

- 使用 `clang-format`，参考该仓库中的 `.clang-format` 文件。

## 优秀项目

### smallchat

- smallchat: <https://github.com/antirez/smallchat>

#### smallchat-server

- 程序结构
  - 初始化
  - 一个 while 循环，在 while 循环不断检测包括监听 fd 在内的读事件，如果是监听
    fd，则接受连接，并将接受的 fd 挂到 select 函数，如果是普通 fd，处理读事件
    （收数据）。
- 一个连接 fd 对应一个 client 对象。
- 管理所有 client 的对象，`struct chatstate* Chat`
- 知识点：select 函数的用法
  - select 函数的第一个参数
  - select 函数的第二三四参数
  - select 函数的第五参数
  - select 函数的返回值
  - `FD_ZERO`，`FD_SET`，`FD_ISSET`

#### smallchat-client

- 程序结构
  - 初始化：选择一个本机可用 IP 地址，并通过用户传进来的端口号建立连接。
  - 使用一个 while 循环，将 connectfd 和 stdin-fd 挂载到 select 函数上，
    判断读事件。如果是 connectfd 的读事件，则从服务端收取数据。如果是 stdin-fd
    的读事件，则从控制台收集数据。

## cmake 编译

```sh
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j8
# make VERBOSE=1
make install
```

## mysql

```sh
# ubuntu 安装 mysql
sudo apt install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev

# 启动数据库
sudo service mysql start
sudo service mysql stop
sudo service mysql restart  # 重启数据库
sudo service mysql status  # 查看数据库是否启动
netstat -anop | grep 3306  # 查看数据库端口是否在监听中

# 设置 mysql 安全设置
sudo mysql_secure_installation

# 登录 mysql
mysql -h 127.0.0.1 -P 3306 -u root -p
mysql -h 127.0.0.1 -P 3306 -u root -p <database>
```

## Book

- C++ Core Guideline
- C++ 之旅
- Linux 系统编程
- Windows 程序设计（第五版）
- Windows 核心编程
