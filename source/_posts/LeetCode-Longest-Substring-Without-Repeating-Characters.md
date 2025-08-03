title: '[LeetCode] Longest Substring Without Repeating Characters'
author: ''
date: 2023-02-25 19:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
  - Sliding Window
categories:
  - LeetCode
---

題目：[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
難度：Medium

<!--more-->

技巧在於使用 Sliding Window 去解，左右指標會一直移動，直到遍歷整個字串，即左右指標重疊。

{% codeblock lang:cs %}
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        if (s.Length == 0)
        {
            return 0;
        }

        // dict 的 key 為字母， value 為右邊的指標位置
        IDictionary<char, int> dict = new Dictionary<char, int>();
        int left = 0;
        int right = 0;
        int maxLength = 0;

        while (left < s.Length && right < s.Length)
        {
            char c = s[right];

            // 如果該字母存在於字典中，則移動左邊的指標
            // 因為要開一個新的 Window，所以左邊指標要移至【已存在的 c 的位置 +1】
            if (dict.ContainsKey(c))
            {
                left = Math.Max(dict[c] + 1, left);
            }
            
            // 如果該字母存在於字典中，則會更新成右邊指標的位置
            // 如果該字母不存在於字典中，則會新增
            dict[c] = right;

            // 比較【左右指標的位置差】與【前一次的最大長度】，何者比較大
            maxLength = Math.Max(maxLength, right - left + 1);

            right += 1;
        }

        return maxLength;
    }
}
{% endcodeblock %}

## 參考

[演算法筆記系列 — Two Pointer 與 Sliding Window](https://medium.com/%E6%8A%80%E8%A1%93%E7%AD%86%E8%A8%98/%E6%BC%94%E7%AE%97%E6%B3%95%E7%AD%86%E8%A8%98%E7%B3%BB%E5%88%97-two-pointer-%E8%88%87sliding-window-8742f45f3f55)
[YouTube: Longest Substring Without Repeating Characters](https://www.youtube.com/watch?v=Uf6YUcbctIk&t=366s&ab_channel=OneCodeMan)