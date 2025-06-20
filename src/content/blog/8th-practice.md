---
title: '第八次接触（无重复字符的最长子串）'
description: '想不出来了'
pubDate: 'June 14 2025'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

直接写了，写完打彩六去咯🙌

# Leetcode热题100，其之八：无重复字符的最长子串

 发表于：2025-06-14

 给定一个字符串`s`，请你找出其中不含有重复字符的`最长子串`的长度。
 
 **注意：**答案中不可以包含重复的三元组。

 **示例 1**：

 ```
 输入: s = "abcabcbb"
 输出: 3 
 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
 ```
 **示例 2**：
 ```
 输入: s = "bbbbb"
 输出: 1
 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
 ```
 **示例 3**：
 ```
 输入: s = "pwwkew"
 输出: 3
 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
 ```
**提示：**
- 0 <= s.length <= $5*10^4$
- s由英文字母、数字、符号和空格组成
 ## 题解1：滑动窗口
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> occ;
        int n = s.size();
        int right = -1,length = 0;
        for(int left = 0 ; left < n ; ++left){
            if(left != 0){
                occ.erase(s[left - 1]);
            }
            while (right + 1 < n && !occ.count(s[right + 1])){
                occ.insert(s[right + 1]);
                ++right;
            }
            length = max(length,right - left + 1);
        }
        return length;
    }
};
```
**思路：**

以示例一中的字符串 abcabcbb 为例，找出从每一个字符开始的，不包含重复字符的最长子串，那么其中最长的那个字符串即为答案。对于示例一中的字符串，我们列举出这些结果，其中括号中表示选中的字符以及最长的字符串：

- 以 (a)bcabcbb 开始的最长字符串为 (abc)abcbb；
- 以 a(b)cabcbb 开始的最长字符串为 a(bca)bcbb；
- 以 ab(c)abcbb 开始的最长字符串为 ab(cab)cbb；
- 以 abc(a)bcbb 开始的最长字符串为 abc(abc)bb；
- 以 abca(b)cbb 开始的最长字符串为 abca(bc)bb；
- 以 abcab(c)bb 开始的最长字符串为 abcab(cb)b；
- 以 abcabc(b)b 开始的最长字符串为 abcabc(b)b；
- 以 abcabcb(b) 开始的最长字符串为 abcabcb(b)。
 
可以看出如果我们依次递增地枚举子串的起始位置，那么子串的结束位置也是递增的。这里的原因在于，假设我们选择字符串中的第k个字符作为起始位置，并且得到了不包含重复字符的最长子串的结束位置为$r_k$。那么当我们选择第k+1个字符作为起始位置时，首先从k+1到$r_k$的字符显然是不重复的，并且由于少了原本的第k个字符，我们可以尝试继续增大$r_k$​，直到右侧出现了重复字符为止。

这样一来，我们就可以使用「滑动窗口」来解决这个问题了：

- 我们使用两个指针表示字符串中的某个子串（或窗口）的左右边界，其中左指针代表着上文中「枚举子串的起始位置」，而右指针即为上文中的$r_k$；

- 在每一步的操作中，我们会将左指针向右移动一格，表示 我们开始枚举下一个字符作为起始位置，然后我们可以不断地向右移动右指针，但需要保证这两个指针对应的子串中没有重复的字符。在移动结束后，这个子串就对应着 以左指针开始的，不包含重复字符的最长子串。我们记录下这个子串的长度；

- 在枚举结束后，我们找到的最长的子串的长度即为答案。

判断重复字符:

在上面的流程中，还需要使用一种数据结构来判断是否有重复的字符，常用的数据结构为哈希集合（C++ 中的std::unordered_set）。在左指针向右移动的时候，我们从哈希集合中移除一个字符，在右指针向右移动的时候，我们往哈希集合中添加一个字符。

**复杂度分析：**
- 时间复杂度：$O(n)$。其中`n`为数组中的元素数量,左指针和右指针分别会遍历整个字符串一次。
- 空间复杂度：$O(|\Sigma|)$。其中$\Sigma$表示字符集（即字符串中可以出现的字符），∣$\Sigma$∣表示字符集的大小。
  
