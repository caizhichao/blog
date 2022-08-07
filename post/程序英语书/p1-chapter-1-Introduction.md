---
title: p1-chapter-1-Introduction
categories:
  - 程序英语书
  - Mastering EMACS
date: 2019-08-24 09:46:41
tags: 
archives:
---

Chapter 1
Introduction
“I’m using Linux. A library that emacs uses to
communicate with Intel hardware.”
– Erwin, #emacs,Freenode.

我在用linux，一个emacs用来连接互联网的的库

Thank You
Thank you for purchasing Mastering Emacs. This book has
been a long time coming. When I started my blog, Master-
ing Emacs, in 2010, it was at the recommendation of a good
friend, Lee, who suggested that I share my thoughts on
Emacs and work flow in Emacs. At the time I had accrued
in an org mode file titled blogideas.org a large but random
assortment of ideas and concepts that I’d learned about and
wished someone had taught me. The end result of that file
is the blog and now this book.

感谢你买了这本Mastering Emacs.这本书的到来可以追溯到很久之前。
在我开始写博客那时候，有个朋友建议我分享自己关于emacs的思考，
和在emacs中工作的流程。当时我积累了一个名为blogideas.org的文件，
里面随机分类了大量的想法和我学会的概念，或者希望别人教我的东西。
这个org文件最后变成了这个博客和这本书。

Special Thanks
I would like to thank the following people for
their encouragement, advice, suggestions and
critiques:
Akira Kitada, Alvaro Ramirez, Arialdo Mar-
tini, Bob Koss, Catherine Mongrain, Chandan
Rajendra, Christopher Lee, Daniel Hannaske,
Edwin Ong, Evan Misshula, Friedrich Paetzke,
Gabriela Hajduk, Gabriele Lana, Greg Sieranski,
Holger Pirk, John Mastro, John Kitchin, Jonas
Enlund, Konstantin Nazarenko, Lee Cullip,
Luis Gerhorst, Lukas Pukenis, Manuel Uberti,
Marcin Borkowski, Mark Kocera, Matt Wilbur,
Matthew Daly, Michael Reid, Nanci Bonfim,
Oliver Martell, Patrick Mosby, Patrick Martin,
Sebastian Garcia Anderman, Stephen Nelson-
Smith, Steve Mayer, Tariq Master, Travis
Jefferson, Travis Hartwell.

我要感谢以下这些人的鼓励，建议，见解，评论。


Like a lot of people, I was thrust into the world of Emacs
without knowing anything about it; in my case it was in
my first year of University where the local computer soci-
ety was made up primarily of Vim users. It was explained
to me, in no uncertain terms, that “you use Vim — that’s
it.” Not wanting to be told what to do, I picked the polar
opposite of Vim and went with Emacs.

和很多人一样，我在对EMACS毫不知情的情况下，被推入到EMACS的世界里。
在大学的第一年里，当地的电脑社团全是Vim用户。有人对我解释，
不需要理由，你给我用Vim。并且不想告诉我为什么要这么做，
于是我选择了一个一个完全相反的一极，走向EMACS。

Emacs proved to be a stable and reliable editor in all those
years, but it was a tough one to get to know. Despite the
extensive user documentation, it never helped me to learn
and understand Emacs.

Emacs这些年被证明是一个稳定可靠编辑器，但理解这一点还是很难。
尽管有着广泛的文档存在，但这并不能帮助我学习理解Emacs。

As it turns out, Emacs is a philosophy or even a religion. So,
the joke about the “Church of Emacs” is eerily accurate in
many ways, as you will find out in the next chapter.

事实证明，Emacs是一种哲学，甚至是一种宗教。所以，在很多情况下，
Emacs教会这个玩笑说的很准确，这点你可以在下一章找到例证。

Intended Audience
目标受众

It’s a bit weird talking about the intended audience when
you’ve already bought the book on the subject. But it bears
mentioning anyway so no matter your Emacs skill level you
will get something out of this book.

讲目标受众这个东西很奇怪，因为毕竟你们已经买了这本书。
但还是要提下，不管你Emacs是什么水平，你还是能从这本书里得到些东西。

The first and (most obvious) audience are people new to
Emacs. If you’ve never used Emacs before in your life, you
will hopefully find this book very useful. However, if
you’re new to Emacs and non-technical, then you’re going
to have a harder time. Emacs, despite being suitable for
much more than just programming, is squarely aimed at
computer-savvy people. Although it’s perfectly possible
to use Emacs anyway, this book will assume that you’re
technically inclined, but not necessarily a programmer.

最首要的受众是那些Emacs新手。如果你从来没用过Emacs，
你会发现这本书非常有用。然而，如果你是Emacs新手并且不是技术人员，
你会经历一段痛苦的时期。Emasc，不仅适用于编程人员，
同时也适用于喜欢捣鼓电脑的人。任何情况下都使用Emacs是可能的，
这本书假设你是有技术倾向的人，但是不是专业的程序人员。


If you’ve tried Emacs before but given up, then I hope this
book is what convinces you to stick with it. But it’s fine if
you don’t; some languages or environments don’t (contrary
to what a lot of Emacs users would claim) work well with
Emacs. If you’re primarily a Microsoft Windows developer
working with Visual Studio, using Emacs is going to be a
case of two steps forward, one step back: you gain unprece-
dented text editing and tool integration but lose all the ben-
efits a unified IDE would give you.

假如你曾经尝试过Emacs但放弃了，希望这本书能说服你坚持下去。
但是你不坚持也ok。因为一些语言和环境（很多Emacs声称）不能在
Emacs中很好的工作。如果你使用Visual Studio工作，使用Emacs将会
给你带来前所未有的编辑和工具集成体验，但会丧失集成开发工具带来
的好处。

