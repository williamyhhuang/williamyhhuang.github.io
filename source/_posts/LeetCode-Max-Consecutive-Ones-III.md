title: '[LeetCode] Max Consecutive Ones III
'
author: ''
date: 2023-09-23 23:30:00
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

題目：[Max Consecutive Ones III]()
難度：Medium

<!--more-->

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`'s in the array if you can flip at most `k` `0`'s.

**Example 1:**
> **Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
> **Output:** 6
> **Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**
> **Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
> **Output:** 10
> **Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

- `1 <= nums.length <= 10^5`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

---

能想到這題解法的人真的是天才啊...看到留言區有人說這個解法是藝術真的一點也不為過

解題技巧為 Sliding Window，設定 i、j 兩個 pointer
j 會一直往右走，j 碰到 0 時 `k` 減 1
i 在 `k` 小於 0 且 nums[i] 為 0 時 `k` + 1，使 k 最後不會小於 0，完成後 i 往右走
詳細的迴圈步驟可以看附於參考的 YouTube 連結，這邊就不再贅述

討論串有個[留言](https://leetcode.com/problems/max-consecutive-ones-iii/solutions/247564/java-c-python-sliding-window/comments/326294)提到重點：
> i, j definity NOT the exact index for the largest range!

迴圈跑完後 i 跟 j 並不是最終最大長度的位置，它是相對位置。j 遇到 0 就是無腦的翻成 1，即 k - 1；而 i 則是幫忙還債，幫 j 把多翻的 0 給還給 k

最後有發現作者提供的[解答](https://leetcode.com/problems/max-consecutive-ones-iii/solutions/247564/java-c-python-sliding-window/?envType=study-plan-v2&envId=leetcode-75)，python 版本最後回傳解答是 `j - i + 1`，而 Java、C++ 版本是 `j - i`，原因是 python 版本是跑 foreach 迴圈，Java、C++ 版本每跑完一次迴圈 j 都會 +1，故 python 版本最後要再 +1  

時間複雜度：因會遍歷整個矩陣，故時間複雜度為 O(N)，N 為矩陣長度
空間複雜度：因使用的變數為常數，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int LongestOnes(int[] nums, int k) {
        int i = 0;
        int j = 0;

        while (j < nums.Count())
        {
            // j 遇到 0，將 0 翻成 1，即 k - 1
            if (nums[j] == 0)
            {
                k--;
            }

            if (k < 0)
            {
                // 如果 j 有多翻的 0，i 遇到 0 要還給 k
                if (nums[i] == 0)
                {
                    k++;
                }

                i++;
            }

            j++;
        }

        return j - i;
    }
}
{% endcodeblock %}

## 參考

- [YouTube - LeetCode Max Consecutive Ones III Solution Explained - Java](https://www.youtube.com/watch?v=97oTiOCuxho)
- [YouTube - leetcode 中文 | Max Consecutive Ones III | Sliding Window練習 | Leetcode 1004 | Python](https://www.youtube.com/watch?v=0t8j9pgdolc)
- [LeetCode - Solution 解答參考](https://leetcode.com/problems/max-consecutive-ones-iii/solutions/247564/java-c-python-sliding-window/?envType=study-plan-v2&envId=leetcode-75)
