---
title: p4-chapter-2-Guiding-Philosoph-LISP
categories:
  - 程序英语书
  - Mastering EMACS
date: 2019-08-24 10:01:06
tags:
archives:
---


Emacs is powered by its own LISP implementation called
Emacs Lisp or just elisp. Many are put off or intimidated by
this esoteric language; that’s a shame, because it’s a practical
and fun way to learn LISP in an editor built up around the
idea of LISP. Every part of Emacs can be inspected, evaluated
or modified because the editor is approximately 95 percent
elisp and 5 percent C code. It’s also a practical way to learn
a radical paradigm: that code and data are interchangeable
and malleable; that the language, owing to its simple syntax,
is trivially extensible with macros.

*啥是LISP*
Emacs是靠它自己的LISP解释器支持的。很多人被劝退，或者被这门神秘的
语言吓到。这样就太可惜了，因为在围绕着LISP方法的编辑器中学LISP
这才是有效和有趣的学习LISP的方式。Emacs的每一部分都可以被审查，评价，
和修改，因为这个编辑器由95%的elisp代码和5%的c代码组成。这也是学习本质
范例的一个方法，数据交换，代码适应性，这语言还拥有简单的语法，和琐细
的宏扩展。

Unfortunately, there’s no getting around learning elisp at
some point. In this book, I will talk about the Customize in-
terface: a dynamically generated interface of customizable
options in Emacs. However, something as simple as rebind-
ing a key means you’ll have to interact with elisp. But it’s
not all bad. Most of the problems you’re likely to encounter
have already been solved by someone else a long time ago;
it’s a simple matter of searching the Internet for a solution
to your problems.

不幸的是，我们不谈学习Emacs。在这书里，我将讨论自定义接口：Emacs中
一个动态创建自定义接口的选项。然而，像重新绑定快捷键则不得不需要你
与elisp交互。这也不全是坏消息。大部分你可能遇到的问题已经在很久以前
就被人解决了。到互联网上搜索你要解决的问题是件很容易的事情。

Despite the relative unpopularity of elisp versus more “mod-
ern” languages like Python, Ruby and JavaScript, I doubt
Emacs would have had the same power of extensibility if
a more traditional imperative/object-oriented language had
been used. What makes LISP such a fantastic language is that
source code and data structures are intrinsically one and the
same: the LISP source code you read as a human is almost
identical to how the code is manipulated as a data structure
by LISP — the distinction between the questions “What is
data?” and “What is code?” are nil.

尽管elisp相对与大多数主流语言例如Python，Ruby，js，来说不太流行。
我怀疑Emacs跟传统命令式/面向对象语言相比是否具有一样的扩展能力。
数据结构和代码本质上是一样的这点让LISP很令人惊讶。LISP源码读起来让人
感觉在通过LISP修改数据结构。关于什么是数据什么是代码这个问题，没有答案。

The data-as-code, the macro system and the ability to “advise”
 arbitrary functions – meaning you can modify the be-
havior of existing code without copying and modifying the
original – give you an unprecedented ability to alter Emacs
to suit your needs. What would in most software projects
be considered code smells or poor architecture is actually a
major benefit in Emacs: you can hook, replace or alter existing
 routines in Emacs to suit your needs without rewriting
large swathes of someone else’s source code.

数据也是代码，这个宏系统“推荐”函数作为参数，这意味着你不需要修改复制
源码就能修改代码的表现。这给了你前所未有的能力去修改Emacs来适应你的需求。
绝大部分项目中会在乎代码味道和少的结构,而这两点正是Emacs的优势.你可以
挂钩子,替换,修改现存的常规Emacs代码区适应你的需求,而不需要为此重写捆绑
在一起的大量别人的源码.

