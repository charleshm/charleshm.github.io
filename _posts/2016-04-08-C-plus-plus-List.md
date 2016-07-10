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

在编程中主要在两种情况下使用：

- head可能被删除
- head可能被修改

----------

- 前驱：prev
- 后继：next
- 当前结点：cur

----------

#### 头插法

![此处输入图片的描述][2]

{% highlight c++ %}
s->next = head->next;　　//第一步
head->next = s;　　　　　   //第二步
{% endhighlight %}


----------

#### 尾插法

![此处输入图片的描述][3]

{% highlight c++ %}
prev->next = s;
s->next = nullptr;
{% endhighlight %}

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

> 将当前结点的 next 字段值改成 prev（上一个结点的指针）的值

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
    //最后一个结点会返回作为头结点
    if (　head == NULL || head->next == NULL) return head;
    //先反转后面的链表
    ListNode  *newHead = reverse(head->next);
    head->next->next = head; //让下一个结点指向当前结点
    head->next = NULL;
    return newHead;
}
{% endhighlight %}

#### Partition List

----------

#### 链表反转局部

![此处输入图片的描述][4]

{% highlight c++ %}
ListNode* reverseBetween(ListNode* head, int m, int n) {
    ListNode dummy(INT_MIN);
    dummpy.next = head;
    ListNode *prev = &dummpy;

    for (int i = 0; i < m - 1; ++i) prev = prev->next;
    ListNode *head2 = prev;
    prev = prev->next;
    ListNode *cur = prev->next;
    for (int i = m; i < n; ++i) {
        prev->next = cur->next;
        cur->next = head2->next;
        head2->next = cur;
        cur = prev->next;
    }
    return dummy.next;

}
{% endhighlight %}

----------

#### [删除链表中的重复元素][5]

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


----------

#### [Remove Duplicates from Sorted List II][6]

{% highlight c++ %}
ListNode* deleteDuplicates(ListNode* head) {
    if (head == nullptr) return head;
    // 因为头指针可能变化，所以我们定义头结点来记录其位置
    ListNode dummy(INT_MIN);
    ListNode *prev = &dummy, *cur = head;
    while (cur != nullptr) {
        // 记录当前变量是否重复
        bool duplicated = false;
        while (cur->next != nullptr && cur->val == cur->next->val) {
            duplicated = true;
            ListNode *temp = cur;
            cur = cur->next;
            delete(temp);
        }
        // 删除重复的最后一个元素
        if (duplicated) {
            ListNode *temp = cur;
            cur = cur->next;
            delete(temp);
            continue;
        }
        prev->next = cur;
        prev = cur;
        cur = cur->next;
    }
    prev->next = cur;
    return dummy.next;
}
{% endhighlight %}

----------

#### Partition List
拆分成两段链表来做，再合起来

{% highlight c++ %}
ListNode* partition(ListNode* head, int x) {
    ListNode left_dummy(-1), right_dummy(-1);
    ListNode *left_prev = &left_dummy, *right_prev = &right_dummy;
    ListNode *cur = head;
    while (cur) {
        if (cur->val < x) {
            left_prev->next = cur;
            left_prev = cur;
        } else {
            right_prev->next = cur;
            right_prev = cur;
        }
        cur = cur->next;
    }
    left_prev->next = right_dummy.next;
    right_prev->next = nullptr;
    return left_dummy.next;
}

{% endhighlight %}

----------

#### Rotate List
将链表连接成环，然后在指定位置断开即可

![此处输入图片的描述][7]

{% highlight c++ %}
ListNode* rotateRight(ListNode* head, int k) {
    if (head == nullptr || k == 0) return head;
    int len = 1;
    ListNode *ptr = head;
    while (ptr->next) {
        ++len;
        ptr = ptr->next;
    }
    ptr->next = head;
    int steps = len - k % len;
    while (steps) {
        --steps;
        ptr = ptr->next;
    }
    head = ptr->next;
    ptr->next = nullptr;
    return head;
}
{% endhighlight %}

----------

#### Reverse Nodes in k-Group

头指针会发生变化，所以这道题用上了三指针

{% highlight c++ %}
ListNode* swapPairs(ListNode* head) {
    if (head == nullptr || head->next == nullptr) return head;
    ListNode dummy(-1);
    dummy.next = head;
    ListNode *prev = &dummy, *cur = prev->next, *next = cur->next;
    while (next) {
        // swap
        prev->next = next;
        cur->next = next->next;
        next->next = cur;

        //update
        prev = cur;
        cur = cur->next;
        next = cur ? cur->next : nullptr;
    }
    return dummy.next;
}
{% endhighlight %}

----------

#### Reverse Nodes in k-Group

{% highlight c++ %}
//本题关键在于合理转化问题，设计到链表反转，链表连接中的一些细节问题
ListNode* reverseKGroup(ListNode* head, int k) {
    if (head == nullptr || head->next == nullptr || k < 2) return head;
    ListNode dummy(-1);
    dummy.next = head;

    ListNode *prev = &dummy, *end = head;
    while (end) {
        int steps = k;
        while (--steps && end) end = end->next;
        if (end == nullptr) break;//长度不足K
        prev = reverse(prev, end);//链表反转
        end = prev->next; //end更新为下一组的头指针
    }

    return dummy.next;
}

//常规链表反转
ListNode* reverse(ListNode *prev, ListNode *end) {
    ListNode *begin = prev->next;
    ListNode *cur = prev->next, *next = cur->next;
    while (cur != end) {
        ListNode *tmp = next->next;
        next->next = cur;
        cur = next;
        next = tmp;
    }
    prev->next = cur;
    begin->next = next;
    return begin;
}
{% endhighlight %}

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/headnode.jpg
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/1-140F9152T3201.jpg
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/tail_insert.jpg
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/reverse_link.png
  [5]: https://leetcode.com/problems/remove-duplicates-from-sorted-list/
  [6]: https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/circle.png
