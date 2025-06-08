---
title: '第二次接触（字母异位词分组）'
description: '哈希表的二次印象加深，LeetCode第二题'
pubDate: 2025-06-08
heroImage: '../../assets/blog-placeholder-3.jpg'
---

我去，沉迷绝区零没注意时间，AC之后已经过24点了，连胜在坚持一天后惨遭资本做局中断，米哈游你赢了。不过还没到25时，我依旧按6月8日算了，反正9号晚上还有一题😋

# Leetcode热题100，其之二：字母异位词分组

 发表于：2025-06-08

 给你一个字符串数组，请你将**字母异位词**组合在一起。可以按任意顺序返回结果列表。
 **字母异位词**是由重新排列源单词的所有字母得到的一个新单词。

 **示例 1**：
 ```
 输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
 输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```
 **示例 2**：
 ```
 输入: strs = [""]
 输出: [[""]]
```
 **示例 3**：
 ```
 输入: strs = ["a"]
 输出: [["a"]]
 ```
**提示：**
- 1 <= strs.length <= $10^4$
- 0 <= strs[i].length <= 100
- strs[i]仅包含小写字母
 ## 题解1：暴力枚举
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        vector<vector<string>> output;
        for(int i = 0 ; i <= strs.size() - 1 ; ++i){
            string containing = strs[i];
            sort(containing.begin(),containing.end());
            mp[containing].push_back(strs[i]);
        }
        for (auto& pair : mp) {
            output.push_back(pair.second);  // 将每组异位词加入结果
        }
        return output;
    }
};
```
**思路：**

按照昨天做的哈希表类似的套路来做，知识点是重复的，就是哈希表的`value`类型从`int`变为`vector<string>`。

由于互为字母异位词的两个字符串包含的字母相同，因此对两个字符串分别进行排序之后得到的字符串一定是相同的，故可以将排序之后的字符串作为哈希表的键。

**复杂度分析：**
- 时间复杂度：$O(nklogk)$，其中`n`是`strs`中的字符串的数量，`k`是`strs`中的字符串的的最大长度。需要遍历`n`个字符串，对于每个字符串，需要 $O(klogk)$ 的时间进行排序以及 $O(1)$ 的时间更新哈希表，因此总时间复杂度是 $O(nklogk)$。
- 空间复杂度：$O(nk)$，其中`n`是`strs`中的字符串的数量，`k`是`strs`中的字符串的的最大长度。需要用哈希表存储全部字符串。
  
