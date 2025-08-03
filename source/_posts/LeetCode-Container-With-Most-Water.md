title: '[LeetCode] Container With Most Water'
author: ''
date: 2023-09-15 07:30:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目[Container With Most Water](https://leetcode.com/problems/container-with-most-water)
難度：Medium

<!--more-->

時間複雜度：因要遍歷整個矩陣，故為 O(N)
空間複雜度：因變數只有三個數字，兩個 pointer 及一個當前乘載的水量，故為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public int MaxArea(int[] height) {
        // 在頭跟尾定義一個 pointer
        int i = 0;
        int j = height.Count() - 1;
        int result = 0;

        // 跑 while 迴圈，直到兩個 pointer 指到相同位置
        while(i < j)
        {
            // 計算當前 pointer 所能承載的水量
            int water = Math.Min(height[i],height[j]) * (j-i);

            // 如果當前承載的水量大於 result，則置換
            if (water > result)
            {
                result = water;
            }

            // 移動 pointer，哪個小就移動哪個
            if (height[i] < height[j])
            {
                i++;
            }
            else
            {
                j--;
            }
        }

        return result;
    }
}
{% endcodeblock %}
