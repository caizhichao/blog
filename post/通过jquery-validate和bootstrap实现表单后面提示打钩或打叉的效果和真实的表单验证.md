---
title: 通过jquery-validate和bootstrap实现表单后面提示打钩或打叉的效果和真实的表单验证
categories:
  - 程海迷航
date: 2019-08-24 09:41:00
tags:
archives:
---


<!-- wp:paragraph -->
<p>首先，需要下载以下框架的代码，版本没有严格的要求。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>一、工具文件准备</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>1、jquery.js &nbsp;2、bootstrap.css 3、jquery.validate.js</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>这三个文件非常容易下载，下载然后看看包里有名字跟以上名字一致的就是了。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>二、在html中引用css文件和js文件</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>三、创建一个新的的js文件，这个文件使用来调用jquery.validate.js中封装的方法的，代码如下</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>

以下如error和element等都是jq对象，可以使用jq方法

</p>
<!-- /wp:paragraph -->

```js
$().ready(function() {
    var validaObj = $("#form_login").validate({ //创建一个validate对象，可以自己查询使用一些该对象的衍生方法
        rules: {
            username: {
                required: true,
                minlength: 4,
                maxlength: 8
            }
        },
        messages: {
            username: {
                required: "",
                minlength: "",
                maxlength: ""
            }

        },
        success: function(e) { //关键方法，该方法会在validate完成该输入框的判断以后进行一次调用并且会取消默认的显示错误的方法（大概会吧）
            //如果进行调试，会方法输入框的class属性会在根据是否合法改变其样式为  "error"和"valid"中的一种，通过判断这个进行判断
            if (e.prev().hasClass("valid")) { //此处e相当于错误信息这个html标签本身，然后找e的前一个标签是否有"valid"类样式，有则说明当前输入框验证正确
                e.removeClass("glyphicon-remove"); //boostrap的样式，查手册，bootstrap3的全局样式里的表单里有样式说明，添加了对应样式span标签会变为图形。
                //比如在引入bootstrap.css的情况下 <span class = "glyphicon glyphicon-ok form-control-feedback" ></span> 就是一个打钩的图案
            e.addClass("glyphicon-ok");
         }
         if(e.prev().hasClass("error")){  // 判断输入框里有样式"error"则把打叉换成打钩e.removeClass("glyphicon-ok");
                e.addClass("glyphicon-remove");
            }

        },
        errorPlacement: function(error, element) { //决定错误信息要放在哪里的方法，error是label对象，element是当前验证的输入框对象
            error.addClass('glyphicon glyphicon-remove form-control-feedback');
            error.appendTo(element.parent());

        }

    });
```

<!-- wp:paragraph -->
<p>当然使用框架很爽，那些什么点击验证，多少长度什么格式都封装好了，注意逗号的分割规律使用就没什么大问题<br></p>
<!-- /wp:paragraph -->
