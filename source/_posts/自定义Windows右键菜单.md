---
title: 自定义Windows右键菜单
date: 2023-02-06 14:07:47
tags: 杂
categories: windows配置
---
1、win+R快捷键，输入regedit后回车，打开注册表编辑器；

在注册表编辑器中找到 “计算机\HKEY_CLASSES_ROOT\Directory\Background\shell”，
<!--more-->
2、右键点击shell，新建项，命名为“xxx”；

3、右键点击“xxxx”，新建项，命名为“command”；这里注意：子项的名称必须为command

4、双击，将“command”的数值数据修改为“xxxx.exe --cd "%V"。

