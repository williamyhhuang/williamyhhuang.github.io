title: '[LeetCode] Increasing Triplet Subsequence'
author: ''
date: 2023-08-28 22:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence)
難度：Medium

<!--more-->

題目是要在矩陣中找到三個遞增的數字 N1、N2、N3，如果存在則回傳 true，反之回傳 false。
解題想法是：

<ol>
<li>先定義 N1、N2 為 int.MaxValue</li>
<li>然後開始迭代，如果當前數字 num 小於 N1，則 N1 = num</li>
<li>如果當前數字 num 大於 N1 且 小於 N2，則 N2 = num</li>
<li>如果當前數字 num 大於 N1 且 大於 N2，則 N3 = num，即滿足題目需求</li>
</ol>

在討論串看到，如果 input 是 `[2, 5, 0, 6]`，得到的答案會是 `[0, 5, 6]`，但應該是 `[2, 5, 6]`
這不影響最後的答案，因為問題是要找**是否存在三個遞增的數字**，以上面的 input 為例，只要有數字小於 5 即可，不管它是 2 還是 0

{% codeblock lang:cs %}
public class Solution {
    public bool IncreasingTriplet(int[] nums) {
        // 如果長度小於 3，則直接 return false
        if (nums.Length < 3)
        {
            return false;
        }

        int smallest = int.MaxValue;
        int secondSmallest = int.MaxValue;

        foreach(int num in nums)
        {
            // 如果當前數字小於 smallest
            if (num <= smallest)
            {
                smallest = num;
            }
            // 如果當前數字大於 smallest 且小於 secondSmallest           
            else if (num <= secondSmallest)
            {
                secondSmallest = num;
            }
            // 如果當前數字大於 smallest 且大於 secondSmallest
            // 滿足題目需求，找到三個遞增的數字            
            else
            {
                return true;
            }
        }

        return false;
    }
}
{% endcodeblock %}
