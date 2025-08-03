title: '[LeetCode] Reverse Words in a String'
author: ''
date: 2023-08-25 09:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string)
難度：Medium

<!--more-->

{% codeblock lang:cs %}
public class Solution {
    public string ReverseWords(string s) {
        // 將字串分隔
        string[] words = s.Split(' ');
        // 過濾掉空字串
        string[] filteredWords = words.Where(i => String.IsNullOrEmpty(i) == false).ToArray();
        List<string> reversedWords = new List<string>();

        // 反組字串
        for (int i = filteredWords.Count() - 1; i >= 0; i--)
        {
            reversedWords.Add(filteredWords[i]);
        }

        string ans = String.Join(" ", reversedWords);

        return ans;
    }
}
{% endcodeblock %}
