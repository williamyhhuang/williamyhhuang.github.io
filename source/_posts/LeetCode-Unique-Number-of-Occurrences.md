title: '[LeetCode] Unique Number of Occurrences'
author: ''
date: 2023-09-26 10:00:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences)
難度：Easy

<!--more-->

Given an array of integers `arr`, return `true` if the number of occurrences of each value in the array is unique or `false` otherwise.

**Example 1:**
> **Input:** arr = [1,2,2,1,1,3]
> **Output:** true
> **Explanation:** The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

**Example 2:**
> **Input:** arr = [1,2]
> **Output:** false

**Example 3:**
> **Input:** arr = [-3,0,1,-3,1,1,1,-3,10,0]
> **Output:** true

**Constraints:**

- `1 <= arr.length <= 1000`
- `-1000 <= arr[i] <= 1000`

---

時間複雜度：因要遍歷整個矩陣一次，故為 O(N)，N 為矩陣長度
空間複雜度：因需要一個 dictionary 儲存各數字出現的次數，故為 O(N)，N 為矩陣長度

{% codeblock lang:cs %}
public class Solution {
    public bool UniqueOccurrences(int[] arr) {
        IDictionary<int, int> dict = new Dictionary<int, int>();

        for (int i = 0; i < arr.Length; i++)
        {
            // 若 dict 尚未有 arr[i]，則新增
            if (dict.ContainsKey(arr[i]) == false)
            {
                dict.Add(arr[i], 1);
            }
            // 若 arr[i] 已存在於 dict，則累加
            else
            {
                dict[arr[i]]++;
            }
        }

        return dict.Keys.Count == dict.Values.ToHashSet().Count;
    }
}
{% endcodeblock %}
