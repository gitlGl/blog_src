---
title: kmp算法
date: 2022-12-08 14:23:31
tags: 算法
categories: C语言
---
__字符串匹配算法__
1.暴力求解代码如下：
<!--more-->
```C
#include <stdio.h>
#include <string.h>
int match(char *src, int size_r, char *target, int size_)//直接暴力求解
{
    if (size_r < size_)
        printf("error");
    int each_sr = 0, each_target = 0, count = 0;
    while (each_target < size_ && each_sr < size_r)
    {
        if (src[each_sr] == target[each_target])
        {
            each_target++;
            each_sr++;
        }
        else
        {
            each_sr = ++count;//匹配失败，回到前面重新开始
            each_target = 0;
        }
    }

    if (each_target == size_)
        return each_sr;

    else
        return 0;
}
int main()
{
    char *p1 = "dffhhgahabhghhgaabjbdfh898jhgabjjdffj";
    char *p2 = "hgabj";

    int p1_size = strlen(p1), p2_size = strlen(p2);
    int index = match(p1, strlen(p1), p2, strlen(p2));//返回值是匹配成功后，子串中的最后一个字符索引
    if (index)
    {
        char *p3 = p1 + index - p2_size;
        for (int i = 0; i < p2_size; i++)
        {
            printf("%c", p3[i]);
        }
        return 0;
    }
}
```
2.kmp字符串匹配算法代码如下：
```C
int kmp(char *src, int size_r, char *target, int size_)//利用前面记录求解
{
    待续
}
int main()
{
    char *p1 = "dffhhgahabhghhgaabjbdfh898jhgabjjdffj";
    char *p2 = "hgabj";

    int p1_size = strlen(p1), p2_size = strlen(p2);
    int index = kmp(p1, strlen(p1), p2, strlen(p2));//返回值是匹配成功后，子串中的最后一个字符索引
    if (index)
    {
        char *p3 = p1 + index - p2_size;
        for (int i = 0; i < p2_size; i++)
        {
            printf("%c", p3[i]);
        }
        return 0;
    }
}
```

