---
title: p3-chapter-2-Guiding Philosophy
categories:
  - 程序英语书
  - Mastering EMACS
date: 2019-08-24 10:00:02
tags:
archives:
---

Emacs is a tinkerer’s editor. Plain and simple. People who
hack on Emacs do it because almost every facet of it is
extensible. It is the original extensible, customizable, selfdocumenting
editor. If you come from other text editors,
the idea of being able to change anything may seem like an
unnecessary distraction from your work – and indeed, a
lot of Emacs hacking does happen at the expense of one’s
real job – but once you realize that you can shape your
editor to do what you want it to do, it opens up a world of
possibilities.

Emacs是一个多面手的编辑器.平文本并且简单.之所以Emacs被用来hack编程,
是因为它几乎所有层面上的可扩展性.它是一个本身可扩展,可定制,自文档的
编辑器.如果你从别的文本编辑器过来,这种改变所有事的想法看起来没有必要,
只是分散你对工作的注意力.事实上，很多的Emacs神操作确实很消耗工作时间，
但是只要你认识到你可以把你的编辑器改造成任何你想要的样子的时候，新世界
大门就向你敞开了。

That means you can truly rebind all of Emacs’s keys to your
liking; you are not hidebound by your IDE's undocumented
and buggy  API nor the limitations that would follow if you
did change things — such as your custom navigation keys
not working in, say, the search & replace window or in the
internal help files. Truly, in Emacs, you can change every-
thing — and people do. Vim users are migrating to Emacs
because, well, Emacs is often a better Vim than Vim.

你可以重新绑定所有快捷键。你不用被包裹在ide的无文档或者一车车的api中，
例如你设置的快捷键不生效，如说明，或搜索替换窗口，或内部帮助文件。
确实，你在Emacs中你能改变任何事情，正如人们做的那样。Vim用户会想要
迁移到Emacs中，毕竟，Emacs是一个更加Vim的Vim。

Emacs pulls you in. Once you start using Emacs for the edit-
ing, you realize that using Emacs for IRC, email, database ac-
cess, command-line shells, compiling your code or surfing the
 Internet is just as easy as editing text – and you get to
keep your key bindings, theme and all the power of Emacs
and elisp to configure or alter the behavior of everything.

Emacs把你拉入进来。只要你开始使用Emacs去编辑，你会意识到使用Emacs进行，
IRC，email，数据库访问，命令行，编译你的代码，或者在Internet上冲浪，
都会像编辑文本一样简单。你可以保持你的快捷键，主题，所有Emacs能量和使用
elisp来配置和修改任何事的表现。

And when everything is seamlessly tied together you avoid
the usual context switches of going from application to ap-
plication: most Emacs users use little more than the editor,
a browser and maybe a dedicated terminal application.

当任何事都无缝的绑定在一起，你可以避免通常的从一个应用到另外一个应用的
文本切换，大部分Emacs使用者使用编辑器，浏览器，或许还有一个命令行。

     Emacs’s history
     Emacs’s source code repository (now in Git)
     stretches back over 30 years and has more than
     130,000 commits and nearly 600 committers.

     Emacs的历史。
     Emacs的源码仓库（现在在github）可以追溯到30年前，并且有超过
     130,000次提交，以及将近600个贡献者。

If you want to modify Emacs, or any of the myriad pack-
ages available to you, Emacs Lisp (also known informally as
elisp) is what you will have to write. There have been a few
attempts to graft other languages onto elisp and Emacs but
with no lasting effect. As it turns out, LISP is actually a per-
fect abstraction for a very advanced tool like Emacs. And
most modern languages wouldn’t necessarily stand the test
of time: TCL was briefly considered in the 90s as it was popu-
lar at the time — but that has the distinction of being even
more obscure than LISP, nowadays.

假如你想修改Emacs，或者大量提供给你的包，你需要写elisp语言。
有些人尝试把其他语言移植到elisp和Emacs中，但没有效果。事实证明，
LISP是对高级工具Emacs的完美抽象。其他大多数语言没法经过时间的考验，
TCL曾短暂的被认为考虑在它流行的1990年代，但是它跟Emacs相比有些区别，
从今天看来它甚至更难以理解。

The only downside is that fiddling with your Emacs config-
uration is something you will have to learn to live with (and
in LISP no less, but as I explain in the next part that’s actually
a good thing.) That’s why I reinforced the point that it’s a
tinkerer’s editor. If you hate the idea of tweaking anything
and want everything out of the box, you have two options
left:

唯一的缺陷是，摆弄你的Emacs配置是你不得不学会的(只有Lisp，但是我在
下一部分会解释，这或许是一件好事)这也是我强调的那一点，这是一个思考者
的编辑器。如果你讨厌配置任何东西，并且想要开箱即用，你有两个选项：

*Use a starter kit* There are many free starter kits that come
equipped with additional packages and what the
author thinks are sensible default settings. They can
be a good way to start out but with the caveat that
you don’t know where Emacs ends and the starter
kits’ added functionality begins.
I recommend you look at one of the following starter
kits widely used:
• Steve Purcell’s .emacs.d
https://github.com/purcell/emacs.d
• Bozhidar Batzov’s Prelude
https://github.com/bbatsov/prelude

*一、使用启动工具箱* 有很多启动工具箱已经装备了很多额外的包和作者
认为明智的默认配置。但是请小心，你不知道Emacs的来源和启动工具箱在
哪里添加了启动函数。我建议你查看这下面的两个被广泛使用的工具包。

*Use the defaults* Certainly an option but Emacs, I would
say, is rather lacking out of the box. You are expected
to configure Emacs to your liking or use a starter kit.
For an editor that is so radically different from main-
stream editors, the maintainers are surprisingly conservative 
about changing the defaults for fear of upsetting
the old guard (who, of all people, should know how to
configure Emacs.)

*二、使用默认配置* 这确实是一个选择，但我必须说这缺少很多开箱即用
的东西。你需要配置Emacs或用工具箱。从编辑器的角度，它跟主流的编辑器
有本质区别，维护者是保守主义者，不愿意修改默认配置，因为害怕令那些
老同志失望。（可这群老同志都知道怎么配置Emacs）
