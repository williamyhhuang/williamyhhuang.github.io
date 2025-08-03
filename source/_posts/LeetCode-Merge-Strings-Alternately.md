title: '[LeetCode] Merge Strings Alternately'
author: ''
date: 2023-07-27 09:00:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately)
難度：Easy

<!--more-->

題目為合併兩個字串，因要遍歷整個字串，故時間複雜度為 O(N)，N 為較長之字串長度；空間複雜度為 O(N)，N 為兩個字串加總長度

{% codeblock lang:cs %}
public class Solution {
    public string MergeAlternately(string word1, string word2) {
        var len = word1.Length > word2.Length ?
                  word1.Length : word2.Length;
        
        var result = string.Empty;

        for (var i = 0; i < len; i++)
        {
            // word1
            if (word1.Length > i)
            {
                result += word1[i];
            }

            // word2
            if (word2.Length > i)
            {
                result += word2[i];
            }
        }

        return result;
    }
}
{% endcodeblock %}