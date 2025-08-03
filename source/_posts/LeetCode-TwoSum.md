title: '[LeetCode] Two Sum'
author: ''
date: 2023-02-24 09:00:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Two Sum](https://leetcode.com/problems/two-sum/description/)
難度：Easy

<!--more-->

{% codeblock lang:cs %}
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        var result = new int[2];
        var dict = new Dictionary<int, int>();

        for (int i = 0;i < nums.Length; i++)
        {
            var remain = target - nums[i];
            if (dict.ContainsKey(remain))
            {
                result[1] = i;
                result[0] = dict[remain];

                return result;
            }

            if (dict.ContainsKey(nums[i]) == false)
            {
                dict.Add(nums[i],i);   
            }
        }

        return null;
    }
}
{% endcodeblock %}
