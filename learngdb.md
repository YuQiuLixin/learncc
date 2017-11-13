[TOC]

###GDB调试

----

#### 概述

GDB是GNU开源组织发布的一个强大的UNIX下的程序调试工具

GDB主要帮忙你完成下面四个方面的功能：

​    1.启动你的程序，可以按照你的自定义的要求随心所欲的运行程序
​    2.可让被调试的程序在你所指定的调置的断点处停住.（断点可以是条件表达式）
​    3.当程序被停住时，可以检查此时你的程序中所发生的事
​    4.动态的改变你程序的执行环境

#### 常用命令

- cc -g test.c -o test    形成带有调试信息的程序
- gdb test      启动gdb（参数为程序）
- l（list） 展示程序源代码
- break  line 即可在line处设置断点
- break  func 设置函数断点
- info break  查看断点信息
- r（run）    启动程序
- n（next） 单步调试
- c （continue） 继续运行程序
- p （print） 打印变量值
- bt 查看函数堆栈
- finish 退出函数
- q（quit） 退出gdb
- shell <command string> 可以用来在gdb环境下执行shell命令

#### 启动gdb方式

- ==gdb <program>== 

​       program也就是你的执行文件

- ==gdb <program> core==

​       用gdb同时调试一个运行程序和core文件，core是程序非法执行后core dump后产生的文件。

- ==gdb <program> <PID>==

​        如果你的程序是一个服务程序，那么你可以指定这个服务程序运行时的进程ID。gdb会自动attach上去，并调试他。program应该在PATH环境变量中搜索得到。

​	GDB启动时，可以加上一些GDB的启动开关，详细的开关可以用gdb -help查看。我在下面只例举一些比较常用的参数：

​    -symbols <file> 
​    -s <file> 
​    从指定文件中读取符号表。

​    -se file 
​    从指定文件中读取符号表信息，并把他用在可执行文件中。

​    -core <file>
​    -c <file> 
​    调试时core dump的core文件。

​    -directory <directory>
​    -d <directory>
​    加入一个源文件的搜索路径。默认搜索路径是环境变量中PATH所定义的路径。



####在GDB中运行程序

当以gdb <program>方式启动gdb后，gdb会在PATH路径和当前目录中搜索<program>的源文件。如要确认gdb是否读到源文件，可使用l或list命令，看看gdb是否能列出源代码。

在gdb中，运行程序使用r或是run命令。程序的运行，你有可能需要设置下面四方面的事。

- 程序运行参数。

​    set args 可指定运行时参数。（如：set args 10 20 30 40 50）
​    show args 命令可以查看设置好的运行参数。

- 运行环境。

​    path <dir> 可设定程序的运行路径。
​    show paths 查看程序的运行路径。
​    set environment varname [=value] 设置环境变量。如：set env USER=hchen
​    show environment [varname] 查看环境变量。

- 工作目录。

​    cd <dir> 相当于shell的cd命令。
​    pwd 显示当前的所在目录。

- 程序的输入输出。

​    info terminal 显示你程序用到的终端的模式。
​    使用重定向控制程序输出。如：run > outfile
​    tty命令可以指写输入输出的终端设备。如：tty /dev/ttyb