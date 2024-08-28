---
title: gdb使用以及PostgreSQL源码调试
tags:
  - debug
  - gnu
date: 2024-08-01
categories: " debug"
---
# gdb
## gdb的介绍

gdb是UNIX及UNIX-like下的调试工具一般来说，GDB主要帮助你完成下面四个方面的功能：
1. 启动你的程序，可以按照你的自定义的要求随心所欲的运行程序。
2. 可让被调试的程序在你所指定的调置的断点处停住。（断点可以是条件表达式）
3. 当程序被停住时，可以检查此时你的程序中所发生的事。
4. 你可以改变你的程序，将一个BUG产生的影响修正从而测试其他BUG
## gdb的安装和基本用法
### 安装
利用以下命令安装gdb：
```shell
sudo apt-get install gdb
```
### 使用

1. 利用gdb远程调试postgresql
	启动postgresql进程
	输出命令psql databasename 进入数据库
2. 打开一个新的终端，执行ps -ef | grep postgres来查看数据库进程id
3. 输入sudo gdb来启用gdb
	在gdb界面输入attach 进程id 来远程连接数据库
	之后就可以利用gdb来调试程序了
### gdb调试命令
https://wiki.postgresql.org/wiki/Generating_a_stack_trace_of_a_PostgreSQL_backend
#### 设置断点
#### 显示参数
```shell
b function 在函数function处打断点，此时会给断点分配一个数字序号
d 1        删除序号为1的断点
c          执行调试的目标程序，会在断点处停下
s          单步进入
n          单步跳过
list       展示接下来会执行的代码
print x    输出变量x，如果x是一个结构体，可以先执行set print pretty on命令打开结构体详情
info breakpoints 显示所有断点
```
#### 显示过程
要找出在您的程序中哪里调用了特定文件中的函数，您可以使用GDB的`backtrace`命令（简写为`bt`）来查看当前的函数调用栈。这将显示当前执行的函数以及它们的调用关系，包括文件名和行号。例如：

```gdb
(gdb) bt
```

如果您想要获取更详细的栈帧信息，可以使用`info frame`命令。这将提供当前栈帧的详细信息，包括正在执行的函数和它在源代码中的位置：
切换到具体栈帧

```gdb
(gdb) info frame
frame 0x7fffffffcbd0 
```


此外，如果您已经知道函数的名称，您还可以使用`info function`命令来查找函数的定义位置：

```gdb
(gdb) info function function_name
```

#### 调试操作
- `continue` (或简写 `c`)：继续执行程序，直到遇到下一个断点或程序结束
- `next` (或简写 `n`)：单步执行程序，跳过函数调用
- `step` (或简写 `s`)：单步执行程序，如果遇到函数调用则进入函数
- `finish`：运行程序直到当前函数完成返回
- `info locals`：显示当前栈帧的局部变量的值
- `backtrace` (或简写 `bt`)：显示当前的调用栈信息
```shell
c #continue 的简写，继续运行直到下一个断点
next #(或简写 `n`)：单步执行程序，跳过函数调用
step #(或简写 `s`)：单步执行程序，如果遇到函数调用则进入函数
```
#### gdb设置
```shell
set print pretty on  #使用漂亮格式打印
```

### 在编辑器里使用
#### Visual Studio Code(VScode)

如果不想使用gdb，可以使用vscode来编写和调试pg。 vscode调试方法如下
- 配置launch.json
```json
      {
          "version": "0.2.0",
          "configurations": [
              {
                  "name": "postgresql",
                  "type": "cppdbg",
                  "request": "attach",
                  "program": "pghome/bin/postgres", //输入你的postgres可执行文件地址，在$PGHOME/bin目录下
                  "processId": "${command:pickProcess}",
                  "MIMode": "gdb",
                  "setupCommands": [
                      {
                          "description": "Enable pretty-printing for gdb",
                          "text": "-enable-pretty-printing",
                          "ignoreFailures": true
                      },
                      {
                          "description":  "Set Disassembly Flavor to Intel",
                          "text": "-gdb-set disassembly-flavor intel",
                          "ignoreFailures": true
                      }
              ]
              }
          ]
      }
```
- 在终端打开想要调试的数据库
- 点击vscode中的debug按钮，在文本框输入postgres查找数据库进程，点击之后可以看到已开启调试功能。


## 实际应用
### gdb调试PostgreSQL
1. 编译配置PostgreSQL源代码
配置安装目录以及data目录
```shell
PGHOME=$HOME/pgsql
export PGDATA=$PGHOME/data
export PGHOST=$PGDATA
export PGPORT=5432
export PATH=$PGHOME/bin:$PATH
```
应用配置以及编译安装
```shell
./configure --prefix=$PGHOME  --enable-debug --enable-cassert --enable-depend CFLAGS=-O0
make
make install
```
2. 配置gdb如上所示
3. 使用gdb开启Postgres并进行配置
```shell
# Stop server
pg_ctl -D /path/to/data stop -m fast
# Launch postgres via gdb
gdb --args postgres -D /path/to/data
```
Now, in the GDB shell, use these commands to set up an environment:
```shell
# We have scroll bars in the year 2012!
set pagination off
# Attach to both parent and child on fork
set detach-on-fork off
# Stop/resume all processes
set schedule-multiple on
# Usually don't care about these signals
handle SIGUSR1 noprint nostop
handle SIGUSR2 noprint nostop
# Make GDB's expression evaluation work with most common Postgres Macros (works with Linux).
# Per https://www.postgresql.org/message-id/20130731021434.GE19053@alap2.anarazel.de,
# have many Postgres macros work if these are defined (useful for TOAST stuff,
# varlena stuff, etc):
macro define __builtin_offsetof(T, F) ((int) &(((T *) 0)->F))
macro define __extension__
# Ugly hack so we don't break on process exit
python gdb.events.exited.connect(lambda x: [gdb.execute('inferior 1'), gdb.post_event(lambda: gdb.execute('continue'))])
# Phew! Run it.
run
```
如果没有读到符号表，则需要进行第一步，使用debug选项编译源代码
3. 使用gdb attach到psql
在gdb中使用attach命令
```shell
(gdb)attach psqlid
```
1. 之后即可进行常规的断点等调试操作


