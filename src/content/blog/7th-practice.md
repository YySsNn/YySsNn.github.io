---
title: '第七次接触（接雨水）'
description: '第四道双指针'
pubDate: 'June 13 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

双指针最后一题，没用双指针😂

# Leetcode热题100，其之七：接雨水

 发表于：2025-06-13

 给定`n`个非负整数表示每个宽度为`1`的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 **示例 1**：

 ![例子](/src/assets/q7-example.jpg "示例1")

 ```
 输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
 输出：6
 解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```
 **示例 2**：
 ```
 输入：height = [4,2,0,3,2,5]
 输出：9
```
**提示：**
- 1 <= n <= $2 * 10^4$
- 0 <= height[i] <= $10^5$
- n == height.length
 ## 题解1：动态规划
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        int i = 1 , j = n-2 , sum = 0;
        vector<int> maxLeft(n),maxRight(n);
        maxLeft[0] = height[0];
        maxRight[n-1] = height[n-1];
        while(i >= 1 && i <= n - 1){
            maxLeft[i] = max(maxLeft[i - 1] , height[i]);
            i++;
        }
        while(j >= 0 && j <= n - 2){
            maxRight[j] = max(maxRight[j + 1] , height[j]);
            j--;
        }
        for(int k = 0 ; k < n ; k++){
            sum += min(maxRight[k],maxLeft[k]) - height[k];
        }
        return sum;
    }
};
```
**思路：**

对于下标`i`，下雨后水能到达的最大高度等于下标`i`两边的最大高度的最小值，下标`i`处能接的雨水量等于下标`i`处的水能到达的最大高度减去`height[i]`。

 朴素的做法是对于数组`height`中的每个元素，分别向左和向右扫描并记录左边和右边的最大高度，然后计算每个下标位置能接的雨水量。假设数组`height`的长度为`n`，该做法需要对每个下标位置使用$O(n)$的时间向两边扫描并得到最大高度，因此总时间复杂度是$O(n^2)$。

 上述做法的时间复杂度较高是因为需要对每个下标位置都向两边扫描。如果已经知道每个位置两边的最大高度，则可以在$O(n)$的时间内得到能接的雨水总量。使用动态规划的方法，可以在$O(n)$的时间内预处理得到每个位置两边的最大高度。

 创建两个长度为`n`的数组`leftMax`和`rightMax`。对于`0≤i<n`，`leftMax[i]`表示下标`i`及其左边的位置中，`height`的最大高度，`rightMax[i]`表示下标`i`及其右边的位置中，height 的最大高度。

 显然，`leftMax[0]=height[0]`，`rightMax[n−1]=height[n−1]`。两个数组的其余元素的计算如下：

 - 当`1≤i≤n−1`时，`leftMax[i]=max(leftMax[i−1],height[i])`；

 - 当`0≤i≤n−2`时，`rightMax[i]=max(rightMax[i+1],height[i])`。

 因此可以正向遍历数组`height`得到数组`leftMax`的每个元素值，反向遍历数组`height`得到数组`rightMax`的每个元素值。

 在得到数组`leftMax`和`rightMax`的每个元素值之后，对于`0≤i<n`，下标`i`处能接的雨水量等于`min(leftMax[i],rightMax[i])−height[i]`。遍历每个下标位置即可得到能接的雨水总量。

 动态规划做法可以由下图体现。

![解答](/src/assets/q7-answer.png "题解1")


**复杂度分析：**
- 时间复杂度：$O(n)$。其中`n`为数组中的元素数量。最多被遍历一次
- 空间复杂度：$O(n)$。
  
