---
title: 设计模式笔记
date: 2019-08-03 21:51:37
thumbnail:
categories:
 - 笔记
tags:
 - 设计模式
---
# 设计模式笔记

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

### hastNext方法

返回值为boolean，当存在下一个元素返回true，否则返回false，即已经遍历至集合末尾。该方法主要用于循环终止条件

### next方法

该方法返回类型是Object，该方法返回的是集合中的一个元素。为了能够在下次调用next方法时正确地返回下一个元素，该方法中还需要将迭代器移动至下一个元素（返回当前的元素，并指向下一个元素）

```Java
public interface Iterator {
    public abstract boolean hasNext();
    public abstract Object next();
}
```
## 注意
 - 容易弄错下一个，返回当前的元素，并指向下一个元素
 - 容易弄错最后一个，返回最后一个元素前hasNext会返回true，当返回了最后一个元素后则返回false，理解成“确认接下来是否可以调用next方法即可”