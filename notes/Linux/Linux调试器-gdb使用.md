# Linux调试器-gdb使用

- [Linux调试器-gdb使用](#linux调试器-gdb使用)
    - [1. list/l 行号：](#1-listl-行号)
    - [2. list/l  函数名：](#2-listl--函数名)
    - [3. r或run：](#3-r或run)
    - [4. n或next：](#4-n或next)
    - [5. s或step：](#5-s或step)
    - [6. break/b 行号：](#6-breakb-行号)
    - [7. break函数名：](#7-break函数名)
    - [8. info break：](#8-info-break)
    - [9. finish：](#9-finish)
    - [10. print/p ：](#10-printp-)
    - [11. p 变量：](#11-p-变量)
    - [12. set var：（set var 变量名=表达式）](#12-set-varset-var-变量名表达式)
    - [13. continue（或c）：](#13-continue或c)
    - [14. run（或r）：](#14-run或r)
    - [15. delete breakpoint：](#15-delete-breakpoint)
    - [16. delete breakpoint n：](#16-delete-breakpoint-n)
    - [17. disable breakpoints：](#17-disable-breakpoints)
    - [18. enable breakpoints：](#18-enable-breakpoints)
    - [19. info（或i）breakpoints：](#19-info或ibreakpoints)
    - [20. diaplay 变量名：](#20-diaplay-变量名)
    - [21. undisplay：](#21-undisplay)
    - [22. until X（行号）：](#22-until-x行号)
    - [23. breaktrace（或bt）：](#23-breaktrace或bt)
    - [24. info（或i）locals：](#24-info或ilocals)
    - [25. quit：](#25-quit)
    - [26. 清屏](#26-清屏)
    - [27. 用条件表达式设置断点](#27-用条件表达式设置断点)
    - [28.删除断点](#28删除断点)

---

**开始**

1. 在编译的时候必须加上-g选项，表示gcc在编译的时候假如调试信息
2. **`gdb test`**进入gdb开始调试，但是有好多信息出现在屏幕上，可以加上**`-q`**选项来屏蔽这些信息
3. 也可以先进入到gdb中然后再加载文件。使用**`file test`**就可以了
4. 接下来就可以开始调试了

**退出**

> **`crtl + d`**或者**`quit`**命令

## 1. list/l 行号：

- 显示binFile源代码，接着上次的位置往下列，每次列10行

## 2. list/l  函数名：

- 列出某个函数的源代码

## 3. r或run：

- 运行程序

## 4. n或next：

- 单条执行

## 5. s或step：

- 进入函数调用

## 6. break/b 行号：

- 在某一行设置断点info

## 7. break函数名：

- 在某个函数开头设置断点

## 8. info break：

- 查看断点信息

## 9. finish：

- 执行到当前函数返回，然后停下来等待命令

## 10. print/p ：

- 打印表达式的值，通过表达式可以修改变量的值或者调用函数

## 11. p 变量：

- 打印变量值

## 12. set var：（set var 变量名=表达式）

- 修改变量值

## 13. continue（或c）：

- 从当前位置开始连续而非单步执行程序

## 14. run（或r）：

- 从开始连续而非单步执行程序

## 15. delete breakpoint：

- 删除所有断点

## 16. delete breakpoint n：

- 删除序号为n的断点

## 17. disable breakpoints：

- 禁用断点

## 18. enable breakpoints：

- 启用断点

## 19. info（或i）breakpoints：

- 参看当前设置了哪些断点

## 20. diaplay 变量名：

- 跟踪查看一个变量，每次停下都显示它的值

## 21. undisplay：

- 取消对先前设置的哪些变量的跟踪

## 22. until X（行号）：

- 跳至X行

## 23. breaktrace（或bt）：

- 查看各级函数调用及参数

## 24. info（或i）locals：

- 查看当前栈帧局部变量的值

## 25. quit：

- 退出gdb

## 26. 清屏

- Shell clear

##27. 用条件表达式设置断点

- break 7 if n==6
- 当n等于6的时候，设置断点在第7行

## 28.删除断点

- clear：后面跟上设置断点的行号

- delete：后面跟上设置断点的编号，需要用info breakpoint来查看编号。可以一次删除多个断点，断点编号使用空格隔开；如果delete后面没有编码默认删除所有断点

  

