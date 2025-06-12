---
title: '第六次接触（三数之和）'
description: '第三道双指针'
pubDate: 'June 12 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

本来想着用双指针加哈希来做的，没想到题解直接排序了）

# Leetcode热题100，其之六：三数之和

 发表于：2025-06-12

 给你一个整数数组 nums ，判断是否存在三元组`[nums[i], nums[j], nums[k]]`满足`i != j`、`i != k`且`j != k`，同时还满足`nums[i] + nums[j] + nums[k] == 0`。请你返回所有和为`0`且不重复的三元组。
 
 **注意：**答案中不可以包含重复的三元组。

 **示例 1**：

 ```
 输入：nums = [-1,0,1,2,-1,-4]
 输出：[[-1,-1,2],[-1,0,1]]
 解释：
 nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
 nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
 nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。 
 不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
 注意，输出的顺序和三元组的顺序并不重要。
 ```
 **示例 2**：
 ```
 输入：nums = [0,1,1]
 输出：[]
 解释：唯一可能的三元组和不为 0 。
 ```
 **示例 3**：
 ```
 输入：nums = [0,0,0]
 输出：[[0,0,0]]
 解释：唯一可能的三元组和为 0 。
 ```
**提示：**
- 3 <= nums.length <= $3000$
- -10^5 <= nums[i] <= $10^5$
 ## 题解1：双指针+排序
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int n = nums.size();
        vector<vector<int>> output;
        int temp = 0;
        for(int i = 0 ; i < n - 2 ; i++){
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int j = i + 1;
            int k = n - 1;
            while(j < k){
                int sum = nums[i] + nums[j] + nums[k];
                if(sum == 0){
                    output.push_back({nums[i] , nums[j] , nums[k]});
                    temp++;
                    j++;
                    k--;
                    while (j < k && nums[j] == nums[j - 1]) j++;
                    while (j < k && nums[k] == nums[k + 1]) k--;
                }
                if(sum < 0){
                    j++;
                }
                if(sum>0){
                    k--;
                }
            } 
        }
        return output;
    }
};
```
**思路：**

既然题目要求不能有重复的三元组，那么将原数组`nums`进行排序就可以很大程度上解决这个问题；其次对于重复的元素，直接在遍历过程中跳过即可。

对于三元组第一个数`i`，让它从数组头部遍历到数组倒数第三个数即可（因为要给`j`和`k`留下空间）。

既然题目要求三数之和为`0`,而第二个数`j`在右移后必定增大，第三个数`k`需要减小才能使三数之和再次靠近`0`.这种情况就可以使用双指针向数组中间逼近来完成遍历。

使用双指针，左指针`j`指向`i`后第一个数，右指针`k`指向数组尾部。

判断三数之和，若大于`0`，则需`k`减小，右指针左移；反之若和小于`0`，则需`j`增大，即左指针右移。

当计算得到三数之和为`0`时，将其存入结果数组`output`中。

**复杂度分析：**
- 时间复杂度：$O(n^2)$。其中`n`为数组中的元素数量。
- 空间复杂度：$O(logn)$。这部分空间是排序所需空间。
  
