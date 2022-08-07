---
title: select、poll、epoll详解
tags: 转载
categories:
  - 转载
date: 2019-08-24 09:07:22
archives:
---
[转载链接](https://www.cnblogs.com/zhangxinqi/p/8337207.html)

<!-- wp:paragraph -->
<p>select，poll，epoll都是IO多路复用的机制。I/O多路复用就是通过一种机制使一个进程可以监视多个描述符，一旦某个描述符就绪（一般是读就绪或者写就绪），能够通知程序进行相应的读写操作。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; select，poll，epoll本质上都是同步I/O，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>&nbsp; sellect、poll、epoll三者的区别 :</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;<strong>select：</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;目前支持几乎所有的平台</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;默认单个进程能够监视的文件描述符的数量存在最大限制，在linux上默认只支持1024个socket</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; &nbsp; 可以通过修改宏定义或重新编译内核（修改系统最大支持的端口数）的方式提升这一限制</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;内核准备好数据后通知用户有数据了，但不告诉用户是哪个连接有数据，用户只能通过轮询的方式来获取数据</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; &nbsp; 假定select让内核监视100个socket连接，当有1个连接有数据后，内核就通知用户100个连接中有数据了</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; &nbsp; 但是不告诉用户是哪个连接有数据了，此时用户只能通过轮询的方式一个个去检查然后获取数据</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; &nbsp; 这里是假定有100个socket连接，那么如果有上万个，上十万个呢？</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; &nbsp; 那你就得轮询上万次，上十万次，而你所取的结果仅仅就那么1个。这样就会浪费很多没用的开销</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;只支持水平触发；每次调用select，都需要把fd集合从用户态拷贝到内核态，这个开销在fd很多时会很大</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 同时每次调用select都需要在内核遍历传递进来的所有fd，这个开销在fd很多时也会很大</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>poll：</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;与select没有本质上的差别，仅仅是没有了最大文件描述符数量的限制</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 只支持水平触发</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 只是一个过渡版本，很少用</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>&nbsp; epoll：</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; Linux2.6才出现的epoll，具备了select和poll的一切优点，公认为性能最好的多路IO就绪通知方法</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 没有最大文件描述符数量的限制</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 同时支持水平触发和边缘触发</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 不支持windows平台</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 内核准备好数据以后会通知用户哪个连接有数据了</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; IO效率不随fd数目增加而线性下降</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 使用mmap加速内核与用户空间的消息传递</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>水平触发与边缘触发：</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;水平触发：将就绪的文件描述符告诉进程后，如果进程没有对其进行IO操作，那么下次调用epoll时将再次报告这些文件描述符，这种方式称为水平触发</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;边缘触发：只告诉进程哪些文件描述符刚刚变为就绪状态，它只说一遍，如果我们没有采取行动，那么它将不会再次告知，这种方式称为边缘触发</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 理论上边缘触发的性能要更高一些，但是代码实现相当复杂。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;<strong>select和epoll的特点</strong>：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;&nbsp;select：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; select通过一个select()系统调用来监视多个文件描述符的数组，当select()返回后，该数组中就绪的文件描述符便会被内核修改标志位，使得进程可以获得这些文件描述符从而进行后续的读写操作。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 由于网络响应时间的延迟使得大量TCP连接处于非活跃状态，但调用select()会对所有socket进行一次线性扫描，所以这也浪费了一定的开销。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; epoll:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; epoll同样只告知那些就绪的文件描述符，而且当我们调用epoll_wait()获得就绪文件描述符时，返回的不是实际的描述符，而是一个代表就绪描述符数量的值，你只需要去epoll指定的一个数组中依次取得相应数量的文件描述符即可，这里也使用了内存映射（mmap）技术，这样便彻底省掉了这些文件描述符在系统调用时复制的开销。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp; 另一个本质的改进在于epoll采用基于事件的就绪通知方式。在select/poll中，进程只有在调用一定的方法后，内核才对所有监视的文件描述符进行扫描，而epoll事先通过epoll_ctl()来注册一个文件描述符，一旦基于某个文件描述符就绪时，内核会采用类似callback的回调机制，迅速激活这个文件描述符，当进程调用epoll_wait()时便得到通知。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>select</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>select(rlist,wlist,xlist,timeout=None)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;select函数监视的文件描述符分3类，分别是writefds、readfds、和exceptfds。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&nbsp;调用后select函数会阻塞，直到有描述符就绪（有数据可读、可写、或者有except），或者超时（timeout指定等待时间，如果立即返回设为null即可），函数返回。当select函数返回后，可以通过遍历fdset，来找到就绪的描述符。</p>
<!-- /wp:paragraph -->
