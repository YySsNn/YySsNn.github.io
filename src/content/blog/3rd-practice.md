---
title: '第三次接触（最长连续数列）'
description: '再见了，所有的哈希表'
pubDate: 2025-06-09
heroImage: '../../assets/blog-placeholder-3.jpg'
---

今天做了热题100的第三题，也是最后一道哈希表的题目，虽然我发现用其他方法时间复杂度也不高，但它这么分类了我也就让让它。

# Leetcode热题100，其之三：最长连续数列

 发表于：2025-06-09

 给定一个未排序的整数数组`nums`，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
 请你设计并实现时间复杂度为O(n)的算法解决此问题。

 **示例 1**：
 ```
 输入：nums = [100,4,200,1,3,2]
 输出：4
 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
 **示例 2**：
 ```
 输入：nums = [0,3,7,2,5,8,4,6,0,1]
 输出：9
```
 **示例 3**：
 ```
 输入：nums = [1,0,1,2]
 输出：3
 ```
**提示：**
- 0 <= nums.length <= $10^5$
- $-10^9$ <= nums[i] <= $10^9$
 ## 题解1：排序算法
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int temp = 1 , mark = 1;
        for(int i = 0; i < n - 1 ; i++){
            if (nums[i] == nums[i + 1]) {
                continue; // 跳过重复元素
            }
            if (nums[i] + 1 == nums[i + 1]){
                temp++;
            }
            else{
                mark = max(temp,mark);
                temp = 1;
            }
        }
        return max(temp,mark);
    }
};
```
**思路：**

就是先将数组进行排序，再执行后续的操作，这样做感觉比使用哈希表更好理解而且更简单（可能是我的初学者思维），但题目要求时间复杂度要O(n)。所以这种方法并不能在该题中使用

**复杂度分析：**
- 时间复杂度：$O(nlogn)$。其中`n`为数组中的元素数量。函数的循环部分时间复杂度为O(n)，但瓶颈在于排序，排序的时间复杂度为O(nlogn)。
- 空间复杂度：$O(1)$。
  
 ## 题解2：哈希表
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        unordered_set<int> numSet(nums.begin(), nums.end());
        int maxLen = 0 , currentLen = 0 , currentNum = 0;
        for(int i : numSet){
            if(!numSet.count(i - 1)){
                currentNum = i;
                currentLen = 1;
                while (numSet.count(currentNum + 1)){
                    currentNum++;
                    currentLen++;
                }
                maxLen = max(maxLen , currentLen);
            }
            
        }
        return maxLen;
    }
};
```

**思路：**

先对输入的数组`nums`进行去重存入哈希表，然后从前往后遍历：如果这个数的前一个数不在哈希表中，则开始计算从它开始的连续数列；如果在，则直接跳到下一个数。

**复杂度分析：**
- 时间复杂度：$O(n)$。其中`n`为数组中的元素数量。
- 空间复杂度：$O(n)$。其中`n`为数组中的元素数量，主要为哈希表的开销。

