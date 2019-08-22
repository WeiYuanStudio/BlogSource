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

父类中定义处理流程的框架，子类中实现具体处理的模式

着眼于：
 - 在子类中可以调用父类中调用的方法
 - 可以通过在子类中增加方法以实现新的功能
 - 在子类中重写父类方法可以改变程序行为
 - 期待子类去实现抽象方法
 - 要求子类去实现父类方法
 - 在抽象类中决定处理的流程十分重要

# Factory Method 模式

Factory 有工厂的意思，用Template Method模式来构建生成实例的工厂，这就是Factory Method.

## Product 产品

属于框架方，是一个抽象类，定义了Factory Method中生成的那些实例所持有的接口，但具体的处理由子类实现。

## Creator 创建者

属于框架方，负责生成Product角色的抽象类，不用new关键字来生成实例，**而是调用生成实例的专用方法来生成实例**，由子类实现该方法，防止父类与其他具体类耦合

## ConcreteProduct

具体加工方

## ConcreteCreator

具体加工方

# Singleton

确保只生成一个实例的模式被称作Singleton模式

## Singleton

在Singleton模式下，只有Singleton一个角色，内含一个可以返回唯一实例的static方法。

***为了防止不小心使用构造方法，将Singleton内的构造方法设为private，不允许其他类调用Singleton的构造方法**