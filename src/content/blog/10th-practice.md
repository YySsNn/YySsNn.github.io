---
title: '第十次接触（和为k的子数组）'
description: '想不出来了'
pubDate: 'June 16 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

出门了一天，累成傻逼了😇

# Leetcode热题100，其之十：和为k的子数组

 发表于：2025-06-15

 给你一个整数数组`nums`和一个整数`k`，请你统计并返回该数组中和为`k`的子数组的个数 。
 
 子数组是数组中元素的连续非空序列。

 **示例 1**：

 ```
 输入：nums = [1,1,1], k = 2
 输出：2
 ```
 **示例 2**：
 ```
 输入：nums = [1,2,3], k = 3
 输出：2
 
 ```
**提示：**
- 1 <= nums.length <= $2*10^4$
- -1000 <= nums[i] <= 1000
- $-10^7$ <= k <= $10^7$
 ## 题解1：暴力遍历
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> prefix(n);
        prefix[0] = nums[0];
        for(int i = 1 ; i < n ; i++){
            prefix[i] = prefix[i-1] + nums[i];
        }
        int sum = 0;
        for(int i = 0 ; i < n ; i++){
            if (prefix[i] == k) sum++;
            for(int j = 1 ; j <= i ; j++){
                if(j>0&&prefix[j-1]==prefix[i]-k){
                    sum++;
                }
            }
        }
        return sum;
    }
};

```
**思路：**

首先定义一个数组prefix，用于储存下标为i时，nums[0]到nums[i]的和。

对于所需要寻找的字串[j,……,i]，遍历所有的i和j，寻找满足$pre[j−1]==pre[i]−k$的所有i和j。


**复杂度分析：**
- 时间复杂度：$O(n^2)$。
- 空间复杂度：$O(1)$。
  
