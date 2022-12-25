---
title: 配置java环境
date: 2022-11-03 20:27:49
tags: [java]
categories: [java]
toc: true
---
### java解压方式安装
[下载地址](https://www.oracle.com/java/technologies/downloads/#jdk18-windows#jdk下载地址)

下载后解压,在jdk根目录打开cmd输入以下命令生成jre文件夹
```bash
.\bin\jlink.exe --module-path jmods --add-modules java.desktop --output jre
 ```
把 jdk根目录下的bin，jre\bin目录加入环境变量。

