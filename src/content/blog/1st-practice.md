---
title: '第一次接触（两数之和）'
description: '从零开始打地基，LeetCode第一题'
pubDate: 2025-06-07
heroImage: '../../assets/blog-placeholder-3.jpg'
---

2025年6月4日下午三点，我结束了大学生涯的最后一门课程的期末考试，这意味着我再也没有任何借口可以用来逃避自我提升。经历了两天的放纵，我开始建设我的个人Blog，并开启每日任务：一天一道算法题。这个系列的文章旨在记录我每日LeetCode一题的做题记录，包括题解和相关笔记，一来是将学习的新知识留个档，二来也是监督我每日任务是否有在严格执行。

# Leetcode热题100，其之一：两数之和

 发表于：2025-06-07

 给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出**和为目标值** `target`的那**两个**整数，并返回它们的数组下标。
 你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
 你可以按任意顺序返回答案。

 **示例 1**：
 ```
 输入：nums = [2,7,11,15], target = 9
 输出：[0,1]
 解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```
 **示例 2**：
 ```
 输入：nums = [3,2,4], target = 6
 输出：[1,2]
```
 **示例 3**：
 ```
 输入：nums = [3,3], target = 6
 输出：[0,1]
 ```
**提示：**
- 2 <= nums.length <= $10^4$
- $-10^9$ <= nums[i] <= $10^9$
- $-10^9$ <= target <= $10^9$
- 只会存在一个有效答案
 ## 题解1：暴力枚举
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```
**思路：**

没什么好说的，暴力就完事了（）

**复杂度分析：**
- 时间复杂度：$O(n^2)$。其中`n`为数组中的元素数量，最坏情况下数组中任意两个数都要被匹配一次。
- 空间复杂度：$O(1)$。
  
 ## 题解2：哈希表
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }
};
```

**思路：**

对于每一个`x`，我们首先查询哈希表中是否存在`target - x`，然后将`x`插入到哈希表中，即可保证不会让`x`和自己匹配。

**复杂度分析：**
- 时间复杂度：$O(n)$。其中`n`为数组中的元素数量，对于每一个元素`x`，我们可以$O(1)$地寻找`target - x`。
- 空间复杂度：$O(n)$。其中`n`为数组中的元素数量，主要为哈希表的开销。

## 随题笔记
### C++ 中常用的哈希表容器
| 容器名             | 用途           | 底层结构     | 是否有序 |
|------------------|----------------|--------------|----------|
| `unordered_map`  | 存键值对       | 哈希表       | ❌ 无序   |
| `unordered_set`  | 存唯一键       | 哈希表       | ❌ 无序   |
| `map`            | 有序键值对     | 红黑树（平衡二叉树） | ✅ 有序 |

> 与 `map` / `set` 不同，`unordered_map` 和 `unordered_set` 是基于哈希实现的，效率更高但无序。

### 哈希表的定义
```c++
unordered_map<string, int> mp;  // 键为 string，值为 int
unordered_set<int> s;           // 整型集合
```
### 哈希表常用函数一览
| 函数名             | 功能描述                   |
|------------------|--------------------------|
| `.insert(pair)`  | 插入元素（适用于 `unordered_map`） |
| `.insert(value)` | 插入元素（适用于 `unordered_set`） |
| `.erase(key)`    | 删除键为 `key` 的元素         |
| `.find(key)`     | 查找 `key`，返回迭代器       |
| `.count(key)`    | 返回 `key` 的出现次数（0 或 1） |
| `.size()`        | 返回哈希表中的元素数量        |
| `.clear()`       | 清空哈希表                 |
| `.empty()`       | 判断哈希表是否为空           |

### 使用哈希表的注意事项

| 注意点                             | 说明                                                                 |
|----------------------------------|----------------------------------------------------------------------|
|  哈希冲突                        | 极端情况下性能可能退化为 O(n)，但 STL 做了优化处理                   |
|  浮点数不建议作 key              | 浮点精度误差可能导致哈希失败，建议使用整数或字符串                   |
|  `mp[key]` 会创建 key            | 如果只是查询是否存在，用 `find()` 或 `count()`，避免误插入            |
|  无序遍历                        | `unordered_map` 遍历顺序不确定，如需有序请使用 `map`                  |
|  自定义类型作 key 需提供哈希函数 | 自定义结构体作为 key 时需要实现 `std::hash` 和 `==` 运算符重载        |


