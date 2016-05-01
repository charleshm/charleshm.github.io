---
published: true
author: Charles
layout: post
title:  "排序算法"
date:   2016-02-05 15:30
categories: 数据结构与算法分析
---

**分治法**的基本思想是：将原问题分解为若干个规模更小但结构与原问题相似的子问题，**递归地**解这些子问题，然后将这些子问题的解组合为原问题的解。

- **分解**（divide）：将原问题分解成一系列子问题
- **解决**（conquer）：递归地解决各子问题。若子问题足够小，则直接求解
- **合并**（combine）：将子问题的结果合并成原问题的解


----------

#### 快速排序

利用**分治法**可将快速排序的分为三步：

- 在数据集之中，选择一个元素作为”**基准**”（pivot）。
- 所有小于”基准”的元素，都移到”基准”的左边；所有大于”基准”的元素，都移到”基准”的右边。这个操作称为分区 (partition) 操作，分区操作结束后，基准元素所处的位置就是最终排序后它的位置。
- 对”基准”左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。


![此处输入图片的描述][1]

----------

    #include<iostream>
    #include<vector>

    using namespace std;

    int partition(vector<int> &vi, int low, int up) {
        int pivot = vi[up];  // 直接选最右边的元素为基准元素
        int i = low - 1;
        for (int j = low; j < up; j++) {
            if (vi[j] <= pivot) {
                ++i;
                swap(vi[i], vi[j]);
            }
        }

        swap(vi[i + 1], vi[up]); // 将基准元素放到正确位置上
        return i + 1;
    }

    void quicksort(vector<int> &vi, int low, int up) {
        if (low < up) {
            int mid = partition(vi,low,up);
            quicksort(vi, low, mid - 1);
            quicksort(vi, mid + 1, up);
        }
    }

    int main() {
        vector<int> a{ 3,7,8,4,2,1,9,5,5 };
        for (auto i : a)
            cout << i << " ";
        cout << endl;

        quicksort(a, 0, a.size() - 1);
        for (auto i : a)
            cout << i << " ";
        cout << endl;

        system("pause");

    }

----------

#### 归并排序

- **分解**（divide）：将$$n$$个元素分成各含$$\frac{n}{2}$$个元素的子序列
- **解决**（conquer）：用合并排序法对两个子序列递归地排序
- **合并**（combine）：合并两个已排序的子序列以得到排序结果

![Merge Sort][2]

----------

```c++
int merge(vector<int> &vi, int b, int m, int e) {
    vector<int> tmp(e - b + 1);
    int s1 = b, s2 = m + 1;
    int k = 0;
    //合并两有序数组
    while (s1 <= m && s2 <= e) {
        if (vi[s1] <= vi[s2])
            tmp[k++] = vi[s1++];
        else
            tmp[k++] = vi[s2++];
    }
    //剩余部分
    while (s1 <= m) tmp[k++] = vi[s1++];
    while (s2 <= e) tmp[k++] = vi[s2++];

    for (int i = 0; i < k; ++i) vi[b + i] = tmp[i];
}

int mergesort(vector<int> &vi, int p, int r) {
    if (p < r) {
        int q = (p + r) / 2;
        mergesort(vi, p, q);
        mergesort(vi, q + 1, r);
        merge(vi, p, q, r);
    }
}
```

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/Sorting_quicksort_anim.gif
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/Merge_sort_algorithm_diagram.svg.png?imageView2/2/w/400