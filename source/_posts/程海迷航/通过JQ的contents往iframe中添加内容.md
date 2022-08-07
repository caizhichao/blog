---
title: 通过JQ的contents往iframe中添加内容
categories:
  - 程海迷航
date: 2019-08-24 09:32:38
tags: js
archives:
---

<!-- wp:paragraph -->
前提：引用JQ &nbsp;支持JQ的浏览器（不要用ie） 
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>今天被一个很烦人的问题给困扰了很久。就是怎么通过JQ来修改iframe标签的内容，你问我js处理iframe的方法？而且还要处理ie浏览的情况？出门左拐不送。说正经的，通过JQ处理iframe内容的关键是contents()方法。假如我们直接使用如下代码</p>
<!-- /wp:paragraph -->

```js
$("#iframeID").html("你好");
$("#iframeID").html("你好");</code></pre>
```
<!-- wp:paragraph -->
<p>发现都不能修改iframe中的内容，都不好使。</p>
<!-- /wp:paragraph -->

```js
var a = "测试者inner";
var b = "测试者部门inner";
$("#container02").contents().find("body").html("&lt;div>" + "&lt;span>" + a + "&lt;/span>" + "&lt;span>" + b + "&lt;/span>" + "&lt;/div>"); &lt; /script>/</code></pre>
```


<!-- wp:paragraph -->
<p>则可以顺利的给iframe中添加标签。JQ这contents帮我节约了几小时的生命。。。当然还可以反过来用，通过contents()来获取iframe中的内容然后进行修改。</p>
<!-- /wp:paragraph -->


```js
$(function() {
    $("#container02").contents().find("span").each(function() {
        if ($(this).html() == "测试者inner") //使用text()对ie无效
        {
            $(this).parent("div").remove(); //把包含条件内容的span的父亲div删除
        }
    })
});
```


<!-- wp:paragraph -->
<p>通过以上来完成检索iframe中所有span标签然后删除盛放这个span的div容器。</p>
<!-- /wp:paragraph -->
