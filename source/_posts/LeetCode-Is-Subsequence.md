title: '[LeetCode] Is Subsequence'
author: ''
date: 2023-09-12 09:00:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Is Subsequence](https://leetcode.com/problems/is-subsequence)
難度：Easy

<!--more-->

解題技巧在於 two pointer

{% codeblock lang:cs %}
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int pointer = 0;

        for (int i = 0; i < t.Length; i++)
        {
            // 如果 t[i] 存在於 s，則移動 s 的 pointer
            if (pointer < s.Length && s[pointer] == t[i])
            {
                pointer++;
            }

            // 如果已經找到所有存在於 t 的 s
            // 則回傳 true，不用遍歷所有 t
            if (s.Length == pointer)
            {
                return true;
            }
        }

        return s.Length == pointer;
    }
}
{% endcodeblock %}
