---
title: vscode配置c
date: 2020-03-02 13:43:52
categories: vscode
tags: 
- vscode
- c
---

# vscode配置c

## 前言

通过网上dl的教程我成功配置好c啦! 链接在这里https://hovenjay.github.io/2018/06/01/VSCodeC/

不过配置的过程还是很艰辛的,也有一些弯路,所以记录一下

## 工具

Visual Studio Code:https://code.visualstudio.com/

MinGw-w64:https://sourceforge.net/projects/mingw-w64/ 

## 1. MinGW

### 介绍

MinGW 是一组包含文件和端口库，其功能是允许控制台模式的程序使用微软的标准C运行时（C Runtime）库

如果已经下载了devcpp还有其他什么编译器的可以不用下,在文件夹里找一下,会有的

### 安装

首先下载 MinGW-w64 。下载完成之后我们开始安装 MinGw-w64，安装路径可以自由定义(自己找得到就行)

安装时需要设置的安装选项如下：

- Version ：GCC 版本，直接选最高；
- Architecture ：CPU 架构，系统如果为64位，则选择 x86_64；
- Threads ：API 模式，使用默认选项；
- Exception ：异常处理方式，seh 仅针对 64 位架构，sjlj 则兼容 32 位架构；
- Build revision ：修订版本，使用默认选项；

![2018-06-01_12-44-09](2018-06-01_12-44-09.png)

### 加入环境

在安装路径中找到 bin 文件夹，通常在 `${MinGW-w64安装位置}\mingw64\bin` 

接下来，我们将刚刚获取的 bin 文件夹的路径添加到系统环境变量。

- win10的话,点击设置

- 搜索环境

  ![Snipaste_2020-03-02_12-44-11](Snipaste_2020-03-02_12-44-11.png)

- 编辑系统环境path

  ![image-20200302124619131](image-20200302124619131.png)

- 加入 bin 文件夹的路径

  ![image-20200302124736792](image-20200302124736792.png)

- 检测环境变量是否配置正确

  在命令行输入 gcc –version，如果返回的是已安装的 gcc 的版本信息，那么环境变量就配置正确了。



## 2. vscode插件

1. c/c++
2. code runner
3. chinese ~~><这个选下吼,不得不说太和我的心意了~~

在下图所示位置搜索下载

![image-20200302125123334](image-20200302125123334.png)



## 3. 创建和设置c语言工作开发区

在你的计算机中选择一个合适的位置，作为你的 C 语言开发工作区。建议工作区所在路径仅由字母、数字、下划线组成，不要包含其他的符号。

使用vscode打开你创建的工作区

在工作区新建一个 C 语言源文件命名为 hello.c ，输入以下内容：

```
#include <stdio.h>
int main()
{
    printf("hello world!/n");
}
```

####  

## 4. 配置导入的头文件参数 c_cpp_properties.json

