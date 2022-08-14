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

# 排查cpu死循环
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
> phpdbg -e debug.php

[Welcome to phpdbg, the interactive PHP debugger, v7.4.3]
To get help using phpdbg type "help" and press enter
[Please report bugs to <http://bugs.php.net/report.php>]
prompt>

// 这一步需要前面启动phpdbg时 参数带上 -e debug.php run 才能正常执行
prompt> run 

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
发现死循环在 debug.php:10 的第十行

# 排查内存泄漏
```php
<?php
ini_set('memory_limit', "10M");


class T1
{
    public T1 $t1;
}

function test()
{
    $n = 10000000;
    $arr = [];
    while ($n--) {
        $t1 = new T1();
        $arr[] = $t1;
    }
}

while (true){
    echo PHP_EOL;
    test();
}

```


```
> phpdbg -e debug.php

[Welcome to phpdbg, the interactive PHP debugger, v7.4.3]
To get help using phpdbg type "help" and press enter
[Please report bugs to <http://bugs.php.net/report.php>]
prompt>

// 这一步需要前面启动phpdbg时 参数带上 -e debug.php run 才能正常执行
prompt> run 
[PHP Fatal error:  Allowed memory size of 10485760 bytes exhausted (tried to allocate 4194312 bytes) in C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php on line 16]

// 打印当前堆栈的opcodes
prompt> p
[Stack in test() (20 ops)]
L10-18 test() C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php - 0x2459c282000 + 20 ops
 L10   #0     EXT_NOP
 L12   #1     EXT_STMT
 L12   #2     ASSIGN                  $n                   10000000
 L13   #3     EXT_STMT
 L13   #4     ASSIGN                  $arr                 array(0)
 L14   #5     EXT_STMT
 L14   #6     JMP                     J16
 L15   #7     EXT_STMT
 L15   #8     NEW                     "T1"                                      @2
 L15   #9     EXT_FCALL_BEGIN
 L15   #10    DO_FCALL
 L15   #11    EXT_FCALL_END
 L15   #12    ASSIGN                  $t1                  @2
 L16   #13    EXT_STMT
 L16   #14    ASSIGN_DIM              $arr                 NEXT
 L16   #15    OP_DATA                 $t1
 L14   #16    POST_DEC                $n                                        ~6
 L14   #17    JMPNZ                   ~6                   J7
 L18   #18    EXT_STMT
 L18   #19    RETURN<-1>              null

// 打印打印当前的堆栈信息
prompt> back
frame #0: test() at C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php:16
frame #1: {main} at C:\Users\Administrator\AppData\Roaming\JetBrains\IntelliJIdea2022.1\scratches\debug.php:22

// 检查$t1 变量
prompt> ev $t1
T1 Object
(
)

// 检查$arr 变量
prompt> ev print_r(count($arr)) 
655361  
```
发现是$arr这个变量太大了,问题解决

# 总结
因为phpdbg好像可以不受 `memory_limit` 的限制并且进程崩溃后还能继续调试崩溃前的内存信息所以可以尝试用来处理内存泄漏问题,
或者通过`get_declared_classes()` `get_defined_vars()` 查找所有类的静态变量以及所有声明变量来dump数据慢慢分析.
如果是死循环问题, 一旦遇到, 可以结合上面的back p n等命令快速定位死循环的位置,快速定位.
但是phpdbg有个缺点,不能attach运行中的程序,是个遗憾.

* 命令n 执行到下一行
* 命令s 步入函数
* b 数字 添加断点到php源码的第几行
* c 继续运行
* ctrl+c 暂停运行
* ev php表达式, 不能空格,有时需要分号


