---
title: '第五次接触（盛最多水的容器）'
description: '第一道双指针'
pubDate: 'June 11 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

我去，最简单的一题，十分钟做出来了，我是算法领域大神

# Leetcode热题100，其之五：盛最多水的容器

 发表于：2025-06-11

 给定一个长度为`n`的整数数组`height`。有`n`条垂线，第`i`条线的两个端点是`(i, 0)`和`(i, height[i])`。

 找出其中的两条线，使得它们与`x`轴共同构成的容器可以容纳最多的水。
 
 返回容器可以储存的最大水量。

 **示例 1**：

 ![例子](/src/assets/q5-example.jpg "示例1")

 ```
 输入：[1,8,6,2,5,4,8,3,7]
 输出：49 
 解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```
 **示例 2**：
 ```
 输入：height = [1,1]
 输出：1
```
**提示：**
- 2 <= n <= $10^5$
- 0 <= height[i] <= $10^5$
- n == height.length
 ## 题解1：双指针
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int v = 0,left = 0,right = height.size()-1;
        while(left<right){
            int temp = (right - left) * min(height[right],height[left]);
            v = max(v,temp);
            if (height[left]<=height[right]){
                left++;
            }
            else{
                right--;
            }
        }
        return v;
    }
};
```
**思路：**

使用双指针，左指针指向数组头部，右指针指向数组尾部。

开始循环，计算左指针与右指针指向的两个数之间的容积。

判断左右指针指向的数的大小，将指向较小值的指针向中间移动（因为将较大值的指针向里移动必定会得到更小的容积，所以排除这种操作）

循环遍历所有可能的容积，取其中最大值作为最终结果




**复杂度分析：**
- 时间复杂度：$O(n)$。其中`n`为数组中的元素数量。最多被遍历一次
- 空间复杂度：$O(1)$。
  
