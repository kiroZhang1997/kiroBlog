---
title: C++:关于常量const
tags:
- C++
- Programming
---
指向常量的指针（pointer to const）不能用于改变其指向对象的值，但可以指向一个非常量对象
```
const int a = 66;
int *ptr_a = &a; //错误，因为类型不符
const int *ptr_a = &a; //正确

int b = 77;
const int *ptr_b = &b; //正确，常量指针可以指向非常量对象 
```

常量指针（const pointer）和指向常量的指针是不同的概念。
常量指针是指把指针定义为常量，常量指针必须初始化，而且一旦初始化就不能改变。
```
int a = 233;
int *const ptr_a = &a; //ptr_a将一直指向a

const double pi = 3.1415926;
const double *const ptr_pi = &pi; //ptr_pi是指向常量pi的常量指针
```

指针本身是常量不意味着不能通过指针修改其所指对象的值，能否这样做只依赖于其所指对象的类型。
```
*ptr_pi = 8844.43; //错误，因为*ptr_pi所指的pi是一个常量

*ptr_a = 666; //正确，a不是常量

```

顶层常量（top-level const)与底层常量(low-level const)
顶层常量：指针本身是常量；底层常量：指针指向的对象是常量
```
int top = 80;
int *const ptr_top = &top; //ptr_top是顶层常量，不能改变ptr_top的值

const int low = 90;
const int *ptr_low = &low; //ptr_low是底层常量，不能改变*ptr_low的值

const int *const ptr_tl = ptr_low; //ptr_tl既是顶层常量又是底层常量

```




