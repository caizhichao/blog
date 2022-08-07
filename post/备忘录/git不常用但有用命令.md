---
title: git不常用但有用命令
date: 2019-08-21 23:29:47
tags:
categories: 备忘录
---

### 删除对某些文件的版本管理
* git rm -r --cached  "bin/"      
* git commit -m" remove bin folder all file out of control"    
* 查看结果没问题了再push 

### 通过命令行创建一个新的远程版本库并推送到远程
* touch README.md
* git init
* git add README.md
* git commit -m "first commit"
* git remote add origin ssh://admin@192.168.1.23:29418/qusouti.git
* git push -u origin master 

## 远程已经存在分支,将本地推送到那
git remote set-url origin http://***
git push
