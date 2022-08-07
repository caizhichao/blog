---
title: 使用新浪SAE和PHPthink遇到的坑总结
categories:
  - 程海迷航
date: 2019-08-24 09:37:56
tags:
archives:
---

#### 纪念用,以前放在新浪博客来着,刚学编程的时候,hx1603, 16年的3月报的传一培训

<!-- wp:paragraph -->
<p>1、使用微信官方的示例复制到代码中进行测试，经常不能产生作用，必要的时候还是要自己打最最简单的代码进行测试。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>2、新浪sae不支持在服务器上通过代码进行读写，就是那些fopen之类的函数都不能用。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>3、由于微信端和我方服务器之间的交互基本上依靠xml和json完成，所有使用最简单的字符串拼接会使得代码十分容易理解，少用网上复制的经常会看不懂。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>4、还有千万写代码的时候要小心，自己曾遇到的坑包括占位符写错（就是这么傻），路径两个斜杠，文件后缀写错，phpthink忘记namespace等等，都是泪啊。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>5、使用微信jssdk时候，页面的签名一定要做成动态的，因为页面分享以后会自动在后面加入其它代码导致签名错误，动态获取当前页面的url可以使用</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$url = 'http://' . $_SERVER['HTTP_HOST'] . $_SERVER['PHP_SELF'] . '?' . $_SERVER['QUERY_STRING'];来获取。</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>6、还有有时会遇到invalid domain的错误提示，这说明你js安全域名不对，注意这里是tm的域名不是url。</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>7、微信jssdk有官方demo，挺好用的但是其中一些事件根本不是什么点击事件，我们看看function里面的内容就好了，外面的点击事件不用太关心。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>8、新浪sae的使用中，有时候需要使用其他应用的数据比如你想建两个应用一个用来处理前台逻辑，一个用来给当做数据管理统计后台，这时候就需要使用新浪sae提供的应用授权功能，完成授权以后，在另外那个应用通过主库地址和数据库名用户名秘密就可以访问了，phpthink是在模块conf文件夹里的config.php里设置就可以了。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>9、有大量的默认参数在使用新浪sae时会被自动设置，比如comman里的数据库设置一般是无效的，需要通过home或其他自定义模块内的config配置来进行，相关的配置优先级在phpthink手册里都有说。</p>
<!-- /wp:paragraph -->
