---
title: 设计模式笔记
date: 2019-08-03 21:51:37
thumbnail:
categories:
 - 笔记
tags:
 - 设计模式
---

>设计模式（Design Pattern）是一套被反复使用、多数人知晓的、经过分类的、代码设计经验的总结。

<!--more-->
# Iterator 模式

Iterator 模式用于在数据集合中按照顺序遍历集合。英语单词 Iterator 有反复做某件事情的意思，汉语称为“迭代器”。

## Aggregate 接口

Aggregate 接口是所要遍历的集合的接口。Aggregate 有“使聚合”，“集合”的意思。

在该接口中声明的方法只有一个—— iterator 方法。该方法会生成一个用于遍历集合的迭代器。调用该方法生成一个实现了Iterator接口的类的实例。
```Java
public interface Aggregate {
    public abstract Iterator iterator();
}
```
## Iterator 接口

Iterator接口声明两个方法

### hastNext 方法

返回值为boolean，当存在下一个元素返回true，否则返回false，即已经遍历至集合末尾。该方法主要用于循环终止条件

### next 方法

该方法返回类型是Object，该方法返回的是集合中的一个元素。为了能够在下次调用next方法时正确地返回下一个元素，该方法中还需要将迭代器移动至下一个元素（返回当前的元素，并指向下一个元素）

```Java
public interface Iterator {
    public abstract boolean hasNext();
    public abstract Object next();
}
```
## 注意
 - 容易弄错下一个，返回当前的元素，并指向下一个元素
 - 容易弄错最后一个，返回最后一个元素前hasNext会返回true，当返回了最后一个元素后则返回false，理解成“确认接下来是否可以调用next方法即可”，先调用hasNext()再调用next()


# Adapter 模式

Adapter模式也被称为Wrapper模式。Wrapper有“包装器”的意思，替我们包装某样东西，使其能够用于其他用途的东西就被成为包装器。

Adapter模式有两种情况

 - 类适配器模式（使用继承的适配器）

该模式通过**继承要被适配的类**并**实现相应的接口**完成适配器工作

 - 对象适配器模式（使用委托的适配器）

该模式内置一个**要被适配的类的对象**并**继承或者实现相应的接口**完成适配器工作

## Targe（对象）

该角色负责定义所需的方法。以本章开头的例子来说，即让笔记本电脑正常工作所需的直流12伏特电源。在示例程序中，由Print接口或者类扮演此角色

## Client（请求者）

该角色负责使用Target角色所定义的方法进行具体处理。如在示例代码中，由Main类扮演此角色

## Adaptee（被适配）

Adapt-ee（被适配），Adaptee是一个持有既定方法的角色。


## Adapter（适配）

Adapter模式的主人公。使用Adaptee角色的方法来满足Target角色的需求，这是Adapter模式的目的，也是Adapter角色的作用。

# Template Method 模式