在编写完毕并保存之后，你可能会看到 #include 这句下面会有绿色波浪线，这是由于编译器没办法找到你所使用的头文件的所在位置。将光标移动到该行，行号左边会出现 `黄色小灯泡` ，点击会出现一个提示按钮：`Add include path to setting` ，继续点击该提示，则会在工作区 `.vscode` 下生成 `c_cpp_properties.json` 文件。将文件修改成下面内容：

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                //改成自己位置!!!!!!
                "D:\\mingw64\\x86_64-w64-mingw32\\include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "D:\\mingw64\\bin\\gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```



## 5. 配置调试程序 launch.json

打开已经编写好的 hello.c ，然后按 `F5` 调试。因为是第一次调试，系统会弹出 `选择环境` 面板，这里选择 `C++(GDB/LLDB)` 。

![2018-06-01_12-44-04](2018-06-01_12-44-04.png)

选择运行环境后，VS Code 会在工作区 `.vscode` 文件夹下创建 `luanch.json` 模板文件并打开，将文件内容清空，复制下面的内容到文件中并保存：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            // 配置 VS Code 调试行为：
            "name": "GDB Debug", // 设置在启动配置下拉菜单中显示调试配置的名称。
            "preLaunchTask": "Compile", // 调试会话开始前要运行的任务。
            "type": "cppdbg", // 设置要使用的基础调试器。使用 GDB 或 LLDB 时必须是 cppdbg 。
            "request": "launch", // 设置启动程序还是附加到已经运行的实例。启动或附加 ( launch | attach ).
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe", // 调试器将启动或附加的可执行文件的完整路径。
            "externalConsole": false, // 设置是否显示外部控制台。
            "logging": { // 用于确定应该将哪些类型的消息记录到调试控制台。
                "exceptions": true, // 是否应将异常消息记录到调试控制台。默认为真。
                "moduleLoad": false, // 是否应将模块加载事件记录到调试控制台。默认为真。
                "programOutput": true, // 是否应将程序输出记录到调试控制台的可选标志。默认为真。
                "engineLogging": false, // 是否应将诊断引擎日志记录到调试控制台。默认为假。
                "trace": false, // 是否将诊断适配器命令跟踪记录到调试控制台。默认为假。
                "traceResponse": false // 是否将诊断适配器命令和响应跟踪记录到调试控制台。默认为假。
            },
            // 配置目标应用程序：
            "args": [], // 设置调试时传递给程序的命令行参数。
            "cwd": "${workspaceFolder}", // 设置调试器启动的应用程序的工作目录。
            "environment": [], // 设置调试时添加到程序环境中的环境变量，例如: [ { "name": "squid", "value": "clam" } ]。
            // 自定义 GDB 或者 LLDB：
            "windows": {
                "MIMode": "gdb", // 指定 VS Code 连接的调试器，必须为 gdb 或者 lldb。
                "miDebuggerPath": "D:\\mingw64\\bin\\gdb.exe" // 调试器的路径，修改为你的安装路径
            },
            "miDebuggerArgs": "", // 传递给调试器的附加参数
            "stopAtEntry": false, // 设置调试器是否停止在目标的入口（附加时忽略）。默认值为 false。
            "setupCommands": [
                { // 执行下面的命令数组以设置 GDB 或 LLDB
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing", // 鼠标悬停查看变量的值，需要启用 pretty-printing 。
                    "ignoreFailures": true // 忽略失败的命令，默认为 false 。
                }
            ]
        }
    ]
}
```

**留意** `luanch.json` **中注释内容**，记得把 “miDebuggerPath” 参数修改成你自己安装位置里的 `gdb.exe`

`gdb.exe` 位于 `{MinGW-w64安装位置}\mingw64\bin` 下面。



==注意!!!==

`"externalConsole": false, // 设置是否显示外部控制台。`

这段配置,我在网上搜索的时候,无一例外都是改成true的

true---->像是在dev啊,vs啊之类的,会弹出一个黑色的框,用来互交,输出

false---->在终端中显示出来

本来我也是改成true的,但是弹框总是会闪退,查了很久所有解决办法都试过了,但是没法解决,能力有限

所以就改成false了,意外的好用.互交,输出都在终端中进行,如下图

![Snipaste_2020-03-02_13-19-52](Snipaste_2020-03-02_13-19-52.png)



## 6. 配置调试前执行的任务 task.json

再按一次 `F5` ，会弹出“找不到任务”的提示窗口，点击 `配置任务` 按钮，如下图所示：

![2018-06-01_12-44-05](2018-06-01_12-44-05.png)

然后在弹出的命令面板选择 `使用模板创建 task.json 文件` ，如下图所示：

![2018-06-01_12-44-06](2018-06-01_12-44-06.png)

继续选择 `Others 运行任意外部命令的示例` ，如下图所示：

![2018-06-01_12-44-07](2018-06-01_12-44-07.png)

完成以上步骤之后，会在工作区的 `.vscode` 目录下生成 `tasks.json` 文件，并自动打开 `task.json` 文件。

![2018-06-01_12-44-08](2018-06-01_12-44-08.png)

接下来我们将 `task.json` 文件内容清空，复制下面的内容到文件中并保存：

```json
{
    // 有关 tasks.json 格式的参考文档：https://go.microsoft.com/fwlink/?LinkId=733558 。
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Compile",
            "type": "shell",
            "windows": {
                "command": "gcc",
                "args": [
                    "-g",
                    "\"${file}\"",
                    "-o",
                    "\"${fileDirname}\\${fileBasenameNoExtension}.exe\""
                ]
            },
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "reveal": "silent",
                "focus": false,
                "echo": false,
                "panel": "dedicated"
            },
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": [
                    "relative",
                    "${workspaceFolder}"
                ],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        {
            "type": "shell",
            "label": "gcc.exe build active file",
            "command": "D:\\mingw64\\bin\\gcc.exe",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "D:\\mingw64\\bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }
    ]
}
```

## 7. ojbk

回到hello.c,重新按下F5,yeah!!!