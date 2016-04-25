---
published: true
author: Charles
layout: post
title:  "链表"
date:   2016-04-08 14:28
categories: 数据结构与算法
---

#### 链表定义
{% highlight c++ %}
// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class ListNode {
    int val;
    ListNode *next;

    ListNode(int val) {
        this->val = val;
        this->next = NULL;
    }
};
{% endhighlight %}

