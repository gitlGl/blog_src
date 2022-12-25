---
title: C语言多进程
date: 2022-11-12 23:50:54
tags: [C语言]
categories: [多进程]
---

__c语言多进程写法__
在linux下：
<!--more-->
```C
#include <stdio.h>
#include <unistd.h>
int main()
{
   /*这段代码在linux中运行，fork函数有两次返回，
   即调用一次，返回两次。在父进程返回子进程的pid，在子进程返回0，
   如果返回负数则表明fork失败。
   所以，我们根据返回值来判断当前进程是父进程还是子进程。*/
   printf("父进程 id: %d\n", getpid());
   int f_id;
   for (int i = 0; i < 3; i++)
   {
      f_id = fork();
      if (f_id == 0)
      {
         printf("子进程 id: %d\n", getpid());
         printf("子进程里的f_id : %d\n", f_id);
         return 0;
      }
      printf("进父程里的f_id : %d\n", f_id);
   }
}
     ```