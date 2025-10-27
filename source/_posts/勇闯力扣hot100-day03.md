---
title: 勇闯力扣hot100-day03
date: 2025-10-27 15:05:31
tags: 算法
---

# 勇闯力扣hot100-day03

> 2025.10.17

## 1.无重复字符的最长字串

### 题目

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

### 解题过程

使用map来存字符最后一次出现的位置，使用`i`来指向最长子串的最左边。

每次循环开始的时候判断在map中是否存在该字符，如果存在，则判断`i`和该字符当前所在位置的大小，取大值；如果不存在则说明目前的子串还是不重复的，对子串长度进行修改。并且每次都判断是不是最大的。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character,Integer> map = new HashMap<>();
        int max=0,i=-1;
        for(int j = 0;j<s.length();j++){
            if(map.containsKey(s.charAt(j))){
                i = Math.max(i,map.get(s.charAt(j)));
            }
            map.put(s.charAt(j),j);
            max = Math.max(max,j-i);
        }
        return max;
    }
}
```



## 2.找到字符串中所有的字母异位词

### 题目

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

### 解题过程

1. 将传入的字符串`p`的每个元素存入到数组`arrP`中，用来记录每个元素出现的次数。
2. 循环传入的字符串`s`的元素，存入到数组`arrS`中，用`left`记录数组`arrS`的最左侧，`right`存储当前添加的元素下标。如果发现`arrS`涵盖了`arrP`的元素。那么将`arrS`的第一个元素也就是`left`对应的下标的元素存入到最终结果`arr`数组中。
3. 将`arrS`的第一个元素移出。开始下一次循环。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int[] cntS = new int[26];
        int[] cntP = new int[26];
        int len = p.length();
        List<Integer> arr = new ArrayList<>();
        // 记录子串的所有字母个数
        for (int i = 0; i < p.length(); i++) {
            cntP[p.charAt(i) - 'a']++;
        }

        for (int i = 0; i < s.length(); i++) {
            // 将当前元素加入到cntS中，
            cntS[s.charAt(i) - 'a']++;
            // 左指针的位置
            int left = i - len + 1;
            // 如果左指针小于0说明当前要判断的子字符串元素个数的不够的，可以直接跳过
            if (left < 0) {
                continue;
            }
            // 比较cntP和cntS两个数组中存放的数是否一致，一致就说明左指针和右指针选中的子串是p的字母异位词
            if (Arrays.equals(cntP, cntS)) {
                arr.add(left);
            }
            // 将左指针对应的字母移出数组。
            cntS[s.charAt(left) - 'a']--;
        }
        return arr;
    }
}
```

