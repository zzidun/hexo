---
title: c++标准draft翻译
categories:
  - 学习和理论
tags:
  - C++
  - C++语言律师
  - 工地英语
  - 有生之年
abbrlink: 87a42267
date: 2022-03-05 15:37:28
---

* 将来会翻译完的(心虚);

<!-- more -->

## 前言

有时候,我们会遇到一些语法不能确定的地方.

这时候我回去翻一下标准.

那应该不止我一个人会去标准里找答案,所以我把我阅读过的部分翻译一下.

随着我越看越多,这里也会越来越多.

## 6 基本[basic]

### 6.9 程序执行[basic.exec]

#### 6.9.3 开始和终止[basic.start]

##### 6.9.3.3 动态初始化和非阻塞变量[basic.start.dynamic]

1. 在静态存储区的非阻塞变量的动态初始化顺序是未被指定的,通常顺序会更加偏向于内联变量.

## 7 表达式[expr]

## 7.6 复合表达式[expr.compound]

### 7.6.1 后缀表达式[expr.post]

#### 7.6.1.5 类成员访问[expr.ref]



## 11 类[class]

### 11.4 类成员[class.mem]

#### 11.4.9 静态成员[class.static]

##### 11.4.9.1 通常[class.static.general]

1. 可以使用`X::s`的方式来表示某个类`X`的某个静态成员`s`.不一定需要使用类成员访问[expr.ref]的方式来表示静态成员. 静态成员也可以使用类成员访问的方式来表示,在这种情况下计算对象表达式.

```cpp
struct process {
  static void reschedule();
};
process& g();

void f() {
  process::reschedule();        // OK, no object necessary
  g().reschedule();             // g() is called
```

2. 静态成员遵守对象成员访问规则[class.access].当在类成员的声明中使用的时候,`static`只能用于出现在类定义中的成员声明中.

> 不能用在出现在命名空间的成员声明中.

##### 11.4.9.2 静态成员函数[class.static.mfct]

1. [class.mfct]描述的规则同样适用于静态成员函数.

2. 静态成员函数没有`this`指针[expr.prim.this],静态成员函数不能用`const`, `volatile` 或 `virtual`修饰[dcl.fct]

##### 11.4.9.3 静态数据成员[class.static.data]

1. 静态数据成员不是类的对象的一部分.`thread_local`的静态数据成员会被每个线程拷贝一份.如果静态数据成员没有声明`thread_local`,那么类的所有成员共享着一个数据成员.

2. 静态数据成员不该被`mutable`[dcl.stc].静态数据成员不能是匿名类[class.pre], 局部类[class.local]或内嵌类[class.nest]的直接成员[class.meml].

3. 在类定义中的非内联静态数据成员并未被定义,他可能是除`cv void`以外的未完成类型

> 静态数据成员的初始化位于他所隶属的类的作用域内[basic.scope.class]

```cpp
class process {
  static process* run_chain;
  static process* running;
};

process* process::running = get_main();
process* process::run_chain = running;
```

静态数据成员`run_chain`的定义位于全局作用域;符号`process::run_chain`表示`run_chain`是类`process`的成员,,并且在类`process`的作用域内.

> 一旦静态数据成员被定义,他就会存在,哪怕这个类实例化没有任何对象.

在这个例子,即使类`process`没产生任何对象,`running`和`run_chain`依旧存在.

[basic.start.static], [basic.start.dynameic]和[basic.start.term]描述了静态数据成员的初始化和销毁.

4. 如果一个非内联.非`volatile`的常量静态数据成员是一个整数或枚举类型,在类定义中声明语句中,它可以直接使用一个常量通过赋值表达式来初始化.

5. 在一个合法的程序中一个静态数据成员只能被定义一次.[basic.def.odr]

6. 命名空间访问中的类的静态数据成员具有类名称的链接.[basic.link]
