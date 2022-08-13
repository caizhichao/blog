---
title: phpdbg实践笔记
categories:
  - 程海迷航
date: 2022-08-13 22:55:56
tags:
  - php
  - phpdbg
  - 源码乱炖
archives:
---

# 前言
* 为了寻找在php发生死循环和内存泄漏的时候能够定位问题的方案,而尝试了phpdbg
 
# 安装
window下通过phpstudy安装好php7.4.23后,phpdbg已经自带了

# 命令使用, 网上的资料很不齐, 开两个控制台一个操作一个翻help说明,也算用得过去
```php
<?php

function test(){
    $a = 123;
    return 1 + $a;
}

while (true)
{
    $b = 5 + test();
}
```

```
> phpdbg debug.php

[Welcome to phpdbg, the interactive PHP debugger, v7.4.3]
To get help using phpdbg type "help" and press enter
[Please report bugs to <http://bugs.php.net/report.php>]
prompt>

// 这一步需要前面启动phpdbg时 参数带上debug.php run debug.php 才能正常执行
prompt> run debug.php

// 然后按 ctrl+c 表示开始断点,此时断在哪是随机的,因为还没开始添加断点
[Program received signal SIGINT]
prompt> 

// 打印当前堆栈的opcodes
prompt> p
[Stack in C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php (7 ops)]
L1-12 {main}() C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php - 0x262fc860
500 + 7 ops
 L8    #0     JMP                     J5
 L10   #1     INIT_FCALL              128                  "test"
 L10   #2     DO_FCALL                                                          @0
 L10   #3     ADD                     5                    @0                   ~1
 L10   #4     ASSIGN                  $b                   ~1
 L8    #5     JMPNZ                   true                 J1
 L12   #6     RETURN<-1>              1
 
// 打印当前所处的位置
prompt> back
frame #0: {main} at C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php:10
prompt>
```

* 命令n 执行到下一行
* 命令s 步入函数
* b 数字 添加断点到php源码的第几行
* c 继续运行
* ctrl+c 暂停运行

例如调试一个有概率死循环的时程序, 一旦遇到, 可以结合上面的back p n等命令快速定位死循环原因
