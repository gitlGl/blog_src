---
title: C++对象细节
date: 2022-11-25 09:04:55
tags: C++
categories: C++
---
```C++
class A{
    int X(){}
}
A *a = new A();
a.X();//调用
```
<!--more-->
>调用a.x()的实质是调用x()函数，以对象a作为参数，即：x(&a),&a是this指针。
```C++
class A {
public:
    virtual void vfunc1();
    virtual void vfunc2();
    void func1();
    void func2();
private:
    int m_data1, m_data2;
};
 
class B : public A {
public:
    virtual void vfunc1();
    void func1();
private:
    int m_data3;
};
 
class C: public B {
public:
    virtual void vfunc2();
    void func2();
private:
    int m_data1, m_data4;
};
```
上面类的对象内存布局如图所示：
![](/img/内存.png)

>第一个为虚函数指针，它指向虚函数表，虚函数表编译器期生成，每个存在虚函数的类均有一个虚函数表，且位于代码段。虚函数指针在运行期间由new生成，生成后指向对应的代码段的虚函数表。多数规则、特性等是处于编译期内。