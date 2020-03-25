---
title: Leet Code 刷题笔记 一
date: 2019-10-09 21:30:00
thumbnail:
categories:
 - 笔记
 - 算法
tags:
 - LeetCode
 - 算法
---

刷题笔记

<!--more-->

## 两数相加

### 题目描述

给出两个**非空**的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。  
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。  
您可以假设除了数字**0**之外，这两个数都不会以**0**开头。

示例

```text
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

链接：<https://leetcode-cn.com/problems/add-two-numbers>

由于鸽子了一个假期没写过算法，都在写业务型的代码，经过万分努力。终于让代码通过编译，成功跑过第一个测试用例。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int sum = 0; //两数之和加进位
        int digital = 0; //和模10
        int temp = 0; //是否进位
        ListNode *head = new ListNode(0);//Node头指针,该链表为带头指针链。return时将头指针的next传递
        ListNode *tail = NULL; //尾指针

        while (l1 || l2) {
            sum = l1->val + l2->val + temp; //两数之和加进位
            temp = 0;//进位归零

            l1 = l1->next; //指针移动
            l2 = l2->next; //指针移动

            if(temp = sum >= 10) {
                digital = sum % 10; //取模
                temp = 1; //进位
            } else {
                digital = sum;
            }

            ListNode *temp = new ListNode(digital);
            if (head->next == NULL) {
                tail = head->next = temp;
            } else {
                tail->next = temp;
                tail = tail->next;
            }
        }
        return head->next;
    }
};
```

然后提交后就卡测试用例8了,原因是两个不同位数的数相加，造成的空指针错误~~果然中档题不是我等没玩过OI的人能随便碰的~~

```text
执行出错信息： Line 19: Char 33: runtime error: member access within null pointer of type 'struct ListNode' (solution.cpp)
最后执行的输入：
                [1,8]
                [0]
```

问题出在这几行

1. 当位数未对其的情况下，其中一个已经访问到尽头的list指针进行l_->val操作会造成空指针错误
2. 指针的移动操作，如果当前指针已经空了，还访问next指针，造成空指针错误

```cpp
    ...
    while (l1 || l2) {
>       sum = l1->val + l2->val + temp; //两数之和加进位
        temp = 0;//进位归零
    ...
```

```cpp
    ...
>    l1->next; //指针移动
>    l2->next; //指针移动
    ...
```

修改代码，判断当前指针是否已经空了，如果空了，即该位可以以0代替

```cpp
    ...
    while (l1 || l2) {
>       sum = (l1 != NULL ? l1->val : 0) + (l2 != NULL ? l2->val : 0) + temp; //两数之和加进位
        temp = 0;//进位归零
    ...
```

修改代码，判断当前节点是否已空，不能执行指针移动操作

```cpp
    ...
>   if(l1 != NULL)l1 = l1->next; //指针移动
>   if(l2 != NULL)l2 = l2->next; //指针移动
    ...
```

通过了上一个卡住的测试用例了，然后卡在了第1364条测试用例

```text
输入：
    [5]
    [5]
输出：
    [0]
预期：
    [0,1]
```

原因是遇到5 + 5这种进位后两链表都到了尽头，但是却存在着进位还未输出的情况

```cpp
    ...
>   while (l1 || l2) {
        sum = (l1 != NULL ? l1->val : 0) + (l2 != NULL ? l2->val : 0) + temp; //两数之和加进位
        temp = 0;//进位归零
...
```

这个好整，判断一下进位是否空即可

```cpp
    ...
>   while (l1 || l2 || temp != 0) {
        sum = (l1 != NULL ? l1->val : 0) + (l2 != NULL ? l2->val : 0) + temp; //两数之和加进位
        temp = 0;//进位归零
...
```

成功通过的代码完整如下

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int sum = 0; //两数之和加进位
        int digital = 0; //两数之和的%10余
        int temp = 0; //存进位
        ListNode *head = new ListNode(0);//Node头指针,该链表为带头指针链。return时将头指针的next传递
        ListNode *tail = NULL; //尾指针

        while (l1 || l2 || temp != 0) {
            sum = (l1 != NULL ? l1->val : 0) + (l2 != NULL ? l2->val : 0) + temp; //两数之和加进位
            temp = 0;//进位归零

            if(l1 != NULL)l1 = l1->next; //指针移动
            if(l2 != NULL)l2 = l2->next; //指针移动

            if(sum >= 10) { //进位处理条件
                digital = sum % 10; //取模
                temp = 1; //进位
            } else {
                digital = sum; //无需进位处理
            }

            ListNode *temp = new ListNode(digital); //建立当前位数节点
            if (head->next == NULL) { //如果头指针指空，设置首元节点
                tail = head->next = temp;
            } else { //尾插节点
                tail->next = temp;
                tail = tail->next;
            }
        }
        return head->next; //返回首元节点
    }
};
```
