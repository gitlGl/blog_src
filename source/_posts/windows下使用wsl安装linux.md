---
title: windows下使用wsl安装linux
date: 2022-12-03 11:24:45
tags: wsl
categories: linux
---
__1.打开虚拟服务以及wsl子系统功能__
打开 控制面板，选择 程序和功能，选择 启用和关闭Windows功能，勾选图中红框内的两项。
如图所示：<!--more-->
![](/img/wsl.png)
然后重启系统。
__2.安装linux系统（ubuntu）__
打开cmd输入命令(默认安装状最新版本)：
```bash
wsl --install -d ubuntu
```
__3.配置用户__
安装完成后会自动打开bash(ubuntu下)引导你创建一个用户，需要输入用户名以及密码。
设置完成后打开cmd输入“bash”默认进入新创建的用户而不是root，root默认没有密码，切换前可先设置root密码。
打开cmd输入命令：
```bash
sudo passwd root 
```
然后输入root密码完成密码设置，再输入su命令后输入密码切换用户。
更改默认进入用户为root用户，打开cmd输入以下命令：
```bash
ubuntu config --default-user root
```
__4.换源__
 [打开清华源网站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
复制对版本的软件源配置内容，粘贴到ubuntu系统 __/etc/apt/sources.list__ 文件下。
使用 __apt安装__ 如果出现如下错误：
>  Certificate verification failed: The certificate is NOT trusted. The certificate chain uses expired......

则打开cmd输入bash，再输入date命令查看时间是否与网络时间相一致。如果不一致则使用以下命令安装ntp时间校准服务，且输入命令前把软件源配置内容中的https更改为http。
```bash
apt intstall ntp
```
然后把前面所述http更改回https再测试，如果再出现问题则尝试用以下命令重新安装cer证书：
```bash
sudo apt install apt-transport-https ca-certificates
```
__5.安装g++__
打开cmd 输入bash，输入以下命令：
```bash
apt install g++
```
> 另外打开cmd输入bash时所在目录为:/mnt/c...或:/mnt/d...,mnt是linux系统挂载硬盘目录，wsl上的linux系统把windows系统上的盘符挂载在mnt目录上实现了文件共享，不需要跨系统复制文件。

__6.将Ubuntu迁移到C盘外的磁盘__
cmd中输入以下命令查看linux系统发行版。
```bash
wsl -l
```
结果如下：
```bash
C:\Users\Administrator\Desktop>wsl -l
适用于 Linux 的 Windows 子系统分发版:
ubuntu (默认)
```
输入以下命令将linux系统导出：
```bash
wsl --export ubuntu d:\ubuntu2204lts.tar
```
然后输入以下命令注销C盘上的linux系统：
```bash
wsl --unregister ubuntu
```
输入以下命令新建文件夹
```bash
mkdir d:\ubuntu 
```
输入以下命令把导出的linux系统导入：
```bash
 wsl --import ubuntu d:\Ubuntu d:\ubuntu2204lts.tar
```
把以下路径的相关文件删除：
C:\Users\用户名\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\