If you’re a Vim refugee, then welcome to the dark side! If
your primary objective is to use Emacs’s Vim emulation lay-
ers, then some of this book is redundant; it concerns itself
with the default Emacs bindings and it teaches “the Emacs
way”of doing things.But not to worry:a lot of the tips and
advice herein are still applicable, and who knows — maybe
you’ll switch away from Evil mode in time.

假如你是一个Vim难民，欢迎来到黑暗的另一面。假如你的主要目的是
使用Emacs中的Vim仿真，那么这本书对你来说是多于的。这本书主要关注
Emacs绑定按键和Emacs做事的方式。这有很多提示和建议仍然很实用，
谁知道呢，也许你会半路转到Evil模式。

And finally, if you’re an existing Emacs user but struggling
to take it to the next level,or maybe you just need a refresher
course “from the ground up,” then this book is also for you.

最后，徦如你已经是一个Emacs用户，挣扎着想提高自己的Emacs水平，
或者只是想来个复习，这本书都适合你。

What You’ll Learn
你将学到什么？

Covering all of Emacs in just one book would be a Sisyphean
task.Instead,I aim to teach you what you need to be produc-
tive in Emacs, which is just a small subset of Emacs’s capabil-
ity. Hopefully, by the end of this book, and with practice,
you will know enough about Emacs to seek out and answer
questions you have about the editor.

覆盖所有Emacs知识在一本书里是徒劳的任务。相反，我想教你在Emacs中
生产需要的东西，这会是Emacs能力的一部分。希望，到了这本书的最后，
并通过练习，你能对Emacs有足够多的了解来解决关于这个编辑器相关的问题。

To be more specific, I will teach you, in broad terms, six
things:
更具体的，我将教你以下六点。

What Emacs is about: A thorough explanation of impor-
tant terminology and conventions that Emacs uses
which in many cases differs greatly from other editors.
You will also learn what the philosophy of Emacs is,
and why a text editor even has a philosophy. I will
also talk about Vim briefly and the Editor Wars and
what the deal is with all those different keys.

什么是Emacs，Emacs就是一套贯穿始终的术语和在很多方面
区别于其他编辑器的约定。你还会学到什么是Emacs的哲学，
和为什么一个编辑器会有哲学。我简单的聊下vim，以及编辑器之争，
。。。

Getting started with Emacs How to install Emacs,how to
run it, and how to ensure you’re using a reasonably
new version of Emacs. I explain how to modify Emacs and 
what you need to do to make your changes permanent.
I will introduce the Customize interface and how to load
a color theme. And finally,I'll talk about the user interface
of Emacs and some handy tips in case you get stuck.

首先从怎么安装Emacs，怎么运行它，怎么确认是最新版的Emacs。
我会解释怎么修改Emacs和怎么让你的修改常驻。我会介绍个性化
界面，和怎么加载一个顔色主题。最后，我会讲讲Emacs的用户界面，
和一些便利的提示来防止你卡住。

Discovering Emacs Emacs is self-documenting; but what
does it mean and how can you leverage that aspect
to discover more about Emacs or answer questions
you have about particular features? I will show you
what I do when I have to learn how to use a new
mode or feature in Emacs, and how you can use the
self-documenting nature of Emacs to find things for
which you’re looking.

发现Emacs，Emacs是一个自文档的。但是，这意味什么以及你怎么
利用这个特点来挖掘Emacs或回答你遇到的特别问题。我会向你
展示当我使用Emacs的一个新模式或特性时会怎么做，以及当你
在查找东西时怎么利用Emacs的自文档特性。

Movement How to move around in Emacs.At first glance a
simple thing to do, but in Emacs there are many ways
of going from where you are to where you need to
go in the fewest possible keystrokes. Moving around
is probably half the battle for a developer and know-
ing how to do it quickly will make you more efficient.
Some of the things you’ll learn: moving by syntactic
units, and what exactly syntactic units are; using win-
dows and buffers; searching and indexing text; select-
ing text and using the mark.

怎么在Emacs中移动。首先，找个简单的来试，但其实Emacs中
有很多简单有效的快捷键来移动。移动方式几乎占了开发者一半
的争论点，以及知道怎么快速移动可以让你更高效。你将会学习：
通过语法单元移动、什么是语法单元、使用窗口和缓存、搜索和
索引文档、选择文本和使用标记。

Editing As in the chapter on movement, I will show you
how to edit text using a variety of tools offered to you
by Emacs. This includes things like editing text by bal-
anced expressions, words, lines, paragraphs; creating
keyboard macros to automate repetitive tasks; 
search-ing and replacing; registers; multi-file editing;
 abbreviations; remote file editing; and more.

。。。 我将会向你展示怎么使用Emacs提供的一系列工具进行文本编辑。
包括表达、词语、行、段落的操作。创建宏来自动化重复任务：
搜索、替换、登记；多文件编辑、缩写、远程文件编辑；其他。

Productivity Emacs can do more than just edit text and this
chapter is only a taste of what attracts so many people
to Emacs: its tight integration with hundreds of exter-
nal tools.I will whet your appetit eand show you some
of the more interesting things you can do when you
choreograph Emacs’s movement and editing.

Emacs的生产效率不仅仅包括文本编辑，这节将尝试说明Emacs吸引这么
多人的原因：与大量的扩展工具有紧密结合。我将会打磨你的兴趣，并向你
展示当你编排Emacs的移动和编辑时你可以做些什么。
