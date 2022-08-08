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

# 2022-08-08 22:45:00
从启动开始看发现 `swoole_websocket_server.cc` 里有个 `websocket_message_compress` 方法,才知道swoole支持了
websock消息压缩, [WebSocket 帧压缩](https://wiki.swoole.com/#/websocket_server?id=websocket%e5%b8%a7%e5%8e%8b%e7%bc%a9-%ef%bc%88rfc-7692%ef%bc%89)
[RFC-7692](https://www.rfc-editor.org/rfc/pdfrfc/rfc7692.txt.pdf) 阅读压缩协议对我来说有点困难就不展开了
```php
static bool websocket_message_compress(swString *buffer, const char *data, size_t length, int level)
{
    // ==== ZLIB ====
    if (level == Z_NO_COMPRESSION)
    {
```

