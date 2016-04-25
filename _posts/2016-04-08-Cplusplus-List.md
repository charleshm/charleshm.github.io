---
published: true
author: Charles
layout: post
title:  "链表"
date:   2016-04-08 14:28
categories: 数据结构与算法
---

#### 头节点和头指针
- 头结点是为了操作的统一与方便而设立的，放在第一个元素结点之前，其数据域一般无意义（当然有些情况下也可存放链表的长度、用做监视哨等等）。
- 有了头结点后，对在第一个元素结点前插入结点和删除第一个结点，其操作与对其它结点的操作统一了。

![此处输入图片的描述][1]

----------

- 前驱：prev
- 后继：next
- 当前结点：cur

----------


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
    head->next->next = head; //让下一个结点指向当前节点
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


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/headnode.jpg