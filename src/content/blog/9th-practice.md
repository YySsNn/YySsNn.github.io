---
title: '第九次接触（找到字符串中所有字母异位词）'
description: '想不出来了'
pubDate: 'June 15 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

出门了一天，累成傻逼了😇

# Leetcode热题100，其之八：无重复字符的最长子串

 发表于：2025-06-15

 给定两个字符串`s`和`p`，找到`s`中所有`p`的异位词的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

 **示例 1**：

 ```
 输入: s = "cbaebabacd", p = "abc"
 输出: [0,6]
 解释:
 起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
 起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
 ```
 **示例 2**：
 ```
 输入: s = "abab", p = "ab"
 输出: [0,1,2]
 解释:
 起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
 起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
 起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
 
 ```
**提示：**
- 1 <= s.length , p.length <= $3*10^4$
- s和p只包含小写字母
 ## 题解1：滑动窗口
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int sLen = s.size(), pLen = p.size();

        if (sLen < pLen) {
            return vector<int>();
        }

        vector<int> ans;
        vector<int> sCount(26);
        vector<int> pCount(26);
        for (int i = 0; i < pLen; ++i) {
            ++sCount[s[i] - 'a'];
            ++pCount[p[i] - 'a'];
        }

        if (sCount == pCount) {
            ans.emplace_back(0);
        }

        for (int i = 0; i < sLen - pLen; ++i) {
            --sCount[s[i] - 'a'];
            ++sCount[s[i + pLen] - 'a'];

            if (sCount == pCount) {
                ans.emplace_back(i + 1);
            }
        }

        return ans;
    }
};

```
**思路：**

在字符串 s 中构造一个长度为与字符串 p 的长度相同的滑动窗口，并在滑动中维护窗口中每种字母的数量；当窗口中每种字母的数量与字符串 p 中每种字母的数量相同时，则说明当前窗口为字符串 p 的异位词。

使用数组来存储字符串 p 和滑动窗口中每种字母的数量，当字符串 s 的长度小于字符串 p 的长度时，字符串 s 中一定不存在字符串 p 的异位词。但是因为字符串 s 中无法构造长度与字符串 p 的长度相同的窗口，所以这种情况需要单独处理。


**复杂度分析：**
- 时间复杂度：$O(m+(n−m)×\Sigma)$，其中 n 为字符串 s 的长度，m 为字符串 p 的长度，Σ 为所有可能的字符数。我们需要 O(m) 来统计字符串 p 中每种字母的数量；需要 O(m) 来初始化滑动窗口；需要判断 n−m+1 个滑动窗口中每种字母的数量是否与字符串 p 中每种字母的数量相同，每次判断需要 O(Σ) 。因为 s 和 p 仅包含小写字母，所以 Σ=26。
- 空间复杂度：$O(\Sigma)$。用于存储字符串 p 和滑动窗口中每种字母的数量。
  
