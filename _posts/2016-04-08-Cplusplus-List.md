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


----------


#### 链表反转

> 将当前节点的 next 字段值改成 prev（上一个节点的指针）的值

- 非递归解法

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

- 递归解法

{% highlight c++ %}
/* 使用递归的方法 */
ListNode *reverse(struct node* head) {
    //最后一个节点会返回 作为头部
    if (　head == NULL || head->next == NULL) return head;
    //先反转后面的链表
    struct node * newHead = reverse(head->next);
    head->next->next = head; //让下一个节点指向当前节点
    head->next = NULL;
    return newHead;
}
{% endhighlight %}


----------

#### 删除链表中的重复元素

{% highlight c++ %}
ListNode *deleteDuplicates(ListNode *head) {
    if (head == NULL) return head;
    ListNode *fast = head->next;
    ListNode *slow = head;

    while (fast != NULL) {
        if (fast->val != slow->val) {
            slow->next = fast;
            slow = fast;
        }
        fast = fast->next;
    }
    slow->next = NULL;
    return head;
}
{% endhighlight %}