This book will not teach elisp in any great detail: Emacs has
a built-in elisp introduction[3](https://www.gnu.org/software/emacs/manual/eintr.html) and I highly recommend it if
you are curious — and honestly you should be. LISP is fun
and this is a great way to learn and use a powerful language
in a practical environment. Don’t let the parentheses scare
you; they are actually its greatest strength.

这本书不会详细的教elisp,Emacs有个内置的elisp介绍.我高度推荐它,
如果你有任何困惑的话--老实说如果你有的话. LISP 时有趣的,用Emacs
是个绝佳的学习实践这门语言的机会.别让括号吓到你,这其实是它的优点.

*Emacs as an Operating System*

When you run Emacs you are in fact launching a tiny C
core responsible for the low-level interactions with your operating
system’s ABI. That includes mundane things like filesystem
And network access; drawing things to the screen or
printing control codes to the terminal.

*Emacs当作一个操作系统*

当你运行Emacs时,你其实是登录了一个微型C内核,这个内核可以跟你系统的ABI
接口进行底层交互.包括通常的文件系统和网络使用权,在屏幕上绘制东西,或者
打印控制命令到命令行.

The cornerstone of Emacs though is the elisp interpreter 
without it, there is no Emacs. The interpreter is creaky and
old; it’s struggling. Modern Emacs users expect a lot from
their humble interpreter: speed and asynchrony are the two
main issues. The interpreter runs in a single thread and intensive
tasks will lock the UI thread. But there are workarounds;
the issues, manifold though they are, do not deter people
from writing ever-more sophisticated packages.

Emacs的基石是elisp解释器,如果没有它,就没有Emacs.这个解释器是
破旧的.苦苦挣扎.现代Emacs用户对这个简陋的解释器有所期待:主要
有快速异步.这个解释器跑在单线程上,密集的任务将会锁住ui线程.
但是有解决方法了,虽然这些问题多种多样,但是仍然挡不住人们继续
写越来越复杂的包


When you write elisp you are not just writing snippets of
code run in a sandbox, isolated from everything — you are
altering a living system; an operating system running on an
operating system. Every variable you alter and every func-
tion you call is carried out by the very same interpreter you
use when you edit text.

当你开始写elisp,你并不是在修改沙盒中的一小段代码,而是在修改一个
时时的系统.一个运行在操作系统之上的操作系统.你可以修改每个变量
每个你调用的函数,他们被搭载在你编辑文本用的这个编辑器上.


Emacs is a hacker’s dream because it is one giant, mutable
state. Its simplicity is both a blessing and a curse. You can
re-define live functions; change variables left and right; and
you can query the system for its state at any time — state that
changes with every key stroke as Emacs responds to events
from your keyboard to your network stack. Emacs is selfdocumenting
because it is the document. There are no other
editors that can do that. No editor comes close.

Emacs是一个极客的梦幻,因为它是一个伟大的可变的状态.它的简单特点,
既是祝福也是诅咒.你可以重新定义时时函数,彻底改变变量.可以在任何时间
查询系统的状态.每一帧的状态改变都是Emacs在响应你的键盘事件或网络事件.
Emacs又是自文档的编辑器.没有其它编辑器能做到这个,甚至接近.

And yet Emacs never crashes — not really, anyway. Emacs
has an uptime counter to prove that it doesn’t (M-x emacsuptime)
— multi-month uptimes are not uncommon.

Emacs从来没有崩溃过,无论真假.Emacs有个运行时间计数器来证明这不是虚的
比如几个月的运行时间并不奇怪.

So when you ask Emacs a question – as I will show you how
to do later – you are asking your Emacs what its state is. Because
of this, Emacs has an excellent elisp debugger and unlimited
access to every facet of Emacs’s own interpreter and
state — so it has excellent code completion too. Any time
you encounter a LISP expression you can tell Emacs to evaluate
it, and it will: from adding numbers to setting variables
to downloading packages.

当你问Emacs问题时,你是向Emacs询问现在的state是什么,这正如我演示的.
因为这样,Emacs有一个聪明的elisp调试器和无限制的访问Emacs自己的解释器
的所有方面,也有一个杰出的编码自动完成功能.任何时候你遇到一个LISP表达式,
你都能让Emacs解析它,无论是从数字加法到设置变量到下载扩展包.
