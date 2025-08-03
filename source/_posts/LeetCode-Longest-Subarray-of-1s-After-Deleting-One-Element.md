title: '[LeetCode] Longest Subarray of 1''s After Deleting One Element'
author: ''
date: 2023-09-24 22:30:00
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

題目：[Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
難度：Medium

<!--more-->

Given a binary array `nums`, you should delete one element from it.

Return the size of the longest non-empty subarray containing only `1`'s in the resulting array. Return `0` if there is no such subarray.

**Example 1:**
> **Input:** nums = [1,1,0,1]
> **Output:** 3
> **Explanation:** After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

**Example 2:**
> **Input:** nums = [0,1,1,1,0,1,1,0,1]
> **Output:** 5
> **Explanation:** After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

**Example 3:**
> **Input:** nums = [1,1,1]
> **Output:** 2
> **Explanation:** You must delete one element.

**Constraints:**

- `1 <= nums.length <= 10^5`
- `nums[i]` is either `0` or `1`.

---

這題跟[這題](/2023/09/23/LeetCode-Max-Consecutive-Ones-III/)很像，只要把 `k` 設為 1 即可
注意，因為無論如何都要移除一個元素，故答案為 `j-i-1`

時間複雜度：因會遍歷整個矩陣，故時間複雜度為 O(N)，N 為矩陣長度
空間複雜度：因使用的變數為常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int LongestSubarray(int[] nums) {
        int i = 0;
        int j = 0;
        int k = 1;

        while (j < nums.Length)
        {
            if (nums[j] == 0)
            {
                k--;
            }

            if (k < 0)
            {
                if (nums[i] == 0)
                {
                    k++;
                }

                i++;
            }

            j++;
        }

        return j-i-1;
    }
}
{% endcodeblock %}
