title: '[LeetCode] Maximum Average Subarray I'
author: ''
date: 2023-09-20 23:30:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm
- Sliding Window

categories:

- LeetCode

---

題目：[Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
難度：Easy

<!--more-->

You are given an integer array nums consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose length is equal to `k` that has the maximum average value and return this value. Any answer with a calculation error less than `10^-5` will be accepted.

Example 1:
> **Input:** nums = [1,12,-5,-6,50,3], k = 4
> **Output:** 12.75000
> **Explanation:** Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75

Example 2:
> **Input:** nums = [5], k = 1
> **Output:** 5.00000

Constraints:
<ul>
<li>n == nums.length</li>
<li>1 <= k <= n <= 10^5</li>
<li>-10^4 <= nums[i] <= 10^4</li>
</ul>

---

解題技巧在於 Sliding Window，以 Example 1 當作範例：
> nums = [1,12,-5,-6,50,3], k = 4

- 算出前 k 項，即 i = 0 到 i = 3 的和，sum = 1 + 12 + (-5) + (-6) = 2
- 先將 maxSum 設為 sum (2)
- 開始找矩陣中最大的 maxSum，已經知道 i = 0 ~ i = 3 的和，現在要計算 i = 1 ~ i = 4、i = 2 ~ i = 5 的和
  - i = 1 ~ i = 4
    - sum = sum - nums[0] + nums[4] = 2 - 1 + 50 = 51
      - 51 大於當前的 maxSum (2)，設定 maxSum = 51
  - i = 2 ~ i = 5
    - sum = sum - nums[1] + nums[5] = 51 - 12 + 3 = 42
    - 42 小於當前的 maxSum (51)，maxSum 維持不變
- maxSum / k = 12.75，即為結果

---

時間複雜度：O(N) `前 k 項的和` + O(N) `遍歷整個矩陣` = O(N)，N 為矩陣長度
空間複雜度：變數皆為常數，故 O(1)

{% codeblock lang:cs %}
public class Solution {
    public double FindMaxAverage(int[] nums, int k) {
        double sum = 0;

        // 先算出前 k 項的和
        for (int i = 0; i < k; i++)
        {
            sum += nums[i];
        }

        double maxSum = sum;

        // 開始找出最大的和
        for (int j = 0; (j+k) < nums.Count(); j++)
        {
            // 減前一項
            sum -= nums[j];

            // 加後一項
            sum += nums[j+k];

            maxSum = Math.Max(sum, maxSum);
        }

        return maxSum / k;
    }
}
{% endcodeblock %}
