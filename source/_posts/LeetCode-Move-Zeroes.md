title: '[LeetCode] Move Zeroes'
author: ''
date: 2023-08-28 22:30:00
tags:
  - C#
  - LeetCode
  - Easy
  - Data Structure
  - Algorithm
  - Dynamic Programming
categories:
  - LeetCode
---

題目：[Move Zeroes](https://leetcode.com/problems/move-zeros)
難度：Easy

<!--more-->

注意，題目是說**照原本順序排列**，並不需要由小到大排列

程式邏輯：
<ol>
<li>設定兩個 pointer，p1 指向 nums[0]，p2 指向 nums[1]</li>
<li>如果 p1 不等於 0，則移動 p1</li>
<li>如果 p1 等於 0，則移動 p2，直到 p2 找到不等於 0 的數字</li>
<li>p2 找到不等於 0 的數字，p1、p2 交換</li>
<li>持續步驟3、4，直到 p2 遍歷整個矩陣</li>
</ol>

{% codeblock lang:cs %}
public class Solution {
    public void MoveZeroes(int[] nums) {
        int p1 = 0;
        for (int p2 = 1; p2 < nums.Length; p2++)
        {
            if(nums[p1] == 0)
            {
                if(nums[p2] == 0)
                {
                    continue;
                }
                else
                {
                    nums[p1] = nums[p2];
                    nums[p2] = 0;
                }
            }

            p1++;
        }
    }
}
{% endcodeblock %}
