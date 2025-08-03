title: '[LeetCode] Kids With the Greatest Number of Candies'
author: ''
date: 2023-07-28 09:00:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies)
難度：Easy

<!--more-->

{% codeblock lang:cs %}
public class Solution {
    public IList<bool> KidsWithCandies(int[] candies, int extraCandies) {
        // 找出在給 extraCandies 之前，最大的糖果數
        var greatestBeforeGiving = candies.ToList().OrderBy(i => i).Last();

        var result = new bool[candies.Length];

        for (var i = 0; i < candies.Length; i++)
        {
            result[i] = (candies[i] + extraCandies) >= greatestBeforeGiving ?
                        true : false;
        }

        return result;
    }
}
{% endcodeblock %}