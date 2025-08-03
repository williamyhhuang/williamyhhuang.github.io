title: '[LeetCode] Product of Array Except Self'
author: ''
date: 2023-08-26 20:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
  - Dynamic Programming
categories:
  - LeetCode
---

題目：[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self)
難度：Medium

<!--more-->

## 我的解法

很暴力的把各種情境都列出來，還有簡化的空間
時間複雜度為 O(N)，N 為矩陣長度；因 total、hasZeroCount 是常數已固定，故空間複雜度為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int total = 1;
        int hasZeroCount = 0;
        int[] result = new int[nums.Length];

        // 找出所有數字相乘的結果，且要排除0
        for (int i = 0; i < nums.Length; i++)
        {
            if (nums[i] != 0)
            {
                total = total * nums[i];
            }
            // 如果找到0，則設定 hasZeroCount
            else
            {
                hasZeroCount = hasZeroCount + 1;
            }
        }

        // 開始組結果
        for (int i = 0; i < nums.Length; i++)
        {
            // 如果當前數字不等於0，且 nums 中不包含0
            if (nums[i] != 0 && hasZeroCount == 0)
            {
                int num = total / nums[i];
                result[i] = num;
            }
            // 如果當前數字不等於0，且 nums 中包含0
            else if (nums[i] != 0 && hasZeroCount > 0)
            {
                result[i] = 0;
            }
            // 如果當前數字等於0，且 nums 中不包含0
            if (nums[i] == 0 && hasZeroCount == 0)
            {
                result[i] = total;
            }
            // 如果當前數字等於0，且 nums 中包含0
            else if (nums[i] == 0 && hasZeroCount > 0)
            {
                // 如果除了當前數字，還有其他 0
                if (hasZeroCount > 1)
                {
                    result[i] = 0;
                }
                // 如果整個 nums 中，只有當前數字為 0
                else
                {
                    result[i] = total;
                }
            }
        }

        return result;
    }
}
{% endcodeblock %}

## Dynamic Programming

看到有解題方式是用 `Dynamic Programming space optimization` 去解
還沒讀到 Dynamic Programming，先把解題方式記起來，之後再來看

{% codeblock lang:cs %}
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int[] output = new int[nums.Length];

        var pre = 1;
        for(int i = 0; i < nums.Length; i++) {
            output[i] = pre;
            pre *= nums[i];
        }

        var post = 1;
        for(int i = nums.Length - 1; i > -1; i--) {
            output[i] *= post;
            post *= nums[i];
        }        

        return output;
    }
}
{% endcodeblock %}
