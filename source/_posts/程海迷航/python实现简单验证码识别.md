---
title: python实现简单验证码识别
categories:
  - 程海迷航
date: 2019-08-24 09:12:33
tags: 
- python
- 验证码识别
archives:
---

<!-- wp:paragraph -->
<p>

1. 安装python和pip不作介绍，保证打开cmd输入pip和python有反应。需设置环境变量。[python3.6安装包](https://pan.baidu.com/s/1i51pccl<br>2.参考http://www.jianshu.com/p/9e97c9b7dab6)
2. 在cmd中执行以下命令([怎么启动cmd](https://jingyan.baidu.com/article/046a7b3e67f1e1f9c37fa976.html))
<!-- /wp:paragraph -->

```
pip install PIL
pip install Pillow
pip install pytesseract
```

3. 目标图片
![验证码](index.png)

4. 识别代码(python3)
***如果出现无法导入需要自己解决依赖问题***
```python
from PIL import Image
from pytesseract import * 


im = Image.open('index.png')

im = im.convert('L')
def initTable(threshold=140):
    table = []
    for i in range(256):
        if i < threshold:
            table.append(0)
        else:
            table.append(1)

    return table

binaryImage = im.point(initTable(), '1')
#binaryImage.show()

print(image_to_string(binaryImage, config='-psm 7'))
```

5. 结果截图
![结果](003fKHE6zy7gw43IqODfb690.jpg)



#### 其它
[参考链接](http://www.jianshu.com/p/9e97c9b7dab6)
