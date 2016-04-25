---
published: true
author: Charles
layout: post
title:  "lianbiao"
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


----------


#### 链表反转

{% highlight c++ %}
ListNode *reverse(ListNode *head) {
    ListNode *prev = NULL;
    while (head != NULL) {
        ListNode *temp = head->next;
        head->next = prev;
        prev = head;
        head = temp;
    }
    return prev;
}
{% endhighlight %}