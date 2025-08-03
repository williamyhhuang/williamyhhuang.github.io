title: '[LeetCode] Reverse Vowels of a String'
author: ''
date: 2023-08-25 09:00:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string)
難度：Easy

<!--more-->

題目要求要交換母音位置，使用 two pointer 方法，一個在頭一個在尾
要注意的是題目有說字串 `consist of printable ASCII characters`，所以母音字串表要包含大寫及小寫的字母
因會遍歷整個字串一次．故時間複雜度為 O(N)，N 為字串長度；因有用一個矩陣儲存結果，故空間複雜度為 O(N)，N 為字串長度

{% codeblock lang:cs %}
public class Solution {
    public string ReverseVowels(string s) {
        char[] word = s.ToCharArray(0,s.Length);
        int start = 0;
        int end = s.Length - 1;
        String vowels = "aeiouAEIOU";

        while (start < end)
        {
            // 從頭開始找母音
            while (start < end && vowels.Contains(word[start]) == false)
            {
                start++;
            }

            // 從尾開始找母音
            while (start < end && vowels.Contains(word[end]) == false)
            {
                end--;
            }

            // 交換母音
            char tmp = word[end];
            word[end] = word[start];
            word[start] = tmp;

            // 繼續尋找下一組母音
            start++;
            end--;
        }

        var ans = new String(word);
        return ans;
    }
}
{% endcodeblock %}
