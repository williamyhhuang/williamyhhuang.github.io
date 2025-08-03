title: '[LeetCode] Find the Highest Altitude'
author: ''
date: 2023-09-25 08:30:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude)
難度：Easy

<!--more-->

There is a biker going on a road trip. The road trip consists of `n + 1` points at different altitudes. The biker starts his trip on point `0` with altitude equal `0`.

You are given an integer array `gain` of length `n` where `gain[i]` is the net gain in altitude between points `i`​​​​​​ and `i + 1` for all `(0 <= i < n)`. Return the highest altitude of a point.

**Example 1:**
> **Input:** gain = [-5,1,5,0,-7]
> **Output:** 1
> **Explanation:** The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.

**Example 2:**
> **Input:** gain = [-4,-3,-2,-1,4,3,2]
> **Output:** 0
> **Explanation:** The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0.

**Constraints:**

- `n == gain.length`
- `1 <= n <= 100`
- `-100 <= gain[i] <= 100`

---

時間複雜度：因會遍歷整個矩陣，故時間複雜度為 O(N)，N 為矩陣長度
空間複雜度：因使用的變數為常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int LargestAltitude(int[] gain) {
       int max = Int32.MinValue;
       int currentSum = 0;

        for (int i = 0; i < gain.Length; i++)
        {
            currentSum += gain[i];
            max = Math.Max(max, currentSum);
        }

       return max > 0 ? max : 0; 
    }
}
{% endcodeblock %}
