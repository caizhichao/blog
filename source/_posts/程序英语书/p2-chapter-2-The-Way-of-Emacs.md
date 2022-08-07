---
title: p2-chapter-2-The Way of Emacs
categories:
  - 程序英语书
  - Mastering EMACS
date: 2019-08-24 09:58:35
tags:
archives:
---

The Way of Emacs

Emacs的套路

“The purpose of a windowing system is to put some amusing fluff 
around your one almighty emacs window.”– Mark, gnu.emacs.help.

一个emacs窗口的目的是把一些可笑的杂毛放在窗口的周围。


If you imagine the span of the modern computing era be-
ginninginth 1960s,then Emacs has been there longer than
just about everything else. It was first written by Richard
Stallman as a set of macros on top of another editor, called
TECO, back in 1976.[1](https://www.gnu.org/software/emacs/manual/html_mono/efaq.html#Origin-of-the-term-Emacs) TECO is now mostly remembered for be-
ing even more obtuse and hard to understand than Emacs
and DOS-era WordPerfect combined. Since then, there have
been many competing implementations of Emacs but today
you’re only likely to encounter XEmacs and GNU Emacs.


你可以想象一下现代计算机时代开始于1960年，然鹅，Emacs比所有已有的东西
更加的古老。Richard Stallman撰写了Emacs的第一版，这是另一个顶级
编辑器TECO的宏的集合。TECO最为人所记住的是它比Emacs加DOS时代的WordPrefect
加一块还更让人难以理解和困惑.自那时起,出现了很多互相竞争的Emacs实现,但是
你现在只要世道XEmacs和GNU Emacs就行.

This book will only concern iteself with GNU Emacs. Once upon a time 
XEmacs was the more advanced and feature rich editor, but this is no 
longer the case: from Emacs 22 onwards GNU Emacs is the best Emacs 
out there.The history of XEmacs and GNU Emacs is an interesting one.
It was one of the first major forks[2](http://www.jwz.org/doc/lemacs.html) in a free software project and 
both XEmacs and GNU Emacs are developed in parallel to this day.


这本书只涉及GNU Emacs.曾经在一段时间上,XEmacs层是最高级和特性丰富的编辑器,
但是这已经过去了,从Emacs 22 开始GNU Emacs就是最好的Emacs了.XEmacs和GNU Emacs
的历史也是一件趣事.他俩曾是一个开源项目的第一个主要两条分支,他俩并行开发到
现在.

Note
To almost everyone, the word Emacs refers specifically
to GNU Emacs. I will only spell out the full
name when I am distinguishing between different
implementations. When I mention Emacs, I
always talk about GNU Emacs.

Note
对大多数人来说,Emacs这个词就是指GNU Emacs. 在我要区分两个不同的Emacs实现时,
我会完整的拼出这个名字.但当我提到Emacs,我一直是在说GNU Emacs.

Because of Emacs's age there are a number of... oddities.
Weird choices of terminology and historical anachronisms
persist because in most cases Emacs was ahead of the editor-
IDE curve for many decades and thus had to invent its own
terminology for things. There are talks of replacing Emacs's
own vernacular with words familiar to everyone, but that
is still a long way off.

因为Emacs的出现时代,它带有很多怪异的东西.历史原因导致的意识和术语上的
怪异选择仍然存在,因为Emacs在一定的时间里在IDE这条线上是领先的,所以不得不为
很多事创建术语.有讨论说要用更让人熟悉的词语替换Emacs的术语方言,但这条
路还很长.

Despite the lack of marketing, a small core of Emacs developers,
the anachronisms and terminology that predqates the
modern Personal Computing-era, there are many people
out there who just love using Emacs. When Sublime Text
showed off its mini-map feature (a miniature display of the
source code) someone immediately coded up a minimap
package doing the same thing in Emacs. In fact, it is this
extensibility that attracts some to – and repels others from
– Emacs.

除了市场缺失,一小部分核心Emacs开发者,早于现代个人PC时代的历史错误和术语外,
还是有很多人无视这些而只是单纯的喜欢使用Emacs.当Sublime展示了它对源码的
缩略图功能后,有人可以开发了一个缩略图的包给Emacs用.事实上,就是这种扩展性
吸引了哪些排斥其他死板编辑器的人.
-来自Emacs

This chapter will talk about the Way of Emacs: the terminology
and what Emacs means to a lot of people, and why understanding
where Emacs comes from will make it easier to
adopt it.

这一章将讨论Way of Emacs这个标题:
通过讨论术语和对大多数人来说Emacs意味着什么,和为什么知道Emacs的来源
会让你更容易采用它.
