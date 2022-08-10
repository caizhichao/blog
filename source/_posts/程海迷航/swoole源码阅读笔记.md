---
title: swoole源码阅读笔记
categories:
  - 程海迷航
date: 2022-08-08 22:55:56
tags:
  - php
  - swoole
  - 源码乱炖
archives:
---

# 前言
* 读源码是个浪漫
* 读源码能缓解点进步焦虑, 虽然可能人还没搞清自己为什么要焦虑
* 一点快乐,可以发现很多百度不到的有趣东西
* 公司用的swoole-4.4.23 , 所以这个阅读也仅限这个版本
* 下面那些标题表示几号看了哪些文件的意思

# 2022-08-08 22:45:00 `swoole_websocket_server.cc`

从启动开始看发现, 里有个 `websocket_message_compress` 方法,才知道swoole支持了
websock消息压缩, [WebSocket 帧压缩](https://wiki.swoole.com/#/websocket_server?id=websocket%e5%b8%a7%e5%8e%8b%e7%bc%a9-%ef%bc%88rfc-7692%ef%bc%89)
[RFC-7692](https://www.rfc-editor.org/rfc/pdfrfc/rfc7692.txt.pdf) 阅读压缩协议对我来说有点困难就不展开了
```php
static bool websocket_message_compress(swString *buffer, const char *data, size_t length, int level)
{
    // ==== ZLIB ====
    if (level == Z_NO_COMPRESSION)
    {
```
# 2022-08-010 23:26:00 `CMakeLists.txt`
c++ 代码用其他编辑器都不好阅读, 最后想到visual studio , 跳转和查看所有引用的快捷键分别为F12 shift+F12
makefile的文件都没怎么细看过,首先我希望了解下哪些源码参与了编译,`file`关键字表示要引用哪些资源
```cmakefile
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

SET(CMAKE_BUILD_TYPE Debug)

file(GLOB_RECURSE SRC_LIST FOLLOW_SYMLINKS src/*.c src/*.cc thirdparty/boost/asm/combined.S)
file(GLOB_RECURSE HEAD_FILES FOLLOW_SYMLINKS include/*.h)
file(GLOB_RECURSE HEAD_WAPPER_FILES FOLLOW_SYMLINKS include/wrapper/*.hpp)

SET(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/lib)
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
```

[官方文档](https://cmake.org/cmake/help/latest/command/file.html#:~:text=CMake%20file%20file%20%C2%B6%20File%20manipulation%20command.%20This,and%20path%20manipulation%20requiring%20access%20to%20the%20filesystem.)
这么一看一下就看懂了 `SRC_LIST` 是这个 `out-var` , `FOLLOW_SYMLINKS` 是省略掉的参数,后面跟着的是其他搜索目录的表达式
`file({GLOB | GLOB_RECURSE} <out-var> [...] [<globbing-expr>...])`
