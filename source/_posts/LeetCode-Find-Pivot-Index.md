title: '[LeetCode] Find Pivot Index'
author: ''
date: 2023-09-25 09:00:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)
難度：Easy

<!--more-->

Given an array of integers `nums`, calculate the pivot index of this array.

The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

If the index is on the left edge of the array, then the left sum is `0` because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return `-1`.

**Example 1:**
> **Input:** nums = [1,7,3,6,5,6]
> **Output:** 3
> **Explanation:**
> The pivot index is 3.
> Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
> Right sum = nums[4] + nums[5] = 5 + 6 = 11

**Example 2:**
> **Input:** nums = [1,2,3]
> **Output:** -1
> **Explanation:**
> There is no index that satisfies the conditions in the problem statement.

**Example 3:**
> **Input:** nums = [2,1,-1]
> **Output:** 0
> **Explanation:**
> The pivot index is 0.
> Left sum = 0 (no elements to the left of index 0)
> Right sum = nums[1] + nums[2] = 1 + -1 = 0

**Constraints:**

- 1 <= nums.length <= 10^4
- -1000 <= nums[i] <= 1000

---

時間複雜度：因會遍歷整個矩陣，故時間複雜度為 O(N)，N 為矩陣長度
空間複雜度：因使用的變數為常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int PivotIndex(int[] nums) {
        int sum = 0;
        int leftSum = 0;

        // 找出所有數字的和
        for(int i = 0; i < nums.Length; i++)
        {
            sum += nums[i];
        }
        
        // 遍歷整個矩陣
        for (int j = 0; j < nums.Length; j++)
        {
            // 如果 nums[j] 左邊的和等於右邊的和 (右邊的和為 `sum - nums[j] - leftSum`)
            // 則 j 為 pivot index
            if (leftSum == (sum - nums[j] - leftSum))
            {
                return j;
            }
            
            // 如果還沒找到 pivot index，則累加 leftSum
            leftSum += nums[j];
        }
        
        return -1;
    }
}
{% endcodeblock %}
