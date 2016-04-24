---
published: true
author: Charles
layout: post
title:  "常用编程思想"
date:   2016-02-22 14:28
categories: 数据结构与算法
---

http://www.jianshu.com/p/b5f9ac6de184
http://blog.csdn.net/ohmygirl/article/details/7850068

#### 双指针法
常见问题：

- in place 的更新数组，需要一个index记录更新之后的数组，另一个index跑遍原来的数组； 

{% highlight c++ %}
int removeElement(vector<int>& nums, int val) {
    int i = -1;
    for(int j = 0; j < nums.size(); ++j){
        if(nums[j] != val)
            nums[++i] = nums[j];
    }
    return i + 1;
}
{% endhighlight %}

- 找到数组里面的N个数使得这几个数满足一定的条件（如几个数之和必须为某一个特定的数）；
- 还有就是一类特殊的问题-雨水储存问题。


