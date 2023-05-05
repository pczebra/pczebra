---
layout: post
title: "VSCODE集成gdb调试"
date: 2023-04-19
categories: C++
---

## VSCODE 集成 gdb 调试

1. 通过 remote-ssh 实现远程连接服务器
2. 在 vscode 扩展商店中搜索 GDB Debug 并安装，安装好之后，点击“运行与调试”按钮，创建 launch.json 文件
3. 替换 launch.json 文件内容如下：

   ```json
   {
     // 使用 IntelliSense 了解相关属性。
     // 悬停以查看现有属性的描述。
     // 欲了解更多信息，请访问: <https://go.microsoft.com/fwlink/?linkid=830387>
     "version": "0.2.0",
     "configurations": [
       {
         "name": "(gdb) 启动", //配置名称，显示在配置下拉菜单中
         "type": "cppdbg", //配置类型
         "request": "launch", //请求配置类型，可以是启动或者是附加
         "program": "${workspaceFolder}/test", //程序可执行文件的完整路径，${workspaceFolder}表示远程连接的初始路径
         "args": [], //传递给程序的命令行参数
         "stopAtEntry": false, //可选参数，如果为true,调试程序应该在入口（main）处停止
         "cwd": "${workspaceFolder}", //目标的工作目录
         "environment": [], //表示要预设的环境变量
         "externalConsole": false, //如果为true，则为调试对象启动控制台
         "MIMode": "gdb", //要连接到的控制台启动程序
         "setupCommands": [
           //为了安装基础调试程序而执行的一个或多个GDB/LLDB命令
           {
             "description": "为 gdb 启用整齐打印",
             "text": "-enable-pretty-printing",
             "ignoreFailures": true
           }
         ]
       }
     ]
   }
   ```

   其中，需要根据要调试的文件路径修改 program 参数。
   至此，环境配置完毕。

4. gdb 调试方法：在源代码中直接添加断点，设置好断点之后，点击“运行与调试”--(gdb)启动，即可进入调试页面。
5. 常用调试按键：

    ```txt
      F5    开始调试
      F10   单步跳过
      F11   单步调试
      shift + F11   单步跳出
      ctrl + shift + F5  重启调试
      shift + F5  停止调试
    ```

## gdb 命令调试

1. 在需要调试的地方打断点
2. 对程序运行 gdb: **`gdb ./test/log_index_port_test`**,进入 gdb
3. 寻找断点：**`b log_index_port_test.cc:636`**,其中 **b** 是指令，"log_index_port_test.cc"是要调试的文件，**:636** 是断点所在的位置
4. 开始调试： **`r`**
5. 进入：**`s`**
6. 下一步： **`n`**
7. 打印相关变量: **`p a`**,a 是相关变量，a.b 是 a 的 b 变量
8. 结束当前函数: **`finish`**
9. 退出 gdb: **`q`**
10. 程序继续下去： **`c`**
