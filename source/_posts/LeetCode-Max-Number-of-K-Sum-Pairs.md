title: '[LeetCode] Max Number of K-Sum Pairs'
author: ''
date: 2023-09-17 13:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs)
難度：Medium

<!--more-->

You are given an integer array `nums` and an integer `k`.
In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.
Return the maximum number of operations you can perform on the array.

Example 1:
> **Input:** nums = [1,2,3,4], k = 5
> **Output:** 2
> **Explanation:** Starting with nums = [1,2,3,4]:
>
> - Remove numbers 1 and 4, then nums = [2,3]
> - Remove numbers 2 and 3, then nums = []
>
> There are no more pairs that sum up to 5, hence a total of 2 operations.

Example 2:
> **Input:** nums = [3,1,3,4,3], k = 6
> **Output:** 1
> **Explanation:** Starting with nums = [3,1,3,4,3]:
>
> - Remove the first two 3's, then nums = [1,4,3]
>
> There are no more pairs that sum up to 6, hence a total of 1 operation.

Constraints:
<ul>
    <li>1 <= nums.length <= 10^5 </li>
    <li>1 <= nums[i] <= 10^9 </li>
    <li>1 <= k <= 10^9 </li>
</ul>

---

以下為參考 Discussion 中，[作者 hi-malik 的解答](https://leetcode.com/problems/max-number-of-k-sum-pairs/solutions/2005922/going-from-o-n-2-o-nlogn-o-n-meme/?envType=study-plan-v2&envId=leetcode-75)

## 解法1. 列舉所有排列組合

時間複雜度：因要用兩個迴圈列舉所有排列組合，故時間複雜度為 O(N^2)
空間複雜度：因只有使用常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int MaxOperations(int[] nums, int k) {
        int operationCount = 0;

        for (int i = 0; i < nums.Count(); i++)
        {
            if (nums[i] == -1)
            {
                continue;
            }

            for (int j = i + 1; j < nums.Count(); j++)
            {
                if (nums[j] == -1)
                {
                    continue;
                }

                if (nums[i] + nums[j] == k)
                {
                    operationCount += 1;

                    // 因 Constraiint 有說，矩陣中的數字至少大於等於1，故將 match 的數字置換為 -1
                    nums[i] = -1;
                    nums[j] = -1;

                    break;                   
                }
            }
        }

        return operationCount;
    }
}
{% endcodeblock %}

## 解法2. Two Pointer

先將矩陣做排序，再於頭跟尾設置 pointer
如果倆 pointer 對應數字的合等於 k，則移動倆 pointer

時間複雜度：`矩陣排序，由小排到大` O(NlogN) + `遍歷整個矩陣` O(N) = O(NlogN)，N 為矩陣長度
空間複雜度：因只有使用常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int MaxOperations(int[] nums, int k) {
        // 矩陣排序，由小排到大
        Array.Sort(nums);
        int operationCount = 0;
        int i = 0;
        int j = nums.Count() - 1;

        while(i < j)
        {
            int sum = nums[i] + nums[j];
            
            if(sum == k) 
            {
                operationCount++;
                i++;
                j--;
            }
            // 如果數字的合大於 k，代表 j 對應的數字太大了，則把 j 向左移動
            else if(sum > k) 
            {
                j--;
            }
            // 如果數字的合小於 k，代表 i 對應的數字太小了，則把 i 向右移動
            else 
            {
                i++;
            }
        }

        return operationCount;
    }
}
{% endcodeblock %}

## 解法3. Dictionary (Map)

時間複雜度：因會遍歷整個矩陣，故為 BigO(N)，N 為矩陣長度
空間複雜度：需要儲存所有數字出現的次數，故 BigO(N)，N 為矩陣長度

{% codeblock lang:cs %}
public class Solution {
    public int MaxOperations(int[] nums, int k) {
        // 統計每個數字出現的次數，key 為該數字，value 為該數字的庫存
         Dictionary<int, int> numCounts = new Dictionary<int, int>();
        int operationCount = 0;

        foreach (int num in nums)
        {
            int remain = k - num;

            // 如果 dict 裡存在 remain，且 remain 的庫存大於 0
            if (numCounts.ContainsKey(remain) && numCounts[remain] > 0)
            {
                // 操作次數 +1，庫存 -1
                operationCount++;
                numCounts[remain]--;
            }
            else
            {
                // 如果 dict 裡還沒有該值，則新增
                if (numCounts.ContainsKey(num) == false)
                {
                    numCounts[num] = 1;
                }
                // 如果 dict 裡已經有該值，則 +1
                else
                {
                    numCounts[num]++;
                }
            }
        }

        return operationCount;
    }
}
{% endcodeblock %}
